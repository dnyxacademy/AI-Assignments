# 3. Write an MCP Server

**Week 4 - Agents, MCP and shipping · Wed · Hard · about 4 hours**

## What you will practise

- MCP: hosts, clients and servers
- Tools, resources and prompts
- Writing a small server in Python

## Before you start

Finish 02. Workshop day. Install the MCP Python SDK before the session.

## The task

Yesterday your tools worked only inside your own script. MCP is the standard that lets any compatible host use them.

1. Understand the three parts and write one sentence each:
   - **host**: the application a person uses - an editor, a desktop assistant
   - **client**: the host's connector to one server
   - **server**: your code, exposing capabilities
2. Use an **existing** MCP server first - a filesystem or fetch server - connected to a host that supports MCP. Seeing one work makes writing one far easier.
3. Write your **own server** in Python exposing one genuinely useful tool from your project. The obvious choice is your RAG document lookup: that turns your month of work into something any MCP host can query.
4. Expose one **resource** as well as the tool, and write down the difference: a tool is something the model *does*; a resource is something it *reads*.
5. Connect your server to a real host and use it. Screenshot it working and commit the screenshot.
6. Handle errors properly. A tool that raises an unhandled exception breaks the host's conversation - return a clear error message instead.
7. Write a README for your server: what it exposes, how to configure it, what it needs. Someone else should be able to run it from your instructions alone.

Writing the server is the mark of the day, but step 2 first will save you hours of confusion.

## Example

A complete, working MCP server in about fifteen lines:

```python
# server.py
from mcp.server.fastmcp import FastMCP

mcp = FastMCP('agri-advisor')

@mcp.tool()
def lookup_guidance(query: str, top_k: int = 5) -> str:
    """Search the agricultural advisory documents.

    Use for questions about crops, irrigation, pests or soil.
    Returns the most relevant passages with their sources.
    """
    try:
        hits = collection.query(query_texts=[query], n_results=top_k)
    except Exception as e:
        return f'Search unavailable: {e}'          # a message, not a crash
    return '\n\n'.join(
        f"[{m['filename']} chunk {m['chunk']}] {d}"
        for d, m in zip(hits['documents'][0], hits['metadatas'][0])
    )

@mcp.resource('docs://list')
def list_documents() -> str:
    """The documents available to search."""
    return '\n'.join(sorted(set(m['filename'] for m in all_metadatas)))

if __name__ == '__main__':
    mcp.run()
```

The docstring is not a comment - it is what the model reads to decide whether to call your tool. Write it for the model, the way you would write an API doc for a colleague.

Registering it with a host:

```json
{
  "mcpServers": {
    "agri-advisor": { "command": "python", "args": ["/absolute/path/to/server.py"] }
  }
}
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] Host, client and server are each explained in one sentence
- [ ] An existing MCP server was connected to a host and used
- [ ] Your own server exposes at least one working tool
- [ ] A resource is exposed too, and the tool-against-resource difference is written down
- [ ] The server is connected to a real host, with a screenshot committed
- [ ] Tool errors return a message rather than raising
- [ ] A README lets someone else run your server from scratch

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Understanding** | All three parts explained correctly | Roughly right | Confused |
| **Your server** | Tool and resource both working in a real host | Tool works | Does not run |
| **Tool description** | Docstring precise enough to guide the model well | Present | Vague or missing |
| **Documentation** | Someone else could run it unaided | Partial | None |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [MCP - introduction](https://modelcontextprotocol.io/docs/getting-started/intro)
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)
- [Example MCP servers](https://github.com/modelcontextprotocol/servers)
