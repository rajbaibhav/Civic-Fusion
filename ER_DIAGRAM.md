# Civic Fusion - Database ER Diagram

## Entity Relationship Diagram

```mermaid
erDiagram
    USER ||--o{ PROJECT : creates
    USER ||--o{ ISSUE : raises
    USER ||--o{ COMMENT : writes
    USER ||--o{ ISSUE_RESPONSE : responds
    USER ||--o{ PROGRESS_HISTORY : updates
    USER ||--o{ BUDGET_HISTORY : updates
    PROJECT ||--|| BUDGET : has
    PROJECT ||--o{ ISSUE : contains
    PROJECT ||--o{ COMMENT : receives
    ISSUE ||--o{ ISSUE_RESPONSE : has
    ISSUE ||--o{ USER : "escalatedTo"

    USER {
        string _id PK
        string name
        string email UK
        string password
        string role "citizen,volunteer,official,admin"
        boolean isActive
        boolean isBlocked
        timestamp createdAt
        timestamp updatedAt
    }

    PROJECT {
        string _id PK
        string title
        string description
        string department
        string location
        string status "planned,ongoing,completed,stalled"
        number progress "0-100"
        string createdBy FK
        boolean isArchived
        timestamp createdAt
        timestamp updatedAt
    }

    ISSUE {
        string _id PK
        string project FK
        string raisedBy FK
        string title
        string description
        string category
        string status "open,in_progress,resolved"
        string priority "low,medium,high"
        boolean isEscalated
        string escalatedTo FK
        timestamp createdAt
        timestamp updatedAt
    }

    ISSUE_RESPONSE {
        string _id
        string respondedBy FK
        string response
        timestamp createdAt
        timestamp updatedAt
    }

    BUDGET {
        string _id PK
        string project FK "unique"
        number allocatedAmount
        number spentAmount
        timestamp createdAt
        timestamp updatedAt
    }

    BUDGET_HISTORY {
        string _id
        number allocatedAmount
        number spentAmount
        string updatedBy FK
        string notes
        timestamp createdAt
        timestamp updatedAt
    }

    COMMENT {
        string _id PK
        string project FK
        string user FK
        string content
        boolean isDeleted
        string deletedBy FK
        timestamp createdAt
        timestamp updatedAt
    }

    PROGRESS_HISTORY {
        string _id
        number progress "0-100"
        string updatedBy FK
        string notes
        array images
        timestamp createdAt
        timestamp updatedAt
    }
```

## Key Relationships

- **USER** (Central Entity)
  - Roles: citizen, volunteer, official, admin
  - Can create projects, raise issues, write comments
  - Track updates to progress and budget history

- **PROJECT**
  - Created by users
  - Has one-to-one relationship with Budget
  - Contains multiple issues and comments
  - Tracks progress history

- **ISSUE**
  - Belongs to a project
  - Raised by users
  - Can have multiple responses
  - Can be escalated to specific users
  - Statuses: open, in_progress, resolved
  - Priorities: low, medium, high

- **BUDGET**
  - One-to-one relationship with Project
  - Tracks allocated and spent amounts
  - Maintains history of budget changes

- **COMMENT**
  - Links users to projects for discussions
  - Supports soft delete (marked as deleted)
  - Ordered by creation time

- **Embedded Schemas** (stored within documents)
  - Progress History: Tracks project progress updates with images
  - Budget History: Tracks budget allocation and spending changes
  - Issue Responses: Tracks responses to issues
