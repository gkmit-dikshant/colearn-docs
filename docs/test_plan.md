# Test Plan

This document outlines the test plan for the Colearn project, detailing the acceptance criteria for major modules.


| Module | Description | 
|--------|-------------|
| [User Authentication](#1-user-authentication--acceptance-criteria) |Signup, Login, Protected Routes |  
|  [Project CRUD](#2-crud-operations-on-project--acceptance-criteria)| Create, Read, Update, Delete |  
| [Project Joining & Approval](#3-project-joining-and-approval-workflow--acceptance-criteria) | Apply, Approve, Reject |  

---

## 1. User Authentication — Acceptance Criteria

### 1.1 Signup
- User must be able to sign up using Google OAuth or email/password.
- User must be able to create an account with:
  - Valid email
  - Password meeting required length
  - OTP verification
- System must validate missing or invalid fields.
- Duplicate email registration must be rejected.
- Password must be hashed securely in the database.
- Successful signup must return:
  - 201 status
  - User data (excluding password)
  - Auth token

---

### 1.2 Login
- User must be able to log in using:
  - Valid email
  - Correct password
- Wrong password: 401 Unauthorized  
- Non-existing email: 404 User Not Found  
- On successful login:
  - JWT token must be returned
  - User details must be returned
  - Token must allow accessing protected routes

---

### 1.3 Protected Routes
- Access without token: 401 Unauthorized  
- Access with invalid or expired token: 403 Forbidden  
- Access with valid token must succeed  

#### Authentication module passes if:
- Valid users can sign up and log in  
- Invalid credentials are rejected  
- JWT protects secure routes  
- Passwords are never exposed  

---

## 2. CRUD Operations on Project — Acceptance Criteria

### 2.1 Create Project
- Only authenticated users can create projects.
- Required fields:
  - title
  - description
  - required skills
  - location
- Missing fields: 400 Bad Request
- Successful creation must return:
  - 201 status
  - Full project object
  - Owner set as logged-in user

---

### 2.2 Read (Get Projects)
- Any user can view project listings.
- Must support filtering by:
  - Skill
  - Title
  - Location
- No matching results must return an empty array.

---

### 2.3 Read (Get Single Project)
- Must return full project details.
- Invalid project ID: 404 Not Found
- Only project members can access project dashboard details such as chat, resources.

---

### 2.4 Update Project
- Only the project owner can update a project.
- Unauthorized update: 403 Forbidden
- Successful update returns the updated project object.

---

### 2.5 Delete Project
- Only the owner can delete a project.
- Deleting must also remove:
  - Pending applications
  - Member relations (if any)
- Successful delete: 200 or 204 status

#### Project CRUD passes if:
- Owners can manage their projects  
- Non-owners cannot modify or delete  
- Filters and search work correctly  
- Validation errors are meaningful  

---

## 3. Project Joining and Approval Workflow — Acceptance Criteria

### 3.1 Apply to Project
- Only authenticated users can apply.
- User cannot apply to their own project.
- User cannot apply more than once.
- Successful application must:
  - Store status as "pending"
  - Appear in owner's dashboard

---

### 3.2 View Applications (Owner Only)
- Only project owners can see the list of applicants.
- Unauthorized access: 403 Forbidden

---

### 3.3 Approve Application
- Only owner can accept a pending application.
- After approval:
  - User becomes a project member
  - Status updates to "accepted"
  - Apply button must show Accepted

---

### 3.4 Reject Application
- Only the owner can reject an application.
- Status must update to "rejected"
- Applicant must be able to see their rejection state

---

### 3.5 Member Cannot Reapply
- Accepted users cannot reapply.
- Rejected users cannot reapply unless explicitly allowed.

#### Join and Approval workflow passes if:
- Users can apply only once  
- Owners can accept or reject applications  
- RBAC ensures correct permissions  
- Application statuses update correctly  
- UI reflects correct state transitions  

---
