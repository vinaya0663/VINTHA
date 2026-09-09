# VINTHA — UX Specification

**Product:** VINTHA  
**Tagline:** See it. Save it. Don’t forget it.  
**Version:** MVP v1  
**Status:** Design Specification

---

## 1. Purpose

This document defines the user experience and screen flow for the VINTHA MVP.

VINTHA helps users turn places they discover online into real-world experiences.

The core experience is:

**Discover → Save → Remember → Experience**

---

## 2. Core MVP User Flow


User sees a place online
        ↓
Shares it to VINTHA
        ↓
VINTHA receives the URL / image / input
        ↓
AI extracts useful place clues
        ↓
Place providers are searched
        ↓
VINTHA matches and ranks possible places
        ↓
User confirms the result
        ↓
Place is saved
        ↓
Place appears on the user's map
        ↓
Time passes
        ↓
User comes near the saved place
        ↓
VINTHA can send a nearby reminder
        ↓
User opens the place
        ↓
View place / original source / directions
        ↓
User marks the place as visited
3. Screen Specifications
Screen 1 — Splash
Purpose

Introduce the VINTHA brand while the application initializes.

Content
VINTHA logo
VINTHA name
Tagline:
"See it. Save it. Don't forget it."
Minimal loading/transition animation
Design Direction
Dark premium background
Green/teal accent
Minimal interface
Clean modern typography
Screen 2 — Onboarding

The onboarding consists of three short screens.

Slide 1 — Save What You Discover

Headline:
"Save what you discover"

Description:
"See a place you love on Instagram? Send it to VINTHA."

Slide 2 — Never Forget a Place

Headline:
"Never forget a place"

Description:
"VINTHA remembers your discoveries so you don't have to."

Slide 3 — The Right Reminder

Headline:
"We'll remind you at the right time"

Description:
"Get reminded when you're near somewhere you wanted to visit."

Navigation
Skip
Next
Get Started on final slide
Progress indicators
Screen 3 — Login / Sign Up
Purpose

Allow users to create or access their VINTHA account.

Authentication Options
Continue with Google
Continue with Apple
Continue with Email
Design Principle

Authentication should be low-friction.

VINTHA should not request background location permission during initial authentication.

Location permission should be requested when the user enables or uses nearby functionality, with a clear explanation of why location is needed.

Screen 4 — Home
Purpose

Provide the user's personal discovery dashboard.

Content
Header
Greeting
User name
Notification access
Search
Near You

Display saved places that are currently nearby.

Your Saved Places

Display saved discoveries with:

Thumbnail
Place name
Category
Location
Saved/visited status
Navigation

Bottom navigation:

Home | Map | Add | Nearby | Profile

Product Principle

VINTHA is not a social-media feed.

The Home screen focuses on the user's own discoveries.

Screen 5 — Add / Share
Purpose

Allow users to give VINTHA something they discovered.

Supported Inputs
Share from Instagram

Users can share an Instagram URL directly to VINTHA through the operating system share flow.

Paste a Link

Users can paste links from:

Instagram
TikTok
YouTube
Websites
Other supported URLs
Add a Screenshot

VINTHA can analyze an uploaded screenshot for useful place information.

Enter Manually

Users can manually provide a place name or location.

Processing

The backend receives the input and begins place identification.

Screen 6 — AI Processing
Purpose

Show that VINTHA is processing the submitted discovery.

Example States

Analyzing your discovery...

Possible progress stages:

Reading available information
Extracting place clues
Searching places
Verifying the best match
Important Principle

AI processing should not imply that the AI is always correct.

The result must be verified before saving.

Screen 7 — Confirm Place
Purpose

Allow the user to verify the place identified by VINTHA.

Example

"We found this place 👀"

Display:

Place name
Address
Category
Place image
Map preview
Source platform
Original source information
Actions

Yes, save it

That's not right

Principle

The AI suggests.

Place providers provide real-world information.

The user makes the final confirmation.

