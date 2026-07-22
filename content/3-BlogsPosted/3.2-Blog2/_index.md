---
title: "New Amazon Bedrock AgentCore Features for Building Smarter AI Agents"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---


While exploring articles on the **AWS Machine Learning Blog**, I came across the article **"New in Amazon Bedrock AgentCore: Build agents with broader knowledge and continuous learning."** At first, I thought it was simply another update that introduced a few additional features to Amazon Bedrock AgentCore. However, after reading the article, I realized that AWS is aiming for much more than adding new capabilities. Their goal is to build a complete platform that enables AI agents to work more effectively in enterprise environments.

What impressed me most is that the article does not introduce a brand-new AI model. Instead, it focuses on solving the challenges that many AI agents still face today. According to AWS, even a powerful language model cannot perform at its full potential if it lacks access to the right knowledge, real-time information, or mechanisms for continuous improvement after deployment. To address these issues, AWS has introduced several new AgentCore capabilities that allow AI agents to access broader knowledge, learn continuously from production data, and operate more securely.

---

## Overview

The article introduces several new capabilities added to **Amazon Bedrock AgentCore**, AWS's platform for building, deploying, and optimizing AI agents.

Rather than focusing only on Large Language Models (LLMs), AgentCore has been expanded in several directions to help AI agents perform better in real-world production environments. The latest updates mainly focus on four areas:

- Expanding access to knowledge sources.
- Supporting continuous learning and optimization.
- Strengthening security and governance.
- Simplifying AI agent development and deployment.

From my perspective, AWS is positioning AgentCore as a complete platform for enterprise AI agents instead of simply providing infrastructure for running AI models.

---

## Why Is AWS Continuing to Expand AgentCore?

According to the article, modern Large Language Models already demonstrate impressive reasoning, planning, and content generation capabilities. However, many AI agents still struggle to deliver reliable performance in practical business scenarios.

The main limitation is not the AI model itself but the lack of contextual information.

For example, a customer support agent cannot provide accurate answers without access to internal company documents. Similarly, a market research agent cannot generate complete reports if it only relies on pre-trained knowledge. A financial assistant also requires access to real-time market information or premium financial data to provide better recommendations.

These examples helped me realize that the effectiveness of an AI agent depends not only on the intelligence of the model but also on the quality of the knowledge and tools it can access.

According to AWS, an enterprise AI agent should have three essential capabilities:

- Access to appropriate knowledge sources.
- The ability to use external tools and services.
- Continuous monitoring and improvement after deployment.

These ideas form the foundation of the latest AgentCore enhancements.

---

## AgentCore Introduces Three Knowledge Layers for AI Agents

One of the most interesting parts of the article is that AWS divides an AI agent's knowledge into three different layers. This allows AI agents to go beyond pre-trained knowledge and leverage multiple information sources when making decisions.

### Internal Knowledge Layer (Managed Knowledge Base)

The first layer is **Amazon Bedrock Managed Knowledge Base**.

According to AWS, valuable enterprise data is often distributed across services such as SharePoint, Google Drive, Confluence, or Amazon S3. Traditionally, developers needed to build their own Retrieval-Augmented Generation (RAG) systems, manage vector databases, and optimize data retrieval pipelines.

With AgentCore, AWS manages most of these tasks automatically. Developers only need to connect their data sources, while AgentCore handles indexing, retrieval, ranking, and search optimization. In my opinion, this significantly reduces development effort and enables AI agents to generate responses based on enterprise knowledge instead of relying only on pre-trained models.

### Global Knowledge Layer (Web Search)

The second layer is **Web Search**.

While internal knowledge helps AI agents understand enterprise information, Web Search enables them to access the latest information available on the Internet.

According to AWS, Web Search is built on Amazon's search infrastructure and Knowledge Graph technology, allowing agents to retrieve verified information such as company profiles, stock prices, and current events.

What I found particularly useful is that the entire search process remains within the AWS ecosystem. This improves security while allowing AI agents to access up-to-date information without relying on multiple third-party services.
### Premium Knowledge Layer (AgentCore Identity and Payment)

The final knowledge layer introduced by AWS is **AgentCore Identity and Payment**.

According to the article, not every valuable data source is freely available. In many industries such as finance, scientific research, and market analysis, high-quality information is often protected behind paid APIs or subscription-based services. Without access to these resources, AI agents may not be able to generate complete or accurate responses.

