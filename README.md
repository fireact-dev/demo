# Fireact.dev Demo Application

This repository hosts a fully functional demonstration of the Fireact.dev application, created using the `create-fireact-app` CLI tool. It showcases all core functionalities and serves as a reference implementation for developers building their own SaaS applications.

## Features Demonstrated

The demo application includes:

### Authentication & User Management
- ✅ User sign-up with email/password
- ✅ User sign-in/sign-out
- ✅ Password reset functionality
- ✅ Email verification
- ✅ User profile management
- ✅ Password change

### Subscription Management
- ✅ Multiple subscription plans
- ✅ Subscription creation with Stripe
- ✅ Plan upgrades and downgrades
- ✅ Subscription cancellation
- ✅ Billing portal access

### Payment Processing
- ✅ Payment method management
- ✅ Default payment method selection
- ✅ Payment method removal
- ✅ Invoice history viewing
- ✅ Billing details updates

### Team Collaboration
- ✅ Team member invitations
- ✅ Role-based permissions
- ✅ User invitation acceptance/rejection
- ✅ Team member removal
- ✅ Subscription ownership transfer

### Multi-language Support
- ✅ Multiple language options
- ✅ Dynamic language switching
- ✅ Internationalized UI components

## Live Demo

Visit our live demonstration at [https://fireact.dev/demos/](https://fireact.dev/demos/) to explore the full range of features and capabilities without setting up locally.

## Running Locally

### Prerequisites

- Node.js (v18 or higher)
- Firebase CLI: `npm install -g firebase-tools`
- Stripe CLI: [Installation Guide](https://stripe.com/docs/stripe-cli)
- Firebase project with web app configured
- Stripe test account

### Setup Steps

1. **Navigate to the demo directory**:
   ```bash
   cd demo
   ```

2. **Install dependencies**:
   ```bash
   # Install app dependencies
   npm install

   # Install functions dependencies
   cd functions
   npm install
   cd ..
   ```

3. **Configure Firebase**:
   ```bash
   firebase login
   firebase use --add
   # Select your Firebase project
   ```

4. **Update configuration files**:

   Edit `src/config/firebase.config.json` with your Firebase project settings.

   Edit `functions/src/config/stripe.config.json` with your Stripe test keys.

5. **Build the application**:
   ```bash
   npm run build
   cd functions
   npm run build
   cd ..
   ```

6. **Start Firebase Emulators**:
   ```bash
   firebase emulators:start
   ```

   This starts:
   - Hosting: http://localhost:5002
   - Auth Emulator: http://localhost:9099
   - Firestore Emulator: http://localhost:8080
   - Functions Emulator: http://localhost:5001
   - Emulator UI: http://localhost:4000

7. **Set up Stripe webhook** (in a separate terminal):
   ```bash
   stripe listen --forward-to http://127.0.0.1:5001/<your-firebase-project-id>/us-central1/stripeWebhook
   ```

   Copy the webhook secret (whsec_...) and update `functions/src/config/stripe.config.json`, then rebuild functions.

### Testing the Demo

Once running, you can test:

1. **Sign Up**:
   - Create a new account
   - Verify email (check emulator UI)
   - Complete profile

2. **Create Subscription**:
   - Choose a subscription plan
   - Enter test card: `4242 4242 4242 4242`
   - Complete payment setup

3. **Manage Subscription**:
   - View subscription details
   - Invite team members
   - Update billing details
   - Change payment methods
   - View invoices

4. **Team Features**:
   - Invite users via email
   - Accept/reject invitations
   - Manage user permissions
   - Remove team members

5. **Test Scenarios**:
   - Upgrade/downgrade plans
   - Cancel subscription
   - Transfer ownership
   - Change language

## Project Structure

```
demo/
├── src/                    # React application
│   ├── components/        # UI components
│   ├── contexts/          # React contexts
│   ├── hooks/             # Custom hooks
│   ├── config/            # Configuration
│   └── i18n/              # Translations
├── functions/              # Cloud Functions
│   └── src/
│       ├── functions/     # Backend functions
│       └── config/        # Backend configuration
├── public/                 # Static assets
├── deploy/                 # Production configs (not in git)
├── cloudbuild.yaml        # CI/CD configuration
├── firebase.json          # Firebase configuration
├── firestore.rules        # Security rules
└── package.json           # Dependencies
```

## CI/CD with Google Cloud Build

This demo includes a production-ready CI/CD pipeline using Google Cloud Build.

### How It Works

1. **Trigger**: Push to main branch
2. **Build**: Cloud Build executes `cloudbuild.yaml`
3. **Configuration**: Fetches production configs from Cloud Storage
4. **Deploy**: Deploys to Firebase Hosting and Functions
5. **Verification**: Runs post-deployment checks

### Cloud Build Configuration

The `cloudbuild.yaml` file defines the build steps:

```yaml
steps:
  # 1. Fetch production configs from Cloud Storage
  # 2. Install dependencies
  # 3. Build React application
  # 4. Build Cloud Functions
  # 5. Deploy to Firebase
  # 6. Clean up
```

### Production Configuration

The `deploy/` folder is designed for production configurations:

**Files (not in repository)**:
- `firebase.config.json` - Production Firebase settings
- `stripe.config.json` - Production Stripe keys
- `plans.config.json` - Production subscription plans
- `.firebaserc` - Firebase project reference

**Security**:
- Production configs stored in Google Cloud Storage bucket
- Retrieved during build process
- Never committed to version control
- Access controlled via IAM permissions

### Setting Up CI/CD

1. **Create Cloud Storage bucket**:
   ```bash
   gsutil mb gs://your-project-configs
   ```

2. **Upload production configs**:
   ```bash
   gsutil cp deploy/* gs://your-project-configs/
   ```

3. **Configure Cloud Build trigger**:
   - Go to Google Cloud Console → Cloud Build → Triggers
   - Create trigger for your repository
   - Set branch: main
   - Set build configuration: cloudbuild.yaml

4. **Grant permissions**:
   ```bash
   # Allow Cloud Build to access configs
   gsutil iam ch serviceAccount:YOUR_PROJECT_NUMBER@cloudbuild.gserviceaccount.com:objectViewer gs://your-project-configs
   ```

## Use Cases

This demo serves as:

### Reference Implementation
- See how all features work together
- Understand component integration
- Learn best practices

### Testing Ground
- Test user workflows
- Verify payment processing
- Explore subscription management

### Starting Point
- Clone and customize for your project
- Use as template for new features
- Reference for troubleshooting

### Demo for Stakeholders
- Show potential of Fireact.dev
- Demonstrate SaaS capabilities
- Present to clients or investors

## Customization

To customize this demo for your own project:

1. **Branding**:
   - Update logo in `public/`
   - Modify colors in `tailwind.config.js`
   - Change app name in configuration

2. **Features**:
   - Add custom components in `src/components/`
   - Extend Cloud Functions in `functions/src/functions/`
   - Modify subscription plans in configuration

3. **Internationalization**:
   - Add languages in `src/i18n/locales/`
   - Update translations for new features

4. **Deployment**:
   - Update `cloudbuild.yaml` for your pipeline
   - Configure your own Cloud Storage bucket
   - Set up your Firebase project

## Documentation

For detailed documentation:

- **Getting Started**: [docs.fireact.dev/getting-started](https://docs.fireact.dev/getting-started)
- **Custom Development**: [docs.fireact.dev/custom-development](https://docs.fireact.dev/custom-development)
- **API Reference**: [docs.fireact.dev/app](https://docs.fireact.dev/app)
- **Architecture**: [../ARCHITECTURE.md](../ARCHITECTURE.md)
- **Troubleshooting**: [../TROUBLESHOOTING.md](../TROUBLESHOOTING.md)

## Support

- **Website**: [fireact.dev](https://fireact.dev)
- **Documentation**: [docs.fireact.dev](https://docs.fireact.dev)
- **GitHub Issues**: [Report bugs and issues](https://github.com/fireact-dev/demo/issues)
- **Discussions**: Community support and questions

## License

This demo application is open source and available under the [MIT License](../LICENSE).
