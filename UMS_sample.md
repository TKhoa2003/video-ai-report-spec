
# Topic: VSS - app/VSS
### Epic 1: Video Assistant - app/VSS/Video Assistant
**a) Pain Point:** Users need to perform broad, system-wide investigations across multiple live cameras and archived clips simultaneously without manually scrubbing through hours of raw footage. **Comment: ([Epic] help user solve pain points)**

**b) Objective:**
1. Quickly locate specific events using natural-language queries (e.g., "Find the red truck seen yesterday").
2. Summarize general security activities and manage past search histories.
3. Perform targeted investigations with AI analysis of extracted video segments.

**c) Exit Epic:**
1. Click on **Logo IconButton** to go **[Main of Home]**  
2. Click on **Topic IconButton in Navigation Rail** to go **[Main of [Topic Name]]**  
3. Click on **Epic Tab in TabBar** to go **[Main of [Epic Name]]**  

**d) Enter feature:**
1. **[Main of Video Assistant]** consists of the Chat Interface (**Main**) and a Secondary Left Sidebar (**Aside**) for history management.
2. Click on **VSS Setting** (Extra Button in Header) to open **VSS Overlay**, then click **[Input & context setup] tab**.

#### Feature 1: *Input & Context Setup*

**a) Why:** To allow users to precisely define the scope of the AI's analysis by selecting specific cameras, archived clips, or generated alerts to serve as the context for the chat. **Comment: (Without [Features], [Epic] cannot do "action" for the user.)**

**b) Feature purpose:**
1. Users access the **VSS Overlay** via the **VSS Setting** button on the right of the header bar to configure the AI's knowledge base.
2. Within the **VSS Overlay**, the **[Input & context setup] tab** allows users to add clips, videos, and notifications as Chatbot context.
3. Users can define tasks and instructions for the AI before saving the configuration to the current chat session.

**c) Acceptance Scenarios:**
1. **Given** the User is in the **[Main of Video Assistant]**; **When** the User clicks the **VSS Setting** button; **Then** the system opens the **VSS Overlay**.
2. **Given** the **VSS Overlay** is open; **When** the User selects the **[Input & context setup] tab**; **Then** the system displays sections for "Description", "Instruction", and "Knowledge".
3. **Given** the User is in the "Knowledge" section; **When** the User selects specific video clips or alerts; **Then** those items are added to the AI's context.
4. **Given** the context is configured; **When** the User clicks the "Save" **Common Button**; **Then** the system attaches the selection to the chat and closes the **VSS Overlay**.

#### Feature 2: *Chat History Management*

**a) Why:** Enable users to find, resume, or delete previous AI analysis sessions, ensuring ongoing investigations are easily accessible.

**b) Feature purpose:**
1. Users can find previous sessions via **ListTile** components within the **Aside** container (Secondary Left Sidebar).
2. The **Aside** container includes a **Search Bar** to quickly locate sessions by name or keyword, and a **Filter Bar** to sort by date or status.
3. Sessions can be resumed with a click or deleted via a **Contextual Menu** (triggered by right-click or long-press).

**c) Acceptance Scenarios:**
1. **Given** the User is in the **[Main of Video Assistant]**; **When** the User scrolls through the **Aside** container; **Then** the system displays previous chat sessions as **ListTile** items.
2. **Given** the User is looking for a specific session; **When** the User types a keyword into the **Search Bar**; **Then** the history list filters immediately to display matching **ListTile** items.
3. **Given** the User wants to sort history; **When** the User selects "This Week" from the **Menu** in the **Filter Bar**; **Then** the history list updates to show only recent sessions.
4. **Given** a list of sessions; **When** the User clicks a specific **ListTile**; **Then** the system resumes that chat session in the **Main** area.
5. **Given** a session in the list; **When** the User triggers the **Contextual Menu** and selects "Delete"; **Then** the system removes the session from the history.

#### Feature 3: *General Q&A*

**a) Why:** Provide a seamless natural language interface for broad system queries, allowing the AI agent to analyze multiple data points based on user text or voice.

