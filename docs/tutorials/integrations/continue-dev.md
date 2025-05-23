---
sidebar_position: 13
title: "⚛️ Continue.dev VSCode Extension with Open WebUI"
---

:::warning
This tutorial is a community contribution and is not supported by the Open WebUI team. It serves only as a demonstration on how to customize Open WebUI for your specific use case. Want to contribute? Check out the contributing tutorial.
:::

# Integrating Continue.dev VSCode Extension with Open WebUI

### Download Extension

You can download the VSCode extension here on the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Continue.continue)

Once installed you should now have a 'continue' tab in the side bar. Open this.

Click on the Assistant selector above the main chat input. Then hover over "Local Assistant" and you should see a settings icon (looks like a cog).

Once you click on the settings icon, a `config.yaml` should open up in the editor.

Here you'll be able to configure continue to use Open WebUI.

---

Currently the 'ollama' provider does not support authentication so we cannot use this provider with Open WebUI.

However Ollama and Open WebUI both have compatibily with OpenAI API spec. You can see a blog post from Ollama [here](https://ollama.com/blog/openai-compatibility) on this.

We can still setup Continue to use the openai provider which will allow us to use Open WebUI's authentication token.

---

## Config

In `config.yaml` all you will need to do is add/change the following options.

### Change provider to openai

```yaml
provider: openai
```

### Add or update apiBase

Set this to your Open Web UI domain on the end.

```yaml
apiBase: http://localhost:3000/ #If you followed Getting Started Docker
```

### Add apiKey

```yaml
apiKey: sk-79970662256d425eb274fc4563d4525b # Replace with your API key
```

You can find and generate your api key from Open WebUI -> Settings -> Account -> API Keys

You'll want to copy the "API Key" (this starts with sk-)

## Example Config

Here is a base example of config.yaml using Open WebUI via an openai provider. Using Granite Code as the model.
Make sure you pull the model into your ollama instance/s beforehand.

```yaml
name: Local Assistant
version: 1.0.0
schema: v1
models:
  - name: Granite Code
    provider: openai
    model: granite-code:latest
    env:
      useLegacyCompletionsEndpoint: false
    apiBase: http://YOUROPENWEBUI/ollama/v1
    apiKey: sk-YOUR-API-KEY
    roles:
      - chat
      - edit

  - name: Model ABC from pipeline
    provider: openai
    model: PIPELINE_MODEL_ID
    env:
      useLegacyCompletionsEndpoint: false
    apiBase: http://YOUROPENWEBUI/api
    apiKey: sk-YOUR-API-KEY
    roles:
      - chat
      - edit

  - name: Granite Code Autocomplete
    provider: openai
    model: granite-code:latest
    env:
      useLegacyCompletionsEndpoint: false
    apiBase: http://localhost:3000/ollama/v1
    apiKey: sk-YOUR-API-KEY
    roles:
      - autocomplete

prompts:
  - name: test
    description: Write unit tests for highlighted code
    prompt: |
      Write a comprehensive set of unit tests for the selected code. It should setup, run tests that check for correctness including important edge cases, and teardown. Ensure that the tests are complete and sophisticated. Give the tests just as chat output, don't edit any file.
```

Save your `config.yaml` and thats it!

You should now see your model in the Continue tab model selection.

Select it and you should now be chatting via Open WebUI (and or any [pipelines](/pipelines) you have setup )

You can do this for as many models you would like to use, altough any model should work, you should use a model that is designed for code.

See the continue documentation for additional continue configuration, [Continue Documentation](https://docs.continue.dev/reference/Model%20Providers/openai)
