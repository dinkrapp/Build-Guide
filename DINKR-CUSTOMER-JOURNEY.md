# DiNKR Customer Journey & Technical Architecture
## Living Wireframe Document v1.0

> **Purpose:** This document maps out the complete DiNKR ecosystem - what exists, what each element does, and what needs to be built. Use this as your single source of truth for development.
>
> **How to Use:** Sections marked with `🔲 FILL IN:` require your input. Save this file and share with the development team after completing.

---

## Table of Contents

1. [Platform Overview](#1-platform-overview)
2. [Repository Structure](#2-repository-structure)
3. [Deployment Architecture](#3-deployment-architecture)
4. [Web App - Complete Mapping](#4-web-app---complete-mapping)
5. [Marketing Website - Complete Mapping](#5-marketing-website---complete-mapping)
6. [iOS & Android Mobile Apps](#6-ios--android-mobile-apps)
7. [Watch Apps](#7-watch-apps)
8. [User Roles & Journeys](#8-user-roles--journeys)
9. [API Endpoints](#9-api-endpoints)
10. [React to React Native Workflow](#10-react-to-react-native-workflow)
11. [Server Folder Structure](#11-server-folder-structure)
12. [Development Workflow](#12-development-workflow)
13. [Next Steps Checklist](#13-next-steps-checklist)

---

## 1. Platform Overview

DiNKR is a multi-platform pickleball ecosystem consisting of:

| Platform | URL/Location | Technology | Repository |
|----------|--------------|------------|------------|
| Marketing Website | `dinkr.co` | React | `dinkr-website` |
| Web App | `dinkr.co/webapp` | React | `dinkr-webapp` |
| iOS App | App Store | React Native | `dinkr-mobile` |
| Android App | Google Play | React Native | `dinkr-mobile` |
| iOS Watch App | App Store | Swift/SwiftUI | `dinkr-watch-ios` |
| Android Watch App | Google Play | Kotlin/Wear OS | `dinkr-watch-android` |

### Brand Tagline
> **"Never miss another match."**
>
> **PLAY HARD • PLAY FAIR • PLAY MORE**

### Brand Colors

```css
/* Primary Brand */
--dinkr-blue: #294C6B           /* Main brand blue */
--dinkr-yellow: #D6FF00         /* Main brand yellow */

/* Feature Colors */
--dinkr-dinkedin: #005693       /* DinkedIn */
--dinkr-ads: #612987            /* Ads */
--dinkr-charity: #8E489B        /* Charity */
--dinkr-dating: #AC1E2E         /* Dating / Love-Match */
--dinkr-tournaments: #7D2432    /* Tournaments */

/* Game & Stats */
--dinkr-leagues: #B55133        /* Leagues */
--dinkr-score: #DF581D          /* Score */
--dinkr-watch: #F3BB34          /* Watch */
--dinkr-rating-changes: #256139 /* Rating changes */
--dinkr-matches: #4F361B        /* Matches */
--dinkr-partner-info: #6E1215   /* Partner info */
--dinkr-series: #8C260F         /* Series */

/* UI Colors */
--dinkr-dashboard: #386D77      /* Dashboard */
--dinkr-kitchen: #4E5458        /* The KiTCHN */
--dinkr-events: #008790         /* Events */
```

### Typography
- **Headlines:** Bebas Neue
- **Body:** DM Sans

---

## 2. Repository Structure

### GitHub Repositories (Development)

```
github.com/DiNKR/
├── dinkr-website/          # Marketing site (React)
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/          # Page-level components
│   │   └── styles/         # CSS files
│   ├── public/
│   ├── package.json
│   └── README.md
│
├── dinkr-webapp/           # Web application (React)
│   ├── src/
│   │   ├── components/     # Shared UI components
│   │   ├── pages/          # Page-level components
│   │   ├── modals/         # Modal dialogs
│   │   ├── hooks/          # Custom React hooks
│   │   ├── utils/          # Utility functions
│   │   ├── data/           # Sample data (replace with API)
│   │   ├── icons/          # SVG icon components
│   │   └── styles/         # CSS files
│   ├── public/
│   ├── package.json
│   └── README.md
│
├── dinkr-mobile/           # iOS + Android (React Native)
│   ├── src/
│   │   ├── components/     # Shared with webapp (80%+)
│   │   ├── screens/        # Mobile-specific screens
│   │   ├── navigation/     # React Navigation config
│   │   └── native/         # Platform-specific code
│   ├── ios/                # Xcode project
│   ├── android/            # Android Studio project
│   ├── package.json
│   └── README.md
│
├── dinkr-watch-ios/        # Apple Watch (Swift)
│   ├── DiNKRWatch/
│   │   ├── Views/          # SwiftUI views
│   │   ├── Models/         # Data models
│   │   └── Services/       # API/Watch connectivity
│   └── README.md
│
└── dinkr-watch-android/    # Wear OS (Kotlin)
    ├── app/
    │   ├── src/main/
    │   │   ├── java/       # Kotlin code
    │   │   └── res/        # Resources
    └── README.md
```

---

## 3. Deployment Architecture

### Two Parallel Deployment Flows

The web properties (website + webapp) and mobile apps follow **separate deployment paths**. When you update features, you'll typically update both simultaneously, but they deploy through different pipelines.

---

### HOSTING OPTIONS

Choose **Option A (Azure)** for enterprise/complex needs, or **Option B (Railway)** for simplicity and faster setup.

| Feature | Azure | Railway |
|---------|-------|---------|
| GitHub Integration | Requires DevOps setup | Native, 1-click |
| Auto-deploy | Requires pipeline config | Built-in on every push |
| Complexity | More complex | Very simple |
| Pricing | Pay for resources | Pay for usage (~$5-20/mo) |
| Scaling | Manual configuration | Automatic |
| Best for | Enterprise, complex needs | Startups, MVPs, small teams |

---

## OPTION A: Azure Hosting

### FLOW 1A: Website & Web App (Azure)

**Repos:** `dinkr-website` and `dinkr-webapp`  
**Hosting:** Azure App Service  
**No Fastlane** - these are served directly from your Azure server to browsers.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         LOCAL DEVELOPMENT                                │
│                                                                          │
│  Developer works in:                                                     │
│  • dinkr-website repo (marketing site)                                  │
│  • dinkr-webapp repo (React app)                                        │
│                                                                          │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                │
│  │   GitHub    │────▶│   Local     │────▶│   GitHub    │                │
│  │   Clone     │     │   Dev       │     │   Push      │                │
│  └─────────────┘     └─────────────┘     └─────────────┘                │
└─────────────────────────────────────────────────────────────────────────┘
                                │
                                │ Push to `staging` branch
                                ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         AZURE STAGING                                    │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                     Azure DevOps Pipeline                        │    │
│  │  • Triggered on push to `staging` branch                        │    │
│  │  • Run tests                                                     │    │
│  │  • Build React app                                               │    │
│  │  • Deploy to staging servers                                     │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
│  ┌─────────────────────┐     ┌─────────────────────┐                    │
│  │  staging.dinkr.co   │     │ staging.dinkr.co/   │                    │
│  │  (Website)          │     │ webapp              │                    │
│  └─────────────────────┘     └─────────────────────┘                    │
│                                                                          │
│  ✓ Team tests in browser                                                │
│  ✓ QA approval required                                                 │
└─────────────────────────────────────────────────────────────────────────┘
                                │
                                │ Merge to `main` branch (after approval)
                                ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        AZURE PRODUCTION                                  │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                     Azure DevOps Pipeline                        │    │
│  │  • Triggered on merge to `main` branch                          │    │
│  │  • Build production bundle                                       │    │
│  │  • Deploy to production servers                                  │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
│  ┌─────────────────────┐     ┌─────────────────────┐                    │
│  │     dinkr.co        │     │   dinkr.co/webapp   │                    │
│  │    (Website)        │     │     (Web App)       │                    │
│  └─────────────────────┘     └─────────────────────┘                    │
│                                                                          │
│  ✓ LIVE - Users access via browser                                      │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### FLOW 2: iOS & Android Mobile Apps (App Store Distribution)

**Repo:** `dinkr-mobile` (React Native - single codebase for both platforms)  
**CI/CD:** GitHub Actions (free) + Fastlane  
**Testing:** TestFlight (iOS) + Google Play Internal Testing (Android)  
**Production:** Fastlane → App Store + Google Play  

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         LOCAL DEVELOPMENT                                │
│                                                                          │
│  Developer works in:                                                     │
│  • dinkr-mobile repo (React Native)                                     │
│  • Updates components to match webapp changes                           │
│  • Tests on iOS Simulator + Android Emulator                            │
│                                                                          │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                │
│  │   GitHub    │────▶│   Local     │────▶│   GitHub    │                │
│  │   Clone     │     │   Dev       │     │   Push      │                │
│  └─────────────┘     └─────────────┘     └─────────────┘                │
└─────────────────────────────────────────────────────────────────────────┘
                                │
                                │ Push to `staging` branch
                                ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         BETA TESTING                                     │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                   GitHub Actions + Fastlane                      │    │
│  │  • Triggered on push to `staging` branch                        │    │
│  │  • Build iOS archive (.ipa)                                      │    │
│  │  • Build Android bundle (.aab)                                   │    │
│  │  • Run Fastlane beta lanes                                       │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
│         ┌─────────────────────────────────────────────┐                 │
│         │              FASTLANE (Beta)                │                 │
│         │  • Code signing                             │                 │
│         │  • Upload to test platforms                 │                 │
│         └─────────────────────────────────────────────┘                 │
│                        │                   │                            │
│                        ▼                   ▼                            │
│  ┌─────────────────────────┐     ┌─────────────────────────┐           │
│  │      TestFlight         │     │  Google Play Internal   │           │
│  │        (iOS)            │     │      Testing            │           │
│  │                         │     │                         │           │
│  │  • Invite beta testers  │     │  • Invite beta testers  │           │
│  │  • Test on real iPhones │     │  • Test on real Android │           │
│  └─────────────────────────┘     └─────────────────────────┘           │
│                                                                          │
│  ✓ Team tests on physical devices                                       │
│  ✓ QA approval required                                                 │
│  ✓ Apps connect to staging API for testing                             │
└─────────────────────────────────────────────────────────────────────────┘
                                │
                                │ Merge to `main` branch (after approval)
                                ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      APP STORE PRODUCTION                                │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                   GitHub Actions + Fastlane                      │    │
│  │  • Triggered on merge to `main` branch                          │    │
│  │  • Build release versions                                        │    │
│  │  • Run Fastlane release lanes                                    │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
│         ┌─────────────────────────────────────────────┐                 │
│         │            FASTLANE (Release)               │                 │
│         │  • Increment version numbers                │                 │
│         │  • Code signing (production certs)          │                 │
│         │  • Upload screenshots & metadata            │                 │
│         │  • Submit for review                        │                 │
│         └─────────────────────────────────────────────┘                 │
│                        │                   │                            │
│                        ▼                   ▼                            │
│  ┌─────────────────────────┐     ┌─────────────────────────┐           │
│  │       App Store         │     │      Google Play        │           │
│  │        (iOS)            │     │       Store             │           │
│  │                         │     │                         │           │
│  │  • Apple review (1-2d)  │     │  • Google review (hrs)  │           │
│  │  • Published to users   │     │  • Published to users   │           │
│  └─────────────────────────┘     └─────────────────────────┘           │
│                                                                          │
│  ✓ LIVE - Users download from App Store / Google Play                   │
│  ✓ Apps connect to production API                                       │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### FLOW 3: Watch Apps (Native - Separate Repos)

**Repos:** `dinkr-watch-ios` (Swift) and `dinkr-watch-android` (Kotlin)  
**Distribution:** Bundled with main mobile apps in App Store / Google Play

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         WATCH APP DEPLOYMENT                             │
│                                                                          │
│  Watch apps are built separately but bundled with mobile apps:          │
│                                                                          │
│  ┌──────────────────────┐          ┌──────────────────────┐             │
│  │  dinkr-watch-ios     │          │  dinkr-watch-android │             │
│  │  (Swift/SwiftUI)     │          │  (Kotlin/Wear OS)    │             │
│  └──────────┬───────────┘          └──────────┬───────────┘             │
│             │                                  │                         │
│             ▼                                  ▼                         │
│  ┌──────────────────────┐          ┌──────────────────────┐             │
│  │  Bundled into iOS    │          │  Bundled into Android│             │
│  │  app during build    │          │  app during build    │             │
│  └──────────┬───────────┘          └──────────┬───────────┘             │
│             │                                  │                         │
│             ▼                                  ▼                         │
│  ┌──────────────────────┐          ┌──────────────────────┐             │
│  │  App Store           │          │  Google Play         │             │
│  │  (iOS + Watch)       │          │  (Android + Watch)   │             │
│  └──────────────────────┘          └──────────────────────┘             │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### Typical Update Workflow (Feature Release)

When adding a new feature (e.g., "Add game reminders"):

```
DAY 1-2: Development
├── Update dinkr-webapp (React) with new feature
├── Update dinkr-mobile (React Native) with same feature  
├── Update dinkr-watch-ios if watch functionality needed
└── Update dinkr-watch-android if watch functionality needed

DAY 3: Staging Deployment
├── Push all repos to `staging` branch
├── Website + WebApp → Azure Staging (immediate)
├── Mobile Apps → TestFlight + Google Play Internal (10-30 min build)
└── Team tests everything together

DAY 4-5: QA & Approval
├── QA tests on staging.dinkr.co (browser)
├── QA tests on TestFlight (real iPhones)
├── QA tests on Google Play Internal (real Android)
└── Approval to proceed

DAY 5-6: Production Release
├── Merge all repos to `main` branch
├── Website + WebApp → Azure Production (immediate, users see it)
├── Mobile Apps → Fastlane → App Store + Google Play
├── iOS: Apple review (1-2 days)
└── Android: Google review (few hours)

DAY 6-8: Fully Live
└── All platforms updated and available to users
```

### Azure Resources Required

🔲 **FILL IN:** Azure subscription details

| Resource | Purpose | Configuration |
|----------|---------|---------------|
| Azure App Service (Website) | Host dinkr.co | `🔲 FILL IN: Plan tier` |
| Azure App Service (WebApp) | Host dinkr.co/webapp | `🔲 FILL IN: Plan tier` |
| Azure App Service (API) | Host api.dinkr.co | `🔲 FILL IN: Plan tier` |
| Azure SQL Database | User data, games, ratings | `🔲 FILL IN: Tier` |
| Azure Blob Storage | Images, media files | `🔲 FILL IN: Tier` |
| Azure DevOps | CI/CD pipelines | Connected to GitHub |
| Azure Key Vault | Secrets management | API keys, certs |

---

## OPTION B: Railway Hosting (Simpler Alternative)

### FLOW 1B: Website & Web App (Railway)

**Repos:** `dinkr-website` and `dinkr-webapp`  
**Hosting:** Railway  
**No pipelines to configure** - Railway auto-detects and deploys.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         LOCAL DEVELOPMENT                                │
│                                                                          │
│  Developer works in:                                                     │
│  • dinkr-website repo (marketing site)                                  │
│  • dinkr-webapp repo (React app)                                        │
│                                                                          │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                │
│  │   GitHub    │────▶│   Local     │────▶│   GitHub    │                │
│  │   Clone     │     │   Dev       │     │   Push      │                │
│  └─────────────┘     └─────────────┘     └─────────────┘                │
└─────────────────────────────────────────────────────────────────────────┘
                                │
                                │ Push to `staging` branch
                                ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                       RAILWAY STAGING                                    │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                   Railway Auto-Deploy                            │    │
│  │  • Detects push to `staging` branch                             │    │
│  │  • Auto-detects React/Node                                       │    │
│  │  • Builds and deploys automatically                              │    │
│  │  • No pipeline configuration needed                              │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
│  ┌─────────────────────────────────┐  ┌─────────────────────────────┐   │
│  │  dinkr-website-staging.         │  │  dinkr-webapp-staging.      │   │
│  │  up.railway.app                 │  │  up.railway.app             │   │
│  └─────────────────────────────────┘  └─────────────────────────────┘   │
│                                                                          │
│  ✓ Team tests in browser                                                │
│  ✓ QA approval required                                                 │
└─────────────────────────────────────────────────────────────────────────┘
                                │
                                │ Merge to `main` branch (after approval)
                                ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      RAILWAY PRODUCTION                                  │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                   Railway Auto-Deploy                            │    │
│  │  • Detects merge to `main` branch                               │    │
│  │  • Builds production bundle                                      │    │
│  │  • Deploys to production environment                             │    │
│  │  • Custom domain support                                         │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
│  ┌─────────────────────┐     ┌─────────────────────┐                    │
│  │     dinkr.co        │     │   dinkr.co/webapp   │                    │
│  │  (Custom Domain)    │     │  (Custom Domain)    │                    │
│  └─────────────────────┘     └─────────────────────┘                    │
│                                                                          │
│  ✓ LIVE - Users access via browser                                      │
└─────────────────────────────────────────────────────────────────────────┘
```

### Railway Setup (One-Time)

```
1. Go to railway.app → Connect GitHub
2. Select repo (dinkr-website or dinkr-webapp)
3. Railway auto-detects React
4. Set environment variables if needed
5. Deploy → Get URL
6. Add custom domain (dinkr.co)
```

### Railway Resources

| Service | Purpose | Est. Cost |
|---------|---------|-----------|
| Railway (Website) | Host dinkr.co | ~$5/mo |
| Railway (WebApp) | Host dinkr.co/webapp | ~$5/mo |
| Railway (API) | Host api.dinkr.co | ~$10/mo |
| Railway (PostgreSQL) | Database | ~$5/mo |
| **Total** | | **~$25/mo** |

---

## MOBILE APPS (Works with Azure OR Railway)

Mobile apps just need an API URL to connect to - works with either hosting option.

### 4.1 Login/Authentication Screen

**What It Does:** Entry point for users. Handles sign-in, sign-up, and social authentication.

| Element | Purpose | Status | Backend Required |
|---------|---------|--------|------------------|
| Apple Sign-In Button | Auth via Apple ID | ✅ UI Ready | ✅ Needs Apple Auth API |
| Google Sign-In Button | Auth via Google | ✅ UI Ready | ✅ Needs Google Auth API |
| Email Input | Manual email login | ✅ UI Ready | ✅ Needs Auth API |
| Password Input | Password entry | ✅ UI Ready | ✅ Needs Auth API |
| Sign In Button | Submit credentials | ✅ UI Ready | ✅ Needs Auth API |
| Create Account Link | Toggle to signup mode | ✅ UI Ready | ✅ Needs Registration API |
| Demo Credentials | Testing access (demo@demo.com) | ✅ Working | Remove for production |

🔲 **FILL IN:** Additional auth providers needed?
```
[ ] Facebook
[ ] Email Magic Link
[ ] Phone/SMS
[ ] Other: ________________
```

---

### 4.2 Home Page (Main Dashboard)

**What It Does:** Central hub showing personalized content, quick access to all features, and activity feed.

#### Hero Section
| Element | Purpose | Data Source |
|---------|---------|-------------|
| Greeting | "Good Morning, [Name]" | User profile API |
| Tagline | "Never miss another match" | Static |
| PLAY HARD • PLAY FAIR • PLAY MORE | Brand tagline | Static |

#### Love-Match Banner (Conditional)
| Element | Purpose | Visibility |
|---------|---------|------------|
| Heart Icon | Visual indicator | When not opted-in |
| "Find Your Perfect Match" | CTA headline | When not opted-in |
| Description | Feature explanation | When not opted-in |
| Enable Button | Opens age verification | When not opted-in |
| X (Close) Button | Dismisses banner | Always |

#### Players Near You Section
| Element | Purpose | Data Source |
|---------|---------|-------------|
| Section Header | "Players Near You" | Static |
| See All Link | Navigate to Players page | Navigation |
| Player Cards (horizontal scroll) | Show nearby players | `/api/players/nearby` |

**Player Card Contents:**
| Element | Purpose | Notes |
|---------|---------|-------|
| Photo | Player avatar | From profile |
| Name | Player name | From profile |
| DiNKR Rating | Skill rating (e.g., 3.92) | From rating system |
| Skill Level | Self-reported (e.g., "4.0") | From profile |
| Tag Badge | "PRO" (yellow) or "NEW" (teal) | Conditional |
| Love Heart | Pink heart if both users opted-in | Only when mutual |

#### Your Squads Section
| Element | Purpose | Data Source |
|---------|---------|-------------|
| Section Header | "Your Squads" | Static |
| Manage Link | Navigate to Squads page | Navigation |
| Squad Cards (horizontal scroll) | Show user's squads | `/api/users/{id}/squads` |

**Squad Card Contents:**
| Element | Purpose |
|---------|---------|
| Squad Name | e.g., "Morning Crew" |
| Player Count | e.g., "6 players" |
| Member Avatars | Up to 3 shown + "+X" |
| Invite Squad Button | Quick action |

#### Venue Lobbies Section
| Element | Purpose | Data Source |
|---------|---------|-------------|
| Section Header | "Venue Lobbies" | Static |
| Explore Link | Navigate to Venues page | Navigation |
| Venue Cards | Show nearby venues | `/api/venues/nearby` |

**Venue Card Contents:**
| Element | Purpose |
|---------|---------|
| Photo | Venue image |
| Type Badge | "Club", "Public", "Pay-to-Play" |
| Name | Venue name |
| Location + Distance | e.g., "South Park - 0.8 mi" |
| Court Count | e.g., "12 courts" |
| Availability | e.g., "Available" |
| Rating | Star rating (e.g., 4.8) |

#### Upcoming Games Section
| Element | Purpose | Data Source |
|---------|---------|-------------|
| Section Header | "Upcoming Games" | Static |
| View All Link | Navigate to Games page | Navigation |
| Completed Game Card | Rate recent game | `/api/games/completed` |
| Upcoming Game Cards | Scheduled games | `/api/games/upcoming` |

**Game Card Contents:**
| Element | Purpose |
|---------|---------|
| Time Block | Time + AM/PM + Date |
| Title | e.g., "Doubles Match" |
| Venue | Location name |
| Status Badge | "confirmed" / "pending" / "open" / "completed" |
| Player Avatars | Participants |
| Spots Counter | e.g., "3/4" |
| Calendar Button | Add to calendar (confirmed only) |
| Rate Now | Tap to rate (completed only) |

#### DiNKR Rating Card
| Element | Purpose | Data Source |
|---------|---------|-------------|
| Current Rating | Large number (e.g., 3.78) | User profile |
| Rating Trend | e.g., "+0.12 this week" | Rating history |
| Wins | Count | Statistics |
| Losses | Count | Statistics |
| Streak | Current streak | Statistics |
| Total | Total games | Statistics |
| View Full Dashboard | Navigate to Dashboard | Navigation |

#### KiTCHN Feed Section
| Element | Purpose | Data Source |
|---------|---------|-------------|
| Section Header | "KiTCHN Feed" | Static |
| View All Link | Navigate to KiTCHN page | Navigation |
| Post Cards | Social feed items | `/api/feed` |

**Post Types:**
| Type | Icon | Purpose |
|------|------|---------|
| Dink | ⚡ | Quick thoughts/updates |
| Drop | 💬 | Longer stories/insights |
| Slam | 🏆 | Victory celebrations |
| Love-Match | 💕 | Dating success (only if opted-in) |

**Post Card Contents:**
| Element | Purpose |
|---------|---------|
| Author Avatar | Clickable → profile |
| Author Name | Clickable → profile |
| Timestamp | e.g., "2h ago" |
| Type Badge | Dink/Drop/Slam/Love |
| Content | Post text |
| Like Button + Count | Engagement |
| Comment Button + Count | Opens comments |

---

### 4.3 Bottom Navigation

The persistent navigation bar at the bottom of all screens.

| Position | Icon | Label | Destination | Purpose |
|----------|------|-------|-------------|---------|
| 1 (Left) | 🏠 | Home | Home Page | Main dashboard |
| 2 | 📍 | Venues | Venues Page | Find courts |
| 3 (Center) | ➕ | (none) | Create Modal | Create game/squad/post |
| 4 | 📅 | Games | Games Page | Schedule & join |
| 5 (Right) | 👤 | Profile | Profile Page | User profile |

---

### 4.4 Supporting Pages (via Bottom Nav)

#### Venues Page (`/venues`)

**Purpose:** Discover and browse pickleball courts and facilities

| Section | Contents |
|---------|----------|
| Page Header | Title: "Venue Lobbies", Subtitle: "Find courts and join lobbies near you" |
| Filter Tabs | All Venues \| Clubs \| Public |
| Venue List | Full venue cards with details |

**Filter Behavior:**
- **All Venues:** Show everything
- **Clubs:** Filter where `type === "Club"`
- **Public:** Filter where `type === "Public"` OR `type === "Pay-to-Play"`

🔲 **FILL IN:** Additional venue filters needed?
```
[ ] Distance radius
[ ] Number of courts
[ ] Amenities (lights, indoor, etc.)
[ ] Current availability
[ ] Other: ________________
```

---

#### Games Page (`/games`)

**Purpose:** View, create, and manage scheduled games

| Section | Contents |
|---------|----------|
| Page Header | Title: "Games", Subtitle: "Schedule and join matches" |
| Filter Tabs | All Games \| My Games \| Open |
| Game List | Game cards with actions |

**Filter Behavior:**
- **All Games:** All visible games
- **My Games:** Where `isMyGame === true` AND `status === "confirmed"`
- **Open:** Where `status === "open"`

**Game Actions by Status:**
| Status | Available Actions |
|--------|-------------------|
| `open` | Join Game |
| `pending` | View Details |
| `confirmed` | Add to Calendar, View Details |
| `completed` | Rate Game (opens AI chat) |

🔲 **FILL IN:** Additional game features needed?
```
[ ] Recurring games
[ ] Waitlist functionality
[ ] Game chat/messaging
[ ] Weather integration
[ ] Other: ________________
```

---

#### Active Play Screen (`/play/:gameId`)

**Purpose:** Live scoring interface during active game play. Shows court layout with player positions and real-time score tracking.

**Triggered from:** "Play Game" button on confirmed game card

| Element | Purpose |
|---------|---------|
| Header Bar | Court layout icon, Switch sides icon, END button |
| Match Counter | "MATCH 1 of 3" - current match in series |
| Court Layout | Visual pickleball court with player positions |
| Score Display | Large score boxes for each team |
| Result Text | "Todd & Jordan beat Steve & Dan" |
| Submit Scores Button | Finalize match and calculate ratings |
| View Scores Link | See detailed score breakdown |

**Court Layout Visual:**
```
┌─────────────────────────────────────────┐
│                                         │
│   ┌─────────────┐   │   ┌─────────────┐ │
│   │             │   │   │             │ │
│   │    Todd     │   │   │    Steve    │ │
│   │             │   │   │             │ │
│   ├─────────────┤   │   ├─────────────┤ │
│   │             │   │   │             │ │
│   │   Jordan    │   │   │     Dan     │ │
│   │             │   │   │             │ │
│   └─────────────┘   │   └─────────────┘ │
│                     │                   │
│        TEAM A       │      TEAM B       │
│                     │                   │
│       [ 11 ]        │       [ 7 ]       │
│                                         │
│     Todd & Jordan beat Steve & Dan      │
│                                         │
│         [ Submit Scores ]               │
│                                         │
│            View Scores                  │
└─────────────────────────────────────────┘
```

**Player Position Cards:**
| Element | Purpose |
|---------|---------|
| Player Name | Display name on court position |
| Background Color | Team A (left) vs Team B (right) |
| Position | Top = back court, Bottom = front court |
| Avatar (optional) | Player photo in position |

**Score Controls:**
| Element | Purpose |
|---------|---------|
| Team A Score Box | Tap to increment Team A score |
| Team B Score Box | Tap to increment Team B score |
| Score Color | Winning team highlighted (e.g., gold vs green) |

**Header Actions:**
| Icon | Purpose |
|------|---------|
| Court Layout | Toggle court view options |
| Switch Sides | Swap team positions on court |
| END Button | End game early / forfeit |

**Match Series:**
| Element | Purpose |
|---------|---------|
| Match Counter | Shows current match (e.g., "1 of 3") |
| Dots Indicator | Visual progress through series |

🔲 **FILL IN:** Additional play screen features needed?
```
[ ] Rally counter
[ ] Timeout button
[ ] Serve indicator
[ ] Side-out tracking
[ ] Watch app sync
[ ] Other: ________________
```

---

#### Profile Page (`/profile`)

**Purpose:** Display and manage current user's profile, access settings

| Section | Contents |
|---------|----------|
| Hero Image | User's avatar (blurred background) |
| Badges Row | Love-Match (if enabled), Verified, Skill Level |
| Name | User's full name |
| Meta | Location + DiNKR Rating |
| Stats Grid | Wins, Losses, Streak, Total Games |
| Bio Section | User's biography text |
| Tags Section | Playing style tags |
| Action Buttons | Edit Profile, Settings |
| Secondary Actions | Post to KiTCHN, View Dashboard |

**Profile Badges:**
| Badge | Condition | Style |
|-------|-----------|-------|
| Love-Match | User has Love-Match enabled | Pink gradient |
| Verified | Account verified | Teal |
| Skill Level | Always shown | Gray with skill number |

🔲 **FILL IN:** Additional profile fields needed?
```
[ ] DUPR Rating integration
[ ] Paddle preference
[ ] Playing availability calendar
[ ] Achievements/badges
[ ] Social links
[ ] Other: ________________
```

---

### 4.5 Pages Accessed from Profile

#### Edit Profile Page (`/edit-profile`)

**Purpose:** Modify user profile information

| Field | Type | Validation |
|-------|------|------------|
| Profile Photo | Image upload | Max 5MB, jpg/png |
| Name | Text input | Required |
| Bio | Textarea | Max 500 chars |
| Location | Text input | Auto-complete |
| Skill Level | Dropdown | 2.5 - 5.0 |
| Tags | Multi-select chips | Max 5 |

**Available Tags:**
```
Morning Player, Evening Player, Weekend, Weekday,
Competitive, Casual, Doubles, Singles,
Tournament, Social, Learning, Coach
```

---

#### Settings Page (`/settings`)

**Purpose:** Configure app preferences, manage subscription

| Section | Contents |
|---------|----------|
| **Features** | |
| Love-Match Toggle | Enable/disable dating feature |
| Location Services Toggle | Enable/disable location |
| **Subscription** | |
| Go Premium | Upgrade banner → Premium features |
| For Coaches & Pros | Upgrade banner → Pro tier |
| For Venue Coordinators | Upgrade banner → Venue tier |
| **Account** | |
| Email | Display only |
| Member Since | Display only |
| Privacy | Navigate → Privacy settings |
| Log Out | Sign out action |

**Love-Match Toggle Flow:**
1. User taps toggle → Shows Age Verification modal
2. User confirms 18+ → Shows Love-Match Info modal
3. User confirms → Love-Match enabled
4. If user declines at any step → Toggle reverts to OFF

🔲 **FILL IN:** Additional settings needed?
```
[ ] Notification preferences
[ ] Dark/Light mode
[ ] Language selection
[ ] Distance units (mi/km)
[ ] Privacy controls (profile visibility)
[ ] Blocked users
[ ] Other: ________________
```

---

#### Dashboard Page (`/dashboard`)

**Purpose:** Detailed analytics and game history

| Section | Contents |
|---------|----------|
| Back Button | Return to previous page |
| Header | "DiNKR Dashboard" |
| Rating Summary | Current rating, trend, wins/losses/streak/total |
| Partner Stats | Win % with each partner |
| Game History | List of past games with rating changes |

**Game History Item:**
| Field | Example |
|-------|---------|
| Date | "Jan 22" |
| Teams | "Andrew & Sarah vs Emma & Marcus" |
| Venue | "Lifetime Charlotte" |
| Score | "11-7" |
| Rating Change | "+0.004" or "-0.002" |
| Indicator | Green (win) / Red (loss) |

---

### 4.6 Pages Accessed from Home Sections

#### Players Page (`/players`)

**Purpose:** Discover and connect with other players

| Section | Contents |
|---------|----------|
| Page Header | "Find Players", "Connect with pickleball players nearby" |
| Filter Tabs | All \| Love-Match (if enabled) \| Nearby |
| Player Grid | Player cards in grid layout |

**Note:** Love-Match tab ONLY visible when user has Love-Match enabled.

---

#### Squads Page (`/squads`)

**Purpose:** Manage player groups for easy game invitations

| Section | Contents |
|---------|----------|
| Page Header | "Your Squads" |
| Create Button | Opens Create Squad modal |
| Squad Grid | Squad cards |

---

#### KiTCHN Page (`/kitchen`)

**Purpose:** Social feed with post filtering

| Section | Contents |
|---------|----------|
| Page Header | "The KiTCHN", "Serve up content your way" |
| Filter Tabs | All \| Slams \| Dinks \| Drops \| Love-Match (if enabled) |
| Feed | Filtered post cards |

---

#### Player Profile Page (`/player/:id`)

**Purpose:** View another player's profile

| Section | Contents |
|---------|----------|
| Back Button | Return to previous |
| Hero Image | Player's avatar |
| Badges | Love-Match (if mutual), Verified, Skill |
| Name + Meta | Location, DiNKR Rating |
| Stats Grid | Wins, Losses, Streak, Games |
| Bio | Player's biography |
| Tags | Playing style tags |
| Actions | Message, Schedule Game |

---

### 4.7 Modals

#### Create Game Modal
Triggered from: Center (+) button in bottom nav, "Create" tab in modal

| Field | Type | Options |
|-------|------|---------|
| Game Title | Text | e.g., "Doubles Match" |
| Venue | Dropdown | List of venues |
| Date | Date picker | Calendar |
| Time | Time picker | 24hr format |
| Game Type | Dropdown | Doubles, Singles, Mixed Doubles |
| Spots | Dropdown | 4, 8, 12 |

---

#### Create Squad Modal
Triggered from: Squads page "Create New Squad" button

| Field | Type |
|-------|------|
| Squad Name | Text input |
| Description | Text input |
| Player Search | Search with checkbox selection |

---

#### Search Modal
Triggered from: Header search icon

| Section | Contents |
|---------|----------|
| Search Input | Live filtering |
| Quick Actions | Find Players, Nearby Venues, Open Games, Love-Match (if enabled) |
| Recent Searches | Previously searched terms |
| Results | Players section, Venues section |

---

#### Notifications Modal
Triggered from: Header bell icon

| Notification Types | Actions |
|--------------------|---------|
| Message | Opens chat thread |
| Game Invite | Accept/Decline buttons |
| Love-Match | View profile |
| Rating Update | View dashboard |
| Squad Activity | View squad |
| Post Engagement | View post |

---

#### Rating Chat Modal (AI-Powered)
Triggered from: Tapping "Rate Now" on completed game

**Purpose:** Conversational rating calculation with Claude AI

| Step | Assistant Asks | User Provides |
|------|----------------|---------------|
| 1 | Competition type | Social, League, Tournament Pool, Championship |
| 2 | Rating bracket | e.g., "3.5-4.0" |
| 3 | Division type | Men's, Women's, Mixed, Open |
| 4 | Age range | Open, Under 30, 20-40, 40-60, 60+ |
| 5 | Scoring format | Traditional to 11, Rally to 15/21/25 |
| 6 | Team A players + ratings | Names and current ratings |
| 7 | Team B players + ratings | Names and current ratings |
| 8 | Actual score | Final match score |
| 9 | Competitiveness | Score only, Rally count, Duration, Team feedback |
| 10 | Final | Rating changes displayed |

---

#### Calendar Modal
Triggered from: Calendar button on confirmed games

| Options | Action |
|---------|--------|
| Google Calendar | Opens Google Calendar URL |
| Apple Calendar / .ics | Downloads .ics file |

---

#### Age Verification Modal
Triggered from: Enabling Love-Match

| Content | Action |
|---------|--------|
| "Are you at least 18 years old?" | Yes → Info modal, No → Close |

---

#### Love-Match Info Modal
Triggered from: After age verification

| Content | |
|---------|---|
| What enabling does | Badge on profile, see other users, posts in KiTCHN |
| Actions | Enable (activates feature), Cancel (closes) |

---

#### Create Post Modal
Triggered from: Profile page "Post to KiTCHN" button

| Field | Options |
|-------|---------|
| Post Type | Dink, Drop, Slam, Love (if enabled) |
| Content | Textarea with type-specific placeholder |

---

#### Comment Modal
Triggered from: Comment button on posts

| Section | Contents |
|---------|----------|
| Original Post | Full post content |
| Comments List | Threaded replies |
| Reply Input | Add new comment |

---

#### Message Modal
Triggered from: Message button on player profile

| Section | Contents |
|---------|----------|
| Header | Player name |
| Message Thread | Chat history |
| Input | New message + send |

---

#### Schedule Game Modal
Triggered from: Schedule button on player profile

| Field | Type |
|-------|------|
| Game Type | Dropdown |
| Venue | Dropdown |
| Date | Date picker |
| Time | Time picker |
| Message | Text input |

---

## 5. Marketing Website - Complete Mapping

**URL:** `dinkr.co`

### 5.1 Navigation Structure

| Nav Item | Page | File |
|----------|------|------|
| Rating System | AI rating explanation + demo | `ratings.html` |
| About | Team bios + mission | `about.html` |
| How It Works | 3-phase getting started | `howitworks.html` |
| Love-Match | Dating feature | `love.html` |
| Features | Feature overview | `features.html` |
| FAQ | Questions + Learn Pickleball | `faq.html` |

### 5.2 Page Details

#### Homepage (`index.html`)

| Section | Purpose |
|---------|---------|
| Hero | Full-screen image, tagline, app download buttons |
| Feature Grid | 6 core features overview |
| Love Banner | CTA for Love-Match feature |
| App Screenshots | Visual product showcase |
| KiTCHN Preview | Social feed explanation |
| CTA Section | Download links |
| Footer | Links, social, legal |

#### Rating System (`ratings.html`)

| Section | Purpose |
|---------|---------|
| Hero | "Fair. Transparent. Real." |
| AI Chat Demo | Auto-playing conversation showing rating flow |
| Performance Over Outcomes | Philosophy explanation |
| What Makes Different | 4 pillars with details |
| Why It Matters | Context-aware explanation |
| Works Alongside DUPR | Complementary positioning |

#### About (`about.html`)

| Section | Purpose |
|---------|---------|
| Hero | "The DiNKR Mission" |
| Why We Built | Story and philosophy |
| Team Grid | Dan, Steve, Shamus bios |

#### How It Works (`howitworks.html`)

| Phase | Steps |
|-------|-------|
| Phase 1: Get Started | 1. Profile, 2. Venues, 3. Squads |
| Phase 2: Play | 4. Find Partners, 5. Create Game, 6. Keep Score |
| Phase 3: Improve | 7. Track Rating, 8. Review Stats |

#### Love-Match (`love.html`)

| Section | Purpose |
|---------|---------|
| Hero (pink gradient) | "Find Love on the Court" |
| 3 Feature Cards | Private, Only Opted-In, Activity-Based |
| Privacy Toggle Demo | Visual ON/OFF states |
| How It Works | Step-by-step |
| Why Pickleball Works | Dating benefits |

#### Features (`features.html`)

| Section | Purpose |
|---------|---------|
| Founder Quote | Dan Brooks quote |
| Core Features Grid | 6 features with "Try it now" links |
| Game Scoring | Screenshot + description |
| Player Matching | Screenshot + description |
| KiTCHN | Post types explanation |
| Coming Soon | Pro + Venue features |

#### FAQ (`faq.html`)

| Section | Questions |
|---------|-----------|
| About DiNKR | What is it, Account, Squads, Lobbies, Rating, Love-Match, Cost, Travel |
| Learn Pickleball | What is it, Scoring, Kitchen, Dink |

#### Contact (`contact.html`)

| Field | Type |
|-------|------|
| First Name | Text |
| Last Name | Text |
| Email | Email |
| Message | Textarea |

#### Privacy (`privacy.html`) & Terms (`terms.html`)

🔲 **FILL IN:** Legal content needed from attorneys

---

## 6. iOS & Android Mobile Apps

**Repository:** `dinkr-mobile` (React Native)

### 6.1 Shared Components with Web App

The React Native app shares **80%+ of business logic** with the web app:

| Shared | Platform-Specific |
|--------|-------------------|
| All page components | Navigation (React Navigation vs Router) |
| All modal components | Native gestures (swipe, haptics) |
| Data/state management | Push notifications |
| API calls | Camera/gallery access |
| Validation logic | Biometric auth |
| Icons (SVG) | App Store requirements |

### 6.2 Mobile-Specific Features

| Feature | iOS | Android |
|---------|-----|---------|
| Push Notifications | APNs | FCM |
| Social Auth | Apple Sign-In (required) | Google Play Services |
| Biometric | Face ID / Touch ID | Fingerprint / Face |
| Camera Access | Native | Native |
| Location | Core Location | Location Services |
| Calendar Sync | EventKit | Calendar Provider |
| Watch Connectivity | WatchKit | Wear OS Data Layer |

### 6.3 App Store Requirements

#### iOS (App Store Connect)

| Requirement | Status |
|-------------|--------|
| App Icon (1024x1024) | 🔲 FILL IN |
| Screenshots (6.5", 5.5") | 🔲 FILL IN |
| Privacy Policy URL | 🔲 FILL IN |
| App Description | 🔲 FILL IN |
| Keywords | 🔲 FILL IN |
| Support URL | 🔲 FILL IN |
| Marketing URL | `https://dinkr.co` |

#### Android (Google Play Console)

| Requirement | Status |
|-------------|--------|
| App Icon (512x512) | 🔲 FILL IN |
| Feature Graphic (1024x500) | 🔲 FILL IN |
| Screenshots (phone, tablet) | 🔲 FILL IN |
| Short Description | 🔲 FILL IN |
| Full Description | 🔲 FILL IN |
| Privacy Policy URL | 🔲 FILL IN |

---

## 7. Watch Apps

### 7.1 iOS Watch App (Swift/SwiftUI)

**Repository:** `dinkr-watch-ios`

**Primary Purpose:** Game scoring during play

| Screen | Features |
|--------|----------|
| **Score Screen** | Large score display, tap to add point, team indicators |
| **Game Setup** | Select teams, scoring format, start game |
| **Match Summary** | Final score, duration, sync to phone |
| **Complications** | Show current score on watch face |

**Watch ↔ iPhone Communication:**
```
iPhone App                      Watch App
    │                               │
    │◄────── Sync Game Data ───────►│
    │                               │
    │◄─── Push Score Updates ──────►│
    │                               │
    │◄── Receive Final Score ───────│
```

🔲 **FILL IN:** Additional watch features needed?
```
[ ] Heart rate tracking during games
[ ] Workout integration
[ ] Quick stats glance
[ ] Rally counter
[ ] Other: ________________
```

### 7.2 Android Watch App (Kotlin/Wear OS)

**Repository:** `dinkr-watch-android`

**Same features as iOS watch, adapted for Wear OS:**

| Screen | Features |
|--------|----------|
| **Score Screen** | Large score, rotary input support |
| **Game Setup** | Select teams, format |
| **Match Summary** | Final score, sync |
| **Tiles** | Quick score display |

---

## 8. User Roles & Journeys

### 8.1 User Tiers

All users start as **Players**. They upgrade through Settings.

```
┌────────────────────────────────────────────────────────────────┐
│                           PLAYER                                │
│  • Free tier                                                    │
│  • Create/join games                                           │
│  • Join venue lobbies                                          │
│  • Create squads                                               │
│  • Basic rating tracking                                       │
│  • KiTCHN access                                               │
│  • Love-Match (optional)                                       │
└────────────────────────────────────────────────────────────────┘
                              │
            ┌─────────────────┼─────────────────┐
            ▼                 ▼                 ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│    PREMIUM       │ │      PRO         │ │     VENUE        │
│                  │ │                  │ │   COORDINATOR    │
│ • Unlimited msg  │ │ • Coach profile  │ │ • Venue tools    │
│ • Advanced stats │ │ • Lesson booking │ │ • AI waitlist    │
│ • Priority       │ │ • Client mgmt    │ │ • Court sched    │
│   matching       │ │ • Dinked-In      │ │ • Event hosting  │
│                  │ │   listing        │ │                  │
└──────────────────┘ └──────────────────┘ └──────────────────┘
```

### 8.2 Player Journey (Core Flow)

```
1. DISCOVERY
   └─► Find app (store/website/referral)

2. ONBOARDING
   └─► Download → Create Account → Setup Profile → Join Venues

3. CONNECT
   └─► Browse Players → Send Messages → Build Squads

4. PLAY
   └─► Create/Join Games → Play Match → Score via Watch

5. IMPROVE
   └─► Rate Game (AI Chat) → Track Rating → View Dashboard

6. ENGAGE
   └─► Post to KiTCHN → Comment/Like → Enable Love-Match (optional)

7. UPGRADE (optional)
   └─► Go Premium / Pro / Venue Coordinator
```

### 8.3 Premium User Journey

🔲 **FILL IN:** Premium features and pricing
```
Monthly Price: $________
Annual Price: $________

Premium Features:
[ ] Unlimited messages (vs. ___/day free)
[ ] Advanced analytics
[ ] Priority in matching
[ ] No ads
[ ] Other: ________________
```

### 8.4 Pro (Coach) User Journey

🔲 **FILL IN:** Pro requirements and features
```
Verification Required:
[ ] IPTPA Certification
[ ] PPR Certification
[ ] USAPA Rating
[ ] Other: ________________

Pro Features:
[ ] Dinked-In coach directory listing
[ ] In-app lesson booking
[ ] Client management dashboard
[ ] Revenue: ___% commission per booking
[ ] Other: ________________
```

### 8.5 Venue Coordinator Journey

🔲 **FILL IN:** Venue coordinator features
```
Verification Required:
[ ] Venue ownership proof
[ ] Manager authorization
[ ] Other: ________________

Venue Features:
[ ] AI-powered waitlist management
[ ] Court scheduling tools
[ ] Event creation and management
[ ] Player check-in
[ ] Revenue: $________/month
[ ] Other: ________________
```

---

## 9. API Endpoints

### 9.1 Authentication

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/auth/login` | Email/password login |
| POST | `/api/auth/register` | Create account |
| POST | `/api/auth/apple` | Apple Sign-In |
| POST | `/api/auth/google` | Google Sign-In |
| POST | `/api/auth/logout` | Sign out |
| POST | `/api/auth/refresh` | Refresh token |
| POST | `/api/auth/forgot-password` | Password reset |

### 9.2 Users

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/users/me` | Current user profile |
| PUT | `/api/users/me` | Update profile |
| GET | `/api/users/{id}` | Get user by ID |
| GET | `/api/users/nearby` | Nearby players (location) |
| PUT | `/api/users/me/settings` | Update settings |
| POST | `/api/users/me/avatar` | Upload avatar |

### 9.3 Players

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/players/nearby` | Nearby players with filters |
| GET | `/api/players/{id}` | Player profile |
| GET | `/api/players/love-match` | Love-Match enabled players |
| POST | `/api/players/{id}/message` | Send message |

### 9.4 Squads

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/squads` | User's squads |
| POST | `/api/squads` | Create squad |
| GET | `/api/squads/{id}` | Squad details |
| PUT | `/api/squads/{id}` | Update squad |
| DELETE | `/api/squads/{id}` | Delete squad |
| POST | `/api/squads/{id}/members` | Add member |
| DELETE | `/api/squads/{id}/members/{userId}` | Remove member |
| POST | `/api/squads/{id}/invite` | Invite squad to game |

### 9.5 Venues

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/venues/nearby` | Nearby venues |
| GET | `/api/venues/{id}` | Venue details |
| GET | `/api/venues/{id}/lobby` | Venue lobby members |
| POST | `/api/venues/{id}/join` | Join venue lobby |
| POST | `/api/venues/{id}/leave` | Leave venue lobby |

### 9.6 Games

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/games/upcoming` | User's upcoming games |
| GET | `/api/games/available` | Open games nearby |
| GET | `/api/games/completed` | Completed unrated games |
| GET | `/api/games/history` | Game history |
| POST | `/api/games` | Create game |
| GET | `/api/games/{id}` | Game details |
| PUT | `/api/games/{id}` | Update game |
| DELETE | `/api/games/{id}` | Cancel game |
| POST | `/api/games/{id}/join` | Join game |
| POST | `/api/games/{id}/leave` | Leave game |
| POST | `/api/games/{id}/invite` | Invite player |
| POST | `/api/games/{id}/score` | Submit score (from watch) |

### 9.7 Ratings

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/ratings/chat` | AI rating conversation |
| GET | `/api/ratings/history` | Rating history |
| GET | `/api/ratings/predict` | Pre-match prediction |
| GET | `/api/ratings/partners` | Partner stats |

### 9.8 Feed (KiTCHN)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/feed` | Get posts (with filters) |
| POST | `/api/feed` | Create post |
| GET | `/api/feed/{id}` | Get post details |
| DELETE | `/api/feed/{id}` | Delete post |
| POST | `/api/feed/{id}/like` | Like post |
| DELETE | `/api/feed/{id}/like` | Unlike post |
| GET | `/api/feed/{id}/comments` | Get comments |
| POST | `/api/feed/{id}/comments` | Add comment |

### 9.9 Notifications

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/notifications` | Get notifications |
| PUT | `/api/notifications/read` | Mark all read |
| PUT | `/api/notifications/{id}/read` | Mark one read |
| POST | `/api/notifications/token` | Register push token |

### 9.10 Messages

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/messages` | Get conversations |
| GET | `/api/messages/{conversationId}` | Get messages |
| POST | `/api/messages/{conversationId}` | Send message |

🔲 **FILL IN:** Additional endpoints needed?
```
[ ] Payments/subscriptions
[ ] Coach booking
[ ] Venue management
[ ] Analytics/reporting
[ ] Other: ________________
```

---

## 10. React to React Native Workflow

### 10.1 How Code Sharing Works

The web app (React) and mobile app (React Native) share most business logic:

```
┌─────────────────────────────────────────────────────────────────┐
│                    SHARED CODE (~80%)                            │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │  Components  │  │    Hooks     │  │   Utilities  │          │
│  │              │  │              │  │              │          │
│  │ • PlayerCard │  │ • useAuth    │  │ • formatDate │          │
│  │ • GameCard   │  │ • useGames   │  │ • validation │          │
│  │ • VenueCard  │  │ • useRating  │  │ • api client │          │
│  │ • PostCard   │  │ • usePlayer  │  │ • helpers    │          │
│  │ • Modals     │  │ • useFeed    │  │              │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │    State     │  │     Types    │  │   Constants  │          │
│  │  Management  │  │              │  │              │          │
│  │              │  │ • TypeScript │  │ • Colors     │          │
│  │ • Context    │  │   interfaces │  │ • Config     │          │
│  │ • Reducers   │  │ • API types  │  │ • Routes     │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────────────────────────────────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│   WEB (React)    │ │   iOS (RN)       │ │  Android (RN)    │
│                  │ │                  │ │                  │
│ • react-router   │ │ • React Nav      │ │ • React Nav      │
│ • CSS            │ │ • StyleSheet     │ │ • StyleSheet     │
│ • HTML elements  │ │ • Native views   │ │ • Native views   │
│                  │ │ • Push (APNs)    │ │ • Push (FCM)     │
│                  │ │ • Apple Auth     │ │ • Google Auth    │
└──────────────────┘ └──────────────────┘ └──────────────────┘
```

### 10.2 Development Workflow

```
STEP 1: Develop in Web App (dinkr-webapp)
─────────────────────────────────────────
┌─────────────────────────────────────────┐
│  1. Create/modify component in React    │
│  2. Test in browser                     │
│  3. Commit to GitHub                    │
│  4. Push to staging branch              │
│  5. Auto-deploy to staging.dinkr.co    │
│  6. Test on staging                     │
│  7. Merge to main                       │
│  8. Auto-deploy to dinkr.co/webapp     │
└─────────────────────────────────────────┘
                    │
                    ▼
STEP 2: Port to React Native (dinkr-mobile)
───────────────────────────────────────────
┌─────────────────────────────────────────┐
│  1. Copy shared logic (hooks, utils)    │
│  2. Adapt component for native:         │
│     - <div> → <View>                    │
│     - <span> → <Text>                   │
│     - <img> → <Image>                   │
│     - CSS → StyleSheet                  │
│  3. Add native-specific features        │
│  4. Test on iOS simulator               │
│  5. Test on Android emulator            │
│  6. Commit to GitHub                    │
│  7. Push to staging branch              │
└─────────────────────────────────────────┘
                    │
                    ▼
STEP 3: Deploy via Fastlane
───────────────────────────
┌─────────────────────────────────────────┐
│  1. Run Fastlane build command          │
│  2. Fastlane handles:                   │
│     - Increment version numbers         │
│     - Build iOS archive                 │
│     - Build Android AAB                 │
│     - Code signing                      │
│     - Upload to TestFlight/Play Beta    │
│  3. QA tests on devices                 │
│  4. Approve release                     │
│  5. Fastlane promotes to production     │
└─────────────────────────────────────────┘
```

### 10.3 Component Translation Example

**Web (React):**
```jsx
// components/PlayerCard.jsx
const PlayerCard = ({ player, onClick }) => {
  return (
    <div className="player-card" onClick={() => onClick(player)}>
      <img src={player.image} alt={player.name} />
      <div className="player-name">{player.name}</div>
      <div className="player-rating">{player.rating}</div>
    </div>
  );
};
```

**Mobile (React Native):**
```jsx
// components/PlayerCard.jsx
import { View, Text, Image, TouchableOpacity, StyleSheet } from 'react-native';

const PlayerCard = ({ player, onPress }) => {
  return (
    <TouchableOpacity style={styles.card} onPress={() => onPress(player)}>
      <Image source={{ uri: player.image }} style={styles.image} />
      <Text style={styles.name}>{player.name}</Text>
      <Text style={styles.rating}>{player.rating}</Text>
    </TouchableOpacity>
  );
};

const styles = StyleSheet.create({
  card: { /* equivalent to .player-card CSS */ },
  image: { /* equivalent to img CSS */ },
  name: { /* equivalent to .player-name CSS */ },
  rating: { /* equivalent to .player-rating CSS */ },
});
```

### 10.4 Fastlane Configuration

**iOS (`fastlane/Fastfile`):**
```ruby
lane :beta do
  increment_build_number
  build_app(scheme: "DiNKR")
  upload_to_testflight
end

lane :release do
  build_app(scheme: "DiNKR")
  upload_to_app_store
end
```

**Android (`fastlane/Fastfile`):**
```ruby
lane :beta do
  gradle(task: "bundleRelease")
  upload_to_play_store(track: "internal")
end

lane :release do
  gradle(task: "bundleRelease")
  upload_to_play_store(track: "production")
end
```

---

## 11. Server Folder Structure

### OPTION A: Azure App Service Structure

```
/home/site/wwwroot/
│
├── website/                    # dinkr.co
│   ├── index.html
│   ├── about.html
│   ├── features.html
│   ├── howitworks.html
│   ├── love.html
│   ├── ratings.html
│   ├── faq.html
│   ├── contact.html
│   ├── privacy.html
│   ├── terms.html
│   ├── styles.css
│   ├── icons.css
│   ├── nav.js
│   └── assets/
│       └── images/
│
├── webapp/                     # dinkr.co/webapp
│   ├── index.html
│   ├── static/
│   │   ├── css/
│   │   │   └── main.[hash].css
│   │   └── js/
│   │       ├── main.[hash].js
│   │       └── [chunk].[hash].js
│   ├── assets/
│   │   └── images/
│   └── manifest.json
│
└── api/                        # api.dinkr.co (separate App Service)
    ├── controllers/
    ├── models/
    ├── routes/
    ├── middleware/
    ├── services/
    └── config/
```

### 11.2 Azure Nginx/IIS Routing

```
# dinkr.co routing

/                   → /website/index.html
/about              → /website/about.html
/features           → /website/features.html
/howitworks         → /website/howitworks.html
/love               → /website/love.html
/ratings            → /website/ratings.html
/faq                → /website/faq.html
/contact            → /website/contact.html
/privacy            → /website/privacy.html
/terms              → /website/terms.html

/webapp             → /webapp/index.html
/webapp/*           → /webapp/index.html (SPA fallback)

/api/*              → proxy to api.dinkr.co
```

---

### OPTION B: Railway Structure

Railway deploys each service as a **separate container**. No shared folder structure - each repo is its own service.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         RAILWAY DASHBOARD                                │
│                                                                          │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐         │
│  │  dinkr-website  │  │  dinkr-webapp   │  │   dinkr-api     │         │
│  │  (Service 1)    │  │  (Service 2)    │  │  (Service 3)    │         │
│  │                 │  │                 │  │                 │         │
│  │  dinkr.co       │  │ app.dinkr.co    │  │ api.dinkr.co    │         │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘         │
│                                                                          │
│  ┌─────────────────┐                                                    │
│  │   PostgreSQL    │                                                    │
│  │  (Database)     │                                                    │
│  │                 │                                                    │
│  │ Internal access │                                                    │
│  └─────────────────┘                                                    │
└─────────────────────────────────────────────────────────────────────────┘
```

**Each Service Contains:**

```
dinkr-website/              # Deployed as its own container
├── build/                  # Railway runs `npm run build`
│   ├── index.html
│   ├── about.html
│   ├── features.html
│   └── ...
├── package.json
└── railway.json            # Optional Railway config

dinkr-webapp/               # Deployed as its own container
├── build/
│   ├── index.html
│   ├── static/
│   │   ├── css/
│   │   └── js/
│   └── manifest.json
├── package.json
└── railway.json

dinkr-api/                  # Deployed as its own container
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   └── services/
├── package.json
└── railway.json
```

### 11.3 Railway Domain Configuration

```
# Custom domains in Railway dashboard

Service: dinkr-website
├── Custom Domain: dinkr.co
└── Railway Domain: dinkr-website-production.up.railway.app

Service: dinkr-webapp  
├── Custom Domain: app.dinkr.co
└── Railway Domain: dinkr-webapp-production.up.railway.app

Service: dinkr-api
├── Custom Domain: api.dinkr.co
└── Railway Domain: dinkr-api-production.up.railway.app
```

### 11.4 Railway Environment Variables

```
# Set in Railway dashboard for each service

dinkr-webapp:
├── REACT_APP_API_URL=https://api.dinkr.co
└── NODE_ENV=production

dinkr-api:
├── DATABASE_URL=postgresql://... (auto-injected by Railway)
├── JWT_SECRET=🔲 FILL IN
├── APPLE_CLIENT_ID=🔲 FILL IN
├── GOOGLE_CLIENT_ID=🔲 FILL IN
└── NODE_ENV=production
```

---

## 12. Development Workflow

### 12.1 Git Branching Strategy

```
main (production)
  │
  ├── staging (pre-production testing)
  │     │
  │     ├── feature/user-auth
  │     ├── feature/game-scoring
  │     ├── feature/love-match
  │     └── bugfix/rating-calc
  │
  └── hotfix/* (emergency production fixes)
```

### 12.2 PR Process

1. Create feature branch from `staging`
2. Develop and test locally
3. Push and create PR to `staging`
4. Code review (min 1 approval)
5. Auto-deploy to staging environment
6. QA testing
7. Create PR from `staging` to `main`
8. Final approval
9. Merge → Auto-deploy to production

### 12.3 CI/CD Pipeline (Azure DevOps)

```yaml
# azure-pipelines.yml (simplified)

trigger:
  branches:
    include:
      - main
      - staging

stages:
  - stage: Build
    jobs:
      - job: BuildWebsite
        steps:
          - task: NodeTool@0
            inputs:
              versionSpec: '18.x'
          - script: npm install && npm run build
          - publish: $(Build.ArtifactStagingDirectory)

  - stage: DeployStaging
    condition: eq(variables['Build.SourceBranch'], 'refs/heads/staging')
    jobs:
      - deployment: DeployToStaging
        environment: 'staging'
        strategy:
          runOnce:
            deploy:
              steps:
                - task: AzureWebApp@1
                  inputs:
                    appName: 'dinkr-staging'

  - stage: DeployProduction
    condition: eq(variables['Build.SourceBranch'], 'refs/heads/main')
    jobs:
      - deployment: DeployToProduction
        environment: 'production'
        strategy:
          runOnce:
            deploy:
              steps:
                - task: AzureWebApp@1
                  inputs:
                    appName: 'dinkr-production'
```

---

## 13. Next Steps Checklist

### Immediate (Week 1-2)

- [ ] Review and fill in all `🔲 FILL IN:` sections in this document
- [ ] Finalize user tier pricing (Premium, Pro, Venue)
- [ ] Complete legal documents (Privacy Policy, Terms)
- [ ] Set up GitHub repositories with structure
- [ ] Configure Azure resources

### Short-term (Week 3-4)

- [ ] Migrate prototype to React with proper structure
- [ ] Set up API skeleton with authentication
- [ ] Configure CI/CD pipelines
- [ ] Begin database schema design

### Medium-term (Month 2)

- [ ] Complete web app MVP
- [ ] Begin React Native mobile development
- [ ] Integrate Claude AI for rating chat
- [ ] Start watch app development

### Long-term (Month 3+)

- [ ] Beta testing (TestFlight / Play Beta)
- [ ] App Store submissions
- [ ] Production launch
- [ ] Marketing campaign

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 🔲 FILL IN | Andrew Couch | Initial document |

---

## Notes & Questions for Team

🔲 **FILL IN:** Add your notes, questions, and decisions here:

```
1. 


2. 


3. 


4. 


5. 


```

---

**Document maintained by:** DiNKR Development Team

**Last updated:** January 28, 2026

**For questions:** Contact andrew@ghost081280.com
