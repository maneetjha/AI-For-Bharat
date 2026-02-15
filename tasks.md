# Implementation Plan: AI Study Abroad Counsellor

## Overview

This implementation plan breaks down the AI Study Abroad Counsellor system into discrete, testable tasks. The system is already partially implemented, so this plan focuses on completing missing features, adding comprehensive testing, and ensuring all correctness properties are validated.

The implementation follows a layered approach:
1. Core infrastructure and utilities
2. Authentication and user management
3. Profile and AI Memory systems
4. AI integration and chat
5. University discovery and recommendations
6. Application management features
7. Testing and validation

## Tasks

- [x] 1. Set up testing infrastructure
  - Install and configure fast-check for property-based testing
  - Set up Jest or Mocha test framework
  - Create test database configuration
  - Set up test fixtures and factories
  - Configure CI pipeline for automated testing
  - _Requirements: Testing Strategy_

- [ ] 2. Implement core authentication system
  - [x] 2.1 Implement password hashing and verification
    - Use bcrypt for secure password hashing
    - Implement password comparison function
    - _Requirements: 1.8_
  
  - [ ]* 2.2 Write property test for password hashing
    - **Property 1: Password hashing invariant**
    - **Validates: Requirements 1.8**
  
  - [x] 2.3 Implement OTP generation and validation
    - Generate random OTP codes
    - Hash OTP codes before storage
    - Set 10-minute expiration
    - Validate OTP codes with expiration check
    - _Requirements: 1.4, 1.6_
  
  - [ ]* 2.4 Write property test for OTP expiration
    - **Property 2: OTP expiration enforcement**
    - **Validates: Requirements 1.4, 1.6**
  
  - [ ]* 2.5 Write property test for email verification flow
    - **Property 3: Email verification round trip**
    - **Validates: Requirements 1.1, 1.5**
  
  - [x] 2.6 Implement JWT token generation and validation
    - Generate JWT tokens with user payload
    - Implement token verification middleware
    - Handle token expiration
    - _Requirements: 1.3, 1.7_
  
  - [ ]* 2.7 Write property test for JWT authentication
    - **Property 4: JWT authentication**
    - **Validates: Requirements 1.3, 1.7**
  
  - [x] 2.8 Implement Google OAuth integration
    - Configure Passport.js Google strategy
    - Handle OAuth callback
    - Create or link user accounts
    - Set emailVerified=true for OAuth users
    - _Requirements: 1.2_
  
  - [ ]* 2.9 Write property test for OAuth email verification
    - **Property 5: OAuth email verification**
    - **Validates: Requirements 1.2**

- [x] 3. Checkpoint - Ensure authentication tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 4. Implement profile management system
  - [x] 4.1 Create profile validation logic
    - Validate all required fields
    - Validate data types and ranges
    - Validate budget values (must be positive)
    - Validate enum values (degree types, test types, etc.)
    - _Requirements: 2.2, 2.7_
  
  - [ ]* 4.2 Write property test for profile validation
    - **Property 7: Profile validation**
    - **Validates: Requirements 2.2**
  
  - [ ]* 4.3 Write property test for budget validation
    - **Property 8: Budget validation**
    - **Validates: Requirements 2.7**
  
  - [x] 4.4 Implement profile CRUD operations
    - Create profile endpoint
    - Update profile endpoint
    - Get profile endpoint
    - Handle preferred countries as array
    - Set onboardingCompleted flag
    - _Requirements: 2.3, 2.4, 2.6_
  
  - [ ]* 4.5 Write property test for profile data persistence
    - **Property 6: Profile data persistence**
    - **Validates: Requirements 2.3**
  
  - [ ]* 4.6 Write property test for onboarding completion
    - **Property 9: Onboarding completion**
    - **Validates: Requirements 2.4**
  
  - [ ]* 4.7 Write property test for preferred countries array
    - **Property 10: Preferred countries array handling**
    - **Validates: Requirements 2.6**
  
  - [-] 4.8 Implement profile hash calculation
    - Create hash function using key profile fields
    - Use SHA-256 for hashing
    - Update hash on profile changes
    - _Requirements: 11.4_
  
  - [ ] 4.9 Implement cache invalidation logic
    - Clear aiEvaluationCache when profile hash changes
    - Clear aiTodosCache when profile hash changes
    - Clear strategies when profile hash changes
    - _Requirements: 2.5, 8.7, 10.4, 11.5_
  
  - [ ]* 4.10 Write property test for cache invalidation
    - **Property 11: Profile hash-based cache invalidation**
    - **Validates: Requirements 2.5, 8.7, 10.4, 11.4, 11.5**

