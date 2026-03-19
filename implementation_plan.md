# Video Search and Summary AI Agent (VSS AI Agent) - Discovery

This plan outlines the Hybrid Combination Discovery for the VSS AI Agent, merging Cloud VMS infrastructure concepts with NVIDIA's Agentic Video Search and Summarization (VSS) architecture.

## User Review Required

Please review the finalized **Topic / Epic / Feature** hierarchy below based on your completed structure. 

Let me know if you would like any final adjustments to this structure, or if we are ready to move forward to generate the user stories!

---

## 1. Product Context

| Field | Value |
|------|----------|
| **Product Name** | VSS AI Agent |
| **Product Goal** | Enable security and operations teams to securely manage local site cameras to the cloud, view traditional VMS feeds, and use NVIDIA AI streams to search, verify alerts, and interrogate massive volumes of video data. |
| **Target Users** | Security Operators, Incident Investigators, System Administrators |
| **Core Problem** | Investigating incidents in traditional VMS requires scrubbing hours of timeline footage. Managing disparate isolated sites is complex and alerts are too noisy. |

---

## 2. Product Hierarchy Draft

The application is structured into 6 primary Topics.

### Topic 1: Landing Page
- **User Goal**: Understand the product's value proposition before committing, and access authentication portals or core documentation.
  - **Epic 1.1**: Landing Page Overview
    - *Feature*: Product introduction, services, customer logos, and visual key functions.
    - *Feature*: Navigation portal to Login/Register.
    - *Feature*: Product Documentation (Top-bar links & search).
    - *Feature*: Privacy Policy and Terms & Conditions (Footer links).

### Topic 2: Login & Register
- **User Goal**: Securely authenticate into the platform.
  - **Epic 2.1**: User Registration
    - *Feature*: Account Creation & Email Verification (registration → email verification → mandatory MFA setup).
  - **Epic 2.2**: User Login
    - *Feature*: Secure Authentication (Password login, SSO with auto-account generation).
    - *Feature*: Multi-Factor Authentication (MFA setup on first login, verification on every login, recovery fallback).

### Topic 3: Site (Mini PC & Edge Server Management)
- **User Goal**: Provision and manage hybrid physical sites where Edge Software runs locally on a Mini PC but syncs with the Central VMS Software using a unified UI.
- **Architectural Constraints**: Same UI codebase deployed on Mini PC and Central Software. Mini PC requires internet on install/run. Config DB pushes Mini PC → Central. Activity logs are pulled periodically by Central. Feature access is decentralized by user role.
  - **Epic 3.1**: Camera Management (Kerberos Agent Architecture)
    - *Feature*: Add, edit, remove, and monitor status of local cameras attached to the site (1 Kerberos pod per camera, ONVIF network scan + manual RTSP entry).
    - *Feature*: Live Stream View with display configs (Inverse, Digital Zoom) and PTZ/Mic/Speaker hardware controls.
    - *Feature*: ONVIF-based Camera Configuration — displays per-camera ONVIF profiles (S/T/G/Imaging/PTZ) and exposes all supported parameters for single or bulk configuration. **[Must Have]**
    - *Feature*: Multi-tier Storage Configuration (Camera SD → Mini PC Memory → Central DB, cascading fallback, duration/capacity limit enforcement). Includes configuration for video file chunk lengths (15m/30m/1hr).
  - **Epic 3.2**: VMS Link (Mini PC Installation & Auth)
    - *Feature*: Mini PC Software Setup — requires internet + Central Domain URL on install. Includes Change Central VMS Domain (restricted to System_Admin, with rollback on failure).
    - *Feature*: New Device Registration Authorization — `Install_Device` role required to add a new Mini PC to the Central tenant.
    - *Feature*: Working Device Access Authorization — `Site_Admin/Operator` role required to access an existing operational Mini PC's features.
    - *Feature*: Central UI Site Navigation — role-filtered site list → Mini PC selection → loads feature context. **[Stories Pending]**
    - *Feature*: Device Restore Pipeline — install new Mini PC from backup of inactive Mini PC.
  - **Epic 3.3**: Edge-to-Cloud Sync & Resilience
    - *Feature*: Config & Activity DB Replication (config push, activity pull with configurable frequency).
    - *Feature*: Edge Disconnect Protocol (10 attempts × 3 sessions × 3s apart × 3m between attempts → speaker + popup newsletter + email alert).
  - **Epic 3.4**: Event Management
    - *Feature*: View and manage centralized events/alarms log from cameras and Mini PC hardware.
    - *Feature*: Configure automated time-gated trigger actions (video clip cut, speaker alarm, PTZ preset control).

