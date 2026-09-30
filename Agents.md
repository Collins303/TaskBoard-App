# AGENTS.md - AI Agent Guidelines

## Project Overview
This is a responsive To-Do List web application with cloud data sync and Google Calendar integration.

## Tech Stack
- **Frontend**: HTML5, CSS3, Vanilla JavaScript (ES6+)
- **Backend & Auth**: Firebase Authentication (Google OAuth 2.0), Cloud Firestore
- **Integrations**: Google Calendar API
- **Deployment**: Netlify (Root publish directory `.`)

## Key Architecture & Rules
1. **Directory Structure**:
   - `index.html` - Main UI layout
   - `config.js` - Firebase initialization and API keys
   - `firestore.rules` - Firestore database security policies

2. **Security & Data Isolation**:
   - All user tasks are scoped strictly under `users/{userId}` in Firestore.
   - Security rules enforce `request.auth.uid == userId` for all database reads and writes.

3. **Deployment Settings**:
   - Netlify `publish` directory MUST remain set to `.` (root).
   - Do not alter OAuth redirect origins without updating the Firebase Authorized Domains list.

## Development Workflows
- Always test Firebase authentication and Firestore listeners after UI modifications.
- Ensure state updates immediately reflect across active browser sessions using Firestore real-time snapshots.
