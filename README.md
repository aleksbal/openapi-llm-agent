# LLM function calls to APIs described in OpenAPI 

## Objective
Generates agent descriptions for LLMs based on an OpenAPI specification:

- Parses an OpenAPI 3.0.0 specification file.
- Converts all endpoints, services, and schemas into a simplified JSON based descriptions tailored for LLMs.
- Enables an LLM to understand APIs and invoke them effectively during conversations.

It depends on some key considerations:

A.  Current Capabilities of LLMs
Understanding OpenAPI specs: LLMs can read and interpret OpenAPI specs to some extent, particularly well-documented ones. However, the verbosity and technical depth of OpenAPI specs (e.g., nested schemas, polymorphism) can overwhelm the understanding of current models unless simplified.
LLM Limitations: LLMs are good at understanding simplified JSON structures, especially if these are formatted and annotated to prioritize clarity over technical precision.

B.  Simplified JSON Representation
A tailored JSON description could serve as an intermediate abstraction that focuses on key points:
Endpoint summaries, methods, and paths.
Input/output schemas.
Required headers/authentication mechanisms.
Examples of input and output payloads.
This makes the API more "digestible" for an LLM to understand and use in downstream applications.

C.  Use Case Viability
LLM-based integrations often fail because APIs are not well-documented in terms of use-case-oriented examples. This JSON abstraction can fix that by focusing on actionable examples.
Your idea could enable dynamic API invocation during conversations or tasks, particularly in systems like chatbots or automation tools.

## Challenges
- Complexity of OpenAPI Specs: Complex schemas, relationships, and edge cases in OpenAPI specs (like polymorphism or recursive schemas) can be tricky to translate into a simple JSON structure.
- LLM Fine-Tuning: To use this JSON effectively, the LLM may need specialized training or prompting strategies to understand the abstracted format.
- Error Handling: APIs often fail due to nuanced reasons (e.g., auth tokens, rate limits, malformed requests). Simplified representations might miss these operational details.

## Implementation Outline
Here’s a plan for how this could be developed:

A. Parse OpenAPI Spec
Load and parse the OpenAPI 3.0.0 file.
Extract all paths, methods, parameters, and request/response schemas.
Gather global information (e.g., authentication requirements, base URL).

B. Convert to Simplified JSON
Summarize endpoints in a human-readable way (e.g., GET /users -> "Retrieve user data").
Flatten request and response schemas into concise key-value mappings.
Annotate parameters and responses with examples.

C. Output Example

```json
{
  "api_name": "Example API",
  "base_url": "https://api.example.com",
  "authentication": "Bearer Token",
  "endpoints": [
    {
      "method": "GET",
      "path": "/users/{id}",
      "summary": "Retrieve a user's details by their ID.",
      "parameters": {
        "id": {
          "type": "string",
          "description": "The unique identifier for a user."
        }
      },
      "response": {
        "200": {
          "description": "User object",
          "example": {
            "id": "123",
            "name": "John Doe",
            "email": "john.doe@example.com"
          }
        }
      }
    }
  ]
}
```

D. LLM Prompt Design
Create templates for prompting the LLM to use the above JSON format to:
Understand API usage.
Generate API calls based on conversational inputs.
Process API responses and integrate them into the conversation.

## Would LLMs Understand This JSON?
Modern LLMs like GPT-4 or Claude can understand and work with such a JSON format. They are particularly adept at:

- Extracting the necessary details to construct requests.
- Understanding examples to infer required formats and parameters.
- Using concise and well-documented JSON structures.
- However, for real-time API invocation:

The LLM must be integrated with a runtime that executes API requests and handles dynamic responses.
Post-processing logic (outside the LLM) will be needed to interpret and manage responses effectively.

## Conclusion
This may be a feasible project. It addresses a gap in API usability for LLM-driven applications. However, for full usability, the LLM will need to:

- Be supported by a well-structured JSON schema.
- Leverage external runtime support for executing and validating API interactions.
