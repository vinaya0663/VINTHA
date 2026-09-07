# VINTHA — Product Bible

> **See it. Save it. Don't forget it.**

**Project Status:** Early Development  
**Target Market:** India 🇮🇳  
**Founder & Builder:** Vinaya  
**Started:** 2026

---

# 1. Product Overview

VINTHA is an India-first mobile application designed to turn things people discover online into real-world experiences.

People constantly discover restaurants, cafés, stores, events, attractions, activities, and other interesting places through social media and the internet.

The usual behavior is:

**See something → Save the post → Forget about it.**

VINTHA changes this.

The user can share a discovered place with VINTHA. VINTHA identifies the place, saves it, remembers it, and can remind the user when they are near that place.

The core idea is:

**Discover → Save → Remember → Experience**

---

# 2. The Problem

People discover hundreds of interesting places online but rarely visit them.

Common problems include:

- Saving Instagram posts and forgetting about them.
- Losing interesting places among hundreds of saved posts.
- Not remembering the name of a place later.
- Not knowing where a saved place is located.
- Forgetting about places when they are actually nearby.
- Having saved places scattered across different platforms.
- Having no personal system for turning online discoveries into real experiences.

The problem is not discovery.

The problem is **remembering and acting on discoveries at the right time.**

---

# 3. The Solution

VINTHA creates a bridge between digital discovery and physical experience.

A user discovers something online.

They share it with VINTHA.

VINTHA attempts to identify the relevant place or experience.

The place is saved to the user's personal collection.

When the user is later near the saved location, VINTHA can remind them.

### Core loop

**See → Share → Identify → Save → Nearby → Remind → Experience**

---

# 4. Target Users

## Primary Users

Young and digitally active users in India, especially people who frequently discover places through social media.

Initial target age:

**18–35**

Initial focus cities may include:

- Bengaluru
- Mumbai
- Delhi NCR
- Hyderabad
- Chennai
- Pune
- Kolkata

The product should eventually support users throughout India.

---

# 5. Initial Use Cases

VINTHA should initially support discoveries such as:

### Food
- Restaurants
- Cafés
- Bakeries
- Street food
- Dessert places

### Shopping
- Clothing stores
- Local stores
- Markets
- Specialty shops

### Experiences
- Activities
- Attractions
- Events
- Entertainment

### Travel
- Tourist attractions
- Beaches
- Hiking locations
- Hotels
- Hidden spots

The system should be designed so additional categories can be added later.

---

# 6. Core User Journey

## Step 1 — Discover

The user sees something interesting on Instagram or another supported platform.

Example:

> "Hidden café in Bengaluru you have to try."

## Step 2 — Share

The user taps:

**Share → VINTHA**

## Step 3 — Understand

VINTHA receives the shared content or link and attempts to determine:

- Place name
- Address
- City
- Category
- Location
- Relevant information
- Original source

## Step 4 — Confirm

If VINTHA is confident:

> **We found this place**

If uncertain:

> **We think this is this place. Is that correct?**

The user can confirm or correct the result.

## Step 5 — Save

The place becomes part of the user's VINTHA collection.

## Step 6 — Discover Nearby

At a later time, the user physically enters the area around the saved place.

## Step 7 — Remind

VINTHA sends an appropriate notification.

Example:

> **You're near somewhere you saved 👀**
>
> The café you discovered on Instagram is 600m away.

## Step 8 — Experience

The user opens VINTHA and can:

- View the saved place
- View the original content
- View place information
- Get directions
- Mark the place as visited

---

# 7. MVP

The first version of VINTHA should focus on proving the core concept.

## Required MVP Features

### 7.1 User Accounts

- Sign up
- Login
- Logout
- Basic profile

### 7.2 Share to VINTHA

Allow users to share supported links/content into VINTHA.

Initial priority:

**Instagram**

Other platforms can be added later.

### 7.3 Place Identification

VINTHA attempts to determine what place the shared content represents.

### 7.4 Place Verification

VINTHA should verify identified places using reliable location/place data.

