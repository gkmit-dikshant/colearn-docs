# Technical Use Cases:

## Login/Signup using google account.
- **Actor**: User
- **Precondition**: User is on the login/signup page.
- **Postcondition**: User is logged in and redirected to the dashboard.
- **Main Flow**:
    1. User clicks on "Login with Google" button.
    2. System redirects to Google OAuth page.
    3. User enters Google credentials and grants permissions.
    4. System verifies credentials and logs in the user.
    5. If the user is new, a new account is created in the system and an email with a default password is sent to the user's registered email.

## Signup with Email/Password 
- **Actor**: User
- **Precondition**: User is on the login/signup page.
- **Postcondition**: User is logged in and redirected to the dashboard.
- **Main Flow**:
    1. User selects “Sign up with Email”.
    2. Fills form with name, email, and password.
    3. System validates inputs and checks existing email.
    4. If new email, system generates 6-digit OTP.
    5. OTP sent to user’s email.
    6. User enters OTP for verification.
    7. On success, account created and verified.
    8. User is logged in and redirected to dashboard.

## Create a project.
- **Actor**: Registered User
- **Precondition**: User is logged in and on the project creation page.
- **Postcondition**: New project is created and visible on the platform.
- **Main Flow**:
    1. User navigates to "Create Project" page.
    2. User fills in project details (title, description, required skills, location tags).
    3. User submits the project creation form.
    4. System saves the project and confirms creation.

## Search for projects.
- **Actor**: Registered User
- **Precondition**: User is logged in and on the project discovery page.
- **Postcondition**: User views a list of projects matching the search criteria.
- **Main Flow**:
    1. User navigates to "Discover Projects" page.
    2. User enters search criteria (skills, title, location tags).
    3. User submits the search form.
    4. System retrieves and displays a list of matching projects.
## Apply to join a project.
- **Actor**: Registered User
- **Precondition**: User is logged in and viewing a project detail page.
- **Postcondition**: User's application is submitted for review by the project owner.
- **Main Flow**:
    1. User navigates to a specific project detail page.
    2. User clicks on "Apply to Join" button.
    3. User fills in application note explaining interest and relevant skills.
    4. User submits the application.
    5. System notifies the project owner of the new application.
## Review applicants and accept/reject them.
- **Actor**: Project Owner
- **Precondition**: Project owner is logged in and viewing their project's applicant list.
- **Postcondition**: Applicant is accepted or rejected, and notified accordingly.
- **Main Flow**:
    1. Project owner navigates to their project's applicant list.
    2. Project owner reviews each applicant's details and application note.
    3. Project owner clicks "Accept" or "Reject" for each applicant.
    4. System updates applicant status and sends email notification to the applicant.
<!-- ## Communicate with team members via chat feature.
- **Actor**: Project Team Member
- **Precondition**: Team member is logged in and part of a project team.
- **Postcondition**: Team members can send and receive messages in real-time.
- **Main Flow**:
    1. Team member navigates to the project dashboard.
    2. Team member accesses the chat feature.
    3. Team member types and sends messages.     -->