VINTHA must not automatically save an uncertain AI result.

Screen 8 — Saved Place
Purpose

Provide the complete experience for a place the user has saved.

Content
Hero image
Place name
Category
Rating where available
Price information where available
Address
Map preview
Personal notes
Source information
Saved date
Nearby reminder status
Visit status
Primary Actions

Get Directions

Open Original

Mark as Visited

Additional Actions
Edit notes
Manage reminder
Remove saved place
Screen 9 — Map
Purpose

Display the user's saved discoveries geographically.

Content
User location
Saved place pins
Category indicators
Search
Filters
Map/List toggle
Selected-place preview card
Filters

Initial categories may include:

All
Food
Travel
Shopping
Experiences
Scalability

Future versions should support:

Marker clustering
Geographic grouping
Large numbers of saved places
Additional categories

The map should feel like a personal discovery map rather than a generic maps application.

Screen 10 — Nearby
Purpose

Show saved places that are close to the user.

Content
Current location
Nearby saved places
Distance
Category
Nearby reminder setting
Directions
View Place
Reminder Control

Users can enable or disable nearby reminders.

VINTHA should clearly explain that nearby functionality requires location access.

Product Principle

Nearby should feel useful and intentional rather than invasive.

VINTHA should avoid unnecessary continuous location tracking.

Screen 11 — Notification → Place
Purpose

Deliver the core VINTHA reminder experience.

Example Notification

VINTHA

You saved this place! 👀

The Rameshwaram Cafe

You're only 600 m away.

You discovered this on Instagram earlier.

Actions

View Place

Directions

Product Principle

Notifications should feel like VINTHA is remembering something for the user.

They should not feel like advertising or spam.

Notification frequency and triggering logic must be carefully controlled.

Screen 12 — Profile / Settings
Purpose

Manage the user's account, saved discoveries, privacy and application preferences.

Profile Information
Name
Profile photo
Account information
Useful Statistics
Places saved
Places visited
Places still waiting
Settings
Account
Saved Places
Visited Places
Categories
Notifications
Nearby Reminders
Location & Privacy
App Settings
Help & Feedback
Log Out
4. AI Identification Fallback

VINTHA must handle cases where a place cannot be confidently identified.

Low Confidence

Display:

"We found a few possible places."

Show the best candidates and allow the user to choose.

No Reliable Match

Display:

"Hmm... we couldn't identify this place."

Actions:

Try Again
Add Screenshot
Enter Manually
Cancel

The user must never be trapped in the processing flow.

5. Navigation Structure

The main navigation contains:
Home
Map
Add
Nearby
Profile
6. Design System Direction
Visual Style

VINTHA uses a premium, modern dark interface.

Primary Characteristics
Dark green/black backgrounds
Green/teal accent color
Rounded cards
Clean typography
Clear hierarchy
Minimal visual clutter
Subtle animations
Strong map/location visual language
Brand Personality

VINTHA should feel:

Smart
Helpful
Modern
Personal
Calm
Trustworthy

It should not feel:

Noisy
Social-media-like
Overly technical
Spammy
7. Privacy UX

Location is sensitive information.

VINTHA should:

Clearly explain why location access is needed.
Avoid requesting location permission unnecessarily.
Allow users to disable nearby reminders.
Give users control over location-related settings.
Minimize unnecessary collection of precise location data.
Avoid implying that VINTHA continuously tracks users when that is not required.
8. Source Handling

VINTHA should not depend on scraping a single social-media platform.

The preferred architecture is:

Social Platform / Website
        ↓
Share URL or available content
        ↓
VINTHA Backend
        ↓
AI Extraction
        ↓
Place Search
        ↓
Verification
        ↓
User Confirmation
        ↓
Saved Place

The system should respect the technical capabilities, terms and access policies of each supported platform.

9. Core UX Principle

VINTHA's central promise is:

See it. Save it. Don't forget it.

The product should make the journey from digital discovery to physical experience as simple as possible.

Discover → Save → Remember → Experience
