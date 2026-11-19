**Scottish Sounds**  
**Software Architecture Research Report 1.1**  
Sergio Eneide & Bruce Gray  
07/10/2026

Table of Contents

1. Overview & Meta Data
2. Context & Problem Statement
3. Requirements & Quality Attributes
4. Proposed Architecture & Pattern Analysis
5. Alternative Considerations & Trade-off Analysis
6. Risk Assessment & Mitigation
7. Implementation & Validation Plan
8. References & Context Links

1\. **Overview & Meta Data**

- Title: Scottish Sounds Software Architecture Research Report
- Authors: Sergio Eneide & Bruce Gray
- Date: 07/10/2026
- Status: Draft for review
- Objective: Consolidate our findings during our software architecture sessions to enable us to take a considered approach when recommending our architecture proposal

Scottish Sounds is a web-based tool with the goal of helping Speech and Language Therapy (SLT) students to practise the phonetic transcription of Scottish English. Students can listen to or watch recordings of words or phrases spoken with Scottish accents, transcribe them using a built-in international phonetic alphabet (IPA) keyboard, and check their answers. This document summarises the team’s architecture research with considerations to user context, requirements, proposed architecture, and consideration of alternatives or trade-offs. This is a living document and is subject to change as the project progresses.

2\. **Context & Problem Statement**

**Background**  
SLT students in Scotland need an accessible way to practise transcribing Scottish accents with the IPA and have their answers checked from a web browser. Our solution must deliver this on a £0 budget within the timeline of the academic year whilst being compliant with accessibility standards (WCAG 2.1) and industry standard security and compliance standards.

**Drivers & Triggers**

- **Misidentified accents:** SLT training often uses Southern British English as the norm. As a result, normal Scottish accent features can be mistaken for speech errors.
- **Slow feedback:** transcription practice is currently marked by hand. Students get one attempt and can wait days for feedback.
- **Limited practice:** there’s no easy way for students to practise transcribing Scottish speech on their own outside of lectures.
- **This report:** the user research phase (personas, user stories, task analysis) is complete. An architecture needs agreeing before the system requirements specification and tech stack decisions can begin.

**Scope**

Architecture only; no tech stack is proposed at this stage.

What the user does and requires from the system:

- Interacts with the UI and transcription exercises
- Selects a difficulty level
- Plays the audio or video recording
- Inputs an answer using the IPA keyboard
- Gets feedback on their answer (correct/incorrect) and can view the correct answer(s)
- Navigate to the next question in an exercise

Whilst not necessarily requirements for our implementation at this stage, users may also wish to check their learning statistics within the context of a difficulty/accent/year/lifetime, register an account and sign in, set their accessibility preferences and request account deletion.

**Constraints**  
The current budget for the project is £0, with potential funding available via the UHI Rebel Fund of up to £2,500. We have a fixed deadline of May 2027, or the end of the academic year. Audio or video recordings will currently rely on volunteers, and our access to staff is limited by available work hours.

3\. **Requirements & Quality Attributes**

| Requirement                                           | Type           | Quality Attribute                      | Architectural Implication                                                                                                 |
| ----------------------------------------------------- | -------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Visit & use the app from a web browser                | Functional     | Maintainability, portability           | Browser-based frontend hosted online, no installation                                                                     |
| Select a difficulty level & start the exercise        | Functional     | Usability                              | Exercises stored with an associated difficulty level & the API returns exercises of the selected level                    |
| Toggle audio-only or audio-and-video playback         | Functional     | Usability                              | Frontend media player with video toggle                                                                                   |
| Play exercise recording                               | Functional     | Performance                            | Recordings held in object storage, with the noSQL database storing a reference to each file                               |
| Input answers using the IPA keyboard                  | Functional     | Usability                              | IPA keyboard embedded as a frontend component                                                                             |
| Check answer input against solution(s)                | Functional     | Security & compliance, performance     | Answers checked in the backend API so solutions aren’t exposed to the browser                                             |
| Reveal the correct solution(s)                        | Functional     | Usability                              | API returns the correct solution when requested by the user                                                               |
| Move to the next exercise                             | Functional     | Performance                            | The API serves the next exercise of the selected difficulty                                                               |
| Create an account & log in                            | Functional     | Security & compliance                  | Authentication component, accounts stored in database                                                                     |
| View progress on a dashboard                          | Functional     | Usability                              | The database stores each user’s attempts so the API can calculate progress/statistics                                     |
| Resume progress from a previous session               | Functional     | Usability, availability, reliability   | The database saves each user’s progress so it isn’t lost between sessions                                                 |
| Select a regional accent                              | Functional     | Usability                              | Exercises tagged by accent in the database, so the API can filter by accent                                               |
| Set accessibility preferences                         | Functional     | Usability                              | Preferences saved per user and applied by the frontend                                                                    |
| Request account deletion                              | Functional     | Security & compliance                  | API action that removes the user’s account and all personal data from the database                                        |
| Available online                                      | Non-functional | Availability, reliability              | Scottish Sounds is accessible as a website, hosting must be reliable. Free tiers that spin down when idle may affect this |
| WCAG 2.1 adherence/accessibility considerations       | Non-functional | Usability                              | Frontend designed to WCAG 2.1 standards (e.g. keyboard navigation, colour contrast checked, screen reader support).       |
| Adequately handle a realistic amount of users/traffic | Non-functional | Scalability, availability, reliability | The frontend, API and database can be hosted separately so each can be upgraded independently                             |
| Easy to update                                        | Non-functional | Maintainability                        | Clear separations between frontend, API and data layers                                                                   |
| Functions on different devices & browsers             | Non-functional | Portability                            | Responsive browser-based frontend built with standard web technologies                                                    |
| Protect data and authenticate users                   | Non-functional | Security & compliance                  | Only the API can access the database & all traffic goes over HTTPS                                                        |

