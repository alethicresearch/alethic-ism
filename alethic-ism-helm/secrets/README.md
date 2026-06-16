# Secrets Directory

This directory contains API keys and credentials for various processors.

## Required Files

Create the following files in this directory:

- `openai.api-key` - OpenAI API key
- `anthropic.api-key` - Anthropic API key
- `gemini.api-key` - Google Gemini API key
- `openrouter.api-key` - OpenRouter API key
- `firebase-credentials.json` - Firebase service account credentials (optional)

## Setup

Copy the sample files and add your actual API keys:

```bash
# Copy sample files and add your keys
cp openai.api-key.sample openai.api-key
cp anthropic.api-key.sample anthropic.api-key
cp gemini.api-key.sample gemini.api-key
cp openrouter.api-key.sample openrouter.api-key
cp firebase-credentials.json.sample firebase-credentials.json  # if using Firebase

# Edit each file with your actual credentials
# The .api-key files (without .sample) are gitignored
```

## Security

- All `.api-key` and credential files are gitignored
- Never commit actual API keys to version control
- Use environment variables or secret management systems in production
