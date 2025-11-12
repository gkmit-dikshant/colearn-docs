## Level 0 DFD
The Level 0 DFD provides a high-level overview of the Colearn platform, showing the main processes, data stores, and external entities involved in the system.

v1
```md
flowchart TD
    %% External Entities
    A[User] -->|Login / Signup, Create Project, Search, Apply, Chat| B((Project Collaboration Portal))
    B -->|Response Pages, Notifications| A

    %% External System
    B -->|Authenticate via OAuth| C[Google OAuth Service]
    C -->|Auth Token / Verification| B

    B -->|Send Notifications| D[Email Service]
    D -->|Email Confirmation, Updates| A

    %% Data Stores
    B -->|Read/Write| E[(Database)]
    
```

v2
```md
graph TD

%% External Entities
U[User]
GO[Google OAuth]
PO[Project Owner]
TM[Team Member]

%% Process (System)
S((Project Collaboration Platform))

%% Data Flows
U -->|Login/Signup Request| S
S -->|Redirect to Google OAuth| GO
GO -->|Credentials + Token| S
S -->|Login Success / Account Created / Email Default Password| U

U -->|Create Project Details| S
S -->|Project Created Confirmation| U

U -->|"Search Criteria (skills, title, location)"| S
S -->|List of Matching Projects| U

U -->|Application to Join Project| S
S -->|Notify Project Owner| PO
PO -->|Accept/Reject Applicant| S
S -->|"Notify Applicant (Status)"| U

TM -->|Send / Receive Messages| S
S -->|Real-time Chat Updates| TM

```

## Level 1 DFD
The Level 1 DFD breaks down the main processes of the Colearn platform into more detailed subprocesses, illustrating how data flows between them.

```md
flowchart TD
    %% External Entities
    A[User] -->|Login / Signup| P1
    A -->|Create Project / View / Edit| P2
    A -->|Search Projects| P3
    A -->|Apply / Review Applicants| P4
    A -->|Send / Receive Messages| P5

    %% Processes
    subgraph System[Project Collaboration Portal]
        P1[1. User Authentication]
        P2[2. Project Management]
        P3[3. Project Discovery]
        P4[4. Application Management]
        P5[5. Chat & Communication]
    end

    %% External Systems
    P1 -->|Auth Request| G[Google OAuth]
    G -->|Verification Token| P1
    P1 -->|Send Welcome Email| E[Email Service]

    %% Data Stores
    P1 -->|Read/Write| D1[(User Database)]
    P2 -->|Read/Write| D2[(Project Database)]
    P3 -->|Read| D2
    P4 -->|Read/Write| D3[(Application Database)]
    P5 -->|Read/Write| D4[(Chat Database)]

    %% Responses
    P1 -->|Redirect / Dashboard| A
    P2 -->|Confirmation / Project List| A
    P3 -->|Search Results| A
    P4 -->|Application Status / Notifications| A
    P5 -->|Real-time Messages| A

```

## ERD
```md
erDiagram
    
    USERS {
        int id PK
        varchar(200) name
        varchar(200) email
        string skills
        string password
        varchar(100) google_id
        string bio
    }
    PROJECTS {
        int id PK
        date created_at
        int owner_id FK
        string description
        string skills
        varchar(100) category
        varchar(200) location
        bool status
    }
    INVITATIONS {
        int id PK
        int sender_id FK
        int project_id FK
        string message
        date created_at
        bool status
    }
    MEMBERSHIPS {
        int id PK
        int member_id FK
        int project_id FK
        int created_at
        bool status
    }

    CHATS {
        int id PK
        int project_id FK
        int sender_id FK
        string text
        date created_at
    }

    USERS || -- O{ PROJECTS: r
    USERS || -- O{ MEMBERSHIPS: r
    USERS || -- O{ INVITATIONS: r
    USERS || -- O{ CHATS: r
    PROJECTS || -- O{ MEMBERSHIPS: r
    PROJECTS || -- O{ INVITATIONS: r
    PROJECTS || -- O{ CHATS: r

```

