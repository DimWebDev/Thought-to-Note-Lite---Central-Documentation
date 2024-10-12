# Thought-to-Note Lite - Central Documentation

## Overview

**Thought-to-Note Lite** is a full-stack note-taking application that consists of a Java-based backend API and a React-based frontend client. This document serves as the central point of reference for the entire application, providing insights into how the backend and frontend interact, setup instructions, and a detailed breakdown of each component. The Thought-to-Note Lite system is built to manage user notes seamlessly, using a microservices architecture that ensures scalability, maintainability, and a clean separation of concerns. This guide is designed for developers, stakeholders, and anyone who wants to understand the detailed workings of a modern distributed full-stack system.

The backend, implemented as a Java microservice, provides core data management and CRUD functionalities through RESTful APIs. It uses Spring Boot to handle requests efficiently, and PostgreSQL as a robust database management system. The backend handles critical operations such as authentication, note management, and data persistence. Meanwhile, the frontend is a TypeScript-based React web client that consumes these APIs to provide an intuitive user experience. It offers features like responsive note creation, viewing, updating, and deletion, all while ensuring smooth interaction through modern UI elements built using Material-UI.

This guide will walk you through the system architecture, configuration, and detailed steps required to run and interact with the application as a whole. Furthermore, this document also covers key design choices that were made to ensure the scalability and maintainability of the system, as well as troubleshooting tips for common issues that users or developers may encounter.

## Table of Contents