- [ ] 5. Implement AI Memory system
  - [ ] 5.1 Create AI Memory generation logic
    - Extract key facts from profile (max 150 chars)
    - Detect current application stage
    - Extract shortlisted university names
    - Extract locked university names
    - Store key preferences (countries, budget)
    - _Requirements: 4.1, 4.2, 4.3, 4.4_
  
  - [ ]* 5.2 Write property test for condensed profile length
    - **Property 13: AI Memory condensed profile length**
    - **Validates: Requirements 4.1**
  
  - [ ] 5.3 Implement AI Memory update triggers
    - Update on profile changes
    - Update on university lock/unlock
    - Update on recommendation add/remove
    - _Requirements: 4.5, 4.6, 4.7_
  
  - [ ]* 5.4 Write property test for AI Memory synchronization
    - **Property 14: AI Memory synchronization**
    - **Validates: Requirements 4.5, 4.6, 4.7, 7.2, 3.10**
  
  - [ ] 5.5 Implement AI Memory retrieval for chat
    - Get or create AI Memory for user
    - Use AI Memory instead of full profile in chat context
    - _Requirements: 3.2, 4.8_
  
  - [ ]* 5.6 Write property test for AI Memory creation
    - **Property 15: AI Memory creation on first chat**
    - **Validates: Requirements 3.3**
  
  - [ ]* 5.7 Write property test for AI Memory usage
    - **Property 16: AI Memory usage in chat**
    - **Validates: Requirements 3.2, 4.8**

- [ ] 6. Checkpoint - Ensure profile and AI Memory tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 7. Implement chat system
  - [ ] 7.1 Create chat message storage
    - Store user messages with role='user'
    - Store assistant messages with role='assistant'
    - Include timestamp and userId
    - _Requirements: 3.1, 3.7_
  
  - [ ]* 7.2 Write property test for message persistence
    - **Property 17: Chat message persistence**
    - **Validates: Requirements 3.1, 3.7**
  
  - [ ] 7.3 Implement chat context management
    - Retrieve last 10 user-assistant pairs (20 messages)
    - Include AI Memory in context
    - Filter out system messages for Vertex AI
    - _Requirements: 3.8_
  
  - [ ]* 7.4 Write property test for context window
    - **Property 18: Chat context window**
    - **Validates: Requirements 3.8**
  
  - [ ] 7.5 Implement multi-model fallback for chat
    - Try Gemini 2.5 Pro first
    - Fall back to Gemini 2.5 Flash on failure
    - Fall back to Gemini 3 Flash Preview as last resort
    - Detect 404 and HTML errors for fallback
    - _Requirements: 3.4, 3.5, 3.6_
  
  - [ ]* 7.6 Write property test for model fallback
    - **Property 19: AI model fallback chain**
    - **Validates: Requirements 3.5, 3.6, 15.1**
  
  - [ ] 7.7 Implement chat endpoint
    - Accept user message
    - Load AI Memory and chat history
    - Generate AI response with fallback
    - Store both messages
    - Return assistant response
    - _Requirements: 3.1, 3.2, 3.7_

- [ ] 8. Implement university discovery and recommendations
  - [ ] 8.1 Implement Vertex AI Search integration
    - Create two-step search process (Google Search + JSON extraction)
    - Handle search query construction
    - Parse university data from responses
    - _Requirements: 5.1, 20.3_
  
  - [ ] 8.2 Implement recommendation generation
    - Call AI to generate recommendations
    - Parse recommendation data (category, fitScore, etc.)
    - Validate recommendation structure
    - Store recommendations in database
    - _Requirements: 5.2, 5.3, 5.4, 5.5, 5.6, 5.7, 5.8, 5.9_
  
  - [ ]* 8.3 Write property test for recommendation structure
    - **Property 20: Recommendation structure validation**
    - **Validates: Requirements 5.2, 5.3, 5.4, 5.5, 5.6, 5.7**
  
  - [ ]* 8.4 Write property test for cost analysis
    - **Property 23: Cost analysis completeness**
    - **Validates: Requirements 5.8**
  
  - [ ] 8.5 Implement country filtering
    - Filter recommendations by user's preferred countries
    - Reject recommendations outside preferred countries
    - _Requirements: 5.10_
  
  - [ ]* 8.6 Write property test for country filtering
    - **Property 21: Country filtering enforcement**
    - **Validates: Requirements 5.10**
  
  - [ ] 8.7 Implement recommendation uniqueness constraint
    - Enforce unique userId + universityId in database
    - Handle duplicate attempts gracefully
    - _Requirements: 5.11_
  
  - [ ]* 8.8 Write property test for recommendation uniqueness
    - **Property 22: Recommendation uniqueness**
    - **Validates: Requirements 5.11**

