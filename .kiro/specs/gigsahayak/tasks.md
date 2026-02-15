# Implementation Plan: GigSahayak

## Overview

This implementation plan breaks down the GigSahayak AI-powered WhatsApp assistant into discrete, incremental coding tasks. The approach follows a bottom-up strategy: starting with core data models and infrastructure, then building individual components, and finally integrating them into the complete system. Each task builds on previous work, ensuring no orphaned code.

The implementation uses Python with FastAPI for the backend, PostgreSQL for persistent storage, Redis for caching and session management, and integrates with external services (WhatsApp API, LLM, Vector Database).

## Tasks

- [ ] 1. Set up project structure and core infrastructure
  - Create Python project with FastAPI, SQLAlchemy, Redis client, and testing frameworks
  - Set up database schema and migrations (Alembic)
  - Configure environment variables and secrets management
  - Set up logging and monitoring infrastructure
  - Create Docker configuration for local development
  - _Requirements: 8.1, 8.2, 8.4, 9.2, 9.4_

- [ ] 2. Implement core data models and database layer
  - [ ] 2.1 Create SQLAlchemy models for all database tables
    - Implement WorkerProfile, WelfareScheme, Application, Grievance, Conversation, Message, Feedback, AnalyticsEvent models
    - Add database indexes as specified in design
    - Implement model validation and constraints
    - _Requirements: 1.7, 4.6, 6.5, 9.1, 10.1, 13.3_
  
  - [ ]* 2.2 Write property test for profile persistence
    - **Property 3: Profile Completion Persistence**
    - **Validates: Requirements 1.4, 1.7**
  
  - [ ]* 2.3 Write property test for PII encryption
    - **Property 32: PII Encryption at Rest**
    - **Validates: Requirements 9.1**
  
  - [ ]* 2.4 Write unit tests for model validation
    - Test edge cases for field constraints
    - Test invalid data rejection
    - _Requirements: 1.7, 9.1_

- [ ] 3. Implement Redis caching layer
  - [ ] 3.1 Create Redis client wrapper with connection pooling
    - Implement session management functions
    - Implement profile caching with TTL
    - Implement recommendation caching
    - Implement rate limiting counters
    - _Requirements: 8.4, 11.1, 11.2_
  
  - [ ]* 3.2 Write property test for state persistence
    - **Property 38: State Persistence Across Messages**
    - **Validates: Requirements 11.1**
  
  - [ ]* 3.3 Write property test for state preservation after interruption
    - **Property 39: State Preservation After Interruption**
    - **Validates: Requirements 11.2, 11.3**

- [ ] 4. Implement WhatsApp Interface component
  - [ ] 4.1 Create WhatsApp webhook handler
    - Implement receive_message function to parse incoming webhooks
    - Implement send_text_message function using Twilio/Meta API
    - Implement send_interactive_message for buttons and lists
    - Implement download_media for document uploads
    - Add webhook signature verification for security
    - _Requirements: 1.1, 2.5, 4.3_
  
  - [ ]* 4.2 Write property test for response time
    - **Property 1 (partial): Response within 3 seconds**
    - **Validates: Requirements 1.1**
  
  - [ ]* 4.3 Write property test for language consistency
    - **Property 5: Language Response Consistency**
    - **Validates: Requirements 2.5**
  
  - [ ]* 4.4 Write unit tests for webhook parsing
    - Test various WhatsApp webhook formats
    - Test error handling for malformed webhooks
    - _Requirements: 1.1_

- [ ] 5. Checkpoint - Ensure infrastructure tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 6. Implement AI Engine component with multi-language support
  - [ ] 6.1 Create LLM client wrapper (OpenAI/Groq)
    - Implement process_message function with conversation context
    - Implement detect_language function for Telugu/Hindi/English/code-switching
    - Implement extract_entities function
    - Implement generate_question function for onboarding
    - Add retry logic with exponential backoff
    - Add circuit breaker for LLM API failures
    - _Requirements: 1.3, 2.1, 2.2, 2.3, 2.4, 2.6, 14.1, 14.4_
  
  - [ ]* 6.2 Write property test for multi-language processing
    - **Property 2: Multi-language Processing**
    - **Validates: Requirements 1.3, 2.1, 2.2, 2.3, 2.4**
  
  - [ ]* 6.3 Write property test for AI failure fallback
    - **Property 51: AI Failure Fallback**
    - **Validates: Requirements 14.1**
  
  - [ ]* 6.4 Write property test for API timeout retry
    - **Property 54: API Timeout Retry with Backoff**
    - **Validates: Requirements 14.4**
  
  - [ ]* 6.5 Write unit tests for language detection
    - Test Telugu text detection
    - Test Hindi text detection
    - Test English text detection
    - Test code-switched text detection
    - Test language detection failure edge case
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.6_

