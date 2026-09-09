# VINTHA — Technical Architecture

**Product:** VINTHA  
**Version:** MVP v1  
**Status:** Architecture Specification


## 1. Overview

VINTHA is an India-first mobile application that turns digital discoveries into real-world experiences.

The system allows a user to:

1. Discover a place online.
2. Share the discovery with VINTHA.
3. Provide VINTHA with a URL, screenshot, text, or manual input.
4. Extract useful place clues using AI.
5. Search real-world place providers.
6. Match and verify the best place candidate.
7. Ask the user to confirm the result.
8. Save the verified place.
9. Display saved places on a personal map.
10. Detect when the user is near a saved place.
11. Send an optional nearby reminder.
12. Allow the user to visit and mark the place as visited.


# 2. High-Level Architecture
.
                         USER
                           │
                           ▼
                  ┌─────────────────┐
                  │  Flutter Mobile │
                  │      App        │
                  └────────┬────────┘
                           │
                         HTTPS
                           │
                           ▼
                  ┌─────────────────┐
                  │  FastAPI        │
                  │  Backend        │
                  └────────┬────────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
        AI Service    Place Providers  PostgreSQL
             │             │
             │       ┌─────┴─────┐
             │       │           │
             │    Provider A  Provider B
             │
             ▼
       Place Extraction
             │
             ▼
       Matching Engine
             │
             ▼
       User Confirmation
             │
             ▼
        Saved Place

    3. Technology Stack
Mobile Application

Technology: Flutter

Responsibilities:

User interface
Navigation
Authentication flow
Share/import interface
Map display
Saved-place display
Location permission handling
Nearby experience
Notifications
API communication
Backend

Technology: Python + FastAPI

Responsibilities:

Authentication/session handling
API endpoints
Input processing
AI integration
Place provider integration
Candidate matching
Saved-place management
Visit management
Business logic
Security and validation
Database

Technology: PostgreSQL

Responsibilities:

Users
Saved places
Sources
Visits
Place provider references
User preferences
Application data
AI Layer

The AI layer is responsible for understanding messy user-provided content.

The AI should primarily:

Extract place names
Extract addresses
Identify cities
Identify neighborhoods
Identify categories
Extract useful keywords
Interpret captions or text
Analyze supported images/screenshots
Produce structured place-search clues

The AI should not be treated as the final source of truth for geographic information.

Place Intelligence Layer

VINTHA uses external place providers to obtain real-world location information.

The initial implementation is expected to use a major places/maps provider, with the architecture designed so additional providers can be added later.

The system should be able to obtain information such as:

Place name
Address
Coordinates
Category
Provider place ID
Available place metadata
4. Core Design Principle
AI is not the source of truth

VINTHA follows this process:

User Input
    ↓
AI extracts clues
    ↓
Place Provider Search
    ↓
Candidate Places
    ↓
VINTHA Matching Engine
    ↓
Best Candidate
    ↓
User Confirmation
    ↓
Saved Place

VINTHA must not blindly save an AI-generated location.

The user must confirm the identified place before it becomes a saved place.

5. Place Identification Pipeline
Step 1 — Input

VINTHA may receive:

Shared social-media URL
Pasted URL
Screenshot
User-provided text
Manual place name
Other supported discovery inputs
Step 2 — Input Processing

The backend determines what information is available.

Possible information includes:

URL
Page metadata where available
Caption/text
User-provided description
Image content
OCR text
Platform information

VINTHA must not depend on unrestricted scraping of social-media platforms.

The implementation must respect the technical capabilities and access policies of each supported platform.

Step 3 — AI Extraction

The AI converts available information into structured clues.

Example:

{
  "possible_name": "Example Cafe",
  "city": "Bengaluru",
  "area": "Indiranagar",
  "category": "cafe",
  "keywords": [
    "coffee",
    "dessert"
  ]
}

Some fields may be unknown.

The system should allow partial information.

Step 4 — Place Search

The extracted clues are sent to one or more configured place providers.

Example search:

Place: Example Cafe
Area: Indiranagar
City: Bengaluru
Category: Cafe

The provider returns candidate places.

Step 5 — Candidate Matching

VINTHA compares the AI clues against returned candidates.

Possible matching signals include:

Name similarity
Address similarity
City match
Neighborhood match
Category match
Keyword similarity
Geographic consistency
Other available metadata

The matching engine produces ranked candidates.

Example:

Candidate A — 94%
Candidate B — 67%
Candidate C — 31%
6. Confidence Handling

VINTHA should not treat every result as equally reliable.

High Confidence

If one candidate clearly matches the available evidence:

Best Candidate
      ↓
User Confirmation
Medium / Low Confidence

If multiple candidates are plausible:

Possible Places

1. Candidate A
2. Candidate B
3. Candidate C

Which one did you mean?
No Reliable Match

Display:

Hmm... we couldn't identify this place.

Actions:

Try Again
Add Screenshot
Enter Manually
Cancel

The user must always have a recovery path.

