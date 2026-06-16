# API Workflow Guide

This guide demonstrates how to create and execute workflows using the Alethic-ISM API.

> **Note**: The primary interface for Alethic-ISM is the **Alethic Studio UI**. This guide covers direct API usage for automation, integration, or headless operation.

---

## Prerequisites

All API requests require a JWT token in the Authorization header:

```
Authorization: Bearer <jwt_token>
```

To obtain a token, authenticate via the user endpoints (see [Authentication](#authentication)).

---

## Table of Contents

1. [Authentication](#authentication)
2. [Create a Project](#create-a-project)
3. [Create States](#create-states)
4. [Create Templates](#create-templates)
5. [Create a Processor](#create-a-processor)
6. [Create Routes](#create-routes)
7. [Execute Workflow](#execute-workflow)
8. [Check Status](#check-status)
9. [Retrieve Results](#retrieve-results)

---

## Authentication

### Local Authentication

```bash
POST /api/v1/user/basic
Content-Type: application/json

{
  "email": "researcher@example.com",
  "name": "Research User",
  "credentials": "your-password"
}
```

**Response** (check `Authorization` header for JWT):

```json
{
  "user_id": "abc123-def456-...",
  "email": "researcher@example.com",
  "name": "Research User",
  "tier_id": "TIER1"
}
```

Extract the JWT from the response header:
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

---

## Create a Project

Projects isolate workflows, states, and processors.

```bash
POST /api/v1/project/create
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "project_name": "Ethics Evaluation Study",
  "user_id": "abc123-def456-..."
}
```

**Response**:

```json
{
  "project_id": "proj-789xyz-...",
  "project_name": "Ethics Evaluation Study",
  "user_id": "abc123-def456-...",
  "created_date": "2025-01-15T10:30:00Z"
}
```

---

## Create States

States hold data. You need at least an **input state** and an **output state**.

### Input State (with data)

```bash
POST /api/v1/state/create
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "id": null,
  "project_id": "proj-789xyz-...",
  "state_type": "StateConfig",
  "config": {
    "name": "Ethics Questions",
    "primary_key": [
      {"name": "question_id", "required": true}
    ]
  },
  "columns": {
    "question_id": {
      "name": "question_id",
      "data_type": "str",
      "required": true
    },
    "scenario": {
      "name": "scenario",
      "data_type": "str",
      "required": true
    },
    "question": {
      "name": "question",
      "data_type": "str",
      "required": true
    }
  },
  "data": {
    "question_id": {
      "values": ["q1", "q2", "q3"]
    },
    "scenario": {
      "values": [
        "A self-driving car must choose between two harmful outcomes.",
        "A doctor has one dose of medicine and two patients.",
        "An AI system detects potential fraud but with uncertainty."
      ]
    },
    "question": {
      "values": [
        "What ethical framework should guide this decision?",
        "How should scarce resources be allocated fairly?",
        "When should AI defer to human judgment?"
      ]
    }
  },
  "count": 3
}
```

### Output State (StateConfigLM for LLM execution)

```bash
POST /api/v1/state/create
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "id": null,
  "project_id": "proj-789xyz-...",
  "state_type": "StateConfigLM",
  "config": {
    "name": "Ethics Analysis Results",
    "primary_key": [
      {"name": "question_id", "required": true}
    ],
    "user_template_id": "<template_id>",
    "system_template_id": null,
    "flag_query_state_inheritance_all": true,
    "flag_include_prompts_in_state": false
  },
  "columns": {
    "question_id": {
      "name": "question_id",
      "data_type": "str",
      "required": true
    },
    "scenario": {
      "name": "scenario",
      "data_type": "str",
      "required": true
    },
    "question": {
      "name": "question",
      "data_type": "str",
      "required": true
    },
    "response": {
      "name": "response",
      "data_type": "str",
      "required": true
    }
  }
}
```

**Key Configuration Flags**:

| Flag | Purpose |
|------|---------|
| `flag_query_state_inheritance_all` | Copy all input columns to output |
| `flag_include_prompts_in_state` | Store rendered prompts in output |
| `flag_auto_save_output_state` | Auto-persist results |
| `flag_auto_route_output_state` | Auto-forward to downstream processors |

---

## Create Templates

Templates use **Mako syntax** for variable substitution.

### User Template (Prompt)

```bash
POST /api/v1/template/create
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "template_id": null,
  "template_path": "ethics_analysis_prompt",
  "template_type": "mako",
  "project_id": "proj-789xyz-...",
  "template_content": "You are an ethics researcher specializing in applied ethics and normative reasoning.\n\nScenario:\n${scenario}\n\nQuestion:\n${question}\n\nProvide a structured analysis that:\n1. Identifies the key ethical considerations\n2. Applies relevant ethical frameworks (deontological, consequentialist, virtue ethics)\n3. Discusses potential tradeoffs\n4. Recommends an approach with justification\n\nRespond in JSON format:\n{\n  \"frameworks_applied\": [...],\n  \"key_considerations\": [...],\n  \"tradeoffs\": [...],\n  \"recommendation\": \"...\",\n  \"justification\": \"...\"\n}"
}
```

**Response**:

```json
{
  "template_id": "tmpl-abc123-...",
  "template_path": "ethics_analysis_prompt",
  "template_type": "mako",
  "project_id": "proj-789xyz-...",
  "template_content": "..."
}
```

### Mako Template Syntax

| Syntax | Purpose | Example |
|--------|---------|---------|
| `${variable}` | Variable substitution | `${question}` |
| `% for item in items:` | Loop | `% for opt in options:` |
| `% endfor` | End loop | |
| `% if condition:` | Conditional | `% if score > 0.5:` |
| `% endif` | End conditional | |
| `${variable.get('key')}` | Dict access | `${data.get('name')}` |

---

## Create a Processor

### List Available Providers

```bash
GET /api/v1/project/{project_id}/provider/processors
Authorization: Bearer <jwt_token>
```

**Common Provider IDs**:

| Provider ID | Model |
|-------------|-------|
| `LANGUAGE/MODELS/OPENAI/GPT-4O` | GPT-4o |
| `LANGUAGE/MODELS/OPENAI/GPT-4O-MINI` | GPT-4o Mini |
| `LANGUAGE/MODELS/ANTHROPIC/CLAUDE-3-5-SONNET-LATEST` | Claude 3.5 Sonnet |
| `LANGUAGE/MODELS/ANTHROPIC/CLAUDE-4.0-OPUS-LATEST` | Claude 4 Opus |
| `LANGUAGE/MODELS/GOOGLE/GEMINI-1.5-PRO-001` | Gemini 1.5 Pro |
| `LANGUAGE/MODELS/OPENROUTER/OPENAI/GPT-4O` | GPT-4o via OpenRouter |
| `LANGUAGE/MODELS/OPENROUTER/ANTHROPIC/CLAUDE-3.5-SONNET` | Claude via OpenRouter |
| `CODE/EXECUTOR/PYTHON/PYTHON-EXECUTOR-1.0` | Python code execution |
| `DATA/TRANSFORMERS/MIXER/STATE-ONLINE-CROSS-JOIN-1.0` | Cross-join operation |

### Create Processor

```bash
POST /api/v1/processor/create
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "id": null,
  "name": "Ethics Analyzer - GPT-4o",
  "provider_id": "LANGUAGE/MODELS/OPENAI/GPT-4O",
  "project_id": "proj-789xyz-...",
  "status": "CREATED",
  "properties": {
    "temperature": 0.7,
    "maxTokens": 2048,
    "topP": 1.0,
    "requestDelay": 0,
    "maxBatchSize": 100,
    "maxBatchLimit": 1
  }
}
```

**Properties**:

| Property | Description | Default |
|----------|-------------|---------|
| `temperature` | Sampling temperature | 0.7 |
| `maxTokens` | Max output tokens | 2048 |
| `topP` | Nucleus sampling | 1.0 |
| `topK` | Top-k sampling | 0 |
| `requestDelay` | Delay between requests (ms) | 0 |
| `maxBatchSize` | Max rows per batch | 100 |
| `maxBatchLimit` | Rows per message | 1 |

**Response**:

```json
{
  "id": "proc-xyz789-...",
  "name": "Ethics Analyzer - GPT-4o",
  "provider_id": "LANGUAGE/MODELS/OPENAI/GPT-4O",
  "project_id": "proj-789xyz-...",
  "status": "CREATED",
  "properties": {...}
}
```

---

## Create Routes

Routes connect processors to states with direction (INPUT or OUTPUT).

### Create Input Route (State → Processor)

```bash
POST /api/v1/processor/state/route
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "processor_id": "proc-xyz789-...",
  "state_id": "<input_state_id>",
  "direction": "INPUT",
  "status": "CREATED"
}
```

### Create Output Route (Processor → State)

```bash
POST /api/v1/processor/state/route
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "processor_id": "proc-xyz789-...",
  "state_id": "<output_state_id>",
  "direction": "OUTPUT",
  "status": "CREATED"
}
```

**Note**: Update the output state's `user_template_id` to reference the template you created:

```bash
POST /api/v1/state/create
# Update the output state with template reference
{
  "id": "<output_state_id>",
  ...
  "config": {
    ...
    "user_template_id": "tmpl-abc123-..."
  }
}
```

---

## Execute Workflow

### Execute Full State (All Rows)

```bash
POST /api/v1/processor/state/route/{route_id}
Authorization: Bearer <jwt_token>
```

Where `route_id` is the INPUT route ID (format: `<state_id>:<processor_id>`).

**Response**:

```json
{
  "status": "published",
  "message_id": "msg-123..."
}
```

### Execute Single Entry

```bash
POST /api/v1/state/{state_id}/forward/entry
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "question_id": "q4",
  "scenario": "A company discovers their AI has developed unexpected biases.",
  "question": "What remediation steps should be taken?"
}
```

### Execute with Session (Multi-Turn Conversation)

```bash
POST /api/v1/state/{state_id}/forward/entry
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "session_id": "session-abc123",
  "source": "user-456",
  "input": "Can you elaborate on the deontological perspective?",
  "question_id": "q1"
}
```

---

## Check Status

### Check Processor Status

```bash
GET /api/v1/processor/{processor_id}
Authorization: Bearer <jwt_token>
```

**Response**:

```json
{
  "id": "proc-xyz789-...",
  "status": "COMPLETED",
  ...
}
```

### Check Route Status

```bash
GET /api/v1/project/{project_id}/processor/states
Authorization: Bearer <jwt_token>
```

**Status Codes**:

| Status | Meaning |
|--------|---------|
| `CREATED` | Route created, not executed |
| `ROUTE` | Execution requested |
| `ROUTED` | Data sent to processor |
| `QUEUED` | Processor received, waiting |
| `RUNNING` | Processing in progress |
| `COMPLETED` | Successfully finished |
| `FAILED` | Error occurred |
| `TERMINATE` | Manual termination requested |
| `STOPPED` | Execution stopped |

### View Execution Logs

```bash
GET /api/v1/monitor/{route_id}/logs
Authorization: Bearer <jwt_token>
```

---

## Retrieve Results

### Fetch State with Data

```bash
GET /api/v1/state/{state_id}?load_data=true&offset=0&limit=100
Authorization: Bearer <jwt_token>
```

**Response**:

```json
{
  "id": "<output_state_id>",
  "state_type": "StateConfigLM",
  "count": 3,
  "columns": {...},
  "data": {
    "question_id": {"values": ["q1", "q2", "q3"]},
    "scenario": {"values": [...]},
    "question": {"values": [...]},
    "response": {"values": [
      "{\"frameworks_applied\": [...], \"recommendation\": \"...\"}",
      "{\"frameworks_applied\": [...], \"recommendation\": \"...\"}",
      "{\"frameworks_applied\": [...], \"recommendation\": \"...\"}"
    ]}
  }
}
```

### Export State as Excel

```bash
GET /api/v1/state/{state_id}/export?chunk_size=1000
Authorization: Bearer <jwt_token>
```

Returns: Excel file download

---

## Complete Example: Ethics Evaluation Pipeline

```bash
#!/bin/bash
API_URL="http://localhost/api/v1"
TOKEN="your-jwt-token"

# 1. Create Project
PROJECT=$(curl -s -X POST "$API_URL/project/create" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"project_name": "Ethics Study", "user_id": "user-123"}')

PROJECT_ID=$(echo $PROJECT | jq -r '.project_id')

# 2. Create Input State
INPUT_STATE=$(curl -s -X POST "$API_URL/state/create" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "project_id": "'$PROJECT_ID'",
    "state_type": "StateConfig",
    "config": {"name": "Questions", "primary_key": [{"name": "id"}]},
    "columns": {
      "id": {"name": "id", "data_type": "str"},
      "question": {"name": "question", "data_type": "str"}
    },
    "data": {
      "id": {"values": ["q1"]},
      "question": {"values": ["What is justice?"]}
    },
    "count": 1
  }')

INPUT_STATE_ID=$(echo $INPUT_STATE | jq -r '.id')

# 3. Create Template
TEMPLATE=$(curl -s -X POST "$API_URL/template/create" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "template_path": "ethics_prompt",
    "template_type": "mako",
    "project_id": "'$PROJECT_ID'",
    "template_content": "Analyze this ethics question:\n\n${question}\n\nProvide a thoughtful response."
  }')

TEMPLATE_ID=$(echo $TEMPLATE | jq -r '.template_id')

# 4. Create Output State with Template
OUTPUT_STATE=$(curl -s -X POST "$API_URL/state/create" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "project_id": "'$PROJECT_ID'",
    "state_type": "StateConfigLM",
    "config": {
      "name": "Responses",
      "primary_key": [{"name": "id"}],
      "user_template_id": "'$TEMPLATE_ID'",
      "flag_query_state_inheritance_all": true
    },
    "columns": {
      "id": {"name": "id", "data_type": "str"},
      "question": {"name": "question", "data_type": "str"},
      "response": {"name": "response", "data_type": "str"}
    }
  }')

OUTPUT_STATE_ID=$(echo $OUTPUT_STATE | jq -r '.id')

# 5. Create Processor
PROCESSOR=$(curl -s -X POST "$API_URL/processor/create" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "GPT-4o Analyzer",
    "provider_id": "LANGUAGE/MODELS/OPENAI/GPT-4O",
    "project_id": "'$PROJECT_ID'",
    "status": "CREATED",
    "properties": {"temperature": 0.7, "maxTokens": 1024}
  }')

PROCESSOR_ID=$(echo $PROCESSOR | jq -r '.id')

# 6. Create Routes
# Input route
curl -s -X POST "$API_URL/processor/state/route" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "processor_id": "'$PROCESSOR_ID'",
    "state_id": "'$INPUT_STATE_ID'",
    "direction": "INPUT",
    "status": "CREATED"
  }'

# Output route
curl -s -X POST "$API_URL/processor/state/route" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "processor_id": "'$PROCESSOR_ID'",
    "state_id": "'$OUTPUT_STATE_ID'",
    "direction": "OUTPUT",
    "status": "CREATED"
  }'

# 7. Execute
ROUTE_ID="${INPUT_STATE_ID}:${PROCESSOR_ID}"
curl -s -X POST "$API_URL/processor/state/route/$ROUTE_ID" \
  -H "Authorization: Bearer $TOKEN"

echo "Workflow executing..."

# 8. Poll for completion
while true; do
  STATUS=$(curl -s "$API_URL/processor/$PROCESSOR_ID" \
    -H "Authorization: Bearer $TOKEN" | jq -r '.status')

  if [ "$STATUS" == "COMPLETED" ]; then
    echo "Completed!"
    break
  elif [ "$STATUS" == "FAILED" ]; then
    echo "Failed!"
    exit 1
  fi

  echo "Status: $STATUS"
  sleep 2
done

# 9. Get Results
curl -s "$API_URL/state/$OUTPUT_STATE_ID?load_data=true&offset=0&limit=100" \
  -H "Authorization: Bearer $TOKEN" | jq '.data'
```

---

## Cross-Join Example: Multi-Model Evaluation

Run the same questions across multiple models:

```bash
# Create models state
curl -s -X POST "$API_URL/state/create" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "project_id": "'$PROJECT_ID'",
    "state_type": "StateConfig",
    "config": {"name": "Models", "primary_key": [{"name": "model_id"}]},
    "columns": {
      "model_id": {"name": "model_id", "data_type": "str"},
      "provider_id": {"name": "provider_id", "data_type": "str"}
    },
    "data": {
      "model_id": {"values": ["gpt4o", "claude35", "gemini15"]},
      "provider_id": {"values": [
        "LANGUAGE/MODELS/OPENAI/GPT-4O",
        "LANGUAGE/MODELS/ANTHROPIC/CLAUDE-3-5-SONNET-LATEST",
        "LANGUAGE/MODELS/GOOGLE/GEMINI-1.5-PRO-001"
      ]}
    },
    "count": 3
  }'

# Create cross-join processor
curl -s -X POST "$API_URL/processor/create" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Questions x Models",
    "provider_id": "DATA/TRANSFORMERS/MIXER/STATE-ONLINE-CROSS-JOIN-1.0",
    "project_id": "'$PROJECT_ID'",
    "status": "CREATED"
  }'

# Connect: Questions (primary) x Models (secondary) → Combined State
# Then connect Combined State to individual model processors
```

This produces: `Questions (N) x Models (M) = N*M` evaluation runs.

---

## Error Handling

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| 401 Unauthorized | Invalid/expired JWT | Re-authenticate |
| 404 Not Found | Invalid ID | Check resource exists |
| 422 Validation Error | Invalid request body | Check required fields |
| 500 Internal Error | Server issue | Check logs |

### Check Failed Route

```bash
GET /api/v1/monitor/{route_id}/logs
Authorization: Bearer <jwt_token>
```

Returns exception details and input data context.

---

## Best Practices

1. **Use meaningful names** for states, processors, and templates
2. **Set appropriate batch sizes** for large datasets
3. **Monitor usage** to stay within tier limits
4. **Use sessions** for multi-turn conversations
5. **Export results** for offline analysis
6. **Version templates** by including version in template_path