- [ ] 7. Implement RAG System component
  - [ ] 7.1 Create vector database client (Pinecone/Weaviate)
    - Implement index_document function
    - Implement retrieve_relevant_docs function
    - Implement update_document and delete_document functions
    - Add caching for frequently retrieved documents
    - _Requirements: 7.1, 7.3, 15.2_
  
  - [ ]* 7.2 Write property test for document retrieval
    - **Property 30: Document Retrieval for Queries**
    - **Validates: Requirements 7.1**
  
  - [ ]* 7.3 Write property test for response grounding
    - **Property 31: Response Grounding in Documents**
    - **Validates: Requirements 7.3**
  
  - [ ]* 7.4 Write property test for RAG unavailability graceful degradation
    - **Property 52: RAG Unavailability Graceful Degradation**
    - **Validates: Requirements 14.2**
  
  - [ ]* 7.5 Write unit tests for vector operations
    - Test document indexing
    - Test similarity search
    - Test edge case: empty query
    - _Requirements: 7.1_

- [ ] 8. Implement Onboarding Engine component
  - [ ] 8.1 Create onboarding flow manager
    - Implement start_onboarding function
    - Implement process_response function with validation
    - Implement get_next_question function with 10-question logic
    - Implement complete_onboarding function
    - Implement resume_onboarding function
    - Add logic to handle clarifying questions without incrementing counter
    - _Requirements: 1.2, 1.4, 1.6, 1.7, 11.3_
  
  - [ ]* 8.2 Write property test for fixed question count
    - **Property 1: Fixed Question Count**
    - **Validates: Requirements 1.2**
  
  - [ ]* 8.3 Write property test for clarifying questions don't count
    - **Property 4: Clarifying Questions Don't Count**
    - **Validates: Requirements 1.6**
  
  - [ ]* 8.4 Write unit tests for onboarding flow
    - Test complete onboarding flow
    - Test interruption and resumption
    - Test validation of responses
    - _Requirements: 1.2, 1.4, 1.6_

- [ ] 9. Checkpoint - Ensure core components tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 10. Implement Matching Engine component
  - [ ] 10.1 Create opportunity matching logic
    - Implement find_opportunities function with eligibility evaluation
    - Implement check_eligibility function for individual schemes
    - Implement get_ranked_recommendations function
    - Implement refresh_recommendations function
    - Add caching for matched opportunities
    - _Requirements: 3.1, 3.8_
  
  - [ ]* 10.2 Write property test for complete opportunity identification
    - **Property 6: Complete Opportunity Identification**
    - **Validates: Requirements 3.1**
  
  - [ ]* 10.3 Write property test for profile change triggers re-evaluation
    - **Property 11: Profile Change Triggers Re-evaluation**
    - **Validates: Requirements 3.8**
  
  - [ ]* 10.4 Write unit tests for eligibility checking
    - Test various eligibility criteria combinations
    - Test edge cases (missing profile fields)
    - _Requirements: 3.1_