**b) Feature purpose:**
1. Users interact with the AI via a **Chat Composer** located at the bottom of the **Main** area.
2. The **Chat Composer** includes a text input field, a voice input option (IconButton), and a "Send" **IconButton**.
3. AI responses and status updates populate the messages area within the **Main** region.

**c) Acceptance Scenarios:**
1. **Given** the User is in the **[Main of Video Assistant]**; **When** the User types a query into the **Chat Composer** and clicks "Send"; **Then** the system displays the message and begins processing.
2. **Given** a query is being processed; **When** the AI completes its analysis; **Then** the system displays the aggregated response in the message area.
3. **Given** the **Chat Composer** is visible; **When** the User uses voice input; **Then** the system transcribes the query into the text field for submission.

---

### Epic 2: Video Analytic - app/VSS/Video Analytic
**a) Pain Point:** Users struggle to locate critical events within massive amounts of raw video footage and lack a streamlined way to initiate targeted AI analysis on unreviewed clips. **Comment: ([Epic] help user solve pain points)**

**b) Objective:**
1. Provide a categorized overview of all video assets based on processing status.
2. Enable deep-dive investigation on individual analyzed videos using a specialized interface with transcript and chat capabilities.
3. Allow users to manually trigger or schedule AI analysis tasks on unanalyzed content.

**c) Exit Epic:**
1. Click on **Logo IconButton** to go **[Main of Home]**  
2. Click on **Topic IconButton in Navigation Rail** to go **[Main of [Topic Name]]**  
3. Click on **Epic Tab in TabBar** to go **[Main of [Epic Name]]**  

**d) Enter feature:**
1. **[Main of Video Analytic]** consists of a **Filter Bar** (Top) and a **Video Resources Container** (Below) to show the video grids.
2. Clicking a video in the "Analyzed" state opens a **Lightbox** for deep analysis.
3. Clicking **VSS Setting** (Extra Button in Header) to open **VSS Overlay**, then click **[Setup Analysis] tab** allows setting up tasks.

#### Feature 1: *Video/Clip Management*

**a) Why:** To give the user a clear, categorized overview of all video assets, showing which have AI metadata, which are processing, and which require action.

**b) Feature purpose:**
1. Users can utilize a **Filter Bar** to filter videos by States (Options: Analyzed, Processing, Ignore), by Camera (Options: CAM01, CAM02, CAM03, etc.), by Site (Options: Thanh Khe, Lien Chieu, Hai Chau, etc.), and by Time (Options: 1 hour, 1 day, 1 week, 1 month, 1 year, etc.).
2. Filtered videos are displayed in the **Video Resources Container** as a grid of thumbnails.
3. Real-time processing indicators or metadata are visible on each video item.

**c) Acceptance Scenarios:**
1. **Given** the User is in the **[Main of Video Analytic]**; **When** the User selects "Analyzed" from the States **Menu** in the **Filter Bar**; **Then** the **Video Resources Container** updates to show only analyzed videos.
2. **Given** the User is viewing the video grid; **When** the User selects "Hai Chau" from the Site **Menu** and "1 week" from the Time **Menu**; **Then** the system filters the video list accordingly.

#### Feature 2: *In-Depth Single Video Analysis Window*

**a) Why:** To provide a comprehensive interface for reviewing a single video, integrating playback, event timelines, full transcripts, and a focused AI assistant.

**b) Feature purpose:**
1. Users click an "Analyzed" video to open a **Lightbox** overlay.
2. The **Lightbox** contains the video player, an "AI Summary" section below it, and a smaller **Container** on the right side.
3. The right-side **Container** features a **SegmentedButton** allowing the user to switch between "Transcript" view and "Ask AI" view.

**c) Acceptance Scenarios:**
1. **Given** the User is viewing analyzed videos in the **Video Resources Container**; **When** the User clicks on a specific video; **Then** the system opens a **Lightbox** containing the video player and AI Summary.
2. **Given** the **Lightbox** is open; **When** the User interacts with the **SegmentedButton** in the right-side **Container**; **Then** the system switches the view between the video transcript and the exact contextual AI chat interface.

