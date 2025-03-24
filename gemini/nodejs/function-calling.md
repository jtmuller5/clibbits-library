# Function Calling for Structured Data Outputs

Function calling allows you to get structured data outputs from generative models, which you can use to call other APIs and return relevant response data to the model. This helps connect generative models to external systems to include up-to-date and accurate information in generated content.

## Define an API Function

Create a function that makes an API request within your application code. The Gemini API doesn't call this function directly, allowing you to control execution through your application. This tutorial uses a mock API function that returns requested lighting values:

```javascript
async function setLightValues(brightness, colorTemp) {
  return {
    brightness: brightness,
    colorTemperature: colorTemp
  };
}
```

## Create Function Declarations

Create a function declaration to pass to the generative model, including detailed descriptions that help the model determine which function to select and how to provide parameter values:

```javascript
const controlLightFunctionDeclaration = {
  name: "controlLight",
  parameters: {
    type: "OBJECT",
    description: "Set the brightness and color temperature of a room light.",
    properties: {
      brightness: {
        type: "NUMBER",
        description: "Light level from 0 to 100. Zero is off and 100 is full brightness.",
      },
      colorTemperature: {
        type: "STRING",
        description: "Color temperature of the light fixture which can be `daylight`, `cool` or `warm`.",
      },
    },
    required: ["brightness", "colorTemperature"],
  },
};

// Put functions in a "map" keyed by the function name so it is easier to call
const functions = {
  controlLight: ({ brightness, colorTemperature }) => {
    return setLightValues(brightness, colorTemperature)
  }
};
```

## Declare Functions During Model Initialization

Provide your function declarations when initializing the model object by setting the model's `tools` parameter:

```javascript
const { GoogleGenerativeAI } = require("@google/generative-ai");

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);

const generativeModel = genAI.getGenerativeModel({
  model: "gemini-2.0-flash",
  // Specify the function declaration.
  tools: {
    functionDeclarations: [controlLightFunctionDeclaration],
  },
});
```

## Generate a Function Call

Initialize the model with your function declarations and prompt it using chat prompting (`sendMessage()`), which benefits from the context of previous prompts and responses:

```javascript
const chat = generativeModel.startChat();
const prompt = "Dim the lights so the room feels cozy and warm.";

const result = await chat.sendMessage(prompt);
const call = result.response.functionCalls()[0];

if (call) {
  // Call the executable function
  const apiResponse = await functions[call.name](call.args);

  // Send the API response back to the model
  const result2 = await chat.sendMessage([{functionResponse: {
    name: 'controlLight',
    response: apiResponse
  }}]);

  // Log the text response.
  console.log(result2.response.text());
}
```

## Function Calling Mode

You can modify the execution behavior using the function calling mode parameter. Three modes are available:

- **AUTO**: Default behavior where the model decides to predict either a function call or a natural language response.
- **ANY**: The model is constrained to always predict a function call. Without `allowed_function_names`, it picks from all available function declarations. With `allowed_function_names`, it picks from the set of allowed functions.
- **NONE**: The model won't predict a function call, behaving the same as if no function declarations were passed.

You can specify the function calling mode by setting a `toolConfig` parameter when initializing the generative model:

```javascript
const generativeModel2 = genAI.getGenerativeModel({
  // Setting a function calling mode is only available in Gemini 1.5 Pro.
  model: "gemini-1.5-pro-latest",
  tools: {
    functionDeclarations: [getExchangeRateFunctionDeclaration],
  },
  toolConfig: {
    functionCallingConfig: {
      // Possible values are: Mode.AUTO, Mode.ANY, Mode.NONE
      mode: FunctionCallingMode.ANY
    }
  }
});
```

# Intro to Function Calling with the Gemini API

Function calling allows you to provide custom function definitions to the model. The model doesn't directly invoke these functions but generates structured output specifying a function name and suggested arguments. You can then use these to call an external API and incorporate the results into further queries, enabling more comprehensive responses and additional actions.

## Benefits

Function calling empowers users to interact with real-time information and services like:
- Databases
- Customer relationship management systems
- Document repositories

This enhances the model's ability to provide relevant and contextual answers. Function calling is best for interacting with external systems, whereas code execution is more appropriate for computation without external systems or APIs.

> **Beta**: The function calling feature is in Beta release.

## How Function Calling Works

1. Add structured query data (function declarations) to a model prompt
2. The model analyzes function declarations and the query
3. The model returns an OpenAPI-compatible schema specifying how to call functions
4. You use the returned parameters to call the actual API
5. You can provide that response to the user or take further action

The Gemini API also supports parallel function calling, where the model recommends multiple API function calls based on a single request.

## Function Declarations