- [ ] 11. Implement Recommendation Ranker component
  - [ ] 11.1 Create ranking algorithm
    - Implement score_opportunity function with multi-factor scoring
    - Implement rank_opportunities function
    - Implement apply_ml_scoring function (placeholder for future ML model)
    - Use scoring formula from design: 0.4*eligibility + 0.3*benefit + 0.2*(1-complexity) + 0.1*urgency
    - _Requirements: 3.2, 3.3, 3.4_
  
  - [ ]* 11.2 Write property test for recommendation ranking order
    - **Property 7: Recommendation Ranking Order**
    - **Validates: Requirements 3.2**
  
  - [ ]* 11.3 Write property test for top-N recommendation limit
    - **Property 8: Top-N Recommendation Limit**
    - **Validates: Requirements 3.3**
  
  - [ ]* 11.4 Write property test for pagination preserves ranking
    - **Property 9: Pagination Preserves Ranking**
    - **Validates: Requirements 3.4**
  
  - [ ]* 11.5 Write property test for recommendation completeness
    - **Property 10: Recommendation Completeness**
    - **Validates: Requirements 3.5**
  
  - [ ]* 11.6 Write unit tests for scoring algorithm
    - Test scoring with various input combinations
    - Test edge cases (zero benefit, missing deadline)
    - _Requirements: 3.2_

- [ ] 12. Implement Application Assistant component
  - [ ] 12.1 Create application flow manager
    - Implement start_application function
    - Implement get_next_step function
    - Implement complete_step function with validation
    - Implement prefill_form function using profile data
    - Implement track_application_status function
    - _Requirements: 4.1, 4.4, 4.5, 4.6_
  
  - [ ]* 12.2 Write property test for application guide provision
    - **Property 12: Application Guide Provision**
    - **Validates: Requirements 4.1**
  
  - [ ]* 12.3 Write property test for form pre-filling
    - **Property 15: Form Pre-filling from Profile**
    - **Validates: Requirements 4.4**
  
  - [ ]* 12.4 Write property test for step progression
    - **Property 16: Step Progression**
    - **Validates: Requirements 4.5**
  
  - [ ]* 12.5 Write property test for application tracking initialization
    - **Property 17: Application Tracking Initialization**
    - **Validates: Requirements 4.6**
  
  - [ ]* 12.6 Write unit tests for application flow
    - Test complete application flow
    - Test step validation
    - Test form pre-filling with various profiles
    - _Requirements: 4.1, 4.4, 4.5_

- [ ] 13. Implement Document Helper component
  - [ ] 13.1 Create document validation logic
    - Implement get_document_checklist function
    - Implement validate_document function for format and completeness
    - Implement extract_document_info function (basic OCR if needed)
    - Implement provide_document_guidance function
    - _Requirements: 4.2, 4.3_
  
  - [ ]* 13.2 Write property test for document checklist completeness
    - **Property 13: Document Checklist Completeness**
    - **Validates: Requirements 4.2**
  
  - [ ]* 13.3 Write property test for document validation
    - **Property 14: Document Validation**
    - **Validates: Requirements 4.3**
  
  - [ ]* 13.4 Write unit tests for document validation
    - Test valid document formats (PDF, JPG, PNG)
    - Test invalid formats
    - Test file size limits
    - _Requirements: 4.3_

- [ ] 14. Checkpoint - Ensure application components tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 15. Implement Income Advisor component
  - [ ] 15.1 Create income optimization logic
    - Implement get_income_strategies function
    - Implement get_peak_hours function with demand data
    - Implement get_high_demand_zones function for Hyderabad
    - Implement get_platform_switching_advice function
    - Add seasonal trend incorporation logic
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5, 5.6_
  
  - [ ]* 15.2 Write property test for strategy generation
    - **Property 18: Strategy Generation for Profile**
    - **Validates: Requirements 5.1**
  
  - [ ]* 15.3 Write property test for peak hours based on demand
    - **Property 19: Peak Hours Based on Demand Data**
    - **Validates: Requirements 5.2**
  
  - [ ]* 15.4 Write property test for zone recommendations in Hyderabad
    - **Property 20: Zone Recommendations in Hyderabad**
    - **Validates: Requirements 5.3**
  
  - [ ]* 15.5 Write property test for multi-platform strategy
    - **Property 21: Multi-platform Strategy for Multi-platform Workers**
    - **Validates: Requirements 5.4**
  
  - [ ]* 15.6 Write property test for income tips include estimates
    - **Property 22: Income Tips Include Estimates**
    - **Validates: Requirements 5.5**
  
  - [ ]* 15.7 Write property test for seasonal trend incorporation
    - **Property 23: Seasonal Trend Incorporation**
    - **Validates: Requirements 5.6**
  
  - [ ]* 15.8 Write unit tests for income optimization
    - Test peak hour recommendations
    - Test zone recommendations
    - Test platform switching logic
    - _Requirements: 5.1, 5.2, 5.3, 5.4_

