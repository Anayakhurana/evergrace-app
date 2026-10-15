Step 2: Architecture

Student name: Anaya Khurana
Student ID: 300215854
Course: COMP 370
Project: Mini-Project 1 — Track A: Development

1. Four Architecture Layers

Domain (src/lib/domain.ts): Defines application business rules and calculations. For example, canView() checks whether a viewer's subscription plan allows access to a video.

Queries (src/lib/queries.ts): Retrieves data from the database and prepares it for use by the application. For example, getVideoDetail() retrieves a video's details, progress, transcript, chapters, and saved status.

Actions (src/actions/): Handles operations that change application data. Actions are responsible for tasks such as validating input, checking authorization, writing to the database, and revalidating relevant pages.

Components (src/components/): Presents information to users through the interface, including video players and locked-video messages.

Domain → Queries → Actions → Components


2. Tracing a Feature: Video Access Control

The video access feature demonstrates how the application separates business rules from database access and presentation.

Domain layer: In src/lib/domain.ts, the canView() function compares the viewer's plan rank with the video's access-level rank. Basic viewers can access free videos, members can access free and members-only videos, and premium viewers can access all access levels. Administrators can view videos regardless of access level. Signed-out visitors are treated as Basic viewers.

Query layer: In src/lib/queries.ts, getVideoDetail() retrieves a video and related information using Prisma. It calls toCard() to obtain the video's card data, including its locked status. If the video is locked, the function returns null for sourceUrl and muxPlaybackId, preventing the client from receiving those playback references.

Presentation layer: The video detail page at src/app/library/[videoId]/page.tsx presents the appropriate interface for the video's access state. The architecture documentation describes a locked presentation for inaccessible videos and a player presentation for videos the viewer can access.

Actions layer: Actions layer: The application defines server actions under src/actions/ for operations that modify application data. However, the access decision described here is made by canView() in the domain layer and enforced by getVideoDetail() in the query layer. I would need to inspect the relevant action functions to establish whether a specific action participates in this video-access workflow.
Together, these layers keep the access rule separate from database queries and user-interface rendering. The query layer also protects playback references on the server rather than relying only on hiding a play button.



### 3. What Next.js and Prisma Do Automatically

Next.js provides routing, server rendering, Server Actions, Route Handlers, and revalidation facilities. These features reduce the amount of web-application infrastructure developers need to implement themselves. Prisma provides a database client and a structured way to query and modify records using the application's data model.

However, these tools do not automatically define the application's business rules or guarantee that every operation is authorized correctly. Developers must implement and maintain the application's validation, authorization checks, and access-control logic. For example, the canView() function in src/lib/domain.ts defines which subscription plans can access particular videos, while getVideoDetail() in src/lib/queries.ts prevents playback references from being returned for locked videos.


### 4. What Would Need to Be Built Without a Framework?

Without a framework such as Next.js, developers would need to implement more of the application's web infrastructure themselves, including URL routing, request handling, and coordination between server-side logic and the user interface. They would also need to establish reliable patterns for database access, input validation, error handling, and authorization. Without Prisma, developers could use a database driver and write SQL queries manually, while managing query parameters and mapping database results to application data structures. Frameworks and libraries reduce this infrastructure work, but developers remain responsible for implementing the application's business logic, security checks, and correct behavior.