To address this challenge, AgentCore introduces a mechanism that allows AI agents to authenticate their identities, access paid services, and perform secure payment transactions during execution. At the same time, AWS provides content providers with controls that determine how AI agents can access their services, creating a trusted ecosystem for both developers and data providers.

In my opinion, this is an important improvement because it enables AI agents to work with professional and premium data sources instead of relying solely on publicly available information.

**Figure 1. The Three Knowledge Layers of Amazon Bedrock AgentCore**

![The Three Knowledge Layers of Amazon Bedrock AgentCore](/images/agentcore-knowledge-layers.png)

*Figure 1. AgentCore expands AI capabilities through three knowledge layers: enterprise knowledge, web knowledge, and premium knowledge sources.*

---

## AI Agents Can Learn Continuously

Another feature that impressed me is that AWS not only helps AI agents access more knowledge but also provides tools that continuously improve their performance after deployment.

According to AWS, many AI agent failures are difficult to detect because they do not always generate obvious system errors. For example, an agent may claim that an action has been completed even though it failed, or it may return an incorrect answer when an external API encounters an issue. These problems are often discovered only after users report them or after they have affected many production sessions.

To solve this problem, AgentCore introduces analytics capabilities that collect and evaluate operational data from real-world usage. The platform can identify repeated failures, analyze user intentions, monitor how agents perform individual tasks, and highlight areas that require improvement. Based on these observations, AgentCore can recommend prompt modifications, suggest improvements for tool descriptions, and support large-scale evaluations as well as A/B testing before deploying updates to production.

From my perspective, this data-driven approach makes AI agent optimization much more effective. Instead of relying on trial and error, developers can continuously improve their agents using real operational insights.

**Figure 2. The Continuous Improvement Workflow in AgentCore**

![AgentCore Continuous Improvement Workflow](/images/agentcore-optimization-loop.png)

*Figure 2. AgentCore analyzes production data, recommends improvements, and evaluates changes before deployment.*

---

## AWS Continues to Strengthen Security

As AI agents gain access to more enterprise data and perform increasingly complex tasks, security becomes even more important.

In this update, AWS integrates **Amazon Bedrock Guardrails** directly into AgentCore. These guardrails help control agent behavior, detect prompt injection attacks, filter harmful content, and reduce the risk of exposing sensitive business information.

In addition, AgentCore is designed to integrate with security solutions from partners such as Check Point, Zscaler, Rubrik, Netskope, and SentinelOne. This allows organizations to apply consistent security policies even when AI agents interact with multiple data sources and external tools.

In my opinion, these security enhancements are essential because more capable AI agents also require stronger governance and protection mechanisms.

---

## AgentCore Harness Simplifies AI Agent Development

Another capability introduced in the article is **AgentCore Harness**.

Instead of building the entire AI agent lifecycle from scratch, developers only need to define their AI model, available tools, skills, and operational instructions through configuration.

AgentCore then automatically provisions the runtime environment, manages memory across user sessions, coordinates tool execution, maintains agent state, and provides many built-in components required for production workloads.

I believe this significantly reduces development complexity, especially for teams that are just starting to build Generative AI applications. As applications grow, developers can continue using the same platform without redesigning the entire architecture.

---

## Lessons Learned

After reading the article, I learned several important points:

- A powerful AI model alone is not enough. AI agents also require access to the right knowledge and tools.
- Combining enterprise knowledge, web information, and premium data sources provides richer context for decision-making.
- Monitoring production performance and continuously improving AI agents are essential for maintaining high-quality systems.
- Security mechanisms such as Amazon Bedrock Guardrails play a critical role when AI agents interact with enterprise data and external services.
- Amazon Bedrock AgentCore simplifies the development, deployment, and scaling of enterprise AI agents.

---

## Conclusion

After reading this article, I realized that Amazon Bedrock AgentCore is much more than a collection of new features. AWS is building a complete platform for enterprise AI agents by expanding access to knowledge, enabling real-time information retrieval, supporting continuous learning, and strengthening security.

In my opinion, this article is very valuable for anyone interested in **Generative AI**, **Amazon Bedrock**, or enterprise AI agent development because it provides a clear understanding of the key components required to build AI agents that can operate effectively in real-world production environments rather than simply generating responses.

---

## References

- AWS Machine Learning Blog: *New in Amazon Bedrock AgentCore: Build agents with broader knowledge and continuous learning*
- https://aws.amazon.com/blogs/machine-learning/new-in-amazon-bedrock-agentcore-build-agents-with-broader-knowledge-and-continuous-learning/