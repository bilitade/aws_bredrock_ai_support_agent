# Customer Support AI Agent

Project 2 for the AWS AI/ML Scholarship "Future AWS Agent Engineer" Nanodegree.

An AI customer support agent for an e-commerce platform, built with the Strands Agents SDK
and deployed on Amazon Bedrock AgentCore Runtime.

## What it does

- **Order tracking and refunds** — looks up orders and processes refunds through tools
  exposed by an AgentCore Gateway (one backed by a REST API, one backed directly by Lambda).
- **Knowledge base search** — answers questions about products, policies, and the loyalty
  program using Retrieval Augmented Generation over a Bedrock Knowledge Base.
- **Long term memory** — remembers facts and preferences about a customer across separate
  sessions, using AgentCore Memory.
- **Loyalty discount calculation** — runs the points and discount math in a sandboxed
  AgentCore Code Interpreter instead of trusting inline arithmetic.
- **Web browsing** — can visit a live web page and read its content when asked.

## Project structure

```
main.py                  Agent entrypoint, tools, and memory hook
lambda/
  order_tracker.py        Order and customer lookup (Gateway API target)
  refund_processor.py     Refund tools (Gateway Lambda target)
  lambda_schema           Tool schema for the refund Lambda
product_catalog.txt       Source content for the Knowledge Base
pyproject.toml            Dependencies and Python version
```

## Configuration

`main.py` needs four values for your own AWS setup, near the top of the file:

| Variable      | What it is                                  |
|---------------|----------------------------------------------|
| `GATEWAY_URL` | AgentCore Gateway MCP endpoint                |
| `KB_ID`       | Bedrock Knowledge Base ID                     |
| `REGION`      | AWS region                                    |
| `MEMORY_ID`   | AgentCore Memory resource ID                  |

## Running locally

```bash
uv run main.py '{"prompt": "Hello", "customer_id": "CUST-123", "session_id": "s1"}'
```

## Deploying

```bash
agentcore configure --entrypoint main.py --name customer_support_agent
agentcore deploy
```

If Docker isn't available or the default cloud build fails, build locally instead:

```bash
agentcore deploy --local-build
```

## Testing the deployed agent

```bash
agentcore invoke '{"prompt": "Can you track order ORD-001?", "customer_id": "CUST-123", "session_id": "t1"}'
```

## Cleanup

```bash
agentcore destroy
```

Then remove the Gateway, Memory, Knowledge Base, OpenSearch Serverless collection, S3
bucket, API Gateway, and Lambda functions from the AWS console to avoid ongoing charges.
