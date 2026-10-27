# AI-Based Attendance Monitoring System

An AI-assisted classroom attendance system intended to reduce manual
roll-call time, discourage proxy attendance, and simplify attendance
record management.

> **Project status:** Design and development in progress. This README
> describes the proposed system; update it as features are implemented.

## Overview

The system uses a configured classroom camera and computer-vision
processing to identify enrolled students and help record attendance.
Recognition results include confidence and liveness checks. Uncertain
matches are routed to authorized faculty for review rather than being
automatically accepted.

## Problem Statement

Manual attendance consumes class time, can enable proxy attendance, and
makes record keeping and reporting cumbersome. This project proposes an
automated workflow for attendance capture, review, storage, and
reporting with human verification for uncertain matches.

## Proposed Features

-   Student profiles and controlled facial enrolment.
-   Timetable-based class-session start and stop.
-   Camera-frame capture and face detection.
-   Face matching, liveness checks, and confidence scoring.
-   Faculty review and correction of uncertain matches.
-   Timestamped attendance records and duplicate prevention.
-   Student attendance history, class summaries, percentages, and
    reports.
-   Role-based access control, audit logging, and operational
    monitoring.

## Workflow

1.  Faculty opens a scheduled class session.
2.  The system validates the timetable and activates the configured
    camera.
3.  Frames are passed to the face-recognition pipeline.
4.  The pipeline returns a candidate identity, confidence score, and
    liveness result.
5.  The attendance service applies policy rules and checks for duplicate
    records.
6.  Valid high-confidence results can proceed to recording; uncertain
    results go to faculty review.
7.  Confirmed records are stored and the attendance summary is updated.
8.  Authorized users view attendance history or generate reports.

## Architecture and Modules

  -----------------------------------------------------------------------
  Module                              Responsibility
  ----------------------------------- -----------------------------------
  User Interface                      Student portal, faculty dashboard,
                                      admin screens

  Authentication and Access Control   Authentication, role checks,
                                      permissions, audit events

  Student and Enrolment Management    Student profiles and protected
                                      facial-template references

  Timetable and Session Management    Session validation, start/stop
                                      lifecycle

  Camera Capture Adapter              Frame capture, device checks,
                                      capture errors

  Face Recognition and Liveness       Face detection, matching, liveness,
                                      confidence scoring

  Attendance Processing               Attendance rules, duplicate
                                      prevention, review and correction

  Repository / Data Access            Database reads/writes and
                                      transactions

  Reporting and Notifications         Summaries, percentages, exports,
                                      optional alerts

  Administration and Configuration    Accounts, timetable, thresholds,
                                      system settings

  Logging and Monitoring              Logs, health checks, metrics,
                                      backup and recovery
  -----------------------------------------------------------------------

### Design principles

-   **High cohesion:** Each module has one clear responsibility.
-   **Low coupling:** Modules communicate through documented interfaces
    and typed data objects.
-   **Replaceable adapters:** Camera, recognition model, and database
    implementations should be replaceable without rewriting attendance
    rules.
-   **Human oversight:** Uncertain results should not be silently
    confirmed.
-   **Testability:** Test components independently, then test integrated
    workflows.

## Proposed Technology Stack

These are proposed options, not claims about what is already
implemented. Choose one backend framework and one database for the first
version.

  -----------------------------------------------------------------------
  Layer                               Options
  ----------------------------------- -----------------------------------
  Frontend                            React

  Backend API                         Python with FastAPI or Django REST
                                      Framework

  Computer Vision                     OpenCV and a suitable
                                      face-detection / recognition model

  Database                            PostgreSQL or MySQL

  Testing                             Pytest and API/integration tests

  Development                         Git, Docker, and a suitable
                                      deployment environment
  -----------------------------------------------------------------------

## Privacy and Security

Facial data is sensitive biometric information. A real deployment
should:

-   Obtain appropriate institutional approval and provide clear notice
    about biometric processing.