AI should not blindly create locations without verification.

### 7.5 Save Place

Store:

- Place name
- Address
- Latitude
- Longitude
- Category
- Source URL
- Source platform
- Date saved
- User who saved it

### 7.6 Personal Saved Places

Users can view everything they have saved.

### 7.7 Map

Display saved places on a personal map.

### 7.8 Nearby Detection

Determine when a user is sufficiently close to a saved location.

### 7.9 Notifications

Notify users when an appropriate saved place is nearby.

### 7.10 Place Details

A saved place should display useful information and provide access to the original source.

---

# 8. What VINTHA Is NOT

VINTHA is not intended to be:

- Another social media platform.
- Another generic bookmarking app.
- Another Google Maps replacement.
- A copy of Instagram Saves.
- A restaurant-only application.
- A travel-only application.

VINTHA's core purpose is:

> **Turning online discoveries into real-world experiences.**

---

# 9. Key Differentiator

The most important differentiator is not simply saving a place.

The differentiator is **timing**.

Other services may help users save information.

VINTHA should help users remember it **when it becomes relevant in the real world.**

### Product principle

> **Don't just save it. Experience it.**

---

# 10. AI Responsibilities

AI may be used to understand shared content.

Potential AI tasks:

- Extract place names.
- Extract addresses.
- Understand captions.
- Identify categories.
- Resolve ambiguous place names.
- Extract useful context.
- Determine confidence.
- Help organize saved places.
- Generate summaries.
- Eventually provide personalized recommendations.

AI should not be treated as the final authority for geographic information.

AI results should be validated using reliable place/location data.

---

# 11. Location Intelligence

Location is one of VINTHA's most important systems.

A basic implementation could use distance from the user's current location to saved places.

However, VINTHA should eventually consider:

- Distance
- Walking vs driving context
- Previously notified places
- Previously visited places
- Time of day
- Day of week
- User preferences
- Notification frequency
- Battery impact

The goal is to avoid notification spam.

VINTHA should notify users when a saved place is genuinely relevant.

---

# 12. Notification Philosophy

Notifications should feel useful rather than annoying.

Bad:

> "There is a restaurant near you."

Good:

> "You saved this place. You're only 500m away."

The notification should remind the user of their own discovery.

Possible notification:

> **VINTHA reminder 👀**
>
> You saved this place a while ago.
> You're nearby now.

Users should have control over location reminders.

---

# 13. Privacy Principles

VINTHA may handle sensitive location-related information.

Privacy must therefore be considered from the beginning.

Principles:

- Request only necessary permissions.
- Clearly explain why location access is needed.
- Avoid unnecessary collection of precise location data.
- Minimize stored location history.
- Protect user data.
- Give users control over notifications and location access.
- Never sell personal location data.
- Design location features with battery efficiency in mind.

Privacy requirements will be reviewed before public launch.

---

# 14. Initial Technical Direction

The exact technology stack will be finalized during technical planning.

Initial direction:

### Mobile Application

Flutter

Purpose:

- Android
- iOS
- Shared codebase

### Backend

Python with FastAPI

Purpose:

- Authentication
- API services
- AI integration
- Place processing
- Application logic

### Database

PostgreSQL

Purpose:

- Users
- Saved places
- Locations
- Categories
- Visits
- Preferences

### Maps / Places

A suitable maps and places provider will be selected after comparing:

- Accuracy
- India coverage
- Pricing
- API limits
- Android support
- iOS support
- Licensing

Potential options include Google Maps Platform, Mapbox, and native platform mapping services.

---

# 15. High-Level Architecture

```text
Social Media / Web
        |
        | Share
        v
+-------------------+
|    VINTHA APP     |
+---------+---------+
          |
          v
+-------------------+
|    Backend API    |
+---------+---------+
          |
    +-----+-----+
    |     |     |
    v     v     v
   AI   Places Database
    |     |     |
    +-----+-----+
          |
          v
    Saved Location
          |
          v
 Location Intelligence
          |
          v
      Notification
          |
          v
       User Visit
