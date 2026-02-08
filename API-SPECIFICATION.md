# Backend API Specification - MONETA-ICT Platform

## API Architecture Overview

The MONETA-ICT platform backend follows RESTful API design principles with JWT-based authentication. The API provides endpoints for user management, transaction processing, investment operations, and administrative functions. All responses follow consistent JSON formatting with appropriate HTTP status codes indicating operation results.

## Authentication System

### User Registration

The registration endpoint accepts POST requests to /api/auth/register with a request body containing the user's full name, email address, phone number (formatted according to country), country code (CO or PE), password, and optional referral code. The system validates email uniqueness, phone number format according to country specifications, password strength requirements, and referral code validity if provided.

Successful registration returns a 201 Created status with the response body containing the user object including unique user ID, name, email, phone, country, generated referral code, balance initialized to zero, and creation timestamp. The response also includes JWT access and refresh tokens for immediate authentication. Error responses include 400 Bad Request for validation failures and 409 Conflict for duplicate email addresses.

### User Login

The login endpoint accepts POST requests to /api/auth/login with email and password credentials. The system validates credentials against stored hashed passwords using bcrypt comparison. Successful authentication returns a 200 OK status with user details and both access and refresh tokens. Failed authentication returns 401 Unauthorized with an appropriate error message.

### Token Refresh

The refresh endpoint at /api/auth/refresh accepts POST requests with a valid refresh token. Successful validation returns new access and refresh tokens, enabling continuous authenticated sessions. Expired or invalid tokens return 401 Unauthorized responses.

## User Profile Management

The profile endpoint at /api/user/profile requires authentication through the Authorization header containing the Bearer token. GET requests retrieve complete user information including account details, balance, referral statistics, and account status. PATCH requests update modifiable user information such as phone number and notification preferences. The system prevents modification of email addresses and referral codes after account creation.

## Transaction Operations

### Deposit Requests

Creating deposit requests requires POST requests to /api/deposits with authenticated user context. The request body includes the deposit amount, currency code, and base64-encoded proof of payment. The system validates minimum deposit requirements according to country specifications, accepted file types for proof uploads, and file size limitations.

Successful creation returns 201 Created with the transaction object including unique transaction ID, user ID, deposit amount, currency, proof URL, status set to pending, and creation timestamp. The deposit does not affect account balance until administrative approval. Validation failures return 400 Bad Request responses.

Administrative deposit approval processes through PATCH requests to /api/admin/deposits/:id with status updates. Approval updates the user balance, changes transaction status to approved, records approval timestamp and administrator ID. Rejection updates transaction status with optional administrator notes explaining the rejection reason.

### Withdrawal Requests

Withdrawal creation accepts POST requests to /api/withdrawals with the withdrawal amount, destination bank name, account number, and account type. Validation ensures the withdrawal amount does not exceed available balance, meets minimum withdrawal requirements, and includes complete banking information. The system creates pending withdrawal records without immediate balance deduction.

Administrative withdrawal processing through /api/admin/withdrawals/:id PATCH requests handles approval or rejection. Approval deducts the amount from user balance, updates transaction status to approved, and triggers payment processing workflows. The system records complete audit trails including administrator identity and processing timestamps.

### Investment Creation

Investment creation through POST requests to /api/investments requires plan ID selection and investment amount specification. The system validates that the investment amount meets plan minimum requirements, confirms sufficient account balance, and verifies plan availability. Successful creation immediately deducts the investment amount from available balance, creates the investment record with active status, calculates end date based on plan duration, and initializes daily return tracking.

The investment object includes unique investment ID, user and plan IDs, investment amount, daily return amount, start and end dates, current status, and creation timestamp. The system automatically processes daily returns according to investment schedules.

## Administrative Endpoints

### User Management

The administrative user list endpoint at /api/admin/users provides comprehensive user information including registration details, current balances, investment summaries, and account status. Query parameters enable filtering by country, account status, and registration date ranges. Pagination supports large user bases with configurable page sizes.

Individual user details through /api/admin/users/:id include complete transaction history, active investments, referral information, and detailed account activity. Administrative actions enable account status modification, balance adjustments with audit logging, and user communication.

### Transaction Monitoring

The administrative transactions endpoint at /api/admin/transactions aggregates all system transactions with filtering capabilities by transaction type, status, user, date range, and amount thresholds. The endpoint supports sorting by various fields and includes pagination for efficient data retrieval. Each transaction record includes complete user information, transaction details, current status, and associated metadata.

### System Statistics

The statistics endpoint at /api/admin/stats provides real-time system metrics including total user count and growth rates, pending deposit and withdrawal counts, active investment statistics and total amounts, daily revenue calculations, and user acquisition through referral programs. These metrics support administrative oversight and business intelligence requirements.

## Telegram Bot Integration

The Telegram bot receives webhook notifications for pending approval transactions. Deposit approval requests include user details, deposit amount with currency, proof of payment image, and timestamp. Interactive buttons enable immediate approval or rejection responses.

Withdrawal approval requests present user information, withdrawal amount with currency, complete banking details for verification, and processing actions. Administrative responses through Telegram buttons trigger corresponding API calls to update transaction status and user balances.

The webhook implementation at /api/webhooks/telegram verifies request authenticity, processes approval and rejection commands, updates database records accordingly, sends email notifications to affected users, and logs all actions for audit purposes.

## Security Implementation

All API endpoints implement comprehensive security measures. JWT tokens use secure signing algorithms with reasonable expiration times. Refresh tokens enable session extension without repeated credential submission. Rate limiting prevents abuse through excessive request volume. Input sanitization protects against injection attacks. CORS configuration restricts API access to authorized domains.

Sensitive operations require additional verification including balance modifications, withdrawal approvals, and administrative actions. All password storage uses bcrypt hashing with appropriate salt rounds. File uploads undergo type validation, size restrictions, and malware scanning before storage.

## Error Handling

The API implements consistent error response formatting with HTTP status codes indicating error categories. Response bodies include error codes for programmatic handling, human-readable messages in Spanish, detailed field-level validation errors when applicable, and request identifiers for support purposes.

Common status codes include 400 for bad requests with invalid input, 401 for authentication failures, 403 for insufficient permissions, 404 for resource not found, 409 for conflicts like duplicate emails, 422 for unprocessable entities with validation failures, 429 for rate limit exceeded, and 500 for internal server errors.

## Data Models

The database schema includes users table with authentication credentials, profile information, balance tracking, and status indicators. Transactions table records all financial operations with type classification, amount and currency, status tracking, and reference fields. Investments table maintains investment records with plan associations, return calculations, duration tracking, and completion status.

Additional tables support referrals tracking, daily returns scheduling, notification management, and audit logging. Foreign key relationships maintain referential integrity across tables. Indexes optimize query performance for common access patterns.

## Rate Limiting

API endpoints implement rate limiting to prevent abuse and ensure fair resource allocation. Authentication endpoints allow five attempts per email per fifteen-minute window. Transaction creation endpoints limit to ten requests per user per hour. Profile updates permit five changes per hour. Administrative endpoints enforce stricter limits based on administrator roles.

Exceeding rate limits results in 429 Too Many Requests responses with headers indicating limit reset times. Repeated limit violations may trigger additional security reviews.

## Testing and Documentation

Comprehensive API documentation includes endpoint descriptions, request and response schemas, authentication requirements, error scenarios, and example requests. Interactive API documentation through Swagger or similar tools enables testing and integration development. Automated test suites verify endpoint functionality, authentication flows, authorization rules, input validation, and error handling.
