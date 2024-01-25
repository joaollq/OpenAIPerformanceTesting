# OpenAIPerformanceTesting

This notebook enables the execution of performance testing of a open ai model. The notebook executes requests to the azure open ai endpoints, gathers metrics and creates a trace log in log analytics for each execution. You can then use kusto queries to analyse the results of the test runs.

Each run will have its unique ``runId`` that can be used to retrieve the logs for that specific run.

Each run consists on executing all the prompts for a use case, for all the concurrency levels that were configured, for a specific number of runs

To execute this notebook you must define the following 2 variables on the notebook file itself:
 - Use Case -> The use case that you need to test
 - Deployment names -> the open ai models that will be tested


You will also need to fill the .env file that is supplied in the package with the correct values for your environemnt:
```
APPLICATIONINSIGHTS_CONNECTION_STRING="<key for your application insights instance>"
AZURE_OPENAI_ENDPOINT="<url of the azure open ai instance>"
AZURE_OPENAI_VERSION="<open ai version>"
AZURE_OPENAI_KEY="<api key for open ai>"
OPENAI_TEMPERATURE=<temperature parameter of the openai call>
OPENAI_TOP_P=<top-p parameter of the openai call>
OPENAI_FREQUENCY_PENALTY=<frequency parameter of the openai call>
OPENAI_PRESENCE_PENALTY=<presence parameter of the openai call>
OPENAI_MAX_TOKENS=<max tokens parameter of the openai call>
OTEL_SERVICE_NAME="<Label to be used on app insights to identify this workload>"
```

To setup a use case you need to create a folder for that use case inside the useCases folder.
Inside each use case you need to create a .json for each prompt that will be executed.
The prompts are json files that already in the structure needed to pass to OpenAI, similar to the this example:

```json
[
    {
        "role": "system",
        "content": "You are an assistant. You should greet the user in at least 4 languages"
    }
]
```

Once everything is configured you can run the notebook.

The notebook will execute the following flow:

``````
for each prompt of the use case:
    for each concurrency level:
        for each open ai model:
            execute concurrently the requests to open ai model
