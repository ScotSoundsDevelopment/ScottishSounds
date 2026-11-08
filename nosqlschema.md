``` mermaid
flowchart LR
    subgraph Users ["Collection: users"]
        UserDoc["<b><u>UserDocument</u></b><br/><br/> id: string<br/>displayName: string<br/>email: string<br/>role: 'student' | 'admin'<br/>"]
    end

    subgraph Transcription ["Collection: transcriptions"]
        TranscriptionDoc["<b><u>TranscriptionDocument</u></b><br/><br/>id: string<br/>title: string<br/>description: string<br/>mediaUrl: string<br/>mediaType: 'audio' | 'video'<br/>phoneticTarget: string[]<br/>difficultyLevel: string<br/>user: { id, displayName, role }<br/>"]
    end

    subgraph Feedback ["Subcollection: feedback"]
        FeedbackDoc["<b><u>FeedbackDocument</u></b><br/><br/>id: string<br/>transcriptionId: string<br/>user: { id, displayName, role }<br/>transcriptionAttempt: string<br/>isCorrect?: boolean<br/>feedbackText?: string<br/>"]
    end
    TranscriptionDoc -->|"Subcollection"| FeedbackDoc

    subgraph searches ["Query"]
        CGQ["<b>Summary Query ('feedback')</b><br/>Queries all student submission/feedback documents across all transcriptions globally"]
    end

    CGQ -.->|"Searches"| FeedbackDoc

```

``` mermaid

classDiagram
    class User {
        string id
        string displayName
        string email
        string role
    }
    class Transcription {
        string id
        string title
        string description
        string mediaUrl
        string mediaType
        string[] phoneticTarget
        string difficultyLevel
        Object user
    }
    class Feedback {
        string id
        string transcriptionId
        Object user
        string transcriptionAttempt
        boolean isCorrect
        string feedbackText
    }
    Transcription  -->  Feedback : subcollection

    ```