- [ ] 9. Implement shortlist management
  - [ ] 9.1 Create shortlist query endpoint
    - Group recommendations by category
    - Sort by fitScore within each category
    - Support filtering by category, country, costLevel
    - _Requirements: 6.3, 6.4, 6.5_
  
  - [ ]* 9.2 Write property test for shortlist grouping and sorting
    - **Property 24: Shortlist grouping and sorting**
    - **Validates: Requirements 6.3, 6.4**
  
  - [ ]* 9.3 Write property test for shortlist filtering
    - **Property 25: Shortlist filtering**
    - **Validates: Requirements 6.5**
  
  - [ ] 9.4 Implement add/remove shortlist operations
    - Add recommendation to shortlist
    - Remove recommendation from shortlist
    - Update AI Memory on changes
    - _Requirements: 6.1, 6.2_

- [ ] 10. Checkpoint - Ensure discovery and shortlist tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 11. Implement university locking and strategies
  - [ ] 11.1 Implement university lock operation
    - Create LockedUniversity record with timestamp
    - Update AI Memory with locked university
    - Trigger strategy generation
    - _Requirements: 7.1, 7.2, 8.1_
  
  - [ ]* 11.2 Write property test for lock side effects
    - **Property 26: Lock operation side effects**
    - **Validates: Requirements 7.1, 7.2, 8.1**
  
  - [ ] 11.3 Implement university unlock operation
    - Delete LockedUniversity record
    - Cascade delete ApplicationTasks
    - Cascade delete UniversityStrategy
    - Update AI Memory
    - _Requirements: 7.3, 7.4_
  
  - [ ]* 11.4 Write property test for unlock cascade
    - **Property 27: Unlock cascade deletion**
    - **Validates: Requirements 7.3, 7.4**
  
  - [ ] 11.5 Implement locked university uniqueness
    - Enforce unique userId + universityId constraint
    - _Requirements: 7.5_
  
  - [ ]* 11.6 Write property test for locked university uniqueness
    - **Property 28: Locked university uniqueness**
    - **Validates: Requirements 7.5**
  
  - [ ] 11.7 Implement application submission tracking
    - Update applicationSubmitted flag
    - Query locked universities with submission status
    - _Requirements: 7.6, 7.7_

- [ ] 12. Implement application strategy generation
  - [ ] 12.1 Create strategy generation prompt
    - Build prompt with user profile and university
    - Request structured JSON response
    - Include sections: content, requiredDocuments, timeline, aiGuidance
    - _Requirements: 8.2, 8.3, 8.4, 8.5_
  
  - [ ] 12.2 Implement strategy generation with AI
    - Call AI with strategy prompt
    - Parse JSON response
    - Validate strategy structure
    - Store in database
    - _Requirements: 8.1, 8.2, 8.3, 8.4, 8.5_
  
  - [ ]* 12.3 Write property test for strategy structure
    - **Property 29: Strategy structure completeness**
    - **Validates: Requirements 8.2, 8.3, 8.4, 8.5**
  
  - [ ] 12.4 Implement strategy caching
    - Check for existing strategy before generating
    - Return cached strategy if exists
    - Invalidate cache on profile changes
    - _Requirements: 8.6, 8.7_
  
  - [ ]* 12.5 Write property test for strategy caching
    - **Property 30: Strategy caching**
    - **Validates: Requirements 8.6**
  
  - [ ] 12.6 Implement strategy uniqueness constraint
    - Enforce unique userId + universityId
    - _Requirements: 8.8_
  
  - [ ]* 12.7 Write property test for strategy uniqueness
    - **Property 31: Strategy uniqueness**
    - **Validates: Requirements 8.8**

