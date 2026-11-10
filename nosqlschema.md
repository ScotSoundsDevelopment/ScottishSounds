<!-- First Iterations -->

# First Iterations

### Flowchart

``` mermaid
flowchart LR
    subgraph Exercises ["Collection: exercises"]
        ExerciseDoc["<b><u>ExerciseDocument</u></b><br/><br/>id: string<br/>title: string<br/>description: string<br/>videoUrl: string<br/>audioUrl: string<br/>transcriptionSolution: string[]<br/>difficultyLevel: string<br/>"]
    end

```

<!-- First Iterations -->
### Class Diagram
```mermaid

classDiagram
    class Exercise {
        string id
        string title
        string description
        string videoUrl
        string audioUrl
        string[] transcriptionSolution
        string difficultyLevel
        }
```

    
    
<!-- Future Iterations -->
# Future Iterations

### Flowchart
``` mermaid
flowchart LR
    subgraph Users ["Collection: users"]
        UserDoc["<b><u>UserDocument</u></b><br/><br/> id: string<br/>displayName: string<br/>email: string<br/>role: 'student' | 'admin'<br/>"]
    end

    subgraph Exercise ["Collection: exercises"]
        ExerciseDoc["<b><u>ExerciseDocument</u></b><br/><br/>id: string<br/>title: string<br/>description: string<br/>videoUrl: string<br/>audioUrl: string<br/>transcriptionSolution: string[]<br/>difficultyLevel: string<br/>"]
    end

    subgraph Feedback ["Subcollection: feedback"]
        FeedbackDoc["<b><u>FeedbackDocument</u></b><br/><br/>id: string<br/>exerciseId: string<br/>user: { id, displayName, role }<br/>attemptsCount: int<br/>wrongAnswers: string[]<br/>revealedAnswer: boolean<br/>feedbackText: string<br/>"]
    end

    ExerciseDoc -->|"Subcollection"| FeedbackDoc

    subgraph searches ["Query"]
        CGQ["<b>Summary Query ('feedback')</b><br/>Queries all student submission/feedback documents across all exercises globally"]
    end

    CGQ -.->|"Searches"| FeedbackDoc

```

### Class Diagram

``` mermaid

classDiagram
    class User {
        string id
        string displayName
        string email
        string role
    }
    class Exercise {
        string id
        string title
        string description
        string videoUrl
        string audioUrl
        string[] transcriptionSolution
        string difficultyLevel
        
    }
    class Feedback {
        string id
        string exerciseId
        Object user
        int attemptsCount
        string[] wrongAnswers
        boolean revealedAnswer
        string feedbackText
    }
    Exercise  -->  Feedback : subcollection
    ```

 