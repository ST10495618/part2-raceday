# part2-raceday
# RaceDay API
#SWAGGER

YOUTUBE LINK https://youtu.be/_tuwAzBIQVI 

## 1. Project Overview

RaceDay is an ASP.NET Core Web API developed to manage racing events and participant registrations. The system provides functionality for users to register and log in, organisers to manage events, and participants to enrol in available events.

The project uses a relational database to store information about users, roles, event categories, events, and registrations.

## 2. Project Objectives

The main objectives of the RaceDay API are to:

- Provide secure user registration and login.
- Support role-based access for Participants and Organisers.
- Allow Organisers to create, view, update, and delete their events.
- Allow Participants to register for available events.
- Prevent duplicate registrations for the same event.
- Store event and registration information in a SQL Server database.
- Provide API endpoints that can be tested using Swagger or an API testing tool.

## 3. Technologies Used

- **Programming Language:** C#
- **Framework:** ASP.NET Core Web API
- **Target Framework:** .NET 8
- **Database:** Microsoft SQL Server
- **ORM:** Entity Framework Core
- **Authentication:** JSON Web Tokens (JWT)
- **Password Security:** Password hashing
- **API Documentation:** Swagger / OpenAPI
- **Testing:** xUnit and ASP.NET Core integration testing
- **Development Environment:** Visual Studio 2022
- **Version Control:** Git and GitHub

## 4. Main System Features

### User Authentication
Users can create accounts and log in using their email address and password. Passwords are hashed before being stored, and successful login returns a JWT for authenticated requests.

### Role-Based Access Control
The system supports two primary roles:

- **Participant:** Can view events and register for available events.
- **Organiser:** Can manage events they own.

Endpoints that require authentication reject unauthenticated requests. Endpoints restricted to a particular role reject users who do not have the required role.

### Event Management
Organisers can create, update, and delete events. Event information includes the event name, description, category, date, time, location, entry fee, participant limit, and status.

### Event Registration
Participants can enrol in available events. The API checks whether an event exists, whether it is still available, and whether the participant has already registered.

## 5. Database Design

The RaceDay database uses related tables to organise and maintain system information.

| Table | Purpose |
|---|---|
| Users | Stores user account information. |
| Roles | Stores system roles such as Participant and Organiser. |
| UserRole | Connects users to their assigned roles. |
| Category | Stores event categories. |
| Event | Stores event details and the organiser responsible for each event. |
| Registration | Records participant enrolments in events. |
| Results | Stores event registration results. |
| Route | Stores route information associated with events. |
| Weather | Stores weather forecast information for events. |

### Database Relationships

- Users and Roles are connected through the UserRole table.
- An Organiser can manage multiple events.
- A Category can contain multiple events.
- A User can have multiple registrations.
- An Event can have multiple registrations.
- Each registration links a user to an event.
- Results, routes, and weather records are associated with relevant event or registration records.

Unique constraints help prevent duplicate user emails, duplicate user-role assignments, duplicate event registrations, and duplicate results for the same registration.

## 6. API Endpoints

### Authentication

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Register a new user. |
| POST | `/api/auth/login` | Authenticate a user and obtain a JWT. |

### Events

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/events` | Retrieve available events. |
| GET | `/api/events/{id}` | Retrieve a specific event. |
| POST | `/api/events` | Create an event as an Organiser. |
| PUT | `/api/events/{id}` | Update an event owned by the Organiser. |
| DELETE | `/api/events/{id}` | Delete an event owned by the Organiser. |

### Registrations

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/registrations` | Retrieve registrations according to the user's role. |
| POST | `/api/registrations` | Register as a Participant for an event. |
| DELETE | `/api/registrations/{id}` | Cancel the authenticated Participant's registration. |

**Note:** These endpoints reflect the current controller implementation. Additional endpoints may be added as the project develops.

## 7. Security

The RaceDay API includes the following security measures:

- JWT-based authentication for protected endpoints.
- Role-based authorisation.
- Password hashing instead of storing plain-text passwords.
- Ownership checks to prevent Organisers from managing events belonging to other Organisers.
- Validation to prevent duplicate registrations.
- Server-side validation of incoming requests.

The JWT signing key and database connection string should be configured securely and must not contain production secrets in source control.

## 8. Installation and Setup

### Requirements

- Visual Studio 2022.
- A compatible .NET SDK for the project's target framework.
- Microsoft SQL Server.
- SQL Server Management Studio (SSMS), if required for database management.

### Step 1: Clone the repository

```bash
git clone YOUR_GITHUB_REPOSITORY_URL
```

Replace `YOUR_GITHUB_REPOSITORY_URL` with your actual GitHub repository URL.

### Step 2: Open the project

Open the RaceDay solution file (`.sln`) in Visual Studio 2022.

### Step 3: Configure the database

Update the connection string in `appsettings.json` to point to your SQL Server database.

Example:

```json
{
  "ConnectionStrings": {
    "RaceDayConnection": "Server=YOUR_SERVER;Database=RaceDay;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

Replace `YOUR_SERVER` with your SQL Server instance name. Ensure the database and required tables exist before running the API.

### Step 4: Configure JWT

Configure the JWT issuer, audience, and signing key for your development environment. Use user secrets or environment variables for sensitive values rather than committing real secrets to GitHub.

### Step 5: Restore dependencies

Run the following command in the project directory:

```bash
dotnet restore
```

### Step 6: Build the project

```bash
dotnet build
```

### Step 7: Run the API

```bash
dotnet run
```

Open the Swagger URL shown in the terminal to view and test the available API endpoints.

## 9. Unit and Integration Testing

The project includes planned tests for important API functionality, including:

- Successful user registration.
- Successful login with valid credentials.
- Rejection of invalid login credentials.
- Rejection of unauthenticated requests to protected endpoints.
- Enforcement of Organiser and Participant permissions.
- Event management by authorised Organisers.
- Successful participant event registrations.
- Prevention of duplicate registrations.

To run the test suite after configuring the test project, use:

```bash
dotnet test
```

Tests should be executed locally before changes are pushed to GitHub. The test suite must compile and pass before the project can be reported as fully tested.

## 10. Continuous Integration

GitHub Actions can be configured to restore dependencies, build the solution, and run automated tests whenever changes are pushed or a pull request is opened.

The workflow should use a .NET SDK compatible with the project and test-project target frameworks.

## 11. Future Improvements

Possible improvements include:

- Adding more comprehensive validation and error handling.
- Improving event search and filtering.
- Adding automated tests for all controllers.
- Expanding event results and reporting functionality.
- Integrating weather forecast information.
- Improving API documentation and deployment configuration.
- Deploying the API to a cloud hosting platform.

## 12. Author

**Project:** RaceDay API  
**Developer:** Thuto Sehlapelo  
**Purpose:** Software development project for managing racing events and participant registrations.

## 13. Licence

This project was developed for educational purposes. Add a suitable licence if you intend to distribute or reuse the project publicly.