1. [System Architecture](#system-architecture)
2. [Setup Instructions](#setup-instructions)
   - [Backend Setup](#backend-setup)
   - [Frontend Setup](#frontend-setup)
3. [System Interaction](#system-interaction)
4. [Testing Strategy](#testing-strategy)
   - [Backend Testing](#backend-testing)
   - [Frontend Testing](#frontend-testing)
5. [Key Design Choices](#key-design-choices)
6. [API Summary](#api-summary)
7. [Github CI/CD Workflows](#continuous-integration-ci-pipeline-integration)
8. [Branching Strategies](#branching-strategy)
9. [Troubleshooting & Common Issues](#troubleshooting--common-issues)

## System Architecture

The Thought-to-Note Lite application is designed as a **distributed full-stack system** with the following architecture:

- **Backend**: The backend is implemented as a Java microservice using Spring Boot, which acts as the core server to handle data management and business logic. It ensures that the data layer is abstracted from the user interface, enabling a clean separation of concerns. The backend service manages core functionalities such as note storage, retrieval, updating, and deletion, along with user authentication.

  To dive deeper into the backend part of this application, refer to the ***[BE Repository](https://github.com/DimWebDev/thought-to-note-lite-be/tree/develop)***.

  - **Database**: PostgreSQL is used to manage persistent storage for notes, leveraging its ACID properties to ensure data reliability and integrity.
  - **API Management**: All interactions with the database are performed through RESTful APIs, secured via Basic Authentication, which ensures that only authorized users have access to the data.

- **Frontend**: The frontend is implemented as a React web application using TypeScript, providing a responsive and interactive user interface for users to manage their notes. The frontend is designed with a focus on user experience, utilizing Material-UI to maintain a clean and consistent design.

  To dive deeper into the frontend part of this application, refer to the ***[FE Repository](https://github.com/DimWebDev/thought-to-note-lite-fe/tree/develop)***.

  - **User Interface**: The UI provides features such as note creation, editing, listing, and deletion, all managed through REST API calls to the backend. These features are built to be intuitive, allowing users to easily interact with the application.
  - **API Integration**: The frontend interacts with the backend using a centralized `api.ts` utility file, which manages all API calls. This abstraction helps keep the codebase organized and simplifies updates to backend endpoints.

- **Interaction**: The frontend communicates with the backend via RESTful HTTP calls, allowing for Create, Read, Update, and Delete (CRUD) operations on notes. Each API call requires authentication, ensuring that only authenticated users can modify or view the notes. This architecture ensures data security while maintaining a fluid and responsive experience for the end user.

## Setup Instructions

### Backend Setup 👉 *[For Detailed Instruction Click Here](https://github.com/DimWebDev/thought-to-note-lite-be/tree/develop)*

1. **Clone the Backend Repository**:

   First, clone the repository using the command below. This will allow you to work with the backend code on your local machine:

   ```bash
   git clone <repository-url>
   cd thought-to-note-lite-be
   ```

2. **Set Up Docker and PostgreSQL**:
   Ensure Docker is installed on your machine. Docker will be used to set up a containerized instance of PostgreSQL, making database management easier and more consistent across different environments.

   ```bash
   docker-compose -f docker/compose.yaml up -d
   ```

   This command will start the PostgreSQL container defined in the `docker-compose.yaml` file.

3. **Install Maven Wrapper**:
   The Maven Wrapper ensures that all developers are using a consistent version of Maven, which prevents compatibility issues.

   ```bash
   mvn -N io.takari:maven:wrapper -Dmaven=3.9.8
   ```

4. **Run the Spring Boot Application**:
   Start the Spring Boot application using the Maven Wrapper:

   ```bash
   ./mvnw spring-boot:run
   ```

5. **Verify Backend Availability**:
   After starting the backend, verify its availability by accessing the following URL in your browser or using a tool like Postman:

   ```
   http://localhost:8080/api/notes
   ```

   This endpoint should be accessible, indicating that the backend is running successfully.

### Frontend Setup 👉 *[For Detailed Instruction Click Here](https://github.com/DimWebDev/thought-to-note-lite-fe/tree/develop)*

1. **Clone the Frontend Repository**:

   Clone the frontend repository to get the source code:

   ```bash
   git clone <repository-url>
   cd thought-to-note-lite
   ```

2. **Install Dependencies**:
   Install the necessary dependencies using npm to ensure the application runs as expected:

   ```bash
   npm install
   ```

3. **Run the Application**:
   Start the frontend development server:

   ```bash
   npm start
   ```

4. **Access the Application**:
   Once the development server is running, access the frontend in your browser by navigating to:

   ```
   http://localhost:3000
   ```

   You should see the Thought-to-Note Lite interface ready to use.

## System Interaction

The **frontend client** interacts with the **backend** via HTTP requests. The backend exposes various endpoints that the frontend uses to perform note operations. These endpoints provide full CRUD functionality for managing notes:

- **Get All Notes**: `GET /api/notes` - Retrieves all notes stored in the system.
- **Get Note by ID**: `GET /api/notes/{id}` - Fetches a specific note by its unique ID.
- **Create Note**: `POST /api/notes` - Adds a new note to the system.
- **Update Note**: `PUT /api/notes/{id}` - Updates the content or title of an existing note.
- **Delete Note**: `DELETE /api/notes/{id}` - Deletes a note by its unique ID.

**Authentication** is enforced through Basic Authentication headers. The credentials must be included in every request from the frontend to the backend to ensure that only authorized users can access or modify notes. This is handled centrally in the `api.ts` file within the frontend, simplifying API call management and improving security.

## Testing Strategy

Testing is a crucial aspect of ensuring that the Thought-to-Note Lite application functions correctly. Testing has been implemented for both the backend and frontend to ensure robustness and reliability.

### Backend Testing

- **Unit Tests**: All service classes and API controllers in the backend have been unit tested using JUnit and Mockito. These tests verify that individual components work as expected in isolation. For further insights into the unit tests, please refer to the [BE Repository](https://github.com/DimWebDev/thought-to-note-lite-be/tree/develop).
- **Integration Tests**: Integration tests were implemented using Testcontainers, which creates an isolated PostgreSQL environment to test the entire application stack, ensuring that all components work together as intended. Integration testing validates that the system behaves as expected with real data and database connections. More information is available in the [BE Repository](https://github.com/DimWebDev/thought-to-note-lite-be/tree/develop).

### Frontend Testing

- **Component and Hook Tests**: The frontend components and hooks are tested using Jest and React Testing Library. This ensures that each part of the UI functions correctly and provides the intended user experience. To view details of the implemented tests, please refer to the [FE Repository](https://github.com/DimWebDev/thought-to-note-lite-fe/tree/develop).
- **Mocked API Calls**: To isolate frontend testing from the backend, API interactions were mocked. This allows the UI to be tested independently of the backend, ensuring a seamless user experience even in testing environments. For more information, refer to the [FE Repository](https://github.com/DimWebDev/thought-to-note-lite-fe/tree/develop).



## Key Design Choices

1. **Separation of Concerns**: The application maintains a strict separation of concerns by hosting the frontend and backend in independent repositories. This design choice makes both services independently deployable, scalable, and maintainable. It also allows teams to work on either service without disrupting the other, enabling smoother parallel development.

2. **Layered Backend Architecture**: The backend is designed using a layered architecture, where each layer has a specific responsibility:
   - **Controller Layer**: Manages incoming HTTP requests, processes them, and returns responses to the client.
   - **Service Layer**: Contains the business logic of the application, providing a layer of abstraction between the controllers and the repositories.
   - **Repository Layer**: Handles data access and interaction with the PostgreSQL database.

   This approach makes the backend easier to maintain, extend, and test, as each layer is responsible for a distinct aspect of the application's functionality.

3. **Custom Hook for State Management**: On the frontend, a custom hook (`useNotes`) is used for managing the state related to notes. This hook abstracts API calls and state logic from the UI components, which leads to a cleaner, more modular codebase. This modular approach improves code reusability and simplifies both testing and future modifications.

## API Summary

The **backend API** is well-documented using Swagger, which can be accessed when the backend is running by navigating to `http://localhost:8080/swagger-ui.html`. Swagger provides an interactive interface that helps developers explore and test the API endpoints, view example responses, and understand the request parameters.

To facilitate manual testing or integration with other tools, a **Postman Collection** (`postman_collection.json`) is included in the backend repository under the `api/` directory. This collection contains pre-configured API requests that make it easy to interact with the backend endpoints.

## Continuous Integration (CI) Pipeline Integration

In addition to the core architecture and functionality of **Thought-to-Note Lite**, we have implemented **Continuous Integration (CI) pipelines** for both the backend and frontend repositories. These pipelines are designed to automate the build, testing, and deployment processes, ensuring that the system remains robust and reliable as new code is introduced. This section explains how the CI pipelines work, their role in maintaining the codebase's integrity, and the specific GitHub Actions used for automating the development workflow.

## GitHub CI Pipeline Overview

GitHub Actions has been employed to manage the CI pipeline for both the backend and frontend repositories. These pipelines are triggered automatically on certain events, such as code pushes, pull requests, or manual dispatch, ensuring that code changes are tested and validated before being merged or deployed. By doing so, we maintain a high level of code quality and prevent bugs or regressions from reaching production.

### Backend CI Pipeline

The backend repository's CI pipeline is defined in the `.github/workflows/ci.yml` file and is designed to automate the build and testing of the backend's Java-based Spring Boot application. Key features of the backend pipeline include:

- **Java 21 Environment**: The pipeline uses GitHub Actions to set up a Java 21 environment with the Temurin distribution. This ensures compatibility and consistency across all build processes.
  
- **Maven Caching**: To speed up the build process, Maven dependencies are cached. This reduces the time required for each build by preventing repeated downloading of the same dependencies.

- **Automated Builds and Tests**: The pipeline installs necessary dependencies using Maven and runs unit tests. This is crucial for ensuring that new code does not introduce any failures in the existing system. JUnit and Mockito are used to perform unit testing for service classes and API controllers.

- **Test Results**: Although optional, the pipeline includes steps to upload test reports, which provide detailed feedback on the status of the tests. These reports can be useful for reviewing test results after a pipeline run.

### Frontend CI Pipeline

The frontend repository also has a dedicated CI pipeline, defined in the `.github/workflows/ci.yml` file. This pipeline ensures that the React-based frontend is properly tested and built with every new code change. Important features include:

- **Node.js 20 Environment**: The pipeline uses GitHub Actions to set up a Node.js 20 environment, ensuring that the latest stable version of Node.js is used during builds.

- **NPM Caching**: Similar to the backend pipeline, the frontend pipeline caches node modules. This speeds up dependency installation by reusing previously installed packages where possible.

- **Automated Testing**: The frontend CI pipeline runs tests using Jest and React Testing Library. These tests validate the functionality of the React components and hooks, ensuring that the UI behaves as expected. Mocked API calls are also tested to simulate interactions with the backend.

- **Test Coverage Reporting**: The pipeline collects test coverage reports, which provide insights into the extent of code covered by tests. These reports are uploaded as artifacts and can be reviewed to ensure comprehensive testing.

- **Build Process**: After the tests pass successfully, the application is built for production using the `npm run build` command. The build artifacts are then uploaded, ensuring they are readily available for deployment.

### Pipeline Triggers

Both pipelines are configured to trigger on a variety of events:

- **Push Events**: The pipelines are triggered automatically when code is pushed to certain branches (`main`, `develop`, and feature branches such as `feature/**`, `bugfix/**`, and `hotfix/**`). This ensures that all code changes are validated before they are merged into the main branch.
  
- **Pull Requests**: Pull requests targeting the same branches also trigger the CI pipeline. This allows developers to ensure that their changes are tested before the pull request is merged, preventing potential issues from being introduced into the codebase.

- **Manual Trigger**: The pipelines can also be triggered manually using the `workflow_dispatch` event. This provides flexibility to run the pipeline on-demand, for example, if a developer wants to re-run tests after making changes or after an external dependency has been updated.

### Benefits of CI Pipelines

The integration of CI pipelines into both repositories brings several key benefits:

- **Automated Testing**: Every time code is pushed or a pull request is made, the system automatically runs the tests, ensuring that the code remains functional and any new changes are properly validated.
  
- **Faster Development Cycle**: The caching of dependencies in both the backend and frontend pipelines helps to speed up build times. This ensures that developers receive feedback quickly, which is essential for maintaining a fast and efficient development cycle.

- **Early Bug Detection**: By running tests on every commit, we are able to catch bugs early in the development process. This reduces the likelihood of bugs making it into the production environment, saving time and effort in the long run.

The addition of GitHub CI pipelines in both the **backend** and **frontend** repositories of Thought-to-Note Lite represents a significant improvement in terms of development efficiency and code quality. These pipelines automate the crucial tasks of building, testing, and validating code, ensuring that new features or bug fixes do not introduce regressions or errors into the system. By employing best practices such as dependency caching, automated testing, and environment consistency, the pipelines contribute to the scalability and maintainability of the system, aligning with the broader goals of the application’s architecture.

## Branching Strategy

To ensure organized development and collaboration, **Thought-to-Note Lite** uses a branching strategy based on **Git Flow**. This approach allows smooth integration of new features, bug fixes, and urgent updates while maintaining code stability. Here's a concise breakdown of the strategy:

### Key Branches

1. **Main Branch (`main`)**:  
   Holds the production-ready code. Only stable, tested versions of the application are merged here. Direct development on `main` is avoided.

2. **Development Branch (`develop`)**:  
   The `develop` branch contains the latest working code with new features and fixes. It serves as the integration branch for ongoing development, representing the most up-to-date version of the project before it’s ready for release.

3. **Feature/Bugfix/Hotfix Branches**:  
   - **Feature Branches** (`feature/feature-name`): For developing new features. Merged into `develop` after testing.
   - **Bugfix Branches** (`bugfix/issue-description`): For fixing bugs in `develop`.
   - **Hotfix Branches** (`hotfix/issue-description`): For urgent production fixes. Merged into both `main` and `develop`.

### Workflow

- **feature/bugfix branches are created  from `develop`.**
- **After development and testing, pull requests are opened to merge changes into `develop`.**
- **Once `develop` is stable and tested, it is merged into `main` for production.**
- **Hotfix branches address critical production issues and are merged back into both `main` and `develop` to keep everything aligned.**


This branching strategy ensures an organized workflow, with the `develop` branch containing the most current working version of the code. The `main` branch is reserved for stable, production-ready releases, aligning with industry best practices for version control and team collaboration.

## Troubleshooting & Common Issues

1. **Docker Database Not Starting**:

   - Ensure Docker is installed and running properly. If Docker is not active, the PostgreSQL container will not start.
   - Use `docker ps` to confirm that the PostgreSQL container is active and running.
   - If issues persist, inspect container logs for any errors:
     ```bash
     docker logs <container-id>
     ```

2. **Maven Issues**:

   - Ensure that you are using the Maven Wrapper (`./mvnw`) provided in the repository. This avoids compatibility issues with different Maven versions.
   - Verify that the `JAVA_HOME` environment variable points to the correct Java version (Java 21). Incorrect Java versions can lead to compilation or runtime errors.

3. **CORS Issues During Development**:

   - If facing CORS issues when the frontend attempts to interact with the backend, ensure CORS is configured properly in the Spring Boot application. This can be done by adjusting the `WebMvcConfigurer` setup to allow requests from the frontend's origin.

4. **Frontend Not Loading**:

   - Verify that the backend is up and running properly since the frontend depends on it for data retrieval. Without the backend, the frontend will not function as intended.
   - Check the browser console for errors. This often provides valuable insights into any missing environment variables, incorrect API URLs, or other runtime issues that need to be addressed.

---