When implementing function calling, you create a `tools` object containing one or more function declarations using a subset of the OpenAPI schema format:

```javascript
{
  "name": "function_name",    // Unique identifier
  "description": "Detailed explanation of function purpose",
  "parameters": {
    "type": "object",         // Overall data type
    "properties": {
      "parameter1": {
        "type": "string",     // Data type (string, integer, boolean, etc.)
        "description": "Clear explanation of parameter purpose"
      }
      // Additional parameters...
    },
    "required": ["parameter1"] // Array of mandatory parameter names
  }
}
```

## Best Practices for Function Declarations

- **name**: Use clear, descriptive names without spaces, periods (.), or dashes (-). Use underscores (_) or camelCase instead.

- **description**: Provide detailed, specific function descriptions with examples. Instead of "find theaters", use "find theaters based on location and optionally movie title that is currently playing in theaters".

- **properties > type**: Use strongly typed parameters to reduce model hallucinations:
  - For finite sets, use enum: `"type": "enum", "values": ["now_playing", "upcoming"]`
  - Match parameter type to expected values (e.g., use integer instead of number when appropriate)

- **properties > description**: Provide concrete examples and constraints. Instead of "the location to search", use "The city and state, e.g. San Francisco, CA or a zip code e.g. 95616".

## Function Calling Mode

Three modes are available:

- **AUTO**: Default behavior where the model decides to predict either a function call or a natural language response.
- **ANY**: The model is constrained to always predict a function call.
  - Without `allowed_function_names`, it picks from all available declarations
  - With `allowed_function_names`, it picks from the set of allowed functions
- **NONE**: The model won't predict a function call

Example of setting mode to ANY with allowed functions:

```javascript
"tool_config": {
  "function_calling_config": {
    "mode": "ANY",
    "allowed_function_names": ["find_theaters", "get_showtimes"]
  },
}
```

## Compositional Function Calling

Gemini 2.0 supports compositional function calling, enabling the API to invoke multiple user-defined functions automatically in generating a response. For example, to respond to "Get the temperature in my current location", it might invoke both `get_current_location()` and `get_weather()` that takes the location as a parameter.

Example with code execution:

```python
turn_on_the_lights_schema = {'name': 'turn_on_the_lights'}
turn_off_the_lights_schema = {'name': 'turn_off_the_lights'}

prompt = """
  Hey, can you write run some python code to turn on the lights, wait 10s and then turn off the lights?
  """

tools = [
    {'code_execution': {}},
    {'function_declarations': [turn_on_the_lights_schema, turn_off_the_lights_schema]}
]

await run(prompt, tools=tools, modality="AUDIO")
```

## Multi-tool Use

With Gemini 2.0, you can enable multiple tools simultaneously, and the model decides when to call them:

```python
prompt = """
  Hey, I need you to do three things for me.

  1. Turn on the lights.
  2. Then compute the largest prime palindrome under 100000.
  3. Then use Google Search to look up information about the largest earthquake in California the week of Dec 5 2024.

  Thanks!
  """

tools = [
    {'google_search': {}},
    {'code_execution': {}},
    {'function_declarations': [turn_on_the_lights_schema, turn_off_the_lights_schema]}
]

await run(prompt, tools=tools, modality="AUDIO")
```

> **Note**: Multi-tool use is only supported by the Live API.

## Function Calling Examples

### Single-Turn Example

A single-turn call includes providing the model with a natural language query and a list of functions. The model uses the function declarations to predict which function to call and what arguments to use.

### Single-Turn Example Using ANY Mode

Setting the mode to ANY forces the model to predict a function call:

```javascript
"tool_config": {
  "function_calling_config": {
    "mode": "ANY"
  },
}
```

### Single-Turn Example Using ANY Mode and Allowed Functions

Setting the mode to ANY with allowed functions limits which functions the model can predict:

```javascript
"tool_config": {
  "function_calling_config": {
    "mode": "ANY",
    "allowed_function_names": ["find_theaters", "get_showtimes"]
  },
}
```

### Multi-Turn Examples

Multi-turn function calling involves:
1. Getting a function call response (first turn)
2. Calling the model with the function call response and the actual function response (second turn)

Example of a function call response:

```javascript
"functionCall": {
  "name": "find_theaters",
  "args": {
    "movie": "Barbie",
    "location": "Mountain View, CA"
  }
}
```

## Best Practices

### User Prompt
- Include additional context for the model
- Provide details on how and when to use functions
- Instruct the model to ask clarifying questions for ambiguous queries

### Sampling Parameters
- Use temperature=0 or another low value for more confident results and reduced hallucinations

### API Invocation
- For consequential function calls (orders, database updates), validate with the user before executing