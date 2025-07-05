# ChatBridge Deployment Guide

## Frontend (Vercel) Environment Variables

Set these in your Vercel dashboard under Project Settings → Environment Variables:

```
VITE_API_URL=https://your-backend.onrender.com/api
VITE_SOCKET_URL=https://your-backend.onrender.com
```

**Example:**
```
VITE_API_URL=https://chatbridge-pji1.onrender.com/api
VITE_SOCKET_URL=https://chatbridge-pji1.onrender.com
```

## Backend (Render) Environment Variables

Set these in your Render dashboard under your service → Environment:

```
NODE_ENV=production
PORT=5001
FRONTEND_URL=https://your-frontend.vercel.app
JWT_SECRET=your-super-secret-jwt-key-here
MONGODB_URI=your-mongodb-connection-string
CLOUDINARY_CLOUD_NAME=your-cloudinary-cloud-name
CLOUDINARY_API_KEY=your-cloudinary-api-key
CLOUDINARY_API_SECRET=your-cloudinary-api-secret
```

**Example:**
```
NODE_ENV=production
PORT=5001
FRONTEND_URL=https://chat-bridge-phi.vercel.app
JWT_SECRET=my-super-secret-jwt-key-123456789
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/chatbridge
CLOUDINARY_CLOUD_NAME=mycloud
CLOUDINARY_API_KEY=123456789012345
CLOUDINARY_API_SECRET=abcdefghijklmnopqrstuvwxyz
```

## Important Notes

### Cookie Configuration
- The backend now uses `sameSite: "none"` in production to allow cross-origin requests
- `secure: true` is required when `sameSite` is `"none"`
- This allows cookies to be sent from Vercel (frontend) to Render (backend)

### CORS Configuration
- Backend CORS is configured to allow requests from your Vercel frontend domain
- Both Express CORS and Socket.IO CORS use `process.env.FRONTEND_URL`

### Socket.IO
- Socket.IO client connects to `VITE_SOCKET_URL` in production
- Socket.IO server accepts connections from `FRONTEND_URL`

## Deployment Steps

1. **Push your code changes to GitHub**
2. **Set environment variables in Vercel** (Frontend)
3. **Set environment variables in Render** (Backend)
4. **Deploy backend first** (Render)
5. **Deploy frontend** (Vercel)
6. **Test the application**

## Troubleshooting

### 401 Unauthorized Errors
- Ensure `NODE_ENV=production` is set in Render
- Check that `FRONTEND_URL` matches your exact Vercel domain
- Verify `JWT_SECRET` is set correctly

### Socket.IO Connection Issues
- Ensure `VITE_SOCKET_URL` points to your Render backend root URL (not /api)
- Check that `FRONTEND_URL` is set correctly in Render

### CORS Errors
- Verify `FRONTEND_URL` in Render matches your Vercel domain exactly
- Include protocol (https://) in the URL 