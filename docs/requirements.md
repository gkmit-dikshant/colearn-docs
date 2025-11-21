# Requirements
In this section, we outline the key functional requirements, non-functional requirements, and out-of-scope features for the Colearn platform.
## Functional Requirements
|ID | Requirement |  Description | Priority | Scope |
|---- |----- |---------------|-----------| -------|
| [F1](#f1-authentication) | Authentication | User should be able to login/signup using email and password.  | High | Current |
|[F2](#f2-project-creation) |Project Creation| User should be able to create a project specifying the project description. | High | Current |
|[F3](#f3-project-discovery) |Project Discovery| User should be able to view project details.| High | Current |
| [F4](#f4-project-joining) | Project Joining | User should be able to apply for a project. Project owner should be able to view applicants and accept/reject them. Accepted users should be able to join project team.| High | Current |
| [F5](#f5-project-dashboard) | Project Dashboard | A Dedicated dashboard for accessing all the features. | High | Current |
| [F6](#f6-team-communication) | Team Communication | Team members should be able to communicate via chat feature. | High | Future |
| [F7](#f7-task-management) | Task Management| Project Owner should be able to create and assign tasks. Deadlines for assigned tasks should be set and tracked and reminders sent.| Medium | Future |
| [F8](#f8-progress-tracking) | Progress Tracking| Periodic progress updates should be logged and viewable by all team members. | Medium | Future |
| [F9](#f9-resource-sharing) | Resouce Sharing | Team members should be able to share resources within the team. | Low | Future |

### F1: Authentication
- Users sign up using email and password should verify their email address via a otp sent to their email.
- JWT token based authentication and authorization will be implemented for secure access to the platform.

### F2: Project Creation
- Users should be able to create new projects by providing necessary details such as project title, description, required skills, and location tags.
- Project owners should be able to edit project details after creation.

### F3: Project Discovery
- Users should be able to see projects with skills, title, and location tags.
- Users should be able to view detailed information about each project, including project description, requirements, and team members.

### F4: Project Joining
- Users should be able to apply for projects they are interested in with a note explaining their interest and relevant skills.
- Project owners should be able to view a list of applicants for their projects, along with their application notes.
- Project owners should be able to accept or reject applicants. Accepted users should receive a email notification and be able to access the project dashboard.

### F5: Project Dashboard
- A dedicated project dashboard should be available for each project, accessible only to project team members.
- The dashboard should provide role based access to all project-related features, including communication, task management, progress tracking, and resource sharing.

### F6: Team Communication
- Team members should be able to send and receive messages in real-time using a chat feature on a common forum.
### F7: Task Management
- Project owners should be able to create tasks, assign them to team members, and set deadlines.
- Team members should receive reminders through email for upcoming deadlines and be able to mark tasks as complete.
### F8: Progress Tracking
- System should generate periodic progress based on task creation and completion data.
- Progress updates should be viewable by all team members on the project dashboard.
### F9: Resource Sharing
- Team members should be able to upload and share documents, links, and other resources within the project team on a dedicated resource sharing section.

## Future Scope
#### Features not included in the current project phase    
- Team Communication Feature.
- Task Management Feature.   
- Weekly Progress Tracking.  
- Dedicated Team Resource Sharing.  


## Non - Functional Requirements
| ID |  Requirement | Description | Priority | Scope|
|----|---------------|------- |-----------| -----|
| NF1 | Code Readability | Code should clean and modular. | High | Current |
| NF2 | Reliability |All the request should be processed within miliseconds. | High | Current |
| NF3 | Security |All the passwords should be encrypted. | High | Current |
| NF4 | Scalability |Seperate microservices for email, schedular and chat services| Medium| Future |

