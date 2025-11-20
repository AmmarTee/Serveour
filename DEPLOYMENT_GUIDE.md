# Serveour Deployment Guide

## Quick Start

The Serveour food delivery platform is now fully whitelabeled and ready for deployment. All applications are configured and can be tested immediately using the demo API server.

## Testing the Apps (Using Demo API)

All apps are pre-configured to use the Enatega demo API server for immediate testing:
- **API URL**: https://aws-server.enatega.com/graphql
- This allows you to test full functionality before setting up your own backend

### Customer Mobile App
```bash
cd enatega-multivendor-app
npm install
npx expo start
```
Then scan the QR code with Expo Go app on your phone.

### Rider Mobile App
```bash
cd enatega-multivendor-rider
npm install
npx expo start
```

### Restaurant Mobile App
```bash
cd enatega-multivendor-store
npm install
npx expo start
```

### Admin Dashboard (Web)
```bash
cd enatega-multivendor-admin
npm install
npm run dev
```
Open http://localhost:3000

### Customer Web App
```bash
cd enatega-multivendor-web
npm install
npm run dev
```
Open http://localhost:3000

## Building for Production

### Prerequisites
- Node.js 18-20
- EAS CLI: `npm install -g eas-cli`
- Expo account
- Apple Developer account (for iOS)
- Google Play Developer account (for Android)

### Mobile Apps - Build Process

#### 1. Customer App
```bash
cd enatega-multivendor-app

# Development build
eas build --profile development --platform android
eas build --profile development --platform ios

# Production build
eas build --profile production --platform android
eas build --profile production --platform ios

# Submit to stores
eas submit --profile production --platform android
eas submit --profile production --platform ios
```

#### 2. Rider App
```bash
cd enatega-multivendor-rider

# Production build
eas build --profile production --platform all

# Submit to stores
eas submit --profile production --platform all
```

#### 3. Restaurant App
```bash
cd enatega-multivendor-store

# Production build
eas build --profile production --platform all

# Submit to stores
eas submit --profile production --platform all
```

### Web Apps - Build and Deploy

#### Admin Dashboard
```bash
cd enatega-multivendor-admin
npm run build
npm start  # For production server
```

Deploy to:
- Vercel: `vercel --prod`
- Netlify: `netlify deploy --prod`
- AWS/DigitalOcean/etc: Copy `.next` folder to server

#### Customer Web App
```bash
cd enatega-multivendor-web
npm run build
npm start  # For production server
```

## Production Configuration

### 1. Set Up Your Backend API

**Option A: License Enatega Backend**
- Contact Enatega for backend licensing
- Deploy their backend API
- Update environment files with your API URL

**Option B: Build Custom Backend**
- Implement GraphQL API matching the schema
- See API_CONFIGURATION.md for requirements
- Update environment files with your API URL

### 2. Update Environment Variables

#### Customer App
Edit `enatega-multivendor-app/environment.js`:
```javascript
return {
  GRAPHQL_URL: 'https://your-api.serveour.com/graphql',
  WS_GRAPHQL_URL: 'wss://your-api.serveour.com/graphql',
  SERVER_URL: 'https://your-api.serveour.com/graphql',
  SERVER_REST_URL: 'https://your-api.serveour.com/',
  // ... other configs
}
```

#### Rider App
Edit `enatega-multivendor-rider/environment.ts`:
```typescript
return {
  GRAPHQL_URL: "https://your-api.serveour.com/graphql",
  WS_GRAPHQL_URL: "wss://your-api.serveour.com/graphql",
  // ... other configs
}
```

#### Restaurant App
Edit `enatega-multivendor-store/environment.ts`:
```typescript
return {
  GRAPHQL_URL: "https://your-api.serveour.com/graphql",
  WS_GRAPHQL_URL: "wss://your-api.serveour.com/graphql",
}
```

#### Admin Dashboard
Create `enatega-multivendor-admin/.env`:
```bash
NEXT_PUBLIC_SERVER_URL="https://your-api.serveour.com/"
NEXT_PUBLIC_WS_SERVER_URL="wss://your-api.serveour.com/"
```

#### Customer Web
Create `enatega-multivendor-web/.env`:
```bash
NEXT_PUBLIC_SERVER_URL="https://your-api.serveour.com/"
NEXT_PUBLIC_WS_SERVER_URL="wss://your-api.serveour.com/"
```

### 3. Configure Third-Party Services

