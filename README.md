# Fireact.dev Demo Application

This repository (`/demo`) hosts a demonstration of the `fireact.dev` application, which is typically installed using the `create-fireact-app` CLI tool. It showcases the core functionalities and structure of a Fireact.dev project.

## Running Locally

You can run this demo application locally using the Firebase Emulators. This allows you to test the application's features, including authentication, Firestore, and Cloud Functions, in a local environment without deploying to Firebase.

To get started, follow the instructions in the official Fireact.dev Getting Started documentation: [https://docs.fireact.dev/getting-started/](https://docs.fireact.dev/getting-started/)

Typically, after navigating into the project directory, you would run:
```bash
npm run build && cd functions && npm run build && cd ..
firebase emulators:start
```

For Stripe webhook testing, in a separate terminal:
```bash
stripe listen --forward-to http://127.0.0.1:5001/<your-firebase-project-id>/us-central1/stripeWebhook
```

## CI/CD with Google Cloud Build

This demo repository includes a `cloudbuild.yaml` file, which defines a Continuous Integration/Continuous Deployment (CI/CD) pipeline using Google Cloud Build. This configuration automates the process of building and deploying the application.

### Production Configuration

The `deploy` folder within this repository is designed to host production configuration files. **These files are not included in the Git repository for security reasons.** Instead, they are uploaded to a secure Google Cloud Storage bucket.

During the Cloud Build process, the `cloudbuild.yaml` script retrieves these production configurations from the designated storage bucket. It then uses these configurations to build the production-ready version of the application and deploy it to your Firebase project. This ensures that sensitive production credentials and settings are kept separate from the codebase and are securely managed.
