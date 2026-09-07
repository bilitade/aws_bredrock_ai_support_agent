# Reflection

For the loyalty discount tool, I chose to run the calculations through the AgentCore Code
Interpreter instead of computing them directly in Python. Money math should be easy to trust
and check, so running it as a small script in a sandbox keeps it visible and separate from
the rest of the code.

The hardest problem was long term memory across two sessions. The agent always remembered a
customer's preference, but never their name. I assumed it was a model limitation at first.
After adding debug logging, I found the real cause. The function that builds the memory
namespace lookup was keying its results by strategy type, but both memory strategies in this
project report the same generic type. One strategy was silently overwriting the other, so
the memory holding the name was never searched. Keying the lookup by strategy name instead
fixed it.

On the production side, IAM permissions stood out the most. The execution role only had
access to what the deployment tool set up automatically. Since this project uses an existing
Memory, Knowledge Base, and Browser resource, I had to manually grant access to those
specific resources. It is a good reminder that in production an agent should only reach the
exact data and services it needs. That limits the damage if something goes wrong with the
agent or its credentials.