7. Provider Abstraction

VINTHA should not tightly couple the entire application to one place provider.

Conceptually:

                 PlaceProvider
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
      Provider A  Provider B  Provider C

The backend should communicate with providers through a common internal interface.

Example conceptual interface:

search_places()
get_place_details()

This allows future providers to be added without rewriting the entire application.

Reasons include:

Pricing changes
API limits
Geographic coverage
Availability
Licensing
Reliability
Future international expansion
8. Database Architecture

The initial database contains the following core entities:

USER
  │
  │ 1
  │
  │ many
  ▼
SAVED_PLACE
  │
  ├───────────────┐
  │               │
  ▼               ▼
SOURCE           VISIT
Users

Stores account information.

Conceptual fields:

id
name
email
profile_photo_url
created_at
updated_at

Authentication credentials should be handled using a secure authentication system rather than storing raw passwords.

Saved Places

Stores the user's verified discoveries.

Conceptual fields:

id
user_id
place_name
address
latitude
longitude
category
notes
thumbnail_url
provider
provider_place_id
status
saved_at
visited_at
created_at
updated_at
Sources

Stores where a discovery originated.

Conceptual fields:

id
saved_place_id
platform
url
title
thumbnail_url
captured_at

Possible platforms include:

Instagram
TikTok
YouTube
Website
Screenshot
Manual
Visits

Stores real-world visits.

Conceptual fields:

id
saved_place_id
visited_at
created_at

A saved place may eventually have multiple visit records.

9. API Architecture

The mobile application communicates with the backend through HTTPS APIs.

Initial API structure:

/api/v1

/auth
/users
/places
/imports
/sources
/visits
Authentication

Example operations:

POST /api/v1/auth/...
GET  /api/v1/users/me

Authentication implementation may use a third-party identity provider or secure backend authentication system.

Places

Conceptual operations:

GET    /api/v1/places
POST   /api/v1/places
GET    /api/v1/places/{id}
PATCH  /api/v1/places/{id}
DELETE /api/v1/places/{id}
Imports

Used when a user gives VINTHA something to analyze.

POST /api/v1/imports

Example:

{
  "url": "https://example.com/discovery"
}

The backend processes the input and returns a place candidate or processing status.

Visits

Conceptual operations:

POST   /api/v1/places/{id}/visits
GET    /api/v1/places/{id}/visits
10. Import Processing Flow
Flutter
   │
   │ POST /imports
   ▼
FastAPI
   │
   ▼
Input Processor
   │
   ▼
AI Service
   │
   ▼
Place Search
   │
   ▼
Matching Engine
   │
   ▼
Candidate Result
   │
   ▼
Flutter Confirmation Screen
   │
   │ User confirms
   ▼
POST /places
   │
   ▼
PostgreSQL
11. Saved Place Flow
User confirms place
        ↓
Backend validates information
        ↓
Verified place data stored
        ↓
Saved Place created
        ↓
Place appears on Home
        ↓
Place appears on Map
        ↓
Place becomes eligible for Nearby
12. Location Architecture

Location is a sensitive part of VINTHA.

VINTHA should avoid unnecessary continuous tracking.

The location architecture should be designed around:

User consent
Platform permissions
Battery efficiency
Proximity detection
Privacy
Minimum necessary data collection

The exact implementation will be determined during the location-system development phase.

Possible responsibilities include:

Mobile Device
    ↓
Location Services
    ↓
Proximity Evaluation
    ↓
Nearby Saved Places
    ↓
Reminder Decision

Where possible, unnecessary precise location data should not be transmitted to the backend.

13. Nearby Reminder Logic

Conceptually:

User enables nearby reminders
             ↓
Location permission granted
             ↓
VINTHA knows saved place coordinates
             ↓
Location/proximity event occurs
             ↓
Check distance
             ↓
Is user within configured radius?
             ↓
        YES
             ↓
Check reminder history
             ↓
Should notification be shown?
             ↓
        YES
             ↓
"You saved this place 👀"

The system should include protections against repeated notifications.

14. Notification Architecture

Notifications are used to remind users about saved discoveries.

Example:

VINTHA

You saved this place! 👀

The Example Cafe

You're nearby.

Possible actions:

View Place
Directions
Dismiss

Notifications should feel like useful memory assistance rather than advertising.

15. Security Principles

VINTHA should follow secure-by-default practices.

Important principles include:

HTTPS for network communication
Secure authentication
No raw password storage
Server-side authorization checks
Input validation
API rate limiting where appropriate
Secrets stored outside source code
Environment variables for sensitive configuration
Secure database credentials
Minimal collection of sensitive information
Protection against unauthorized access to saved places

API keys and secrets must never be committed to the public GitHub repository.

16. Environment Configuration

Development secrets should be stored using environment configuration.

Example:

.env

DATABASE_URL=...
AI_API_KEY=...
PLACES_API_KEY=...
AUTH_SECRET=...

A safe template should be committed instead:

.env.example

Example:

