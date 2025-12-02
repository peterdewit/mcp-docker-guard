# Docker Guard + MCP Wrapper

A minimal, safe Docker control layer plus a simple MCP wrapper for LLM use.

## Components
- **docker-guard**: safe API for listing/starting/stopping/restarting allowed containers.
- **docker-guard-mcp**: simple MCP server that exposes docker-guard to LLM agents (OpenWebUI, etc.)

## Running docker-guard
```
docker build -t docker-guard .
docker run -d \
  -p 8080:8080 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $PWD/settings.example.yaml:/app/settings.yaml \
  -e DOCKER_GUARD_API_KEY=yourkey \
  docker-guard
```

## Running MCP wrapper
```
cd mcp
docker build -t docker-guard-mcp .
docker run -d \
  -p 9999:9999 \
  -e DOCKER_GUARD_URL=http://docker-guard:8080 \
  -e DOCKER_GUARD_API_KEY=yourkey \
  docker-guard-mcp
```

## OpenWebUI MCP config
Place in:  `/path/to/openwebui/mcp/docker-guard-mcp.json`

```
{
  "name": "docker-guard-mcp",
  "url": "http://docker-guard-mcp:9999"
}
```