- [ ] 16. Implement Grievance Router component
  - [ ] 16.1 Create grievance handling logic
    - Implement categorize_grievance function using AI
    - Implement get_resolution_steps function
    - Implement route_to_support function
    - Implement track_grievance function
    - Implement detect_patterns function for similar grievances
    - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5, 6.6_
  
  - [ ]* 16.2 Write property test for grievance categorization
    - **Property 24: Grievance Categorization**
    - **Validates: Requirements 6.1**
  
  - [ ]* 16.3 Write property test for resolution guidance provision
    - **Property 25: Resolution Guidance Provision**
    - **Validates: Requirements 6.2**
  
  - [ ]* 16.4 Write property test for escalation routing
    - **Property 26: Escalation Routing**
    - **Validates: Requirements 6.3**
  
  - [ ]* 16.5 Write property test for platform-specific information
    - **Property 27: Platform-Specific Information**
    - **Validates: Requirements 6.4**
  
  - [ ]* 16.6 Write property test for grievance resolution tracking
    - **Property 28: Grievance Resolution Tracking**
    - **Validates: Requirements 6.5**
  
  - [ ]* 16.7 Write property test for pattern detection
    - **Property 29: Pattern Detection and Alerting**
    - **Validates: Requirements 6.6**
  
  - [ ]* 16.8 Write unit tests for grievance handling
    - Test categorization accuracy
    - Test routing logic
    - Test pattern detection with various scenarios
    - _Requirements: 6.1, 6.2, 6.3, 6.6_

- [ ] 17. Implement conversation state management
  - [ ] 17.1 Create conversation manager
    - Implement conversation state tracking in Redis
    - Implement context access for AI responses
    - Implement topic change detection
    - Implement conversation history management
    - _Requirements: 11.1, 11.2, 11.3, 11.4, 11.5_
  
  - [ ]* 17.2 Write property test for context access in responses
    - **Property 40: Context Access in Responses**
    - **Validates: Requirements 11.4**
  
  - [ ]* 17.3 Write property test for topic change detection
    - **Property 41: Topic Change Detection**
    - **Validates: Requirements 11.5**
  
  - [ ]* 17.4 Write unit tests for conversation management
    - Test state updates
    - Test context retrieval
    - Test topic transitions
    - _Requirements: 11.1, 11.4, 11.5_

- [ ] 18. Checkpoint - Ensure advisory and state management tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 19. Implement notification system
  - [ ] 19.1 Create notification scheduler and sender
    - Implement deadline reminder scheduling (3 days before)
    - Implement status change notifications
    - Implement re-engagement trigger (7 days inactive)
    - Implement notification frequency compliance checking
    - Add background worker for scheduled notifications
    - _Requirements: 12.1, 12.3, 12.4, 12.5_
  
  - [ ]* 19.2 Write property test for deadline reminder scheduling
    - **Property 42: Deadline Reminder Scheduling**
    - **Validates: Requirements 12.1**
  
  - [ ]* 19.3 Write property test for status change notifications
    - **Property 43: Status Change Notifications**
    - **Validates: Requirements 12.3**
  
  - [ ]* 19.4 Write property test for re-engagement trigger
    - **Property 44: Re-engagement Trigger**
    - **Validates: Requirements 12.4**
  
  - [ ]* 19.5 Write property test for notification frequency compliance
    - **Property 45: Notification Frequency Compliance**
    - **Validates: Requirements 12.5**
  
  - [ ]* 19.6 Write unit tests for notification scheduling
    - Test reminder scheduling logic
    - Test frequency limiting
    - Test notification delivery
    - _Requirements: 12.1, 12.4, 12.5_

