**Scottish Sounds**  
**Software Architecture Research Report 1.0**  
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

9. **Overview & Meta Data**

- Title: Scottish Sounds Software Architecture Research Report
- Authors: Sergio Eneide & Bruce Gray
- Date: 07/10/2026
- Document Status**:** Draft for review
- Objective: Consolidate our findings during our software architecture sessions to enable us to take a considered approach when recommending our architecture proposal

Scottish Sounds is a web-based tool with the goal of helping Speech and Language Therapy (SLT) students to practice the phonetic transcription of Scottish English. Students can listen to or watch recordings of words or phrases spoken with Scottish accents, transcribe them using a built-in international phonetic alphabet (IPA) keyboard, and check their answers. This document summarises our team architecture research with considerations to user context, requirements, proposed architecture, and consideration of alternatives or trade-offs. This is a living document and is subject to change as the project progresses.

2. **Context & Problem Statement**

- Background:
  SLT students in Scotland need an accessible way to practice transcribing Scottish accents with the IPA and have their answers checked from a web browser. Our solution must deliver this on a 0 zero pounds budget within the timeline of the academic year whilst being compliant with accessibility standards (WCAG 2.1) and industry standard security and compliance standards.  

- Drivers & Triggers
- Scope

What the user does and requires from the system:

- Interacts with the UI and transcription exercises
- Selects a difficult level
- Plays the audio or video recording
- Inputs an answer using the IPA keyboard
- Gets feedback on their answer (correct/incorrect) and can view the correct answer(s)
- Navigate to the next question in an exercise

Whilst not necessarily requirements for our implementation at this stage, users may also wish to check their learning statistics within the context of a difficulty/accent/year/lifetime, register an account and sign in, set their accessibility preferences and request account deletion.

**Constraints**  
The current budget for the project is 0 zero pounds, with potential funding available via the UHI Rebel Fund of up to two thousand five hundred pounds 2,500. We have a fixed deadline of May 2027, or the end of the academic year. Audio or video recordings will currently rely on volunteers, and our access to staff is limited by available work hours.

3. **Requirements & Quality Attributes**

| Requirement                                           | Type           | Quality Attribute                      | Architectural implication                                                                                                 |
| ----------------------------------------------------- | -------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Visit & use the app from a web browser                | Functional     | Maintainability, portability           | Browser-based frontend hosted online, no installation                                                                     |
| Select a difficult level & start the exercise         | Functional     | Usability                              | Exercises stored with an associated difficulty level & the API returns exercises of the selected level                    |
| Toggle audio-only or audio-and-video playback         | Functional     | Usability                              | Frontend media player with video toggle                                                                                   |
| Play exercise recording                               | Functional     | Performance                            | Recordings held in object storage, with the noSQL database storing a reference to each file                               |
| Input answers using the IPA keyboard                  | Functional     | Usability                              | IPA keyboard embedded as a frontend component                                                                             |
| Check answer input against solution(s)                | Functional     | Security & compliance, performance     | Answers checked in the backend API so solutions are not exposed to the browser                                            |
| Reveal the correct solution(s)                        | Functional     | Usability                              | API returns the correct solution when requested by the user                                                               |
| Move to the next exercise                             | Functional     | Performance                            | The API serves the next exercise of the selected difficulty                                                               |
| Create an account & log in                            | Functional     | Security & compliance                  | Authentication component, accounts stored in database                                                                     |
| View progress on a dashboard                          | Functional     | Usability                              | The database stores user attempt so the API can calculate progress/statistics                                             |
| Resume progress from a previous session               | Functional     | Usability, availability, reliability   | The database saves user progress so it is not lost between sessions                                                       |
| Select a regional accent                              | Functional     | Usability                              | Exercises tagged by accent in the database, so the API can filter by accent                                               |
| Set accessibility preferences                         | Functional     | Usability                              | Preferences saved per user and applied by the frontend                                                                    |
| Request account deletion                              | Functional     | Security & compliance                  | API action that removes the user account and all personal data from the database                                          |
| Available online                                      | Non-functional | Availability, reliability              | Scottish Sounds is accessible as a website, hosting must be reliable. Free tiers that spin down when idle may affect this |
| WCAG 2.1 adherence/accessibility considerations       | Non-functional | Usability                              | Frontend designed to WCAG 2.1 standards (e.g. keyboard navigation, colour contrast checked, screen reader support).       |
| Adequately handle a realistic amount of users/traffic | Non-functional | Scalability, availability, reliability | The frontend, API and database can be hosted separately so each can be upgraded independently                             |
| Easy to update                                        | Non-functional | Maintainability                        | Clear separations between frontend, API and data layers                                                                   |
| Functions on different devices & browsers             | Non-functional | Availability                           | Responsive browser-based frontend built with standard web technologies                                                    |
| Protect data and authenticate users                   | Non-functional | Security & compliance                  | Only the API can access the database & all traffic goes over HTTPS                                                        |

4. **Proposed Architecture & Pattern Analysis**

The frontend communicates with a backend data API, and the API communicates with the database. Object storage is linked to the database.