- [ ] 13. Implement task management system
  - [ ] 13.1 Create task generation from strategy
    - Parse requiredDocuments from strategy
    - Parse timeline milestones from strategy
    - Create ApplicationTasks for each item
    - Set priorities and categories
    - Mark as AI-generated
    - _Requirements: 9.1, 9.2_
  
  - [ ]* 13.2 Write property test for task creation
    - **Property 32: Task creation from strategy**
    - **Validates: Requirements 9.1**
  
  - [ ] 13.3 Implement task validation
    - Validate priority in {High, Medium, Low}
    - Validate category in {Documentation, Test, SOP, Forms, Other}
    - _Requirements: 9.3, 9.4_
  
  - [ ]* 13.4 Write property test for task field validation
    - **Property 33: Task field validation**
    - **Validates: Requirements 9.3, 9.4**
  
  - [ ] 13.5 Implement task CRUD operations
    - Create task (manual or AI-generated)
    - Update task (mark completed)
    - Delete task
    - Query tasks with filtering and sorting
    - _Requirements: 9.5, 9.6, 9.7, 9.9_
  
  - [ ]* 13.6 Write property test for task AI generation flag
    - **Property 35: Task AI generation flag**
    - **Validates: Requirements 9.6, 9.7**
  
  - [ ]* 13.7 Write property test for task filtering and sorting
    - **Property 36: Task filtering and sorting**
    - **Validates: Requirements 9.9**
  
  - [ ] 13.8 Implement task uniqueness constraint
    - Enforce unique userId + universityId + title
    - _Requirements: 9.8_
  
  - [ ]* 13.9 Write property test for task uniqueness
    - **Property 34: Task uniqueness**
    - **Validates: Requirements 9.8**

- [ ] 14. Implement profile todo system
  - [ ] 14.1 Create profile evaluation with AI
    - Build evaluation prompt with profile data
    - Call AI to analyze profile
    - Generate ProfileTodos from evaluation
    - Cache evaluation and todos
    - _Requirements: 10.1, 11.1, 11.2, 11.3_
  
  - [ ]* 14.2 Write property test for evaluation caching
    - **Property 41: Evaluation caching with timestamp**
    - **Validates: Requirements 11.2, 11.3**
  
  - [ ]* 14.3 Write property test for evaluation structure
    - **Property 42: Evaluation structure**
    - **Validates: Requirements 11.7**
  
  - [ ] 14.4 Implement todo validation
    - Validate category in {Test, Profile, Documents, General}
    - _Requirements: 10.2_
  
  - [ ]* 14.5 Write property test for todo category validation
    - **Property 37: Todo category validation**
    - **Validates: Requirements 10.2**
  
  - [ ] 14.6 Implement todo caching with profile hash
    - Cache todos in aiTodosCache
    - Store profile hash with cache
    - Invalidate on profile changes
    - _Requirements: 10.3, 10.4_
  
  - [ ]* 14.7 Write property test for todo caching
    - **Property 38: Todo caching with profile hash**
    - **Validates: Requirements 10.3**
  
  - [ ] 14.8 Implement todo CRUD operations
    - Create todo (manual or AI-generated)
    - Update todo (mark completed)
    - Delete todo
    - Query todos with filtering
    - _Requirements: 10.5, 10.6, 10.7, 10.8_
  
  - [ ]* 14.9 Write property test for todo AI generation flag
    - **Property 39: Todo AI generation flag**
    - **Validates: Requirements 10.6, 10.7**
  
  - [ ]* 14.10 Write property test for todo filtering
    - **Property 40: Todo filtering**
    - **Validates: Requirements 10.8**

- [ ] 15. Checkpoint - Ensure task and todo tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 16. Implement mock interview system
  - [ ] 16.1 Create interview question generation
    - Build prompt with profile and target universities
    - Generate 3-5 relevant questions
    - Store questions in chat history
    - _Requirements: 12.1_
  
  - [ ]* 16.2 Write property test for question generation
    - **Property 43: Interview question generation**
    - **Validates: Requirements 12.1**
  
  - [ ] 16.3 Implement interview feedback generation
    - Accept user response to question
    - Generate constructive feedback with AI
    - Store feedback in chat history
    - _Requirements: 12.2, 12.4_
  
  - [ ]* 16.4 Write property test for feedback generation
    - **Property 44: Interview feedback generation**
    - **Validates: Requirements 12.2**
  
  - [ ]* 16.5 Write property test for interview persistence
    - **Property 45: Interview persistence**
    - **Validates: Requirements 12.5**