4\. **Proposed Architecture & Pattern Analysis**

The frontend communicates with a backend data API, and the API communicates with the database. Object storage is linked to the database.

| Component      | Proposed              | Connected to            | Notes                                     |
| -------------- | --------------------- | ----------------------- | ----------------------------------------- |
| Frontend       | React                 | Backend data API        | What users interact with from the browser |
| Backend API    | Node.js or Express.js | Frontend, database      | Lightweight, cheap to host                |
| Database       | NoSQL                 | Backend, object storage | MongoDB free tier was noted               |
| Object Storage | ?                     | Database                |                                           |

**Pattern Analysis**

The proposed structure matches a layered (*n*-tier) architecture:

- **Presentation layer:** the frontend
- **Application layer:** the backend data API
- **Data layer:** the database and object storage

Layers describe how responsibilities are split, while tiers describe where each part runs. Because each layer is hosted separately, the proposed architecture is more specifically three-tier.

| Quality Attribute             | Strength                                                                                                                                                                     | Weakness                                                                                                                                                                                                                                                                                  |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maintainability & portability | A layer can be changed or replaced with little effect on the others. For example, switching database wouldn’t affect the frontend. Each layer can also be tested on its own. | A new feature often means changing every layer. For example, regional accent selection needs UI, API and database changes.                                                                                                                                                                |
| Security & compliance         | The browser never accesses the database directly, every request goes through the API which checks who the user is and what they can access.                                  |                                                                                                                                                                                                                                                                                           |
| Scalability                   | Each tier is hosted separately, so each can be upgraded on its own.                                                                                                          | The whole API has to be scaled together, even if only one feature is busy. With our target user base, this is unlikely to be a problem.                                                                                                                                                   |
| Performance                   |                                                                                                                                                                              | Every request passes through each layer, which adds a small delay. Large recordings could be slow if streamed through the API. One option is for the API to give the browser a time-limited link to load them straight from object storage, as a deliberate exception to strict layering. |
| Availability & Reliability    |                                                                                                                                                                              | The layers depend on each other, so if the API or database goes down, the whole system stops working. How likely this is to be a problem depends mostly on what hosting is available to us.                                                                                               |
| Usability                     | UI and accessibility changes, like WCAG 2.1 fixes, can be made in the frontend without touching the backend.                                                                 |                                                                                                                                                                                                                                                                                           |

The layered pattern suits Scottish Sounds because it is simple, well documented and has few parts to host which is ideal for our £0 budget and academic year deadline. It also lets team members work on different layers at the same time. Its main weaknesses, the performance overhead and the backend scaling as one unit, matter far less at this project’s scale. Alternative patterns are compared in section 5.

5\. **Alternative Considerations & Trade-off Analysis**

