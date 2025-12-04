# Deploying Resume Blaster Server to Render

This guide provides instructions for deploying the Resume Blaster backend server to Render.com.

## Prerequisites

1. A Render account (sign up at https://render.com)
2. Your Gemini API key
3. Git repository with your server code

## Step 1: Prepare Your Server for Deployment

### A. Ensure proper environment variable loading
The server already includes `dotenv/config` import to load environment variables properly.

### B. Verify your package.json
Your server's package.json should be properly configured (which it already is):
- `start` script should run `node dist/generate-docx.js`
- Dependencies should include all required packages

### C. Ensure proper build process
The server already has:
- `build` script: `tsc`
- `dev` script: `nodemon --exec ts-node generate-docx.ts`
- Proper tsconfig.json configuration

## Step 2: Create a Render Web Service

1. Log in to your Render dashboard at https://dashboard.render.com
2. Click "New +" and then "Web Service"
3. Connect your GitHub/GitLab account if deploying from a repository
4. Select your repository containing the server code (in the MobileApp/server directory)

## Step 3: Configure the Web Service

### Basic Configuration:
- **Environment**: Node
- **Branch**: main (or your default branch)
- **Root Directory**: `MobileApp/server` (if your server is in a subdirectory)

### Build Command:
```
npm install && npm run build
```

### Start Command:
```
npm start
```

### Environment Variables:
Add the following environment variables:
- `GEMINI_API_KEY`: Your Gemini API key (make sure to toggle "Secret" to ON)
- `PORT`: Leave blank or set to 10000 (Render will provide this automatically)

## Step 4: Alternative - Manual Deployment Steps

If deploying manually:

1. Make sure your code is pushed to a public Git repository
2. In Render dashboard:
   - Create a new Web Service
   - Connect to your Git provider
   - Select your repository
   - Set the root directory to `MobileApp/server`
   - Use the build and start commands as specified above
   - Add the environment variables

## Step 5: Configure Your Mobile App

After deployment, update your mobile app's environment variables:

1. Get your Render service URL (typically `https://your-service-name.onrender.com`)
2. Update `EXPO_PUBLIC_SERVER_URL` in your mobile app's .env file
3. Rebuild and deploy your mobile app if needed

## Step 6: Verify the Deployment

1. Check that your server is running at the Render URL
2. Verify the endpoint works by accessing: `https://your-service-name.onrender.com/ai/optimize-resume` (should return a 404 or 405 as it expects a POST request)
3. Check the Render dashboard logs for any errors

## Important Notes

- **Security**: Your GEMINI_API_KEY is kept secure on the server side
- **Scaling**: Render will automatically scale your service based on traffic
- **Environment Variables**: Never commit API keys to version control
- **Health Checks**: The server listens on the PORT environment variable provided by Render
- **Build Process**: The TypeScript code will be compiled during the build step

## Troubleshooting

- If your service fails to start, check the logs in the Render dashboard
- Ensure your GEMINI_API_KEY is set with the correct value
- Verify that the PORT environment variable is being used properly (Render provides this automatically)
- If you encounter issues, temporarily expose more detailed error messages during debugging

## Updating Your Deployment

After your initial deployment:
- For manual deploys: Push your changes to the connected Git branch
- If auto-deploy is enabled: Changes to the specified branch will trigger new deployments
- You can also manually trigger deploys from the Render dashboard

Your server will be deployed at a URL like: `https://your-service-name.onrender.com`
This URL should be used for `EXPO_PUBLIC_SERVER_URL` in your mobile app.