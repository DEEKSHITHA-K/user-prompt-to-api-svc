# User Prompt to API Service

A Spring Boot microservice that converts natural language prompts into API calls using OpenAI's GPT models. This project demonstrates the integration of LLMs with traditional REST API interactions.

## Overview

This service acts as a bridge between natural language queries and API calls. The workflow is:

1. **User submits a natural language query** → `/nl/ask` endpoint
2. **LLMService translates the query** → Sends prompt to OpenAI GPT
3. **OpenAI generates structured API request** → Returns JSON with endpoint, method, and params
4. **ApiService executes the API call** → Calls downstream service and returns response

## Architecture

### Components

- **NLController** (`NLController.java`)
    - REST endpoint: `POST /nl/ask`
    - Accepts natural language queries
    - Delegates to NLService

- **NLService** (`NLService.java`)
    - Orchestrates the query processing pipeline
    - Coordinates between LLMService and ApiService
    - Flow: Query → API Request → API Response

- **LLMService** (`LLMService.java`)
    - Translates natural language to structured API requests
    - Calls OpenAI Chat API with custom prompts
    - Extracts and parses JSON responses
    - Supported endpoints:
        - `GET /transactions/last5` (param: userId)
        - `GET /transactions/highest` (param: userId)

- **ApiService** (`ApiService.java`)
    - Executes API calls to downstream services
    - Currently supports GET requests
    - Base URL: `http://localhost:8081`

- **ApiRequest** (`ApiRequest.java`)
    - POJO representing a structured API request
    - Properties: `endpoint`, `method`, `params`

## Technologies

- **Java 17**
- **Spring Boot 4.0.5**
    - Spring Web MVC
    - Spring Data JPA
- **Spring AI 2.0.0-M3** (OpenAI integration)
- **OpenAI GPT-4o-mini** (LLM model)
- **Jackson** (JSON serialization/deserialization)
- **Lombok** (boilerplate reduction)
- **H2 Database** (in-memory)
- **Maven** (build tool)

## Prerequisites

- Java 17+
- Maven 3.6+
- OpenAI API key
- Downstream API service running on `http://localhost:8081`

## Setup & Configuration

### 1. Configure OpenAI API Key

Edit `src/main/resources/application.properties`:

```properties
spring.ai.openai.api-key=YOUR_OPENAI_API_KEY
```
### Git Repo Links
For more information, visit the parent project directory. [OpenAI-with-Java](https://github.com/DEEKSHITHA-K/OpenAI-with-java)
For the Spring Boot microservice providing REST APIs for transaction data queries [Transaction Service](https://github.com/DEEKSHITHA-K/transaction-svc)