DATABASE_URL=
AI_API_KEY=
PLACES_API_KEY=
AUTH_SECRET=

Actual secret values must remain private.

17. Backend Project Structure

Initial structure:

backend/
│
├── app/
│   ├── main.py
│   │
│   ├── api/
│   │   ├── auth.py
│   │   ├── users.py
│   │   ├── places.py
│   │   ├── imports.py
│   │   ├── sources.py
│   │   └── visits.py
│   │
│   ├── models/
│   │   ├── user.py
│   │   ├── saved_place.py
│   │   ├── source.py
│   │   └── visit.py
│   │
│   ├── services/
│   │   ├── ai_service.py
│   │   ├── place_search.py
│   │   ├── place_matcher.py
│   │   ├── location_service.py
│   │   └── notification_service.py
│   │
│   ├── providers/
│   │   ├── place_provider.py
│   │   └── ...
│   │
│   └── database/
│       ├── connection.py
│       └── ...
│
├── tests/
├── requirements.txt
└── .env.example
18. Mobile Project Structure

Initial Flutter structure:

mobile/
│
├── lib/
│   ├── main.dart
│   │
│   ├── core/
│   │   ├── constants/
│   │   ├── theme/
│   │   └── routing/
│   │
│   ├── models/
│   │
│   ├── services/
│   │   ├── api_service.dart
│   │   ├── auth_service.dart
│   │   ├── location_service.dart
│   │   └── notification_service.dart
│   │
│   ├── screens/
│   │   ├── splash/
│   │   ├── onboarding/
│   │   ├── auth/
│   │   ├── home/
│   │   ├── add/
│   │   ├── processing/
│   │   ├── confirm_place/
│   │   ├── saved_place/
│   │   ├── map/
│   │   ├── nearby/
│   │   └── profile/
│   │
│   └── widgets/
│
├── assets/
└── pubspec.yaml
19. API Communication Principle

The Flutter application should communicate with VINTHA through the backend.

Flutter
   │
   ▼
VINTHA API
   │
   ├── Database
   ├── AI
   ├── Place Providers
   └── Business Logic

The mobile app should not contain provider secrets or directly access the production database.

20. Scalability Principles

VINTHA should be built so the MVP can grow without requiring a complete rewrite.

Important principles:

Modular backend services
Provider abstraction
Versioned APIs
Database migrations
Clear separation of UI and business logic
Environment-based configuration
Automated testing
Centralized error handling
Logging and monitoring
Secure secret management
21. MVP Scope

The first technical implementation should focus on:

Authentication
     ↓
Share / Import
     ↓
AI extraction
     ↓
Place search
     ↓
Candidate matching
     ↓
User confirmation
     ↓
Save place
     ↓
View saved places
     ↓
Map
     ↓
Nearby functionality
     ↓
Notifications
     ↓
Visited status

Advanced features such as social sharing between users, trips, AI trip planning, recommendation systems and complex analytics should be implemented after the core loop is working reliably.

22. Long-Term Architecture Direction

VINTHA may eventually support:

Multiple AI providers
Multiple place providers
Global location coverage
Personalized recommendations
Shared collections
Trips
Social discovery
Visit history
AI travel planning
Personal discovery analytics
More input sources

The MVP architecture should leave room for these capabilities without implementing them prematurely.

23. Core Architecture Principle

VINTHA's architecture follows one central rule:

Separate understanding from verification.

AI understands the user's messy discovery.

Place providers provide real-world geographic information.

VINTHA's matching layer connects the two.

The user makes the final decision.

UNDERSTAND
    ↓
SEARCH
    ↓
MATCH
    ↓
VERIFY
    ↓
CONFIRM
    ↓
SAVE
    ↓
REMEMBER
    ↓
EXPERIENCE
24. Current Architecture Status

The following decisions are currently approved for VINTHA MVP planning:

Flutter for mobile
Python + FastAPI for backend
PostgreSQL for database
AI service for place-clue extraction
External place provider for geographic verification
Provider abstraction for future flexibility
User confirmation before saving uncertain AI results
Privacy-conscious location architecture
API-based communication between mobile and backend
Secrets kept outside the public repository

Specific third-party services, API credentials, production hosting and deployment configuration will be finalized during implementation.

25. Final Architecture Summary

VINTHA is structured as a modular mobile application backed by a secure API.

The core architecture is:

                  .       VINTHA
                            │
                     Flutter Mobile
                            │
                          HTTPS
                            │
                     FastAPI Backend
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
          ▼                 ▼                 ▼
      AI Service      Place Providers     PostgreSQL
          │                 │
          └──────────┬──────┘
                     ▼
              Matching Engine
                     │
                     ▼
              User Confirmation
                     │
                     ▼
                Saved Place
                     │
             ┌───────┴────────┐
             ▼                ▼
           Map            Nearby System


  VINTHA's technical goal is simple:

Build a reliable bridge between digital discovery and real-world experience.
                              │
                              ▼
                         Notification
                              │
                              ▼
                            Visit
