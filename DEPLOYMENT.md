# Deployment Guide for AdminJS Cloud

This guide will help you deploy your Budget Compass application to AdminJS Cloud.

## Prerequisites

1. **Node.js**: Version 18 or later
2. **Yarn**: Install globally if not already installed
   ```bash
   npm install -g yarn
   ```
3. **AdminJS Cloud CLI**: Install the deployment tool
   ```bash
   yarn global add @adminjs/cloud-cli
   ```

## Step 1: Build Your Application

Before deploying, you need to build both the backend and frontend:

### Build Backend
```bash
cd backend
npm install
npm run build
```

### Build Frontend
```bash
cd frontend
npm install
npm run build
```

## Step 2: Configure Environment Variables

### Backend Environment Variables

Create a `.env` file in the `backend/` directory with the following variables:

```env
# Server configuration
PORT=5000
NODE_ENV=production

# JWT Secret (use a strong random string in production!)
JWT_SECRET=your-production-jwt-secret-here

# CORS Configuration - Add your frontend URL and AdminJS Cloud URL
# Example: ALLOWED_ORIGINS=https://your-frontend-domain.com,https://your-adminjs-app-url.com
ALLOWED_ORIGINS=https://your-frontend-domain.com

# GIGACHAT Configuration
GIGACHAT_CLIENT_ID=your-gigachat-client-id
GIGACHAT_SECRET=your-gigachat-secret
```

### Frontend Environment Variables

Create a `.env` file in the `frontend/` directory:

```env
REACT_APP_API_URL=https://your-adminjs-backend-url.com/api
```

**Important**: Replace `https://your-adminjs-backend-url.com` with your actual AdminJS Cloud backend URL after deployment.

## Step 3: Create AdminJS Cloud Application

1. Visit the [AdminJS Cloud Hosting Dashboard](https://cloud.adminjs.co)
2. Sign in or create an account
3. Create a new application
4. Wait for application approval
5. Generate an API Key and API Secret from the dashboard

## Step 4: Deploy to AdminJS Cloud

From the project root directory, run:

```bash
adminjs-cloud deploy --apiKey=<YOUR_API_KEY> --apiSecret=<YOUR_API_SECRET>
```

Replace `<YOUR_API_KEY>` and `<YOUR_API_SECRET>` with your actual credentials from Step 3.

## Step 5: Configure Your Deployed Application

After deployment:

1. **Get your backend URL** from the AdminJS Cloud dashboard
2. **Update frontend environment variables**:
   - Update `REACT_APP_API_URL` in your frontend `.env` file with the backend URL
   - Rebuild the frontend: `cd frontend && npm run build`
3. **Update backend CORS settings**:
   - Add your frontend URL to the `ALLOWED_ORIGINS` environment variable in AdminJS Cloud dashboard
   - Format: `ALLOWED_ORIGINS=https://your-frontend-domain.com`

## Step 6: Deploy Frontend (Separate Hosting)

Since AdminJS Cloud primarily hosts Node.js backends, you'll need to deploy your React frontend separately. Options include:

- **Netlify**: Free static hosting
- **Vercel**: Free static hosting with great React support
- **GitHub Pages**: Free for public repos
- **Any static hosting service**

### For Vercel/Netlify:

1. Build your frontend: `cd frontend && npm run build`
2. Deploy the `frontend/build` directory
3. Set environment variable `REACT_APP_API_URL` to your backend URL

## Important Notes

1. **Environment Variables**: Make sure to set all required environment variables in the AdminJS Cloud dashboard after deployment
2. **CORS**: Ensure your `ALLOWED_ORIGINS` includes your frontend domain
3. **Database**: If you're using a database, make sure it's accessible from AdminJS Cloud (consider using a cloud database service)
4. **Build Script**: The backend uses `npm run build` to compile TypeScript, and `npm start` to run the compiled code
5. **Port**: AdminJS Cloud will set the PORT environment variable automatically - your code already handles this

## Troubleshooting

### Build Errors
- Ensure all dependencies are installed: `npm install` in both backend and frontend
- Check that TypeScript compiles: `npm run build` in backend

### CORS Errors
- Verify `ALLOWED_ORIGINS` includes your frontend URL
- Check that the frontend is using the correct `REACT_APP_API_URL`

### Connection Issues
- Verify the backend URL is correct in the frontend environment variables
- Check that the backend is running and accessible via the health endpoint: `https://your-backend-url/health`

## File Structure for Deployment

The `adminjs-cloud.json` file is configured to include:
- Backend source and compiled files
- Frontend build (if serving static files)
- Package files and configuration
- Environment files (`.env`)

Make sure your `.env` files are up to date before deployment!

---

## Deployment to brojs.ru Platform

### Prerequisites

1. **Repository**: Ensure your repository is correctly configured
   - Repository URL: `https://github.com/balamut-ark/TravelFrog.git`
   - Default branch: `main` (not `master`)

### Common Issues and Solutions

#### Issue 1: Wrong Repository URL

**Error**: Pipeline clones `budget-compass-mfe.git` instead of `TravelFrog.git`

**Solution**: 
1. Go to the brojs.ru admin panel
2. Check your project configuration
3. Update the repository URL to: `https://github.com/balamut-ark/TravelFrog.git`
4. Ensure the repository is set to the correct project

#### Issue 2: Branch Not Found

**Error**: `error: pathspec 'master' did not match any file(s) known to git`

**Solution**:
1. Go to the brojs.ru admin panel
2. Update the branch name from `master` to `main`
3. The repository uses `main` as the default branch

#### Issue 3: Pipeline Script Issues

The brojs.ru platform uses custom Jenkins scripts. If you encounter issues:

1. **Check Admin Panel Settings**:
   - Verify repository URL: `https://github.com/balamut-ark/TravelFrog.git`
   - Verify branch: `main`
   - Check if there are any custom script configurations

2. **Repository Structure**:
   - Ensure your repository has both `backend/` and `frontend/` directories
   - Ensure `package.json` files exist in both directories

3. **Jenkinsfile**:
   - A `Jenkinsfile` has been created in the root directory
   - This may be used if the platform supports custom Jenkinsfiles
   - The file specifies the correct repository and branch

### Step-by-Step brojs.ru Deployment

1. **Configure Repository in Admin Panel**:
   - Repository URL: `https://github.com/balamut-ark/TravelFrog.git`
   - Branch: `main`
   - Make sure all settings are saved

2. **Verify Project Structure**:
   ```
   TravelFrog/
   ├── backend/
   │   ├── package.json
   │   └── src/
   ├── frontend/
   │   ├── package.json
   │   └── src/
   └── Jenkinsfile (optional)
   ```

3. **Trigger Deployment**:
   - Use the "Deploy" or "Build" button in the admin panel
   - Monitor the build logs for any errors

4. **Check Build Logs**:
   - If you see `error: pathspec 'master' did not match`, update branch to `main`
   - If you see wrong repository being cloned, update repository URL

### Troubleshooting brojs.ru Deployment

- **Build fails during checkout**: Check repository URL and branch name in admin panel
- **Wrong files being deployed**: Verify repository configuration points to TravelFrog
- **Missing dependencies**: Ensure both backend and frontend have valid `package.json` files
- **Build errors**: Check that all required environment variables are set in the admin panel