- [ ] 20. Implement feedback collection system
  - [ ] 20.1 Create feedback manager
    - Implement rating request after application completion
    - Implement low rating follow-up logic
    - Implement feedback storage
    - Implement feedback request rate limiting (once per week)
    - Implement rating calculation functions
    - _Requirements: 13.1, 13.2, 13.3, 13.4, 13.5_
  
  - [ ]* 20.2 Write property test for rating request after completion
    - **Property 46: Rating Request After Completion**
    - **Validates: Requirements 13.1**
  
  - [ ]* 20.3 Write property test for low rating follow-up
    - **Property 47: Low Rating Follow-up**
    - **Validates: Requirements 13.2**
  
  - [ ]* 20.4 Write property test for feedback storage
    - **Property 48: Feedback Storage**
    - **Validates: Requirements 13.3**
  
  - [ ]* 20.5 Write property test for feedback request rate limiting
    - **Property 49: Feedback Request Rate Limiting**
    - **Validates: Requirements 13.4**
  
  - [ ]* 20.6 Write property test for rating calculation correctness
    - **Property 50: Rating Calculation Correctness**
    - **Validates: Requirements 13.5**
  
  - [ ]* 20.7 Write unit tests for feedback collection
    - Test rating request timing
    - Test follow-up logic
    - Test rate limiting
    - _Requirements: 13.1, 13.2, 13.4_

- [ ] 21. Implement analytics and monitoring
  - [ ] 21.1 Create analytics event tracking
    - Implement interaction tracking (onboarding, applications, views)
    - Implement recommendation logging
    - Implement anomaly detection logic
    - Implement admin alerting for anomalies
    - Add Prometheus metrics endpoints
    - _Requirements: 10.1, 10.2, 10.6_
  
  - [ ]* 21.2 Write property test for interaction tracking
    - **Property 35: Interaction Tracking**
    - **Validates: Requirements 10.1**
  
  - [ ]* 21.3 Write property test for recommendation logging
    - **Property 36: Recommendation Logging**
    - **Validates: Requirements 10.2**
  
  - [ ]* 21.4 Write property test for anomaly alerting
    - **Property 37: Anomaly Alerting**
    - **Validates: Requirements 10.6**
  
  - [ ]* 21.5 Write unit tests for analytics
    - Test event creation
    - Test metric calculation
    - Test anomaly detection thresholds
    - _Requirements: 10.1, 10.2, 10.6_

- [ ] 22. Implement security and privacy features
  - [ ] 22.1 Create security layer
    - Implement PII encryption at rest (already in models, verify)
    - Implement data deletion function
    - Implement audit logging for data access
    - Add rate limiting middleware
    - Add input validation and sanitization
    - _Requirements: 9.1, 9.3, 9.5_
  
  - [ ]* 22.2 Write property test for data deletion completeness
    - **Property 33: Data Deletion Completeness**
    - **Validates: Requirements 9.3**
  
  - [ ]* 22.3 Write property test for audit logging
    - **Property 34: Audit Logging for Data Access**
    - **Validates: Requirements 9.5**
  
  - [ ]* 22.4 Write unit tests for security features
    - Test encryption/decryption
    - Test data deletion
    - Test audit log creation
    - Test rate limiting
    - _Requirements: 9.1, 9.3, 9.5_

- [ ] 23. Checkpoint - Ensure notification, feedback, and security tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 24. Implement error handling and resilience
  - [ ] 24.1 Create error handling framework
    - Implement matching failure alternative (manual browsing)
    - Implement critical error notification to admins
    - Implement automatic recovery logic
    - Add circuit breaker for external services (already in AI Engine, extend to others)
    - Add comprehensive error response formatting
    - _Requirements: 14.3, 14.5, 14.6_
  
  - [ ]* 24.2 Write property test for matching failure alternative
    - **Property 53: Matching Failure Alternative**
    - **Validates: Requirements 14.3**
  
  - [ ]* 24.3 Write property test for critical error notification
    - **Property 55: Critical Error Notification**
    - **Validates: Requirements 14.5**
  
  - [ ]* 24.4 Write property test for automatic recovery
    - **Property 56: Automatic Recovery**
    - **Validates: Requirements 14.6**
  
  - [ ]* 24.5 Write unit tests for error handling
    - Test various error scenarios
    - Test recovery procedures
    - Test admin notifications
    - _Requirements: 14.3, 14.5, 14.6_