#### Feature 3: *Setup & Plan Analysis*

**a) Why:** To allow users to manually trigger or schedule AI analysis tasks for videos that have not yet been processed.

**b) Feature purpose:**
1. This feature is located within the **VSS Overlay**, accessed via the **VSS Setting** button.
2. Users can select videos that require analysis and define processing parameters within the **[Setup Analysis] tab**.
3. Users can schedule the tasks or start them immediately.

**c) Acceptance Scenarios:**
1. **Given** the User has identified unanalyzed videos; **When** the User clicks the **VSS Setting** button and opens the **[Setup Analysis] tab** in the **VSS Overlay**; **Then** the system displays the analysis configuration options.
2. **Given** the configuration options are open; **When** the User defines parameters and clicks a "Start Analysis" **Common Button**; **Then** the system begins processing the selected videos.

---

### Epic 3: Video Alert - app/VSS/Video Alert
**a) Pain Point:** [Video Alert] allows users to detect anomalies based on scenarios described in natural language without having to spend time constantly monitoring the screen. **Comment: ([Epic] help user solve pain points)**  

**b) Objective:**
1. The user can set up and manage alerts that use AI to analyze video segments extracted by the camera based on configured events.
2. The user can also set up and manage action scenarios and use AI-generated alerts to trigger these scenarios.

**c) Exit Epic:**  
1. Click on **Logo IconButton** to go **[Main of Home]**  
2. Click on **Topic IconButton in Navigation Rail** to go **[Main of [Topic Name]]**  
3. Click on **Epic Tab in TabBar** to go **[Main of [Epic Name]]**  

**d) Enter feature:**  
1. **[Main of Video Alert]** consists of **[Notification] Container** (Left) and **Video Alert Container** (Right). In Default mode, **Video Alert Container** shows the detail of the newest Messages/Tasks.
2. Click on **VSS Setting** (Extra Button in Header) to open **VSS Overlay**, then click **[Alert Setting] tab**.
3. Click on **VSS Setting** (Extra Button in Header) to open **VSS Overlay**, then click **[Automation] tab**.

#### Feature 1: *Notification & Rule Management*

**a) Why:** Without [Notification], [Video Alert] cannot report activity to the user. It also serves as the primary entry point for managing the AI rules and automation logic that drive these alerts. **Comment: (Without [Features], [Epic] cannot do "action" for the user.)**  

**b) Feature purpose:**   
1. Users can view a list of recent alert messages via **ListTile** components within the **[Notification] Container**. Users can switch between "Alerts" and "Automation Reports" using a **SegmentedButton**. 
2. Clicking the **VSS Setting** button in the header bar opens the **VSS Overlay** for configuring **Alert Scope**, **Generative AI Conditions**, and **Automation Rules** (Alert Setting and Automation tabs).
3. Provides real-time visibility into the execution status of automated tasks.