#### Firebase (Push Notifications)
1. Create Firebase project at https://console.firebase.google.com
2. Add iOS and Android apps
3. Download `google-services.json` (Android) and `GoogleService-Info.plist` (iOS)
4. Place files in respective app directories
5. Update Firebase config in app.json files

#### Google Maps
1. Get API key from https://console.cloud.google.com
2. Update in app.json files:
```json
"config": {
  "googleMapsApiKey": "YOUR_GOOGLE_MAPS_KEY"
}
```

#### Payment Gateways
Update in backend configuration:
- Stripe: Set Stripe publishable and secret keys
- PayPal: Configure PayPal client ID and secret

#### Sentry (Error Tracking)
1. Create project at https://sentry.io
2. Update DSN in environment files:
```javascript
SENTRY_DSN: "YOUR_SENTRY_DSN"
```

#### Amplitude (Analytics)
1. Create project at https://amplitude.com
2. Update API key in environment files:
```javascript
AMPLITUDE_API_KEY: "YOUR_AMPLITUDE_KEY"
```

### 4. Update App Icons and Splash Screens

#### Customer App
- Icon: `enatega-multivendor-app/assets/icon.png` (1024x1024)
- Splash: `enatega-multivendor-app/assets/splash.png` (1284x2778)

#### Rider App
- Icon: `enatega-multivendor-rider/lib/assets/images/icon.png`
- Splash: `enatega-multivendor-rider/lib/assets/images/black.png`

#### Restaurant App
- Icon: `enatega-multivendor-store/lib/assets/images/icon.png`
- Splash: `enatega-multivendor-store/lib/assets/images/black.png`

### 5. Configure Custom Domains

For web apps, configure your DNS:
- Admin Dashboard: admin.serveour.com
- Customer Web: app.serveour.com or www.serveour.com

Update CNAME records to point to your hosting provider.

## App Store Submission

### iOS App Store
1. Update bundle IDs in app.json (already done):
   - Customer: com.serveour.customer
   - Rider: com.serveour.rider
   - Restaurant: com.serveour.restaurant

2. Prepare assets:
   - App icon (1024x1024)
   - Screenshots (various sizes)
   - Privacy policy URL: https://serveour.com/privacy
   - Terms URL: https://serveour.com/terms

3. Create app listings in App Store Connect

4. Build and submit:
```bash
eas build --profile production --platform ios
eas submit --profile production --platform ios
```

### Google Play Store
1. Update package names in app.json (already done)

2. Prepare assets:
   - App icon (512x512)
   - Feature graphic (1024x500)
   - Screenshots
   - Privacy policy URL: https://serveour.com/privacy

3. Create app listings in Google Play Console

4. Build and submit:
```bash
eas build --profile production --platform android
eas submit --profile production --platform android
```

## Security Checklist

- [ ] Update all API keys and secrets
- [ ] Enable HTTPS for all endpoints
- [ ] Configure CORS properly
- [ ] Set up rate limiting
- [ ] Enable two-factor authentication for admin
- [ ] Configure proper database backups
- [ ] Set up monitoring and alerts
- [ ] Review and update privacy policy
- [ ] Comply with GDPR/local data protection laws

## Monitoring

### Analytics
- Amplitude: Track user behavior and conversions
- Google Analytics: Web traffic analysis

### Error Tracking
- Sentry: Monitor crashes and errors
- Backend logs: Application server logs

### Performance
- New Relic or similar: API performance monitoring
- Firebase Performance: Mobile app performance

## Support and Maintenance

### Regular Updates
- Update dependencies regularly
- Monitor for security vulnerabilities
- Update based on user feedback
- Release new features incrementally

### User Support
- Set up support email: support@serveour.com
- Create FAQ page
- Implement in-app chat support
- Monitor app store reviews

## Scaling Considerations

As your platform grows:
- Implement CDN for static assets
- Use load balancers for API servers
- Optimize database queries
- Implement caching (Redis)
- Consider microservices architecture
- Set up auto-scaling infrastructure

## Resources

- **API Configuration**: API_CONFIGURATION.md
- **Whitelabel Summary**: WHITELABEL_SUMMARY.md
- **Expo Documentation**: https://docs.expo.dev
- **React Native**: https://reactnative.dev
- **Next.js**: https://nextjs.org

---
**Ready to Launch!** 🚀

Your Serveour food delivery platform is fully configured and ready for deployment. Follow this guide step by step for a smooth launch.
