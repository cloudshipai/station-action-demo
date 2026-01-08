# Station Action Demo

Demo repository for testing the [Station GitHub Action](https://github.com/cloudshipai/station-action) with PostHog MCP integration.

## What This Demo Tests

1. **Local Environment Mode**: Run agents from `./environments/default/` in the repo
2. **PostHog MCP Integration**: Test agent execution with PostHog's MCP server
3. **Runtime Variable Injection**: Verify `POSTHOG_API_KEY` is properly templated into MCP config

## Setup

### Required Secrets

Add these secrets to your repository (Settings > Secrets and variables > Actions):

| Secret | Description |
|--------|-------------|
| `OPENAI_API_KEY` | OpenAI API key for agent execution |
| `POSTHOG_API_KEY` | PostHog API key (starts with `phx_`) |

### Running the Workflow

The workflow can be triggered:
- **Manually**: Go to Actions > "Test PostHog Agent" > Run workflow
- **On Push**: Any push to `main` triggers the workflow

## Project Structure

```
station-action-demo/
├── environments/
│   └── default/
│       ├── agents/
│       │   └── posthog-dashboard-reporter.prompt  # Agent definition
│       ├── posthog.json                            # PostHog MCP server config
│       └── variables.yml                           # Runtime variables (templated)
├── .github/
│   └── workflows/
│       └── test-posthog.yml                        # GitHub Actions workflow
└── README.md
```

## How It Works

1. **Station Action installs Station CLI** from the latest release
2. **Environment is loaded** from `./environments/default/`
3. **MCP config is templated** with `POSTHOG_API_KEY` at runtime
4. **Agent runs** and connects to PostHog MCP server
5. **Results are output** to the workflow logs

## Testing Different Scenarios

### 1. Local Environment (Default)
```yaml
- uses: cloudshipai/station-action@v1
  with:
    agent: 'posthog-dashboard-reporter'
    task: 'Give me a summary of my dashboards'
    environment: 'default'
  env:
    OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
    POSTHOG_API_KEY: ${{ secrets.POSTHOG_API_KEY }}
```

### 2. Bundle from URL
```yaml
- uses: cloudshipai/station-action@v1
  with:
    agent: 'posthog-dashboard-reporter'
    task: 'Give me a summary of my dashboards'
    bundle-url: 'https://example.com/my-bundle.tar.gz'
  env:
    OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
    POSTHOG_API_KEY: ${{ secrets.POSTHOG_API_KEY }}
```

### 3. Bundle from CloudShip Registry
```yaml
- uses: cloudshipai/station-action@v1
  with:
    agent: 'posthog-dashboard-reporter'
    task: 'Give me a summary of my dashboards'
    bundle-id: 'your-bundle-uuid'
    cloudship-api-key: ${{ secrets.CLOUDSHIP_API_KEY }}
  env:
    OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
    POSTHOG_API_KEY: ${{ secrets.POSTHOG_API_KEY }}
```

## Related

- [Station](https://github.com/cloudshipai/station) - AI agent runtime
- [Station Action](https://github.com/cloudshipai/station-action) - GitHub Action for Station
- [PostHog MCP](https://mcp.posthog.com) - PostHog's MCP server

## License

Apache 2.0
