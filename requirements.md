# Requirements Document: AI Study Abroad Counsellor

## Introduction

The AI Study Abroad Counsellor is a comprehensive platform that guides students through the study abroad application process using AI-powered counseling. The system provides personalized university recommendations, application strategy generation, task management, and document preparation assistance. It leverages Google Vertex AI (Gemini models) to deliver context-aware guidance based on student profiles, preferences, and application progress.

## Glossary

- **System**: The AI Study Abroad Counsellor platform
- **User**: A student using the platform to apply for study abroad programs
- **Profile**: A user's academic and personal information used for recommendations
- **AI_Counselor**: The AI-powered chat assistant using Google Vertex AI
- **University_Recommendation**: An AI-generated university suggestion with fit analysis
- **Shortlist**: A collection of university recommendations saved by the user
- **Locked_University**: A university the user has committed to applying to
- **Application_Strategy**: An AI-generated plan for applying to a specific university
- **AI_Memory**: A condensed context system for token optimization in AI interactions
- **Profile_Todo**: A task related to improving the user's profile (tests, documents)
- **Application_Task**: A task specific to a university application
- **Fit_Score**: A 0-100 numerical assessment of how well a university matches the user
- **Category**: University classification as Dream (reach), Target (match), or Safe (safety)
- **Mock_Interview**: AI-powered practice interview for university admissions
- **SOP**: Statement of Purpose document for university applications
- **Vertex_AI_Search**: External service for university discovery and information retrieval

## Requirements

### Requirement 1: User Authentication and Account Management

**User Story:** As a prospective student, I want to create an account and log in securely, so that I can access personalized counseling services.

#### Acceptance Criteria

1. WHEN a user registers with email and password, THE System SHALL create a user account with email verification required
2. WHEN a user registers via Google OAuth, THE System SHALL create a user account with automatic email verification
3. WHEN a user provides valid login credentials, THE System SHALL authenticate the user and issue a JWT token
4. WHEN a user requests email verification, THE System SHALL send an OTP code valid for 10 minutes
5. WHEN a user submits a valid OTP code, THE System SHALL mark the email as verified
6. WHEN a user's OTP expires, THE System SHALL reject the verification attempt and require a new OTP
7. WHEN an authenticated user accesses protected routes, THE System SHALL validate the JWT token
8. THE System SHALL store passwords using secure hashing algorithms
9. THE System SHALL maintain user sessions using PostgreSQL-backed session storage

### Requirement 2: User Profile and Onboarding

**User Story:** As a new user, I want to complete my academic profile, so that I can receive personalized university recommendations.

#### Acceptance Criteria

1. WHEN a user completes registration, THE System SHALL prompt the user to complete onboarding
2. WHEN a user submits profile information, THE System SHALL validate all required fields
3. THE System SHALL store the following profile data: current degree, education level, major, GPA, CGPA scale, graduation year, intended degree, target field, target intake year and season, work experience, English test status and scores, GRE/GMAT status and scores, funding type, budget range with currency, preferred countries, research experience, and document statuses
4. WHEN a user completes the profile, THE System SHALL mark onboarding as completed
5. WHEN a user updates their profile, THE System SHALL invalidate cached AI evaluations and todos
6. THE System SHALL support multiple preferred countries as an array
7. THE System SHALL enforce budget values to be positive numbers when provided

### Requirement 3: AI-Powered Counseling Chat

**User Story:** As a user, I want to chat with an AI counselor, so that I can get personalized guidance throughout my application journey.

#### Acceptance Criteria

1. WHEN a user sends a chat message, THE System SHALL store the message with timestamp and role
2. WHEN processing a chat request, THE AI_Counselor SHALL retrieve the user's AI_Memory for context
3. WHEN the AI_Memory does not exist, THE System SHALL generate it from the user's profile and application state
4. WHEN generating a response, THE AI_Counselor SHALL use Gemini 2.5 Pro as the primary model
5. IF Gemini 2.5 Pro fails, THEN THE AI_Counselor SHALL fall back to Gemini 2.5 Flash
6. IF Gemini 2.5 Flash fails, THEN THE AI_Counselor SHALL fall back to Gemini 3 Flash Preview
7. WHEN the AI generates a response, THE System SHALL store it as an assistant message
8. THE AI_Counselor SHALL maintain conversation context using the last 10 messages plus AI_Memory
9. THE AI_Counselor SHALL provide guidance on university selection, application strategy, test preparation, and document writing
10. WHEN a user's profile or application state changes significantly, THE System SHALL update the AI_Memory