| Option/Pattern                      | Pros                                                                                               | Cons                                                           | Verdict                                                                                          |
| ----------------------------------- | -------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Online (browser-based)              |                                                                                                    |                                                                | “Available online” is a core requirement                                                         |
| Offline/cached solution             |                                                                                                    | Trades off against scalability and maintainability             | Under review. Do we need offline availability at this time? (YAGNI)                              |
| Essential MVP features only (YAGNI) | Helps meet time to market requirement (within the academic year)                                   |                                                                | Guiding principle for architecture decisions                                                     |
| Node.js server (backend)            | Lightweight & cheap to host                                                                        |                                                                | Proposed                                                                                         |
| Vercel (free tier)                  | Free                                                                                               | Spins down. Only allows deployment from the app’s owner.       | Undecided                                                                                        |
| Render (free tier)                  | Free                                                                                               | Spins down                                                     | Undecided                                                                                        |
| NoSQL database (e.g. MongoDB)       | MongoDB has a free tier                                                                            |                                                                | Proposed                                                                                         |
| Azure Blob storage                  | Possible free via UHI (unconfirmed)                                                                |                                                                | Undecided                                                                                        |
| Volunteers for recordings           | Free                                                                                               | Availability of volunteers                                     | Current approach                                                                                 |
| Paid actors for recordings          | Higher quality, accuracy                                                                           | Not realistic or required for this implementation.             | Not feasible but would be potentially the solution for a full long-term deployment of the system |
| Layered (*n*-tier) architecture     | Simple to build, deploy, maintain and understand. Clear separation between frontend, API and data. | The backend deploys and scales as one unit                     | Proposed solution. Suits the project’s budget and time constraints                               |
| Microservices                       | Services scale and deploy independently                                                            | High network and operational complexity, more services to host | Overkill for this team size, budget and timeline                                                 |

6\. **Risk Assessment & Mitigation**

| Risk                                                                   | Mitigation                                                                             |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| £0 project budget                                                      | Use free tiers. Possible Rebel Fund of up to £2,500 available                          |
| Free-tier hosting spins down when not in use and has limited resources | \-                                                                                     |
| Vercel only allows deployment from the app’s owner                     | Can be mitigated with some GitHub Actions setup                                        |
| Object storage costs                                                   | Verify if Azure Blob storage is available for free through UHI                         |
| Relying on volunteers for recordings                                   | Recordings can be made by the team but this provides less variance in accent           |
| Project must be delivered within the timeline of the academic year     | Apply YAGNI and focus on essential MVP features                                        |
| Offline support adding complexity                                      | Likely not needed at this stage, weigh this against scalability and maintainability    |
| Not meeting user’s accessibility needs                                 | WCAG 2.1 adherence, accessibility considerations, varied user stories created          |
| Recordings of volunteers have GDPR considerations                      | Get written consent from every volunteer and store recordings/consent records securely |

7\. **Implementation & Validation Plan**

**Implementation**

- Features are broken down into stories
- Each story gets a Fibonacci story-point value to represent relative complexity, uncertainty, and effort
- Stories are grouped into epics, which are the major MVP systems
  - An epic’s cost is the sum of its story points
  - Epic totals show scope and capacity, not difficulty
- Time is only introduced as a cost/constraint once active development begins, using the velocity observed from completed work
- Planning uses a conservative baseline of around 1.5 hours per point. Empirical data shows velocity settling closer to about 1.1 hours per point as familiarity increases.
- Essential MVP features are prioritised (YAGNI) to fit the academic year deadline.
- Next phases: the system requirements specification and tech stack evaluation.

**Validation**

- The team agrees whether or not the trade-offs are acceptable
- Each “Proposed” component is revisited before it is confirmed
- Decide how each quality attribute will be tested, for example accessibility checks against WCAG. Decide measurable targets.

8\. **References & Context Links**

Express (n.d.) *Express.js · Node.js web application framework* \[online\]. Available from https://expressjs.com/ \[7th Oct 2026\]

Fowler, M. (2015) *Yagni* \[online\]. Available from https://martinfowler.com/bliki/Yagni.html \[7th Oct 2026\]

GeeksforGeeks (2025) *ISO/IEC 9126 in Software Engineering - GeeksforGeeks* \[online\]. Available from https://www.geeksforgeeks.org/software-engineering/iso-iec-9126-in-software-engineering/ \[7th Oct 2026\]

Node.js (n.d.) *Node.js — Run JavaScript Everywhere* \[online\]. Available from https://nodejs.org/en \[7th Oct 2026\]

React (n.d.) *Managing State – React* \[online\]. Available from https://react.dev/learn/managing-state \[7th Oct 2026\]

Richards, M. (2022) *Software Architecture Patterns* 2nd edn. Sebastopol, CA: O’Reilly Media \[online\]. Available from https://www.oreilly.com/library/view/software-architecture-patterns/9781098134280/ \[7th Oct 2026\]
