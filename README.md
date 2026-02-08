# MONETA-ICT Investment Platform

A comprehensive dual-currency investment platform for Colombia and Peru, featuring real-time balance management, multi-tier investment plans, referral system, and administrative oversight capabilities.

## System Overview

MONETA-ICT provides a complete frontend-first investment management solution with country-specific features for Colombia (COP) and Peru (PEN). The platform supports deposit management, investment tracking, withdrawal processing, referral programs, and administrative monitoring.

## Technical Architecture

### Core Technologies

The application is built using React 18 with Vite as the build tool, providing optimal development experience and production performance. Routing is handled through React Router v6, ensuring smooth navigation and proper route protection. The styling system combines Tailwind CSS with custom utility classes for a consistent, professional appearance across all components.

State management utilizes React Context API for global authentication and application state, with localStorage providing persistence for user sessions and transaction data. Form validation leverages comprehensive custom validators with real-time feedback. Toast notifications through React Hot Toast ensure users receive immediate feedback on all operations.

### Project Structure

The codebase follows a clear organizational pattern with distinct directories for components, pages, contexts, utilities, hooks, and data. Components are categorized into layout elements, shared UI components, and form-specific components. Pages represent the main application views, while contexts manage global state for authentication and application data.

## Feature Completeness

### Phase 1-2: Authentication System

The authentication system provides complete user registration and login functionality with country-specific validation. Colombian users register with phone numbers in the format +57 followed by ten digits, while Peruvian users use +51 followed by nine digits. Email validation ensures uniqueness across the platform. Users receive a welcome bonus specific to their country (12,000 COP or 10 PEN) which becomes available upon first deposit approval.

### Phase 3-4: Dashboard and Investment Plans

The dashboard presents users with three primary statistics cards showing available balance, active investments, and total earnings. Activity tables display recent transactions with appropriate color coding and status indicators. Quick action buttons provide immediate access to deposit, investment, and withdrawal functions.

Twelve investment plans cater to different investor profiles, ranging from the Starter plan at 50,000 COP (130 PEN) through to the VIP plan at 10,000,000 COP (26,000 PEN). Each plan specifies minimum investment requirements, daily return rates, duration periods, and total return projections. The Gold plan features prominent highlighting as the most popular option.

### Phase 5-6: Deposits and Withdrawals

The deposit system requires users to submit payment proof along with their deposit amount. All deposits enter a pending state awaiting administrative approval, with balances updating only after explicit approval. The withdrawal system collects complete banking information including bank name, account number, and account type. Withdrawals similarly enter pending status without immediate balance deduction, maintaining security and enabling proper verification processes.

### Phase 7-8: Transaction History and Referrals

Complete transaction history provides filtering by transaction type, search by amount, and pagination for large datasets. Each transaction displays comprehensive details including date, type, amount, status, and additional information such as bank details for withdrawals or proof attachment indicators for deposits.

The referral system generates unique six-character codes for each user, creating shareable registration links that pre-populate referral codes. Users can track their referrals, view earned bonuses, and monitor referral activity through a dedicated interface.

### Phase 9: Administrative Panel

The administrative panel provides read-only oversight of system operations. Administrators can view all users, transactions, and investments across the platform. The interface explicitly indicates that approvals must be processed through the Telegram bot integration, maintaining the manual approval workflow specified in the system design.

## Country-Specific Features

### Colombia Configuration

Colombian users operate exclusively in Pesos Colombianos (COP) with amounts formatted as "$ 50.000 COP" using period separators for thousands without decimal places. Minimum deposit requirements are set at 40,000 COP with minimum withdrawals of 25,000 COP. Banking integration references Bancolombia as the primary institution.

### Peru Configuration

Peruvian users work exclusively with Soles Peruanos (PEN), formatted as "S/ 130.00 PEN" using comma separators for thousands with two decimal places. Minimum deposits require 35 PEN with minimum withdrawals of 22 PEN. The system integrates with BCP (Banco de Crédito del Perú) as the primary banking institution.

## Installation and Setup

To initialize the development environment, navigate to the project directory and install dependencies using npm install. Start the development server with npm run dev, which launches the application at http://localhost:5173. For production deployment, execute npm run build to generate optimized static assets in the dist directory.

## Testing and Quality Assurance

The project includes comprehensive testing infrastructure through Vitest. Execute npm test to run the test suite, npm run test:ui to access the visual test interface, and npm run test:coverage to generate coverage reports. ESLint configuration ensures code quality and consistency across the codebase.

## Deployment Considerations

The application is configured for static hosting platforms including Netlify, Vercel, and traditional web servers. The build process generates fully optimized assets with appropriate chunking for optimal loading performance. Environment-specific configurations should be managed through build-time environment variables.

## Backend Integration Preparation

While currently operating with localStorage for data persistence, the application is designed for seamless integration with a REST API backend. The architecture separates data management into context providers, making it straightforward to replace localStorage calls with API requests. Comprehensive API specifications are provided in the documentation directory.

## Security Considerations

All authentication flows include proper validation and error handling. Route protection prevents unauthorized access to restricted pages. File uploads include type and size validation. In production deployment, additional security measures should include HTTPS enforcement, rate limiting, CORS configuration, and proper session management.

## Support and Documentation

Detailed documentation covers deployment procedures, backend integration specifications, testing methodologies, and component usage guidelines. For technical support or questions about implementation, consult the comprehensive documentation in the docs directory.