v2
```md
erDiagram
    USERS {
        int id PK
        varchar(200) name
        varchar(200) email
        string skills
        string password
        varchar(100) google_id
        string bio
        timestamp created_at
    }

    PROJECTS {
        int id PK
        int owner_id FK
        string description
        string skills
        varchar(100) category
        varchar(200) location
        enum status
        timestamp created_at
    }

    INVITATIONS {
        int id PK
        int sender_id FK
        int invitee_id FK
        int project_id FK
        string message
        enum status
        timestamp created_at
    }

    MEMBERSHIPS {
        int id PK
        int member_id FK
        int project_id FK
        varchar(50) role
        bool is_active
        timestamp created_at
    }

    CHATS {
        int id PK
        int project_id FK
        int sender_id FK
        string text
        timestamp created_at
    }

    USERS ||--O{ PROJECTS : owns
    USERS ||--O{ MEMBERSHIPS : joins
    USERS ||--O{ INVITATIONS : sends
    USERS ||--O{ CHATS : sends
    PROJECTS ||--O{ MEMBERSHIPS : has
    PROJECTS ||--O{ INVITATIONS : includes
    PROJECTS ||--O{ CHATS : has

```

v3
```md
erDiagram

    users {
        int id PK
        varchar(200) name
        varchar(200) email
        varchar(200) password
        text bio
        timestamp deleted_at
        timestamp created_at
        timestamp updated_at
    }

    projects {
        int id PK
        int owner_id FK
        varchar(200) title
        text description
        varchar(100) category
        varchar(200) location
        enum status
        timestamp deleted_at
        timestamp created_at
        timestamp updated_at
    }

    skills {
        int id PK
        varchar(100) name
        timestamp created_at
    }

    user_skills {
        int id PK
        int user_id FK
        int skill_id FK
        timestamp created_at
    }

    project_skills {
        int id PK
        int project_id FK
        int skill_id FK
        timestamp created_at
    }

    users_projects {
        int id PK
        int user_id FK
        int project_id FK
        varchar(50) role
        timestamp deleted_at
        timestamp created_at8
    }

    applications {
        int id PK
        int user_id FK
        int project_id FK
        text message
        enum status
        timestamp created_at
        timestamp updated_at
    }

    %% Relationships
    users ||--O{ projects : owns
    users ||--O{ user_skills : has
    users ||--O{ users_projects : joins
    users ||--O{ applications : applies

    projects ||--O{ project_skills : requires
    projects ||--O{ users_projects : includes
    projects ||--O{ applications : receives

    skills ||--O{ user_skills : user_has
    skills ||--O{ project_skills : project_requires

```

v4
```md
erDiagram

    %% ---------------- Core Entities ----------------
    users {
        int id PK
        varchar(200) name
        varchar(200) email
        varchar(200) password
        text bio
        timestamp deleted_at
        timestamp created_at
        timestamp updated_at
    }

    projects {
        int id PK
        varchar(200) title
        text description
        varchar(100) category
        varchar(200) location
        enum status
        timestamp deleted_at
        timestamp created_at
        timestamp updated_at
    }

    skills {
        int id PK
        varchar(100) name
        timestamp created_at
    }

    user_skills {
        int id PK
        int user_id FK
        int skill_id FK
        timestamp created_at
    }

    project_skills {
        int id PK
        int project_id FK
        int skill_id FK
        timestamp created_at
    }



    applications {
        int id PK
        int user_id FK
        int project_id FK
        text message
        enum status
        timestamp created_at
        timestamp updated_at
    }

    %% ---------------- RBAC Entities ----------------

    operations {
        int id PK
        varchar(100) name
        timestamp created_at
    }

    resources {
        int id PK
        varchar(100) name
        timestamp created_at
    }

    permissions {
        int id PK
        int operation_id FK
        int resource_id FK
        timestamp created_at
    }

    roles {
        int id PK
        varchar(100) name
        text description
        timestamp created_at
    }

    roles_permissions {
        int id PK
        int role_id FK
        int permission_id FK
        timestamp created_at
    }

    user_project_role {
        int id PK
        int user_id FK
        int project_id FK
        int role_id FK
        timestamp created_at
        timestamp updated_at
    }

    %% ---------------- Relationships ----------------

    %% User & Project Core Relationships
    users ||--O{ user_skills : has
    users ||--O{ applications : applies

    projects ||--O{ project_skills : requires
    projects ||--O{ applications : receives

    skills ||--O{ user_skills : user_has
    skills ||--O{ project_skills : project_requires

    %% RBAC Relationships
    operations ||--O{ permissions : defines
    resources ||--O{ permissions : controls
    roles ||--O{ roles_permissions : grants
    permissions ||--O{ roles_permissions : assigned_to
    users ||--O{ user_project_role : assigned
    projects ||--O{ user_project_role : scoped
    roles ||--O{ user_project_role : has
```