-   Define access policies, retention periods, and deletion procedures
    before collecting real student data.
-   Protect templates and attendance records using suitable access
    controls and encryption.
-   Keep secrets out of source control and use environment variables or
    a secrets manager.
-   Avoid logging raw images, facial templates, passwords, or access
    tokens.
-   Restrict students to their own records and grant staff only the
    access they need.
-   Keep an audit trail of manual attendance changes.
-   Provide a manual review/correction path for recognition errors.

Storing facial embeddings instead of raw photos does **not** make the
data anonymous or non-sensitive.

## Getting Started

The project is being developed incrementally. Adjust these suggested
commands to the files and dependencies actually present in this
repository.

### Prerequisites

-   Python version supported by the selected libraries
-   Node.js and npm, if the React frontend is present
-   PostgreSQL or MySQL, if using a database-backed implementation
-   Git

### Clone the repository

``` bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_FOLDER>
```

### Backend setup (if `requirements.txt` exists)

``` bash
python -m venv .venv
```

Activate the environment:

**Windows PowerShell**

``` powershell
.venv\\Scripts\\Activate.ps1
```

**macOS / Linux**

``` bash
source .venv/bin/activate
```

Then install dependencies:

``` bash
pip install -r requirements.txt
```

### Frontend setup (if included)

``` bash
cd frontend
npm install
npm run dev
```

These commands require the corresponding project files and configuration
to exist.

## Suggested Project Structure

``` text
ai-based-attendance-monitoring/
├── frontend/
├── backend/
│   ├── api/
│   ├── domain/
│   ├── services/
│   ├── adapters/
│   ├── repositories/
│   ├── security/
│   └── tests/
├── docs/
├── .env.example
├── .gitignore
└── README.md
```

This is a suggested structure, not a claim about the current repository
layout.

## Testing Plan

-   **Unit tests:** Session transitions, confidence rules, duplicate
    prevention, attendance calculations.
-   **Recognition tests:** No-face results, low-confidence matches, and
    liveness failures.
-   **Repository tests:** Create/read/update operations, transactions,
    uniqueness constraints.
-   **API and permission tests:** Unauthenticated requests, role
    restrictions, cross-student data access.
-   **Integration tests:** Session start → capture → recognition →
    attendance decision → stored record.
-   **Failure tests:** Camera disconnects, recognition timeouts,
    database outages, interrupted sessions.
-   **Performance tests:** Measure latency and resource use under
    representative classroom conditions.

Do not publish accuracy or latency figures until they have been measured
and the test conditions documented.

## Roadmap

-   [ ] Finalize module interfaces and database schema.
-   [ ] Set up backend structure and configuration management.
-   [ ] Implement authentication and role-based authorization.
-   [ ] Implement student enrolment and protected template storage.
-   [ ] Implement session and camera-capture interfaces.
-   [ ] Integrate recognition, liveness, and confidence scoring.
-   [ ] Implement attendance processing and faculty review queue.
-   [ ] Add student history, reports, and optional notifications.
-   [ ] Add automated tests, logging, backup, and deployment
    documentation.
-   [ ] Evaluate privacy, security, fairness, and performance before a
    pilot.

## Limitations

Recognition quality depends on enrolment quality, lighting, camera
placement, occlusion, and model limitations. Liveness checks reduce some
spoofing risks but cannot guarantee detection of every attempt.
Appropriate approval, secure biometric-data handling, and institutional
policies are required before deployment.

## Contributing

1.  Keep modules focused on one responsibility.
2.  Prefer interfaces over direct dependencies on concrete camera,
    model, or database implementations.
3.  Add or update tests when behavior changes.
4.  Document setup and configuration changes.
5.  Never commit student biometric data, real attendance records,
    credentials, or other sensitive information.

## License

No license has been specified yet. Add a `LICENSE` file before allowing
others to reuse, modify, or distribute this project.