- [ ] 17. Implement SOP assistance system
  - [ ] 17.1 Create SOP outline generation
    - Build prompt for outline request
    - Generate 7-section outline with AI
    - Return structured outline
    - _Requirements: 13.2_
  
  - [ ]* 17.2 Write property test for outline generation
    - **Property 46: SOP outline generation**
    - **Validates: Requirements 13.2**
  
  - [ ] 17.3 Implement SOP content suggestions
    - Generate example content for each section
    - Personalize based on profile
    - _Requirements: 13.3_
  
  - [ ] 17.4 Implement SOP draft review
    - Accept user's SOP draft
    - Generate feedback on strengths and weaknesses
    - Provide specific improvement suggestions
    - _Requirements: 13.4, 13.5_
  
  - [ ]* 17.5 Write property test for SOP review
    - **Property 47: SOP review feedback**
    - **Validates: Requirements 13.4, 13.5**

- [ ] 18. Implement JSON parsing with fallback
  - [ ] 18.1 Create JSON extraction utilities
    - Direct JSON.parse attempt
    - Extract from markdown code blocks
    - Regex pattern matching
    - Handle multiple JSON objects (use first)
    - _Requirements: 14.1, 14.2, 14.3, 14.4_
  
  - [ ]* 18.2 Write property test for markdown extraction
    - **Property 48: JSON extraction from markdown**
    - **Validates: Requirements 14.2**
  
  - [ ]* 18.3 Write property test for fallback chain
    - **Property 49: JSON parsing fallback chain**
    - **Validates: Requirements 14.1, 14.3, 14.5**
  
  - [ ]* 18.4 Write property test for first valid JSON
    - **Property 50: First valid JSON selection**
    - **Validates: Requirements 14.4**
  
  - [ ] 18.5 Implement JSON schema validation
    - Define schemas for each response type
    - Validate parsed JSON against schema
    - Return validation errors
    - _Requirements: 14.6_
  
  - [ ]* 18.6 Write property test for schema validation
    - **Property 51: JSON schema validation**
    - **Validates: Requirements 14.6**

- [ ] 19. Implement comprehensive error handling
  - [ ] 19.1 Create error response formatter
    - Standardize error response structure
    - Include error code, message, and details
    - _Requirements: Error Handling_
  
  - [ ] 19.2 Implement database error handling
    - Catch and log database errors
    - Return appropriate HTTP status codes
    - Handle unique constraint violations
    - Handle foreign key violations
    - _Requirements: 15.3_
  
  - [ ]* 19.3 Write property test for database error handling
    - **Property 52: Database error handling**
    - **Validates: Requirements 15.3**
  
  - [ ] 19.4 Implement external service error handling
    - Handle Vertex AI Search failures gracefully
    - Handle SendGrid failures without blocking
    - Log all external service errors
    - _Requirements: 15.4, 16.4, 16.5_
  
  - [ ]* 19.5 Write property test for external service failures
    - **Property 53: External service failure handling**
    - **Validates: Requirements 15.4**
  
  - [ ]* 19.6 Write property test for email resilience
    - **Property 56: Email sending resilience**
    - **Validates: Requirements 16.4, 16.5**
  
  - [ ] 19.7 Implement input validation error handling
    - Validate all user inputs
    - Return clear field-specific error messages
    - _Requirements: 15.6_
  
  - [ ]* 19.8 Write property test for input validation
    - **Property 54: Input validation errors**
    - **Validates: Requirements 15.6**
  
  - [ ] 19.9 Implement authentication error handling
    - Return 401 for auth failures
    - Provide clear error messages
    - _Requirements: 15.7_
  
  - [ ]* 19.10 Write property test for auth errors
    - **Property 55: Authentication error responses**
    - **Validates: Requirements 15.7**

- [ ] 20. Implement email notification system
  - [ ] 20.1 Create email templates
    - OTP verification email template
    - Password reset email template
    - _Requirements: 16.1, 16.2_
  
  - [ ] 20.2 Implement SendGrid integration
    - Send OTP verification emails
    - Send password reset emails
    - Handle sending failures gracefully
    - _Requirements: 16.1, 16.2, 16.4, 16.5_
  
  - [ ]* 20.3 Write property test for OTP email delivery
    - **Property 57: OTP email delivery**
    - **Validates: Requirements 16.1**

