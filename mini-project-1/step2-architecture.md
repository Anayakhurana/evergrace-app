### Step 2: Architecture

Student name: Anaya Khurana
Student ID: 300215854
Course: COMP 370
Project: Mini-Project 1 — Track A: Development

### 1. Four Architecture Layers

Domain (src/lib/domain.ts): Defines application business rules and calculations. For example, canView() checks whether a viewer's subscription plan allows access to a video.

Queries (src/lib/queries.ts): Retrieves data from the database and prepares it for use by the application. For example, getVideoDetail() retrieves a video's details, progress, transcript, chapters, and saved status.

Actions (src/actions/): Handles operations that change application data. Actions are responsible for tasks such as validating input, checking authorization, writing to the database, and revalidating relevant pages.

Components (src/components/): Presents information to users through the interface, including video players and locked-video messages.

| **1. DOMAIN** |
|---|
| `src/lib/domain.ts` |
| Defines business rules and decisions. |
| ↓ |
| **2. QUERIES** |
| `src/lib/queries.ts` |
| Reads database data and prepares results. |
| ↓ |
| **3. ACTIONS** |
| `src/actions/` |
| Validates, authorizes, and changes data. |
| ↓ |
| **4. COMPONENTS** |
| `src/components/` |
| Displays the user interface. |


### 2. Tracing a Feature: Follow/Unfollow
The Follow/Unfollow feature allows members to follow categories, instructors (masters), and levels. The feature uses all four architecture layers to enforce the paid-plan rule, read and update follow data, and display the result on the dashboard.

**Domain layer — src/lib/domain.ts:** The canFollow() function checks whether a user's plan allows following. It returns false for BASIC and true for MEMBER and PREMIUM plans. This centralizes the business rule.

**Queries layer — src/lib/queries.ts:** The dashboard query retrieves the user's existing follow records using prisma.follow.findMany(). It compares those records with the available categories, masters, and levels, then prepares the follows data used by the dashboard to show which items the user follows.

**Actions layer — src/actions/member.ts:** The toggleFollow() function checks the signed-in viewer, validates the input with followSchema, and calls canFollow() to enforce the paid-plan rule on the server. If the follow record already exists, it deletes it; otherwise, it creates a new record using Prisma.

**Components layer — src/components/follow-chips.tsx:** The FollowChips component displays the categories, masters, and levels the user can follow. It uses canFollow() to disable following for BASIC users and show an upgrade prompt. When an eligible user selects a chip, the component calls toggleFollow().

The four layers work together to keep the business rule separate from data retrieval, data changes, and presentation. The server-side action checks the paid-plan restriction even if someone attempts to bypass the disabled user-interface control.


### 3. What Next.js and Prisma Do Automatically

Next.js provides routing, server rendering, Server Actions, Route Handlers, and revalidation facilities. These features reduce the amount of web-application infrastructure developers need to implement themselves. Prisma provides a database client and a structured way to query and modify records using the application's data model.

However, these tools do not automatically define the application's business rules or guarantee that every operation is authorized correctly. Developers must implement and maintain the application's validation, authorization checks, and access-control logic. For example, the canView() function in src/lib/domain.ts defines which subscription plans can access particular videos, while getVideoDetail() in src/lib/queries.ts prevents playback references from being returned for locked videos.


### 4. What Would Need to Be Built Without a Framework?

Without a framework such as Next.js, developers would need to implement more of the application's web infrastructure themselves, including URL routing, request handling, and coordination between server-side logic and the user interface. They would also need to establish reliable patterns for database access, input validation, error handling, and authorization. Without Prisma, developers could use a database driver and write SQL queries manually, while managing query parameters and mapping database results to application data structures. Frameworks and libraries reduce this infrastructure work, but developers remain responsible for implementing the application's business logic, security checks, and correct behavior.