### Requirement 4: AI Memory System for Token Optimization

**User Story:** As the system, I want to maintain condensed user context, so that AI interactions remain efficient and cost-effective.

#### Acceptance Criteria

1. WHEN a user's AI_Memory is created or updated, THE System SHALL generate a condensed profile summary of maximum 150 characters
2. THE AI_Memory SHALL store the user's current application stage
3. THE AI_Memory SHALL maintain arrays of shortlisted and locked university names
4. THE AI_Memory SHALL store key preferences including countries and budget range
5. WHEN the user's profile changes, THE System SHALL regenerate the condensed profile summary
6. WHEN the user locks or unlocks universities, THE System SHALL update the locked universities array
7. WHEN the user adds or removes recommendations, THE System SHALL update the shortlisted universities array
8. THE System SHALL use AI_Memory instead of full profile data for chat context

### Requirement 5: University Discovery and Recommendations

**User Story:** As a user, I want to discover universities that match my profile, so that I can build a balanced application list.

#### Acceptance Criteria

1. WHEN a user requests university recommendations, THE System SHALL query Vertex_AI_Search with the user's profile
2. WHEN generating recommendations, THE AI_Counselor SHALL categorize universities as Dream, Target, or Safe
3. WHEN generating recommendations, THE AI_Counselor SHALL calculate a Fit_Score from 0 to 100
4. WHEN generating recommendations, THE AI_Counselor SHALL provide a "whyItFits" explanation
5. WHEN generating recommendations, THE AI_Counselor SHALL identify risk factors as an array
6. WHEN generating recommendations, THE AI_Counselor SHALL assess acceptance chance as Low, Medium, or High
7. WHEN generating recommendations, THE AI_Counselor SHALL assess cost level as Low, Medium, or High
8. WHEN generating recommendations, THE AI_Counselor SHALL provide cost analysis with tuition, living costs, and currency
9. WHEN generating recommendations, THE AI_Counselor SHALL suggest next steps for each university
10. THE System SHALL enforce strict country filtering based on user's preferred countries
11. THE System SHALL prevent duplicate recommendations for the same user and university
12. WHEN a recommendation is created, THE System SHALL optionally link it to the chat message that generated it

### Requirement 6: University Shortlist Management

**User Story:** As a user, I want to save and manage my university shortlist, so that I can track universities I'm interested in.

#### Acceptance Criteria

1. WHEN a user saves a university recommendation, THE System SHALL add it to the user's shortlist
2. WHEN a user removes a university from shortlist, THE System SHALL delete the recommendation
3. WHEN a user views their shortlist, THE System SHALL return recommendations grouped by category
4. WHEN a user views their shortlist, THE System SHALL sort universities by Fit_Score within each category
5. THE System SHALL allow users to filter shortlist by category, country, or cost level

### Requirement 7: University Locking and Application Commitment

**User Story:** As a user, I want to lock universities I'm committed to applying to, so that I can access application-specific features.

#### Acceptance Criteria

1. WHEN a user locks a university, THE System SHALL create a Locked_University record with timestamp
2. WHEN a user locks a university, THE System SHALL update the AI_Memory with the locked university name
3. WHEN a user unlocks a university, THE System SHALL delete the Locked_University record
4. WHEN a user unlocks a university, THE System SHALL delete associated Application_Tasks and University_Strategy
5. THE System SHALL prevent duplicate locked universities for the same user
6. WHEN a user marks an application as submitted, THE System SHALL update the applicationSubmitted flag
7. THE System SHALL allow users to view all locked universities with submission status

### Requirement 8: Application Strategy Generation

**User Story:** As a user, I want an AI-generated application strategy for each locked university, so that I have a clear roadmap for my application.

#### Acceptance Criteria

