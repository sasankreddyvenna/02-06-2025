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

MetaGPT is a multi-agent collaboration AI framework that automates software development workflows based on the collaboration of multiple AI agents, which is open source. The framework splits up the roles for different agents, like Product Manager, Architect, Engineer and the QA to work on different tasks, rather than using a single model for all of them. They are interconnected and collaborate to accomplish intricate software-related jobs.

The repository is primarily a Python-based repository and provides integration with various large language model providers such as OpenAI and Amazon Bedrock. It has capabilities like abstraction layers for AI services, asynchronous task execution, memory management and customisable workflows, offering developers greater flexibility in switching between AI services.

MetaGPT was designed for AI researchers, software developers, and companies looking to create independent AI platforms or explore collaborative agent-based processes. The repository can be particularly helpful for software development professionals seeking to streamline software engineering tasks or exploring the behavior of several AI agents working together.

The project mainly addresses the problem of AI orchestration and workflow automation. It offers a framework for organizing AI agents to work together to perform tasks, rather than relying on one all-encompassing AI system. AI workflows, automation and software engineering buzzwords are all in one collaborative repository.

---

# 3.1.2 Pull Request Description

MetaGPT integration of Amazon Bedrock by supporting temporary AWS credentials via environment variables and updating the Amazon Bedrock foundation model list. Prior to this update, users needed to manually enter AWS credentials (access key, secret key) into the MetaGPT configuration file. This was less convenient and caused issues with users executing deployments in a temporary AWS session, or a cloud-based deployment environment.

The PR simplifies the process of authenticating by allowing the Bedrock provider to automatically retrieve credentials from common AWS environment variables like `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`, and `AWS_DEFAULT_REGION`. It conforms to the AWS authentication paradigm and makes it easier for developers who are familiar with AWS CLI tools/cloud infrastructure.

The upgrade further includes added help for newer Bedrock models like Titan Text Premier, Jamba-Instruct, Command R/R+ and Mistral Large 2. Deprecated or older models are no longer supported to eliminate the issues of compatibility and maintenance. Other changes include enhanced request handling accuracy with updated token limits and provider configurations. Further, support for streaming was introduced in Jamba-Instruct models that helps with runtime response generation for larger outputs.

In summary, the PR simplifies the Bedrock integration for use, maintenance and integration with existing AWS services.

---

# 3.1.3 Acceptance Criteria

The Bedrock provider should be able to authenticate when credentials are provided via environment variables, and should not need to have credentials listed inside of the MetaGPT configuration file.

✓ Use of AWS_SESSION_TOKEN to implement temporary AWS session credentials should be correct.

✓ If a Bedrock model is chosen that is supported by them, the provider should be able to be initialized successfully and request without configuration errors.

Implementation should reject older or unsupported Bedrock models with something that is describing in a meaningful way why the model is deprecated or unsupported.

Streaming responses to Jamba-Instruct models should be correct and shouldn't disrupt the generation process.

You're required to use the new token limits and model metadata correctly for models added after the update.

AWS environment variables should return a clear authentication, or otherwise configuration error, if they are missing or are not valid.

Existing Bedrock configuration methods should continue to work, not compromising backward compatibility.

---

# 3.1.4 Edge Cases

The first edge case is when the AWS Environment Variables are missing.The first edge case is when the AWS Environment Variables are not available.

The provider should provide an authentication error if one or more AWS environment variables is absent, but not return no error message if they are not all present.

---

This article goes through steps for handling expired temporary session tokens.

Temporary AWS credentials can expire while running a service. The flow of session expiring should be correctly identified and an appropriate error message should be returned.

---

Models that are not supported by Bedrock cannot be used.Bedrock models cannot be used if they are not supported.

Where there is no standard Bedrock model that the provider supports, or if the provider's support was deprecated, then the user should be notified about the model and reject the submission with an appropriate validation message.

---

In this Edge Case, incorrect limits on tokens are used.Here the token limits are incorrect.

Generated large prompts/ outputs can be limited based on tokens. The implementation should properly check the token limits for valid results without crashing at runtime.

---

Edge Case 5: Streaming Interruptions

If the application is streaming with a Jamba-Instruct model, it should deal with break in and incomplete streaming replies without crashing.

---

# 3.1.5 Initial Prompt

You are collaborating with the MetaGPT repository, a Python multi-agent AI framework with support for various large language model libraries and services. The repository already has a dedicated provider layer for integration with Amazon Bedrock.

Your goals are to enhance the Amazon Bedrock integration and extend supporting Bedrock model configurations with support for temporary AWS credentials via environment variables.

At present, AWS credentials have to be hardcoded into the MetaGPT configuration file. Make the implementation compatible with the following AWS environment variables being automatically read from the environment by AWS:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_SESSION_TOKEN`
- `AWS_DEFAULT_REGION`

Implementation should be designed to accept temporary AWS session credentials and should be a backward compatible implementation of the existing configuration method.

Add more recent models to the Bedrock model registry/provider configuration:

- Titan Text Premier
- Jamba-Instruct
- Command R
- Command R+
- Mistral Large 2

Remove deprecated or unsupported models, or validate them in a meaningful way that eliminates results that are not models, with an error message.

Additional implementation requirements:

Increase the size of the token limits and the model metadata.
- Add support for streaming response to Jamba-Instruct models
Enhance provider validation and error handling.
- Make sure that unsupported models throw clear validation errors
- Forward-compatible with the Bedrock theme as it is.

Acceptance criteria to satisfy:

Environment based authentication should be accepted as it is all OK.
- Temporary AWS credentials should be successfully authenticated
No errors should be thrown when attempting to load the models of New Bedrock.
Warning or error information is required for deprecation of models
Supporting models should have streaming rightfully.

Some considerations for the edge cases when implementing:

- Missing AWS environment variables
- Expired AWS session tokens
- Invalid model identifiers
- Streaming interruptions
Requests with amount of tokens over the limit.


Testing requirements:

- Create or modify on-unit tests to load credentials.
- Avoid adding additional checks to the initialisation of Bedrock model.
- Test streaming capabilities for Jamba-Instruct models
- Check authentication failure situations
- Maintain ability to add new Bedrock limitations without bugs impacting prior functionality

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