### Topic 4: VMS (Video Management System)
- **User Goal**: Access the core streaming and historical playback functionality of traditional surveillance.
  - **Epic 4.1**: Home
    - *Feature*: Interactive Site Map (OpenStreetMap) — site icons with camera count badge, hover-to-edit, add site (+) FAB overlay.
    - *Feature*: Site Detail Management — edit Name, Address, Coordinates (auto-geocoded), Timezone, Status, Contact, Description, Assigned Mini PCs.
    - *Feature*: Information Display Panel — selectable views: All Cameras, Specific Site, Storage Capacity per tier, Event List.
  - **Epic 4.2**: Live
    - *Feature*: Multi-Camera Live View — drag & drop cameras/sites to canvas, choose grid layout (1x1 to 2+8), reorder tiles, double-click to enter single camera mode.
    - *Feature*: Layout Save & Management — save named layouts with Auditor Shareable / Private / Public visibility, load and restore saved layouts.
    - *Feature*: Single Camera Live Controls — PTZ joystick, microphone/audio, streaming format selector (HLS/DASH/WebRTC/JPEG), digital zoom, dewarping for fisheye cameras.
  - **Epic 4.3**: Playback
    - *Feature*: Recorded Video Playback & Timeline — Live/Playback toggle, scalable/scrollable/draggable timeline, motion marks with snapshot preview, calendar picker, 0.125x–16x playback speed.
    - *Feature*: AI Processing Status & Shortcuts — per-footage badge (Not Yet / Processing / Ready) based on stored chunks; Ready shows shortcut to Video Assistant (Epic 5.1); Not Yet shows quick-add-to-AI-queue action.
    - *Feature*: Evidence Management (Extraction, Archive & Recovery) — snapshot, clip/time-lapse extraction, SD card recovery, and Archive management with batch AI processing.

### Topic 5: VSS (Video Search and Summary Streams)
- **User Goal**: Utilize advanced AI capabilities (NVIDIA VSS) to intelligently analyze, search, and manage video data.
  - **Epic 5.1**: Video Assistant
    - *Feature*: Video Catalog Management — thumbnail grid classified by AI status (Analyzed/Processing/Not Yet), hover tooltip with status-specific info, detailed view with playback + transcript + version control, manage queue (pause/cancel processing, submit unanalyzed with default or saved advanced script).
    - *Feature*: Conversations with VSS Agent — chatbot UI, select context (videos/clips/cameras) then ask, or ask first and get asset suggestions; smart expansion: suggest adding analyzed assets, suggest queuing unanalyzed, suggest re-analysis with matching script, suggest creating new script when no match.
    - *Feature*: Advanced Analysis Configuration — create/manage analysis scripts defining detection behaviors (motion, object, person re-ID, vehicle tracking) and object relationship rules; saved scripts available across catalog and conversation workflows.
  - **Epic 5.2**: Alert
    - *Feature*: Notification — dashboard with Alert and Automation Task summary counters (Total/Active/Inactive) with shortcuts to settings pages; expandable Alert message list and Task execution report list with configurable auto-refresh (5s/15s/30s), time range filter (1w/1m/3m), pagination (10/20/50), keyword+time search, and mark as Read/Important/Unread.
    - *Feature*: Alert Setting — CRUD for alert configurations; define name, natural-language scenario, cameras in scope, active time frame, severity, and notification recipients; toggle Active/Inactive.
    - *Feature*: Automation Task Management — CRUD for automation tasks; link tasks to alert triggers; configure action list per trigger (video clip cut, speaker alarm, PTZ preset, notification); toggle Active/Inactive; execution reports logged to Notification.

### Topic 6: Help and Feedback
- **User Goal**: Access support and provide feedback seamlessly from any active working context.
- **Scope note**: This topic is globally accessible from Topics 2, 3, 4, and 5.
  - **Epic 6.1**: User Support
    - *Feature*: Contextual Help Widget — persistent global help button opens overlay with contextual article suggestions for the current feature; browse all help topics by category.
    - *Feature*: Knowledge Base Search — keyword search across all help articles with ranked results; "No results" links to bug/feedback submission; article helpfulness rating (thumbs up/down).
  - **Epic 6.2**: Feedback Submission
    - *Feature*: Bug Reporting — structured bug report form (title, steps to reproduce, expected vs actual, severity); auto-captures current page URL, feature context, and browser/OS metadata.
    - *Feature*: Feature Feedback — feature request form (title, description, use case); general feedback free-text; upvote existing requests for roadmap prioritization.