1. WHEN a user locks a university, THE System SHALL generate an Application_Strategy using the AI_Counselor
2. THE Application_Strategy SHALL include detailed strategy text
3. THE Application_Strategy SHALL include a list of required documents as JSON
4. THE Application_Strategy SHALL include a timeline with milestones as JSON
5. THE Application_Strategy SHALL include AI guidance for SOP, exams, and forms as JSON
6. WHEN a strategy exists for a university, THE System SHALL cache it and return the cached version
7. WHEN a user's profile changes significantly, THE System SHALL invalidate cached strategies
8. THE System SHALL prevent duplicate strategies for the same user and university
9. WHEN a user views a strategy, THE System SHALL return the complete strategy with all components

### Requirement 9: Application Task Management

**User Story:** As a user, I want to manage tasks for each university application, so that I can track my progress and deadlines.

#### Acceptance Criteria

1. WHEN a strategy is generated, THE AI_Counselor SHALL create Application_Tasks for the locked university
2. WHEN creating a task, THE System SHALL store title, description, priority, category, and deadline hint
3. THE System SHALL support task priorities: High, Medium, Low
4. THE System SHALL support task categories: Documentation, Test, SOP, Forms, Other
5. WHEN a user marks a task as completed, THE System SHALL update the completed flag
6. WHEN a user creates a manual task, THE System SHALL mark isAiGenerated as false
7. WHEN the AI creates a task, THE System SHALL mark isAiGenerated as true
8. THE System SHALL prevent duplicate tasks for the same user, university, and title
9. WHEN a user views tasks, THE System SHALL filter by university and sort by priority and deadline

### Requirement 10: Profile Todo Generation and Management

**User Story:** As a user, I want AI-generated todos for improving my profile, so that I can strengthen my applications.

#### Acceptance Criteria

1. WHEN a user requests profile evaluation, THE AI_Counselor SHALL analyze the profile and generate Profile_Todos
2. THE System SHALL categorize todos as: Test, Profile, Documents, or General
3. WHEN todos are generated, THE System SHALL cache them with the profile hash
4. WHEN the profile changes, THE System SHALL invalidate the cached todos
5. WHEN a user marks a todo as completed, THE System SHALL update the completed flag
6. WHEN a user creates a manual todo, THE System SHALL mark isAiGenerated as false
7. WHEN the AI creates a todo, THE System SHALL mark isAiGenerated as true
8. THE System SHALL allow users to view todos filtered by category and completion status

### Requirement 11: Profile Evaluation and Caching

**User Story:** As a user, I want my profile evaluated by AI, so that I understand my strengths and areas for improvement.

#### Acceptance Criteria

1. WHEN a user requests profile evaluation, THE AI_Counselor SHALL analyze the profile comprehensively
2. THE System SHALL cache the evaluation result in the Profile model
3. THE System SHALL store the evaluation timestamp
4. THE System SHALL generate a profile hash based on key profile fields
5. WHEN the profile hash changes, THE System SHALL invalidate the cached evaluation
6. WHEN a cached evaluation exists and is valid, THE System SHALL return it without calling the AI
7. THE evaluation SHALL include strengths, weaknesses, and improvement recommendations

### Requirement 12: Mock Interview Preparation

**User Story:** As a user, I want to practice interviews with AI, so that I can prepare for university admissions interviews.

#### Acceptance Criteria

1. WHEN a user requests a mock interview, THE AI_Counselor SHALL generate interview questions based on the user's profile and target universities
2. WHEN a user responds to interview questions, THE AI_Counselor SHALL provide feedback on the response
3. THE AI_Counselor SHALL simulate common interview scenarios for study abroad applications
4. THE AI_Counselor SHALL provide tips for improving interview performance
5. WHEN an interview session ends, THE System SHALL store the conversation in chat history

### Requirement 13: Statement of Purpose (SOP) Generation

**User Story:** As a user, I want AI assistance in writing my SOP, so that I can create compelling application documents.

#### Acceptance Criteria

1. WHEN a user requests SOP help, THE AI_Counselor SHALL provide guidance based on the user's profile and target university
2. THE AI_Counselor SHALL generate SOP outlines with key sections
3. THE AI_Counselor SHALL provide example content for each SOP section
4. THE AI_Counselor SHALL review user-written SOP drafts and provide feedback
5. THE AI_Counselor SHALL suggest improvements for clarity, structure, and impact

### Requirement 14: JSON Response Parsing with Fallback

**User Story:** As the system, I want robust JSON parsing from AI responses, so that structured data is reliably extracted.

#### Acceptance Criteria

