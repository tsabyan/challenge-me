# Project Plan: Challenge-Me (MVP)

## Project Overview
**Challenge-Me** is a mobile-first, web-based application designed to help users track their goals and resolutions. The core mechanic revolves around breaking down big targets into manageable *checklists*. A goal is only considered "Achieved" once all checklist items are completed. 

**Note on App Language:** All user interfaces, copies, and features in the application itself will be built in **English**.

## Technical Specifications (Tech Stack)
- **Frontend**: Next.js (App Router)
- **Styling**: Tailwind CSS
- **Database & Backend/Auth**: Supabase
- **Package Manager**: pnpm 
- **UI/UX Design**: Mobile-first approach, light mode default, clean pastel colors, and rounded components to convey a modern, "cute", and friendly aesthetic.

---

## Development Roadmap (Step-by-Step Breakdown)
*These stories are designed to be easily actionable. They are not overly technical but provide clear Acceptance Criteria to ensure the features are built correctly.*

### 📍 Phase 1: Foundation Setup
**Story 1.1: Supabase Project Configuration**
- **Description:** Connect the existing Next.js boilerplate to the Supabase backend by setting up the necessary Environment Variables.
- **Acceptance Criteria:** 
  - `.env.local` is created containing `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` credentials.
  - The application runs locally (`localhost:3000`) without environment or configuration errors.

**Story 1.2: Theming & Basic Layout (Tailwind CSS)**
- **Description:** Design a mobile-view layout wrapper for desktop screens. Register the base color palette ("Cute & Clean") and typography.
- **Acceptance Criteria:**
  - Tailwind configuration (`tailwind.config.ts`) is updated with the custom color palette.
  - The default Next.js font is replaced with a friendly sans-serif Google Font (e.g., *Nunito*, *Quicksand*, or *Outfit*).
  - Basic reusable UI components are created: A rounded Card Component and a main Action Button.

---

### 📍 Phase 2: Database Schema (Supabase)
**Story 2.1: Tables Creation & Row Level Security (RLS)**
- **Description:** Construct the relational database schema in the Supabase dashboard.
- **Acceptance Criteria:**
  - **`goals` table:** Stores main user targets (*id*, *user_id*, *title*, *type*=ENUM('short_term', 'long_term'), *is_achieved*=boolean).
  - **`checklists` table:** Stores sub-tasks for a specific goal (*id*, *goal_id* (foreign key), *title*, *is_completed*=boolean).
  - **Row Level Security (RLS)** is enabled for both tables so users can only READ and UPDATE their own data.

---

### 📍 Phase 3: User Gateway (Authentication)
**Story 3.1: Login & Sign Up Page**
- **Description:** Build the authentication page (`/login`). This form should include conventional Email & Password inputs, alongside a "Continue with Google" quick access button.
- **Acceptance Criteria:**
  - Users can sign up for a new account and log into existing ones.
  - Google OAuth works smoothly using the Supabase Auth helpers.
  - Upon successful login, the user is redirected to the dashboard.

**Story 3.2: Route Protection (Middleware)**
- **Description:** Secure the application's core pages from unauthenticated access.
- **Acceptance Criteria:**
  - Dashboard and inner routes are protected. Unauthenticated (guest) visitors are immediately redirected back to `/login`.

---

### 📍 Phase 4: Goals Management
**Story 4.1: Dashboard & Empty State**
- **Description:** Build the main dashboard page that lists all active user goals, categorized into tabs or sections: "Short-Term" and "Long-Term".
- **Acceptance Criteria:**
  - Active goals are displayed as list items using the Card component.
  - A friendly *Empty State* (e.g., a cute illustration with encouraging text like "No goals yet. Let's make one!") is displayed if the user has no goals.

**Story 4.2: Create a New Goal Interaction**
- **Description:** Create a flow (a modal or a separate route) for the user to define a new target when tapping the "[+] Add Goal" button.
- **Acceptance Criteria:** 
  - Form contains `Title` input and `Type` dropdown (Short-Term/Long-Term).
  - Upon submission, data is inserted into the Supabase database.
  - The dashboard automatically refreshes to display the newly added goal.

---

### 📍 Phase 5: The Core Mechanic (Checklist System)
**Story 5.1: Goal Detail Interface**
- **Description:** When a user taps a Goal Card on the dashboard, navigate them to a specific detail page showing the underlying checklists.
- **Acceptance Criteria:**
  - Page displays the main Goal title and a visual *progress bar* indicating checklist completion percentage.

**Story 5.2: Adding & Toggling Checklists**
- **Description:** Inside the detail page, provide a single-line input field to append new sub-tasks. Existing list items should have a checkbox to toggle completion.
- **Acceptance Criteria:** 
  - Toggling a checkbox updates the `is_completed` boolean column in the server database (ideally with real-time/optimistic UI updates).

---

### 📍 Phase 6: The Achievement
**Story 6.1: Marking Goal as "Achieved"**
- **Description:** Implement completion logic. When a Goal has at least 1 checklist and all its checklists are 100% complete, prompt the user to officially archive the goal.
- **Acceptance Criteria:**
  - A "Mark as Achieved" button lights up/becomes active when checklists are done.
  - Clicking it updates the `is_achieved` flag to true on the server.
  - The goal is removed from the active dashboard and pushed to a separate "Achieved / Completed" tab to keep user motivation high.

---

### 📍 Phase 7: Polish & Deployment
**Story 7.1: UI/UX Navigation Polish**
- **Description:** Fine-tune interactions before release. Prioritize the mobile feel by adding subtle transitions to clicks and page renders.
- **Acceptance Criteria:**
  - Modals and page changes feel smooth. Padding and sizing prevent overlapping content on smaller mobile dimensions.

**Story 7.2: Deploy to Vercel**
- **Description:** Ship the Next.js app to production via Vercel.
- **Acceptance Criteria:** 
  - Production environment variables are correctly set in the Vercel Dashboard.
  - The build process passes, and the application is accessible on the public Internet.

---

## 🚀 Post-MVP Roadmap (Future Iterations)
*These features are NOT part of the MVP and will be developed in future iterations. However, keeping them in mind ensures our architecture remains scalable.*

### Phase 8: Social Challenges & Community
- **Description:** Introduce social mechanics to challenge others and stay accountable.
- **Features to build:** 
  - Add Friends / Follow system.
  - Public profile and option to set goals as "Public" or "Private".
  - Feature to explicitly "Challenge" a friend to complete a specific task using the app.

### Phase 9: Gamification & Rewards
- **Description:** Make goal completion more rewarding and game-like.
- **Features to build:** 
  - Calculate daily / weekly login and completion **Streaks**.
  - Award **Badges** (e.g., "7 Days Unbroken!", "First 10 Goals Achieved").
  - Basic point/level system to keep motivation active.

### Phase 10: Engagement & Retention
- **Description:** Keep users coming back without relying solely on intrinsic motivation.
- **Features to build:** 
  - Push notifications setup (via PWA service workers or native app bridging).
  - Configurable daily or weekly reminder emails.

### Phase 11: Expanded Platforms & Aesthetics
- **Description:** Expand accessibility and aesthetic choices.
- **Features to build:** 
  - **Dark Theme Toggle:** Allow users to switch from the light pastel default to an elegant, eye-friendly dark mode.
  - **Responsive Desktop Layout:** Adapt the mobile-first styling to make excellent use of wide displays (e.g., sidebars or grid-based dashboard).