- [ ] 21. Implement session management
  - [ ] 21.1 Configure PostgreSQL session store
    - Set up connect-pg-simple
    - Configure session expiration
    - _Requirements: 17.1, 17.2_
  
  - [ ] 21.2 Implement session lifecycle
    - Create session on login
    - Destroy session on logout
    - Validate session on requests
    - Reject expired sessions
    - _Requirements: 17.2, 17.3, 17.4_
  
  - [ ]* 21.3 Write property test for session creation
    - **Property 58: Session creation on login**
    - **Validates: Requirements 17.2**
  
  - [ ]* 21.4 Write property test for session destruction
    - **Property 59: Session destruction on logout**
    - **Validates: Requirements 17.3**
  
  - [ ]* 21.5 Write property test for expired sessions
    - **Property 60: Expired session rejection**
    - **Validates: Requirements 17.4**

- [ ] 22. Implement database constraints and validation
  - [ ] 22.1 Verify unique constraints in schema
    - User.email, User.googleId
    - Profile.userId
    - UniversityRecommendation userId+universityId
    - LockedUniversity userId+universityId
    - UniversityStrategy userId+universityId
    - ApplicationTask userId+universityId+title
    - AIMemory.userId
    - _Requirements: 19.1, 19.2, 19.3, 19.4, 19.5, 19.6, 19.7_
  
  - [ ]* 22.2 Write property test for unique constraints
    - **Property 61: Unique constraint enforcement**
    - **Validates: Requirements 19.1, 19.2, 19.3, 19.4, 19.5, 19.6, 19.7**
  
  - [ ] 22.3 Verify cascade delete configuration
    - Ensure onDelete: Cascade on all user relations
    - _Requirements: 19.8_
  
  - [ ]* 22.4 Write property test for cascade deletion
    - **Property 62: Cascade deletion**
    - **Validates: Requirements 19.8**

- [ ] 23. Implement university data management
  - [ ] 23.1 Create university slug generation
    - Generate unique slugs from university names
    - Handle special characters and spaces
    - _Requirements: 20.2_
  
  - [ ]* 23.2 Write property test for slug uniqueness
    - **Property 63: University slug uniqueness**
    - **Validates: Requirements 20.2**
  
  - [ ] 23.3 Implement university filtering
    - Filter by country
    - Filter by program
    - Filter by cost range
    - _Requirements: 20.5_
  
  - [ ]* 23.4 Write property test for university filtering
    - **Property 64: University filtering**
    - **Validates: Requirements 20.5**

- [ ] 24. Final checkpoint - Run all tests
  - Run all unit tests
  - Run all property tests (64 properties)
  - Verify 80%+ code coverage
  - Fix any failing tests
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 25. Integration testing
  - [ ]* 25.1 Write integration test for registration flow
    - Test email registration → OTP → verification → login
    - _Requirements: 1.1, 1.4, 1.5, 1.3_
  
  - [ ]* 25.2 Write integration test for OAuth flow
    - Test Google OAuth → account creation → login
    - _Requirements: 1.2, 1.3_
  
  - [ ]* 25.3 Write integration test for onboarding flow
    - Test profile creation → AI Memory generation → evaluation
    - _Requirements: 2.3, 2.4, 3.3, 10.1_
  
  - [ ]* 25.4 Write integration test for discovery flow
    - Test search → recommendations → shortlist → lock
    - _Requirements: 5.1, 5.2, 6.1, 7.1_
  
  - [ ]* 25.5 Write integration test for application flow
    - Test lock → strategy generation → task creation → completion
    - _Requirements: 7.1, 8.1, 9.1, 9.5_
  
  - [ ]* 25.6 Write integration test for chat flow
    - Test message → AI Memory retrieval → response generation → storage
    - _Requirements: 3.1, 3.2, 3.7_

- [ ] 26. Documentation and cleanup
  - Update API documentation with all endpoints
  - Document environment variables
  - Create deployment guide
  - Add code comments for complex logic
  - Update README with setup instructions

## Notes

- Tasks marked with `*` are optional testing tasks and can be skipped for faster MVP
- Each task references specific requirements for traceability
- Property tests validate universal correctness properties across all inputs
- Unit tests validate specific examples and edge cases
- Integration tests validate end-to-end user flows
- The system is partially implemented, so some tasks may involve refactoring existing code
- All AI interactions should use the multi-model fallback strategy for resilience
- Cache invalidation is critical for maintaining data consistency
