DAY 5 ASSIGNMENT

A) User Manual Procedure: Setting Up a GitHub Repository and Making Your First Commit

Prerequisites
Before you begin, ensure you have:
* A computer with an active internet connection.
* Git installed on your system (download from git-scm.com).
* A registered GitHub account (create one at github.com).
* Visual Studio Code or any code editor installed.

Numbered Procedure
1. Open your web browser, navigate to github.com, and log into your account.
   Expected Result: You are logged into GitHub and redirected to your main dashboard.

2. Click the "+" icon in the upper-right corner of the top navigation bar and select "New repository".
   Expected Result: The "Create a new repository" configuration page opens.

3. In the "Repository name" field, type my-first-repo.
   Expected Result: A green checkmark appears next to the input field indicating the name is available.

4. Select the "Public" option and click the green "Create repository" button at the bottom of the page.
   Expected Result: Your new empty repository page loads, displaying setup commands under "Quick setup".

5. Open your computer's terminal (Command Prompt on Windows or Terminal on macOS).
   Expected Result: The command-line interface window opens.

6. Create a new folder by running mkdir my-first-repo and navigate into it by running cd my-first-repo.
   Expected Result: Your terminal working directory changes to the my-first-repo folder.

7. Initialize local Git tracking by running git init.
   Expected Result: Terminal displays Initialized empty Git repository in....

8. Create a sample file by running echo "# My First Project" > README.md.
   Expected Result: A new file named README.md is created in your folder containing the text # My First Project.

9. Stage the new file for tracking by running git add README.md.
   Expected Result: The file is moved to the staging area with no output errors shown.

10. Record the staged changes by running git commit -m "Initial commit".
    Expected Result: Terminal outputs [main (root-commit)] Initial commit along with file change statistics.

11. Set the main branch name by running git branch -M main.
    Expected Result: Your active branch is renamed to main.

12. Link your local repository to GitHub by running git remote add origin https://github.com/YOUR-USERNAME/my-first-repo.git (replace YOUR-USERNAME with your actual GitHub username).
    Expected Result: The remote origin URL is linked without displaying errors.

13. Upload your commit to GitHub by running git push -u origin main.
    Expected Result: Git uploads the branch, and terminal outputs Branch 'main' set up to track remote branch 'main' from 'origin'.

Screenshot Description
* Screenshot Title: GitHub New Repository Configuration Page.
* What it shows: The GitHub page where the repository details are entered. It shows the "Repository name" field filled with my-first-repo, the "Public" radio button selected, and the green "Create repository" button highlighted at the bottom.

Troubleshooting Note
* Common Error: fatal: Authentication failed for... when running git push.
* Fix: GitHub no longer accepts account passwords for command-line authentication. When prompted for a password in the terminal, use a Personal Access Token (PAT) generated from your GitHub account under Settings > Developer Settings > Personal Access Tokens, or sign in using Git Credential Manager browser login.


B) API Reference Entry: Create Task Endpoint

HTTP Method & Path
POST /api/v1/projects/{project_id}/tasks

Description
Creates a new task within a specified project for an authenticated user.

Request Headers
* Authorization (string, Required): Bearer token format (Bearer <token>).
* Content-Type (string, Required): Must be application/json.

Request Parameters
* Path Parameters:
  - project_id (string, Required): The unique UUID of the project where the task is being created.
* Query Parameters: None.
* Body Parameters (JSON):
  - title (string, Required): The title/name of the task (1-255 characters).
  - description (string, Optional): Detailed explanation or details of the task.
  - assignee_id (string, Optional): The unique user UUID of the person assigned to the task.
  - due_date (string, Optional): Due date in ISO 8601 format (YYYY-MM-DD or YYYY-MM-DDTHH:mm:ssZ).
  - priority (string, Optional): Priority level. Allowed values: "low", "medium", "high". Default value is "medium".

HTTP Response Codes
* 201 Created: Task was successfully created.
* 400 Bad Request: Invalid request body or missing required fields (e.g., empty title or invalid priority value).
* 401 Unauthorized: Missing or invalid authentication token.
* 404 Not Found: The specified project_id or assignee_id does not exist.
* 500 Internal Server Error: Server encountered an error while processing the request.

Example Request Body
{
  "title": "Setup User Authentication Module",
  "description": "Implement JWT login and token refresh logic for API authentication.",
  "assignee_id": "usr_8f9a2b1c-3d4e-5f6a-7b8c-9d0e1f2a3b4c",
  "due_date": "2026-10-15T17:00:00Z",
  "priority": "high"
}

Example Successful Response Body
Status: 201 Created
{
  "success": true,
  "message": "Task created successfully.",
  "data": {
    "task_id": "tsk_11223344-5566-7788-9900-aabbccddeeff",
    "project_id": "prj_99887766-5544-3322-1100-ffeeddccbbaa",
    "title": "Setup User Authentication Module",
    "description": "Implement JWT login and token refresh logic for API authentication.",
    "assignee_id": "usr_8f9a2b1c-3d4e-5f6a-7b8c-9d0e1f2a3b4c",
    "due_date": "2026-10-15T17:00:00Z",
    "priority": "high",
    "status": "todo",
    "created_by": "usr_1a2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d",
    "created_at": "2026-09-15T12:00:00Z",
    "updated_at": "2026-09-15T12:00:00Z"
  }
}


Part C: Brief Technical Report Section — System Architecture Overview

Overview
This technical report section outlines the high-level architecture for the Project Management API system. The application utilizes a modern RESTful microservices architecture designed for scalability, security, and low latency under peak user loads.

Key System Components
* API Gateway: Serves as the single entry point for client requests, handling rate limiting, request routing, and SSL termination.
* Authentication Service: Issues and validates JSON Web Tokens (JWT) to secure user sessions and enforce role-based access control (RBAC).
* Task Management Service: Implements core business logic for project tasks, exposing CRUD endpoints (such as POST /api/v1/projects/{projectId}/tasks).
* Relational Database (PostgreSQL): Stores persistent application data including project schemas, user profiles, and task assignments with strict foreign key constraints.

Recommendations for Production Deployment
To ensure high availability, all services should be containerized using Docker and orchestrated via Kubernetes. Continuous Integration/Continuous Deployment (CI/CD) pipelines must run automated unit and integration test suites prior to deploying updates to production environments.