1. WHEN the AI returns a response, THE System SHALL attempt to parse JSON from the response
2. WHEN JSON is wrapped in markdown code blocks, THE System SHALL extract and parse it
3. WHEN JSON parsing fails, THE System SHALL attempt to extract JSON using regex patterns
4. WHEN multiple JSON objects are present, THE System SHALL parse the first valid object
5. WHEN all parsing attempts fail, THE System SHALL log the error and return a structured error response
6. THE System SHALL validate parsed JSON against expected schemas before using it

### Requirement 15: Error Handling and Fallback Mechanisms

**User Story:** As the system, I want comprehensive error handling, so that the application remains stable and provides helpful feedback.

#### Acceptance Criteria

1. WHEN an AI model fails, THE System SHALL attempt fallback models in order: Gemini 2.5 Pro → Gemini 2.5 Flash → Gemini 3 Flash Preview
2. WHEN all AI models fail, THE System SHALL return a user-friendly error message
3. WHEN database operations fail, THE System SHALL log the error and return appropriate HTTP status codes
4. WHEN external services (Vertex AI Search, SendGrid) fail, THE System SHALL handle gracefully with fallback behavior
5. WHEN JSON parsing fails, THE System SHALL log the raw response for debugging
6. THE System SHALL validate all user inputs and return clear validation error messages
7. WHEN authentication fails, THE System SHALL return 401 Unauthorized with clear error messages

### Requirement 16: Email Notifications

**User Story:** As a user, I want to receive email notifications, so that I stay informed about important events.

#### Acceptance Criteria

1. WHEN a user registers with email, THE System SHALL send an OTP verification email using SendGrid
2. WHEN a user requests password reset, THE System SHALL send a reset link via email
3. THE System SHALL use email templates for consistent branding
4. WHEN email sending fails, THE System SHALL log the error and inform the user
5. THE System SHALL not block user operations if email sending fails

### Requirement 17: Session Management

**User Story:** As a user, I want my session to persist across browser sessions, so that I don't have to log in repeatedly.

#### Acceptance Criteria

1. THE System SHALL use PostgreSQL-backed session storage
2. WHEN a user logs in, THE System SHALL create a session with configurable expiration
3. WHEN a user logs out, THE System SHALL destroy the session
4. WHEN a session expires, THE System SHALL require re-authentication
5. THE System SHALL support concurrent sessions from different devices

### Requirement 18: Data Indexing and Query Performance

**User Story:** As the system, I want optimized database queries, so that the application responds quickly.

#### Acceptance Criteria

1. THE System SHALL maintain indexes on Profile fields: targetField, intendedDegree, targetIntakeYear, budgetMin, budgetMax, preferredCountries
2. THE System SHALL maintain indexes on ChatMessage fields: userId and createdAt
3. THE System SHALL maintain indexes on ApplicationTask fields: userId and universityId
4. THE System SHALL maintain indexes on UniversityStrategy fields: userId and universityId
5. THE System SHALL maintain indexes on UniversityRecommendation fields: userId and category
6. THE System SHALL maintain indexes on ProfileTodo field: userId

### Requirement 19: Data Validation and Constraints

**User Story:** As the system, I want to enforce data integrity, so that the database remains consistent.

#### Acceptance Criteria

1. THE System SHALL enforce unique constraints on User email and googleId
2. THE System SHALL enforce unique constraints on Profile userId
3. THE System SHALL enforce unique constraints on UniversityRecommendation for userId and universityId pairs
4. THE System SHALL enforce unique constraints on LockedUniversity for userId and universityId pairs
5. THE System SHALL enforce unique constraints on UniversityStrategy for userId and universityId pairs
6. THE System SHALL enforce unique constraints on ApplicationTask for userId, universityId, and title combinations
7. THE System SHALL enforce unique constraints on AIMemory userId
8. WHEN a user is deleted, THE System SHALL cascade delete all related records

### Requirement 20: University Data Management

**User Story:** As the system, I want to maintain university information, so that accurate data is available for recommendations.

#### Acceptance Criteria

1. THE System SHALL store university data including name, country, programs, tuition, and other relevant information
2. THE System SHALL use university slugs as unique identifiers
3. THE System SHALL integrate with Vertex_AI_Search for university discovery
4. WHEN university data is updated, THE System SHALL reflect changes in new recommendations
5. THE System SHALL support filtering universities by country, program, and cost range
