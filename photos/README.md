# Photo Portfolio Setup Guide

This guide explains how to set up your photo portfolio with Google Drive integration.

## Quick Setup

### 1. Create Google Drive Folder

1. Open Google Drive (drive.google.com)
2. Create a new folder called "Portfolio Photos"
3. Right-click the folder → Share → Change to "Anyone with the link" → Viewer
4. Copy the folder link
5. Extract the **Folder ID** from the URL:
   - URL format: `https://drive.google.com/drive/folders/FOLDER_ID_HERE`
   - Example: If your URL is `https://drive.google.com/drive/folders/1abc123xyz456`
   - Your Folder ID is: `1abc123xyz456`

### 2. Get Google Drive API Key

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable Google Drive API:
   - Search for "Google Drive API"
   - Click "Enable"
4. Create API credentials:
   - Go to "Credentials" → "Create Credentials" → "API key"
   - Copy your API key
   - Click "Edit API key":
     - Under "API restrictions": Select "Google Drive API"
     - Under "Website restrictions": Add `gzl.dev` and `*.gzl.dev`
     - Click "Save"

### 3. Configure Your Website

Edit `index.html` in the photos folder and update the CONFIG section:

```javascript
const CONFIG = {
  API_KEY: 'YOUR_API_KEY_FROM_STEP_2',
  FOLDER_ID: 'YOUR_FOLDER_ID_FROM_STEP_1',
  PUBLIC_ONLY: true
};
```

### 4. Upload Photos

**From Phone (iPhone/Android):**
1. Open Google Drive app
2. Navigate to "Portfolio Photos" folder
3. Tap "+" → Upload
4. Select photos from your camera roll
5. Photos appear on your website automatically!

**From Computer:**
1. Open Google Drive in browser
2. Drag and drop photos into "Portfolio Photos" folder
3. Done!

## Adding Photo Captions

To add descriptions/captions to your photos:
1. In Google Drive, right-click a photo
2. Select "File information" or "Details"
3. Add a description
4. The description will appear as a caption on your website

## Features

- ✅ Upload from phone or computer
- ✅ Auto-sync from Google Drive
- ✅ Responsive gallery grid
- ✅ Lightbox viewer with keyboard navigation
- ✅ Photo captions support
- ✅ No backend required
- ✅ Matches your website's minimalist design

## Troubleshooting

### Photos not loading?
- Check browser console (F12) for errors
- Verify folder is shared publicly ("Anyone with the link")
- Confirm API key is active
- Make sure Folder ID is correct

### "403 Forbidden" error?
- API key might not be configured properly
- Add your domain to API key restrictions
- Ensure Google Drive API is enabled

### Slow loading?
- Google Drive API can be slow on first load
- Consider compressing images before uploading
- Use apps like "Photo Compress" on iPhone

## Design Notes

The photo portfolio page uses the same design aesthetic as your main website:
- GioCustom font family
- Minimalist black and white color scheme
- Consistent navigation header
- Smooth fade-in animations
- Responsive layout

Enjoy showcasing your photos!
