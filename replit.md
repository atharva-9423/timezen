# TimeZen - College Lecture Tracker

## Overview

TimeZen is a student-built web application designed for Amrutvahini College to track lectures, manage timetables, provide study materials, and facilitate AI-powered academic assistance. The application features both student-facing and administrative interfaces, with real-time data synchronization and push notification capabilities.

The project is developed by the ZenSpace team as a solution to revolutionize education tracking at their college. It provides division-wise timetable management, lecture tracking, holiday notifications, study materials organization, and an AI chatbot for academic queries.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack:**
- Vanilla JavaScript (no frontend framework)
- HTML5 with semantic markup
- CSS3 with custom properties for theming
- Vite as build tool and development server

**Design System:**
- Neubrutalism design pattern with bold borders and shadow effects
- Custom CSS variable system for theming (light/dark mode support)
- Space Grotesk font family for consistent typography
- Color palette using predefined primary colors (purple, yellow, green, red, blue)
- Responsive design with mobile-first approach

**Page Structure:**
- Single-page application (SPA) architecture with multi-page fallback
- Page-based navigation system using JavaScript routing
- Separate HTML files for admin panel, settings, about, and offline pages
- Client-side state management without frameworks

**Animation System:**
- Intro animation sequence on first load showcasing team members
- Scroll-based animations with Intersection Observer API
- Smooth transitions between pages and states
- Custom loading animations for async operations

**Theme System:**
- Dual theme support (light/dark mode)
- Theme persistence using localStorage
- CSS custom properties for dynamic theme switching
- Per-page theme overrides (e.g., about page forces light theme)

### Backend Architecture

**Data Storage:**
- Firebase Realtime Database for primary data persistence
- Database structure organized by divisions (A-Q)
- Real-time data synchronization across clients
- No traditional backend server - Firebase handles all data operations

**Database Schema:**
```
/divisions/{divisionCode}
  /timetable/{dayCode}/{slotId}
  /subjects/{subjectId}
  /completion-status
  /holidays/{holidayId}
  /study-materials/{subjectName}/{materialId}
  /notifications/{notificationId}
```

**Authentication:**
- Simple credential-based admin authentication (hardcoded)
- No user authentication system for students
- Division selection acts as namespace isolation

**State Management:**
- LocalStorage for user preferences and settings
- In-memory state for current division, day, and UI state
- Firebase listeners for real-time data updates

### Data Synchronization

**Real-time Updates:**
- Firebase Realtime Database listeners for live data sync
- Automatic UI updates when database changes occur
- Optimistic UI updates with rollback on errors

**Offline Support:**
- Service worker for offline page display
- Network status detection (online/offline events)
- Dedicated offline page with custom styling

### Push Notification System

**FCM Integration:**
- Firebase Cloud Messaging for web push notifications
- Service worker registration for background message handling
- Topic-based subscriptions per division (division_A, division_B, etc.)
- Foreground and background notification handling

**Notification Architecture:**
- Client-side FCM token management
- Topic subscription for division-specific notifications
- Admin panel for sending notifications to divisions or all students
- Priority-based notification system

**Server-side Component:**
- Firebase Admin SDK for sending notifications (Node.js)
- Topic-based messaging for broadcasting to groups
- Notification payload structure with title, content, and priority

### Admin Panel

**Capabilities:**
- Timetable management (add/edit/delete lectures)
- Holiday management
- Notification broadcasting
- Study materials upload and organization
- Division-wise data management

**Access Control:**
- Hardcoded admin credentials (username: admin, password: admin123)
- Simple session management using page visibility

### Study Materials System

**Organization:**
- Subject-based categorization with slug-based IDs (e.g., "mathematics")
- Division-specific or all-division materials ("ALL" division for shared subjects/materials)
- File metadata storage (name, type, URL)
- Real-time updates when new materials are added
- Subject pre-registration allowing subjects to be created before materials are uploaded
- Uncategorized materials are displayed separately when no matching subject exists

**Admin Features:**
- Add Subjects button for each division to pre-create subject categories
- Duplicate subject prevention via normalized slug matching
- Empty slug validation to prevent data loss from special-character-only names
- Subject management with real-time Firebase updates

**Student Features:**
- Subjects displayed even with 0 materials uploaded
- Automatic merging of division-specific and ALL-division subjects
- Material count per subject with real-time updates
- Uncategorized materials card for orphaned materials
- Subject-based navigation to material listings

**Storage:**
- Firebase Realtime Database for metadata
- Study subjects stored at: `divisions/{divisionId}/studySubjects/{subjectSlug}`
- Study materials stored at: `studyMaterials/{materialId}`
- External file hosting (URLs stored in database)
- Real-time listeners for both subjects and materials with proper lifecycle management

### AI Integration (Zen AI)

**Chat System:**
- Chat history management with localStorage persistence
- Session-based conversations with unique chat IDs
- Loading animations for AI responses
- Message history display

**Architecture:**
- Client-side chat interface
- Separate chat sessions with save/load functionality
- Toast notifications for user feedback

## External Dependencies

### Firebase Services

**Firebase Realtime Database:**
- Primary data store for all application data
- Project ID: edutrack-89d25
- Database URL: https://edutrack-89d25-default-rtdb.asia-southeast1.firebasedatabase.app
- Real-time synchronization for timetables, holidays, notifications, and study materials

**Firebase Cloud Messaging (FCM):**
- Web push notifications
- Background message handling via service worker
- Topic-based subscriptions for division-specific notifications

**Firebase SDK:**
- Version: 10.7.1 (compat version for CDN usage)
- firebase-app-compat.js for core functionality
- firebase-database-compat.js for Realtime Database
- firebase-messaging-compat.js for push notifications

### Third-party Libraries

**Phosphor Icons:**
- Version: 2.1.1
- Icon library for UI elements
- Loaded via CDN (unpkg.com)

**Google Fonts:**
- Space Grotesk font family
- Weights: 400, 500, 700
- Used throughout the application for consistent typography

### Build Tools

**Vite:**
- Version: 5.4.8
- Development server with HMR (Hot Module Replacement)
- Build tool for production bundles
- Multi-page application configuration for admin, about, and GitHub pages

### Service Worker

**Firebase Messaging Service Worker:**
- Handles background push notifications
- Registered at /firebase-messaging-sw.js
- Uses Firebase Messaging compat SDK

### External APIs

**Firebase Admin SDK (Server-side):**
- Used for sending push notifications from backend
- Requires service account credentials (not included in client code)
- Node.js runtime for notification sender script

### Configuration Files

**Firebase Configuration:**
- Exposed client-side configuration in firebase-config.js
- API keys and project identifiers publicly visible (intended for client-side use)
- Realtime Database rules should be configured server-side for security

### Browser APIs

**Intersection Observer:**
- For scroll-based animations
- Triggers element animations when entering viewport

**Notification API:**
- For permission requests and notification display
- Browser compatibility check for notification support

**Service Worker API:**
- For offline support and background notifications
- Registration and lifecycle management

**LocalStorage:**
- For persistent user preferences
- Chat history storage
- Theme settings