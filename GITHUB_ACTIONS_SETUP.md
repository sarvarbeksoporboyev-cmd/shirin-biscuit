# GitHub Actions APK Build Setup

This repository includes a GitHub Actions workflow that builds Android APK files automatically.

## Quick Start

1. **Add the `google-services.json` as a secret** (Required)
2. **Trigger the workflow manually** from GitHub Actions tab
3. **Download your APK** from the workflow artifacts

## Setting Up GitHub Secrets

Go to your repository on GitHub: **Settings → Secrets and variables → Actions → New repository secret**

### Required Secrets

You must add these secrets for the app to work properly:

#### 1. `GOOGLE_SERVICES_JSON`
Your Firebase/Google Services configuration file content.

**How to add:**
1. Open your `google-services.json` file
2. Copy the entire content
3. Create a new secret named `GOOGLE_SERVICES_JSON`
4. Paste the content as the value

#### 2. `FLEETBASE_HOST`
Your Fleetbase API host URL.
- Example: `https://api.fleetbase.io`

#### 3. `STOREFRONT_KEY`
Your Storefront API key from Fleetbase Console.
- Found in: Storefront → Settings → API
- Example: `store_2397bce9b917b31479f62f8e4cc4b960`

#### 4. `FLEETBASE_KEY`
Your Fleetbase secret API key.
- Found in: Developers → API Keys
- Example: `$2y$10$tBRZALDA294JWgmMBlh6Y...`

#### 5. `GOOGLE_MAPS_API_KEY`
Google Maps API key with Maps SDK for Android enabled.
- Example: `AIzaSyDE1Tqnjnl5Dk4nj9Inv_La3tEOSSvZKJQ`

#### 6. `STRIPE_KEY`
Stripe publishable key for payment processing.
- Example: `pk_test_51SmsBjHf6YGiqazf7Wiv1Dmjd1I2n58tn8h0hctwIxdv0OaB8s9a7GmBuq7kmpBQJHXNJFC4zgUgMobnMfxXCvgP00YeKqcZTC`

### Optional Secrets

- `FACEBOOK_APP_ID` - Facebook App ID (defaults to '0')
- `FACEBOOK_CLIENT_TOKEN` - Facebook Client Token (defaults to '0')
- `GOOGLE_CLIENT_ID` - Google OAuth Client ID
- `GOOGLE_IOS_CLIENT_ID` - Google iOS Client ID

## How to Add All Secrets

1. Go to: [https://github.com/sarvarbeksoporboyev-cmd/shirin-biscuit/settings/secrets/actions](https://github.com/sarvarbeksoporboyev-cmd/shirin-biscuit/settings/secrets/actions)
2. Click **New repository secret** for each secret
3. Enter the name exactly as shown above
4. Paste the corresponding value
5. Click **Add secret**

**Important:** Without these secrets, the app will show "Invalid Storefront key" error.

## Running the Workflow

### Method 1: Manual Trigger (Recommended)

1. Go to your repository on GitHub
2. Click on **Actions** tab
3. Select **Build Android APK** workflow from the left sidebar
4. Click **Run workflow** button (top right)
5. Choose build type:
   - **debug** - For testing (faster build, larger file)
   - **release** - For production (optimized, smaller file)
6. Click **Run workflow**

### Method 2: Automatic Trigger

The existing `react-native-ci.yml` workflow automatically builds APKs when:
- You push to the `main` branch
- You create a pull request to `main`
- You create a version tag (e.g., `v1.0.0`)

## Downloading Your APK

After the workflow completes:

1. Go to the workflow run page
2. Scroll down to **Artifacts** section
3. Download the APK file:
   - Debug builds: `shirin-biscuit-debug-{run_number}.zip`
   - Release builds: `shirin-biscuit-release-{run_number}.zip`
4. Extract the ZIP file to get your APK

## Build Times

- **Debug builds**: ~15-25 minutes
- **Release builds**: ~20-30 minutes

Build times depend on GitHub Actions runner availability and caching.

## Troubleshooting

### Build fails with "google-services.json not found"
- Make sure you've added the `GOOGLE_SERVICES_JSON` secret
- Verify the secret contains valid JSON

### Build fails with "SDK not found"
- This is handled automatically by the workflow
- If it persists, check the workflow logs for specific errors

### APK won't install on device
- **Debug builds**: Enable "Install from unknown sources" on your Android device
- **Release builds**: You need to sign the APK with a keystore (see below)

## Signing Release APKs

For production release builds, you should sign your APK. Add these secrets:

- `ANDROID_STOREFRONT_APP_UPLOAD_STORE_FILE` - Base64 encoded keystore file
- `ANDROID_STOREFRONT_APP_UPLOAD_STORE_PASSWORD` - Keystore password
- `ANDROID_STOREFRONT_APP_UPLOAD_KEY_ALIAS` - Key alias
- `ANDROID_STOREFRONT_APP_UPLOAD_KEY_PASSWORD` - Key password

To encode your keystore:
```bash
base64 -i your-keystore.jks | tr -d '\n'
```

## Workflow Files

- `.github/workflows/build-apk.yml` - Manual APK build workflow
- `.github/workflows/react-native-ci.yml` - Automatic CI/CD workflow

## Support

For issues with the build process, check:
1. Workflow logs in GitHub Actions
2. Ensure all required secrets are set
3. Verify your `google-services.json` is valid