**c) Acceptance Scenarios:**
1. **Given** the User is in the **[Notification] Container**; **When** the User clicks the **SegmentedButton** to switch to "Alerts"; **Then** the system displays the list of alert messages using **ListTile** components.  
2. **Given** the User is viewing the alert list; **When** the User changes the refresh interval (5s / 15s / 30s) from the **Menu**; **Then** the system updates the reload interval accordingly.  
3. **Given** the User is viewing the alert list; **When** the User changes the display time range (1 week / 1 month / 3 months) from the **Menu**; **Then** the system filters and updates the list accordingly.  
4. **Given** the User is viewing the alert list; **When** the User changes the number of messages displayed per page (10 / 20 / 50) from the **Menu**; **Then** the system updates the pagination.  
5. **Given** the User is viewing the alert list; **When** the User searches by time and keyword in the **Search Bar**; **Then** the system displays the messages that match the criteria.  
6. **Given** the User is viewing the alert list; **When** the User selects one or all messages to mark them as (Viewed / Important / Unread) via **Contextual Menu** or action icons; **Then** the system updates the status.  
7. **Given** the User is in the **[Notification] Container**; **When** the User clicks the **SegmentedButton** to switch to "Execution Reports"; **Then** the system displays the list of task execution reports using **ListTile** components.  
8. **Given** the User is viewing the task execution list; **When** the User changes the refresh interval (5s / 15s / 30s) from the **Menu**; **Then** the system updates the reload interval.  
9. **Given** the User is viewing the task execution list; **When** the User changes the display time range (1 week / 1 month / 3 months) from the **Menu**; **Then** the system updates the data shown.  
10. **Given** the User is viewing the task execution list; **When** the User changes the number of tasks displayed per page (10 / 20 / 50) from the **Menu**; **Then** the system updates the pagination accordingly.  
11. **Given** the User is viewing the task execution list; **When** the User searches by time and keyword in the **Search Bar**; **Then** the system displays the matching task list.  
12. **Given** the User is viewing the task execution list; **When** the User selects one or all messages to mark them as (Viewed / Important / Unread); **Then** the system updates the status of the selected task execution messages.  

#### Feature 2: *Historical Alert Investigation*

**a) Why:** To enable users to rapidly locate specific past events across large datasets using specialized AI metadata and physical parameters.

**b) Feature purpose:**
1. Users can use a multi-criteria **Filter Bar** (containing **Menu** components for Site, Camera, Alert Rule, Time Range) to narrow down the investigation scope.
2. Users can perform granular searches via the **Search Bar** using AI-generated "meta-tags" (e.g., object color, person attributes).
3. Identified alerts populate the **Video Alert Container** for in-depth review of snapshots and AI reasoning.

**c) Acceptance Scenarios:**
1. **Given** the User is in the **[Main of Video Alert]**; **When** the User selects a specific camera from the "Camera" **Menu** in the **Filter Bar**; **Then** the alert list updates to show only events from that source.
2. **Given** the User is performing a search; **When** the User enters a meta-tag keyword (e.g., "blue car") into the **Search Bar**; **Then** the system filters the results to match the AI-extracted metadata.
3. **Given** the User is investigating historical data; **When** the User selects a date range (e.g., "Last 30 Days") from the **Menu** in the **Filter Bar**; **Then** the system retrieves and displays alerts from that specific period.
4. **Given** the User is using both components; **When** the User filters by "Site A" using the **Filter Bar** and searches "backpack" using the **Search Bar**; **Then** the system combines the logic to show accurate results.
5. **Given** the filtered results are displayed; **When** the User clicks an alert **ListTile**; **Then** the **Video Alert Container** updates to show the full AI analysis and snapshot for that specific event.

#### Feature 3: *Alert Details & AI Review*

**a) Why:** To provide users with the exact AI reasoning, visual evidence, and decision-making tools required to resolve a triggered alert.

**b) Feature purpose:**
1. Users can access deep details of a specific alert via a **Side Sheet** that slides in from the right edge of the interface.
2. The **Side Sheet** presents the AI's "Agent Request" (original logic) and "Agent Description" (narrative of what happened) alongside high-resolution snapshots.
3. Decision-making actions like "Done" (Resolve) or "Delete" are prominent via **Common Buttons** within the **Side Sheet**.

**c) Acceptance Scenarios:**
1. **Given** the User is in the **[Main of Video Alert]** and viewing an alert list; **When** the User clicks the **Badge** (Review) on a specific **ListTile** row; **Then** the system opens a **Side Sheet** from the right.
2. **Given** the **Side Sheet** is open; **When** the User reads the **Agent Request** and **Agent Description** sections; **Then** the system correctly displays the AI's logical reasoning and textual summary of the event.
3. **Given** the User is reviewing an alert; **When** the User clicks the "Done" **Common Button**; **Then** the system marks the alert as resolved and dismisses the **Side Sheet**.
4. **Given** the User is reviewing an alert; **When** the User clicks the "Delete" **Common Button**; **Then** the system removes the alert record and closes the **Side Sheet**.
