# Serveour API Configuration Guide

## Overview
This document describes how to configure API endpoints for the Serveour food delivery platform.

## Current API Configuration

All Serveour apps are currently configured to use the Enatega demo API server for testing purposes:
- **GraphQL API**: `https://aws-server.enatega.com/graphql`
- **WebSocket API**: `wss://aws-server.enatega.com/graphql`
- **REST API**: `https://aws-server.enatega.com/`

## Configuring Your Own API Server

To connect the Serveour apps to your own API server, you need to update the environment configuration files:

### 1. Customer App (enatega-multivendor-app)
File: `/enatega-multivendor-app/environment.js`

Update the following values:
```javascript
GRAPHQL_URL: 'https://your-api-server.com/graphql',
WS_GRAPHQL_URL: 'wss://your-api-server.com/graphql',
SERVER_URL: 'https://your-api-server.com/graphql',
SERVER_REST_URL: 'https://your-api-server.com/',
```

### 2. Rider App (enatega-multivendor-rider)
File: `/enatega-multivendor-rider/environment.ts`

Update the following values:
```typescript
GRAPHQL_URL: "https://your-api-server.com/graphql",
WS_GRAPHQL_URL: "wss://your-api-server.com/graphql",
```

### 3. Restaurant App (enatega-multivendor-store)
File: `/enatega-multivendor-store/environment.ts`

Update the following values:
```typescript
GRAPHQL_URL: "https://your-api-server.com/graphql",
WS_GRAPHQL_URL: "wss://your-api-server.com/graphql",
```

### 4. Admin Dashboard (enatega-multivendor-admin)
File: Create `.env` file based on `.env.example`

```bash
NEXT_PUBLIC_SERVER_URL="https://your-api-server.com/"
NEXT_PUBLIC_WS_SERVER_URL="wss://your-api-server.com/"
```

### 5. Customer Web (enatega-multivendor-web)
File: Create `.env` file based on `.env.example`

```bash
NEXT_PUBLIC_SERVER_URL="https://your-api-server.com/"
NEXT_PUBLIC_WS_SERVER_URL="wss://your-api-server.com/"
```

## API Requirements

Your backend API server must implement the GraphQL schema and resolvers expected by the Serveour apps. The original Enatega backend can be licensed separately or you can implement your own compatible backend.

### Key API Features Required:
- User authentication (email, phone, social login)
- Restaurant management
- Menu and item management
- Order management
- Rider/driver management
- Payment processing (Stripe, PayPal)
- Real-time order tracking (WebSocket)
- Push notifications
- Analytics and reporting

## Testing

To test with the demo API:
1. The apps are pre-configured to use `aws-server.enatega.com`
2. This allows you to test the full functionality
3. Demo credentials are typically available in the Enatega documentation

## Production Deployment

For production:
1. Set up your own backend API server
2. Update all environment files with your production API URLs
3. Configure your payment gateways
4. Set up your Firebase project for notifications
5. Configure your Google Maps API keys
6. Set up Sentry for error tracking (optional)
7. Configure Amplitude for analytics (optional)

## Support

For API backend setup and licensing, refer to the original Enatega documentation or contact the Serveour team for custom backend development.
