# Photography Planner App — Proposed Epics / Milestones

## Project Overview
This project is a branded photography booking application for real estate agents, supported by internal admin operations and optional third-party integrations.

The proposed epic structure is intended to serve both as:

1. A Jira software project structure  
2. A phased delivery and quoting framework

---

# Epic 1 — Discovery, Requirements & Solution Design

## Goal
Translate business requirements into a build-ready specification.

## Deliverables
- Requirements workshops / clarification
- Feature scope definition
- User roles and permissions matrix
- User journey maps
- Wireframes for key screens
- Information architecture
- Technical architecture diagram
- Software Requirements Specification (SRS)
- Initial Jira backlog (stories + acceptance criteria)
- Delivery roadmap and estimate

## Milestone Output
- Signed-off scope
- Build-ready backlog
- Approved technical architecture

---

# Epic 2 — Project Foundation & Infrastructure Setup

## Goal
Establish the technical foundations for the application.

## Deliverables
- Flutter project setup
- AWS Amplify environment setup
- Cognito authentication configuration
- GraphQL/API schema setup
- Core data model setup
- Dev / test / production environment configs
- CI/CD pipeline
- GitHub repository structure
- Logging and error monitoring setup

## Milestone Output
- Working development foundation
- Deployable environments ready

---

# Epic 3 — Authentication & Agent Account Management

## Goal
Enable agents to securely access the platform.

## Deliverables
- Agent login
- Password reset
- Session handling
- Agent profile management
- Agent-business association
- Access control rules

## Milestone Output
- Agents can authenticate and access the system

---

# Epic 4 — Booking Request Creation

## Goal
Deliver the core booking workflow.

## Deliverables
- Create booking request flow
- Property details entry
- Date/time request entry
- Service/package selection
- Notes/instructions field
- Booking submission confirmation
- Booking validation rules

## Milestone Output
- Agents can submit bookings end-to-end

---

# Epic 5 — Booking Tracking & Status Management

## Goal
Allow agents to view and track bookings.

## Deliverables
- Booking list screen
- Booking detail screen
- Booking statuses:
  - Requested
  - In Review
  - Confirmed
  - Completed
- Booking history
- Booking updates

## Milestone Output
- Agents can monitor active bookings

---

# Epic 6 — Admin Booking Management Portal

## Goal
Enable internal staff to manage requests.

## Deliverables
- Admin booking dashboard
- Incoming booking queue
- Booking review interface
- Update status controls
- Assignment / management tools
- Admin notes
- Search and filtering

## Milestone Output
- Internal booking operations functional

---

# Epic 7 — Messaging & Notifications

## Goal
Support communication around bookings.

## Deliverables
- Agent enquiry/contact action
- Booking-related messaging
- Email notification triggers
- Booking confirmation notifications
- Status change notifications

## Milestone Output
- Communication workflow operational

---

# Epic 8 — Services, Packages & Pricing Rules

## Goal
Manage bookable services and pricing.

## Deliverables
- Service catalogue
- Package configuration
- Special pricing rules for selected agents
- Pricing display logic

## Milestone Output
- Service offerings configurable

---

# Epic 9 — Floor Plan Upload & Barfoot Integration

## Goal
Support floor plan publishing to Barfoot & Thompson listings.

## Deliverables
- Upload floor plan file
- Enter listing ID
- Push to B&T listing
- Replace/re-upload workflow
- API integration setup
- Integration testing

## Milestone Output
- Floor plans can sync into B&T listings

---

# Epic 10 — Ubookr / Third-Party Integration (Optional Phase)

## Goal
Enable external booking data integrations.

## Deliverables
- Ubookr integration discovery
- Sync booking data
- Booking update sync logic
- Error handling

## Milestone Output
- External booking data integration enabled

---

# Epic 11 — QA, UAT & Release Readiness

## Goal
Prepare the product for launch.

## Deliverables
- Functional testing
- Device testing
- Bug remediation
- User acceptance testing
- Release checklist
- App Store submission preparation

## Milestone Output
- Production-ready release candidate

---

# Epic 12 — Launch & Deployment

## Goal
Launch the application into production.

## Deliverables
- Production deployment
- App Store submission
- Go-live support
- Production monitoring
- Handover documentation

## Milestone Output
- App launched

---

# Proposed Delivery Phases

## Phase 1 — MVP
Includes:
- Epic 1 — Discovery, Requirements & Solution Design
- Epic 2 — Project Foundation & Infrastructure Setup
- Epic 3 — Authentication & Agent Account Management
- Epic 4 — Booking Request Creation
- Epic 5 — Booking Tracking & Status Management
- Epic 6 — Admin Booking Management Portal
- Epic 7 — Messaging & Notifications

## Outcome
Core branded booking application operational.

---

## Phase 2 — Operational Enhancements
Includes:
- Epic 8 — Services, Packages & Pricing Rules
- Epic 9 — Floor Plan Upload & Barfoot Integration

## Outcome
Enhanced operational functionality and external listing integration.

---

## Phase 3 — Advanced Integrations
Includes:
- Epic 10 — Ubookr / Third-Party Integration

## Outcome
External booking systems connected.

---

## Phase 4 — Release
Includes:
- Epic 11 — QA, UAT & Release Readiness
- Epic 12 — Launch & Deployment

## Outcome
Production release complete.

---

# Jira Hierarchy Example

Initiative:
- Photography Planner App

Epic:
- Booking Request Creation

Story:
- As an agent, I want to submit a booking request so that I can request photography services.

Task:
- Build booking form UI

Sub-task:
- Implement service package dropdown

---

# Future Enhancements (Out of Scope for Initial MVP)

The following are intentionally excluded from initial scope unless quoted separately:

- Live availability scheduling
- In-app payments
- Photographer portal
- File delivery inside app
- Android release (if iOS-first)
- Advanced push notification workflows
- Automated scheduling optimisation
- CRM integrations beyond defined scope

---