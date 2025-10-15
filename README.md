# KarooSync

KarooSync is a React-based WordPress/WooCommerce synchronization application that enables centralized management of e-commerce store data through a modern web dashboard. The application connects to WordPress stores via OAuth authentication and provides real-time synchronization capabilities for products, categories, and store metadata.

## What KarooSync Does

KarooSync serves as a bridge between WordPress/WooCommerce stores and a centralized management interface. It allows users to:

- **Connect WordPress Stores**: Establishes secure OAuth connections to WordPress/WooCommerce installations
- **Synchronize Store Data**: Performs asynchronous synchronization of products, categories, attributes, and metadata
- **Manage Products**: Provides full CRUD operations for products including variations, categories, and attributes
- **Track Sync Progress**: Real-time monitoring of synchronization operations with detailed progress reporting
- **Export Data**: Creates backups and exports store data in multiple formats
- **Handle Subscriptions**: Manages user subscriptions through Stripe integration

## Architecture Overview

### Frontend Architecture

The frontend is a React Single Page Application (SPA) built with modern React patterns:

**Core Framework**: React 19.1.0 with Create React App
**Routing**: React Router v7 for client-side navigation
**Authentication**: AWS Cognito integration with JWT token management
**State Management**: React Context API for global state
**Styling**: Tailwind CSS with responsive design
**API Communication**: Centralized API client with retry logic and error handling

**Key Frontend Components**:
- `App.js`: Main application orchestrator handling routing between sync, progress, and dashboard views
- `AuthContext.js`: AWS Cognito authentication provider with automatic token management
- `SyncForm.js`: WordPress store connection interface with OAuth flow initiation
- `SyncProgress.js`: Real-time sync progress tracking with server polling
- `MainLayout.js`: Dashboard layout with navigation and product management interface
- `api.js`: Comprehensive API client managing all backend communication

### Backend Architecture

The backend follows a serverless architecture using AWS Lambda with modular handler functions:

**Runtime**: Node.js with ES modules on AWS Lambda
**API Gateway**: RESTful API with comprehensive CORS support
**Authentication**: JWT token validation with user extraction
**Storage**: AWS S3 for persistent data storage and backups
**Payment Processing**: Stripe integration with webhook handling

**Lambda Handler Structure**:
- `index.mjs`: Main router dispatching requests to appropriate handlers
- `syncHandler.mjs`: WordPress OAuth flow and asynchronous sync operations
- `productHandler.mjs`: Product CRUD operations, variations, and category management
- `dataHandler.mjs`: Data retrieval, search functionality, and category loading
- `accountHandler.mjs`: Account management, backup creation, and data export
- `stripeHandler.mjs`: Subscription management and payment webhook processing
- `supportHandler.mjs`: Customer support functionality

### Data Flow Architecture

**Authentication Flow**:
1. Users authenticate through AWS Cognito
2. JWT tokens are automatically managed and refreshed
3. All API requests include Bearer token authentication
4. User context is extracted from JWT payload in Lambda functions

**WordPress Synchronization Flow**:
1. User initiates connection by providing WordPress store URL
2. OAuth redirect to WordPress for secure app password generation
3. Credentials are used to start asynchronous sync operation in Lambda
4. Progress tracking through periodic status polling
5. Completion triggers dashboard redirect with synchronized data

**Product Management Flow**:
- Real-time product editing with immediate WordPress API updates
- Batch operations for category and variation management
- Image upload with retry logic and progress tracking
- Advanced search and filtering capabilities across synchronized data

### Technology Stack

**Frontend Dependencies**:
- React 19.1.0 with React Router 7.6.1
- AWS Amplify 6.15.0 for Cognito integration
- Axios 1.9.0 for HTTP communication
- Tailwind CSS 3.4.17 for styling
- Lucide React for iconography
- Framer Motion for animations

**Backend Dependencies**:
- AWS Lambda with Node.js runtime
- AWS S3 for data persistence
- Stripe 18.2.1 for payment processing
- WordPress REST API integration
- JWT token handling

### File Structure

```
karoosync/
├── src/                          # React frontend application
│   ├── App.js                    # Main app component with view routing
│   ├── AuthContext.js            # AWS Cognito authentication provider
│   ├── ThemeContext.js           # Theme management context
│   ├── api.js                    # Centralized API client
│   ├── SyncForm.js               # WordPress connection interface
│   ├── SyncProgress.js           # Real-time sync progress tracking
│   ├── MainLayout.js             # Dashboard layout and navigation
│   ├── Dashboard.js              # Product overview dashboard
│   ├── ProductView.js            # Product management interface
│   ├── CategoryView.js           # Category management interface
│   ├── SettingsPage.js           # Account settings and controls
│   └── [other components]        # Additional UI components
├── Lambda Function/              # Serverless backend functions
│   ├── index.mjs                 # Main Lambda handler with routing
│   └── handlers/                 # Modular handler functions
│       ├── syncHandler.mjs       # WordPress sync operations
│       ├── productHandler.mjs    # Product CRUD operations
│       ├── dataHandler.mjs       # Data retrieval and search
│       ├── accountHandler.mjs    # Account management
│       ├── stripeHandler.mjs     # Payment processing
│       └── supportHandler.mjs    # Support functionality
├── public/                       # Static assets and images
├── demo-data/                    # Sample data for development
└── build/                        # Production build output
```

### Security Architecture

**Authentication & Authorization**:
- AWS Cognito provides secure user authentication
- JWT tokens carry user context and permissions
- All API endpoints require valid authentication except webhooks
- WordPress connections use OAuth app passwords instead of storing credentials

**Data Security**:
- User data is stored in S3 with user-specific prefixes
- Stripe webhook signatures are validated for payment security
- CORS policies restrict cross-origin access
- Input validation on both client and server sides

### Performance Considerations

**Frontend Optimizations**:
- Route-based code splitting for faster initial loads
- React Context for efficient state management
- Intelligent retry logic for API requests
- Responsive design with mobile-first approach

**Backend Optimizations**:
- Serverless architecture for automatic scaling
- Asynchronous processing for long-running sync operations
- Modular handler design for efficient Lambda cold starts
- S3 storage with user-specific partitioning

### Integration Points

**WordPress/WooCommerce Integration**:
- OAuth-based authentication with WordPress stores
- REST API communication for product and category management
- Real-time synchronization with progress tracking
- Support for WooCommerce-specific features (variations, attributes, shipping classes)

**Payment Integration**:
- Stripe subscription management
- Webhook handling for payment events
- Subscription status verification
- Automatic user provisioning based on payment status

**AWS Services Integration**:
- Cognito for user authentication and management
- Lambda for serverless compute
- S3 for persistent data storage
- API Gateway for HTTP API management

This architecture provides a scalable, secure, and maintainable solution for WordPress/WooCommerce store management through a modern web interface.