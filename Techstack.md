Scottish Sounds

# Tech stack justification V1.0

Joshua Keitany

28/10/2026

Tech stack justification

## Frontend

This is the visual part of the app that the users interact with directly in their web browser. It is very important that the best tools are employed when designing it.

###  Language

The proposed Scottish sounds application will use HTML, CSS and JavaScript programming languages for its frontend development. These languages are widely used and are easily understood by all major browsers.

###  Framework
React is the preferred framework for the application. React uses component-based architecture. It achieves maintainability, scalability, and efficient development by breaking up the user interface into discrete, reusable components (Kothapalli M, 2024). This means the development team can achieve faster rendering times and fewer cascading bugs.

###  Library

Tailwind CSS library will be employed to achieve faster development times. Since Tailwind provides out of the box utility classes, the development team doesn’t have to write custom CSS from scratch. Bootstrap is also a great choice, but it is a component-first model which means it doesn’t provide the level of flexibility that Tailwind does. Tailwind CSS is also lightweight which means it can load faster.

## Backend

The backend is responsible for processing the user requests. It handles the logic and securely talks to the database.

###  Language

Since the frontend uses JavaScript, the backend will use node.js. This enables a faster development time and reduces errors as both the client side and the server-side use one single programming language.

Node.js is quite lightweight and efficient, which is an idea fit for input/output intensive websites (Lei, K., Ma, Y. and Tan, Z., 2014)

The development team can also employ python programming language to automate some actions like user statistics.

### Database

The development team will use a NoSQL database preferably MongoDB. NoSQL databases allow for faster changes to data structures without breaking the existing code.

NoSQL databases have high flexibility, horizontal scalability and JSON integration (Bozic R, 2022)

###  API

REST API can be used as it uses standard web command such as GET and POST. This allows for efficient communication between the client-side and server-side.

## References

1. Journal of Scientific and Engineering Research, 2024, 11(1):1-7, Kothapalli M \[Accessed: 22 October 2026\].
2. GeeksforGeeks (no date) Tailwind CSS vs Bootstrap. Available at: https://www.geeksforgeeks.org/web-tech/tailwind-css-vs-bootstrap/ \[Accessed: 25 October 2026\].
3. Lei, K., Ma, Y. and Tan, Z. (2014) 'Performance comparison and evaluation of web development technologies in PHP, Python, and Node.js', 2014 IEEE 17th International Conference on Computational Science and Engineering, pp. 661–668. \[Accessed: 25 October 2026\].
4. Advantages and challenges of NoSQL compared to SQL databases – a systematic literature review, Bozic R, 2022 \[Accessed: 25 October 2026\].



## Revisions

Tech stack justification

### Frontend
The following revisions have been suggested.

    1.  TypeScript

TypeScript is preferred because it offers error detection at the compilation stage as opposed to at runtime. It also has autocompletion features.

    2.  Vite.

Vite provides a fast development server

### Backend

The following revisions have been suggested.

    1.  TypeScript together with Node.js

### Platforms
    1. GitHub

GitHub is preferred as a central repository and collaboration workspace. It offers great features like version tracking and code management.

GitHub Actions enables automation of workflows which fosters continuous integration and continuous deployment.

    2.  Kommit

Kommit provides a comprehensive space for planning, ticket issuing and progress tracking.

    3.  Miro

Miro provides an industry standard space for brainstorming ideas and planning for the development stages.
