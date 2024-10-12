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
7. [Troubleshooting & Common Issues](#troubleshooting--common-issues)

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

