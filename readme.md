# This is a system design repo.

# Senarios

## 1.Logging pipeline(CloudSEK)

Problem:The future of DevOps relies heavily on AI-assisted engineering. For this challenge, we do not want you to write the code yourself. Instead, we want to see how you would direct an AI coding agent to build a complex, highly scalable system. We are testing your system design logic, your ability to break down complex problems, and your proficiency in prompt engineering.

The Scenario & Constraints

Assume you are a DevOps engineer tasked with building a log processing pipeline with the following strict requirements:

•

Traffic Profile: The system must accept a baseline of 5,000 DNS logs per second, but it must seamlessly handle massive, instantaneous spikes of up to 100,000 logs per second during a daily 10-minute sync window without dropping data or lagging.

•

Data Payload: The logs contain DNS requests, resolutions, source/destination IPs, and metadata.

•

Processing & Storage: You need to process, buffer, and index this data for querying.

•

Reporting Requirements: Generate a daily (24hr) report showing: the highest number of DNS resolutions, total resolution counts, and resolution origins. The report must be generated in 3 formats (.pdf, .html, .json).

•

Delivery & Longevity: These reports must be emailed to a set of users. The report attachments must remain accessible via links for months or years without expiring.

•

CRITICAL CONSTRAINT — Budget: You are operating under a strict infrastructure budget of $800/month for this specific pipeline.

Your Task

Create the instruction files (prompts) you would feed to an AI agent (like Gemini, Claude, or ChatGPT). You must break the problem down into a logical sequence of prompts. A single "do everything" prompt will fail. You must independently determine the architectural layers and decide how many files/prompts are necessary to successfully guide the AI.





