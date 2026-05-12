# Part 3: Prompt Preparation

# Selected Pull Request

Repository: FoundationAgents — MetaGPT

Repository Link:  
https://github.com/FoundationAgents/MetaGPT

Selected PR:  
feat(bedrock): Temporary AWS credentials via env vars + supported models update (#1450)

PR Link:  
https://github.com/FoundationAgents/MetaGPT/pull/1450

---

# 3.1.1 Repository Context

MetaGPT is an open-source multi-agent AI framework designed to automate software development workflows using collaboration between multiple AI agents. Instead of relying on a single AI model to handle all tasks, the framework assigns different responsibilities to specialized agents such as Product Manager, Architect, Engineer, and QA. These agents communicate with each other and work together to complete complex software-related tasks.

The repository is mainly written in Python and supports integration with multiple large language model providers including OpenAI and Amazon Bedrock. It includes features such as asynchronous task execution, configurable workflows, memory handling, and provider abstraction layers that allow developers to switch between AI services more easily.

The intended users of MetaGPT are AI researchers, software developers, and organizations interested in building autonomous AI systems or experimenting with collaborative agent-based workflows. The repository is especially useful for developers who want to automate software engineering processes or study how multiple AI agents can coordinate together.

The project mainly addresses the problem of AI orchestration and workflow automation. It provides a structured environment where multiple AI agents can cooperate on tasks instead of depending on a single monolithic AI system. The repository combines AI workflows, automation, and software engineering concepts into a single collaborative framework.

---

# 3.1.2 Pull Request Description

This pull request improves the Amazon Bedrock integration inside MetaGPT by adding support for temporary AWS credentials through environment variables and updating the list of supported Bedrock foundation models. Before this update, users had to manually place AWS credentials such as access keys and secret keys inside the MetaGPT configuration file. This approach was less convenient and created problems for users working with temporary AWS sessions or cloud-based deployment environments.

The PR changes the authentication workflow so that the Bedrock provider can automatically read credentials from standard AWS environment variables including `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`, and `AWS_DEFAULT_REGION`. This follows standard AWS authentication practices and simplifies deployment for developers already using AWS CLI tools or cloud infrastructure.

The update also adds support for newer Bedrock models such as Titan Text Premier, Jamba-Instruct, Command R/R+, and Mistral Large 2. Older or deprecated models were removed to reduce compatibility problems and maintenance overhead. In addition, token limits and provider configurations were updated to improve request handling accuracy. Streaming support was also added for Jamba-Instruct models to improve runtime response generation for larger outputs.

Overall, the PR makes the Bedrock integration easier to use, easier to maintain, and more compatible with current AWS services.

---

# 3.1.3 Acceptance Criteria

✓ When AWS credentials are available through environment variables, the Bedrock provider should authenticate successfully without requiring credentials inside the MetaGPT configuration file.

✓ The implementation should correctly support temporary AWS session credentials using `AWS_SESSION_TOKEN`.

✓ When a supported Bedrock model is selected, the provider should initialize successfully and send requests without configuration errors.

✓ The implementation should reject deprecated or unsupported Bedrock models with a meaningful validation message.

✓ Streaming responses for Jamba-Instruct models should work correctly without interrupting response generation.

✓ Updated token limits and model metadata should be applied correctly for newly added Bedrock models.

✓ When AWS environment variables are missing or invalid, the system should return a clear authentication or configuration error.

✓ Existing Bedrock configuration methods should continue to work without breaking backward compatibility.

---

# 3.1.4 Edge Cases

## Edge Case 1: Missing AWS Environment Variables

The provider should handle situations where one or more required AWS environment variables are missing and return a meaningful authentication error instead of failing silently.

---

## Edge Case 2: Expired Temporary Session Tokens

Temporary AWS credentials may expire during runtime. The implementation should correctly detect expired session tokens and provide a clear error response.

---

## Edge Case 3: Unsupported Bedrock Models

If a user selects an unsupported or deprecated Bedrock model, the provider should reject the request and display an appropriate validation message.

---

## Edge Case 4: Incorrect Token Limits

Large prompts or generated outputs may exceed token limitations. The implementation should correctly validate token limits to avoid runtime failures.

---

## Edge Case 5: Streaming Interruptions

During streaming generation for Jamba-Instruct models, the provider should handle interrupted or incomplete streaming responses gracefully without crashing the application.

---

# 3.1.5 Initial Prompt

You are working on the MetaGPT repository, a Python-based multi-agent AI framework that supports multiple large language model providers. The repository already contains integration support for Amazon Bedrock through a dedicated provider layer.

Your task is to improve the Amazon Bedrock integration by adding support for temporary AWS credentials through environment variables and updating the supported Bedrock model configurations.

Currently, AWS credentials must be manually configured inside the MetaGPT configuration file. Modify the implementation so the provider can automatically read credentials from the following AWS environment variables:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_SESSION_TOKEN`
- `AWS_DEFAULT_REGION`

The implementation should support temporary AWS session credentials and remain backward compatible with the existing configuration method.

Update the Bedrock model registry and provider configuration to support newer models including:

- Titan Text Premier
- Jamba-Instruct
- Command R
- Command R+
- Mistral Large 2

Deprecated or unsupported models should either be removed or validated properly with meaningful error messages.

Additional implementation requirements:

- Update token limits and model metadata
- Add streaming response support for Jamba-Instruct models
- Improve provider validation and error handling
- Ensure unsupported models return clear validation errors
- Maintain compatibility with existing Bedrock functionality

Acceptance criteria to satisfy:

- Environment-based authentication should work correctly
- Temporary AWS credentials should authenticate successfully
- New Bedrock models should initialize without errors
- Deprecated models should return validation warnings or errors
- Streaming functionality should work correctly for supported models

Edge cases to consider during implementation:

- Missing AWS environment variables
- Expired AWS session tokens
- Invalid model identifiers
- Streaming interruptions
- Requests exceeding token limits

Testing requirements:

- Add or update unit tests for environment-based credential loading
- Validate Bedrock model initialization behavior
- Test streaming functionality for Jamba-Instruct models
- Verify authentication failure scenarios
- Ensure existing Bedrock features continue working without regressions

---

# Relevant Repository References

- Repository:
  https://github.com/FoundationAgents/MetaGPT

- PR Reference:
  https://github.com/FoundationAgents/MetaGPT/pull/1450

- Provider implementation:
  https://github.com/FoundationAgents/MetaGPT/tree/main/metagpt/provider

---

# Integrity Declaration

"I declare that all written content in this assessment is my own work, created without the use of AI language models or automated writing tools. All technical analysis and documentation reflects my personal understanding and has been written in my own words."
