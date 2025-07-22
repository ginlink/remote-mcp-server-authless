# Building a Remote MCP Server on Cloudflare (Without Auth)

This example allows you to deploy a remote MCP server that doesn't require authentication on Cloudflare Workers. 

## Get Started

[![Deploy to Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ginlink/remote-mcp-server-authless)

This will deploy your MCP server to a URL like: `remote-mcp-server-authless.<your-account>.workers.dev/sse`

Alternatively, you can use the command line below to create the remote MCP server on your local machine:
```bash
npm create cloudflare@latest -- my-mcp-server --template=https://github.com/ginlink/remote-mcp-server-authless
```

## Customizing your MCP Server

To add your own [tools](https://developers.cloudflare.com/agents/model-context-protocol/tools/) to the MCP server, define each tool inside the `init()` method of [`src/index.ts`](src/index.ts) using `this.server.tool(...)`. 

Here's an example of how to add a simple calculator tool:

```typescript
this.server.tool(
  "add",
  { a: z.number(), b: z.number() },
  async ({ a, b }) => ({
    content: [{ type: "text", text: String(a + b) }],
  })
);
``` 

## Connect to Cloudflare AI Playground

You can connect to your MCP server from the Cloudflare AI Playground, which is a remote MCP client:

1. Go to https://playground.ai.cloudflare.com/
2. Enter your deployed MCP server URL (`remote-mcp-server-authless.<your-account>.workers.dev/sse`)
3. You can now use your MCP tools directly from the playground!

## Connect Claude Desktop to Your MCP Server

You can also connect to your remote MCP server from local MCP clients by using the [mcp-remote proxy](https://www.npmjs.com/package/mcp-remote). 

To connect to your MCP server from Claude Desktop, follow [Anthropic's Quickstart guide](https://modelcontextprotocol.io/quickstart/user) and within Claude Desktop go to **Settings > Developer > Edit Config**.

Update the configuration with the following:

```json
{
  "mcpServers": {
    "calculator": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "remote-mcp-server-authless.<your-account>.workers.dev/sse"
      ]
    }
  }
}
```

**Note:** Replace `<your-account>` with your actual Cloudflare Workers account subdomain. For local development, you can use `http://localhost:8787/sse` instead.

Restart Claude Desktop and you should see the calculator tools become available in your conversations! 