- [ ] 25. Implement administrative interface
  - [ ] 25.1 Create admin API endpoints
    - Implement scheme management endpoints (CRUD)
    - Implement scheme update propagation to RAG
    - Implement content validation before publishing
    - Implement admin authentication and authorization
    - Implement admin action logging
    - _Requirements: 15.1, 15.2, 15.3, 15.4, 15.5_
  
  - [ ]* 25.2 Write property test for scheme update propagation
    - **Property 57: Scheme Update Propagation**
    - **Validates: Requirements 15.2**
  
  - [ ]* 25.3 Write property test for content validation
    - **Property 58: Content Validation Before Publishing**
    - **Validates: Requirements 15.4**
  
  - [ ]* 25.4 Write property test for admin action authentication and logging
    - **Property 59: Admin Action Authentication and Logging**
    - **Validates: Requirements 15.5**
  
  - [ ]* 25.5 Write unit tests for admin interface
    - Test CRUD operations
    - Test validation logic
    - Test authentication
    - _Requirements: 15.1, 15.4, 15.5_

- [ ] 26. Integrate all components into main API
  - [ ] 26.1 Create main FastAPI application
    - Wire WhatsApp webhook endpoint to conversation flow
    - Wire onboarding flow: WhatsApp → AI → Onboarding Engine → Database
    - Wire recommendation flow: Profile → Matching → Ranking → RAG → AI → WhatsApp
    - Wire application flow: Selection → Application Assistant → Document Helper → Database
    - Wire grievance flow: Report → Grievance Router → AI → WhatsApp
    - Wire income advice flow: Request → Income Advisor → AI → WhatsApp
    - Add health check endpoints
    - Add API documentation (OpenAPI/Swagger)
    - _Requirements: All requirements integrated_
  
  - [ ]* 26.2 Write integration tests for end-to-end flows
    - Test complete onboarding flow
    - Test recommendation generation flow
    - Test application submission flow
    - Test grievance handling flow
    - _Requirements: 1.1-1.7, 3.1-3.5, 4.1-4.6, 6.1-6.6_

- [ ] 27. Seed initial data and test with real scenarios
  - [ ] 27.1 Create data seeding scripts
    - Seed 15-20 welfare schemes with eligibility criteria
    - Seed sample worker profiles for testing
    - Seed knowledge base documents for RAG
    - Create test scenarios for each user journey
    - _Requirements: 3.1, 7.1_
  
  - [ ]* 27.2 Perform manual testing with realistic scenarios
    - Test onboarding in Telugu, Hindi, and English
    - Test code-switching scenarios
    - Test recommendation accuracy
    - Test application guidance
    - Test grievance handling
    - _Requirements: 1.1-1.7, 2.1-2.5, 3.1-3.5, 4.1-4.6, 5.1-5.6, 6.1-6.6_

- [ ] 28. Final checkpoint - Comprehensive testing and validation
  - Ensure all tests pass, ask the user if questions arise.
  - Verify all 59 correctness properties are tested
  - Verify all integration tests pass
  - Verify performance targets are met (response time < 3s)
  - Verify security measures are in place

- [ ] 29. Deployment preparation
  - [ ] 29.1 Create deployment configuration
    - Create Docker images for API server and background workers
    - Create Kubernetes manifests or cloud deployment configs
    - Configure environment variables for production
    - Set up database migrations for production
    - Configure monitoring and alerting
    - Create deployment runbook
    - _Requirements: 8.1, 8.2, 8.5, 10.3, 10.5_
  
  - [ ]* 29.2 Perform load testing
    - Test with 100 concurrent users
    - Test with 1000 concurrent recommendation requests
    - Verify response time targets
    - Verify auto-scaling behavior
    - _Requirements: 8.1, 8.2, 8.3_

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation throughout development
- Property tests validate universal correctness properties (minimum 100 iterations each)
- Unit tests validate specific examples, edge cases, and error conditions
- Integration tests validate end-to-end flows across components
- The implementation follows a bottom-up approach: infrastructure → components → integration
- All external services (WhatsApp API, LLM, Vector DB) should be mocked in tests
- Performance testing should be done in a staging environment that mirrors production
