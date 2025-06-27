# 🚀 LangChain Practical Learning Journey

Welcome to my LangChain learning repository! This project showcases my practical exploration of LangChain's powerful features for building advanced language model applications.

## 📚 Project Overview

This repository contains practical implementations and examples of various LangChain features, demonstrating how to build sophisticated AI applications using LangChain's powerful framework. Each implementation includes detailed examples, best practices, and real-world use cases.

## 🛠️ Current Implementations

### 1. Function Calling
- Implementation of OpenAI's function calling capabilities
- Practical examples of how to structure and use function calls with LLMs
- Real-world use cases and best practices
- Integration with custom functions and tools
- Error handling and validation patterns

### 2. LangChain Expression Language (LCEL)
- Hands-on examples of LCEL implementation
- Understanding the power of composable chains
- Building complex workflows with simple, readable code
- Chain composition and transformation
- Input/output processing patterns

### 3. Tagging and Extraction
- Named Entity Recognition (NER) implementation
- Information extraction from unstructured text
- Custom tagging implementations
- Pattern matching and classification
- Data validation and cleaning
- Integration with various data sources

### 4. Tools & Routing APIs
- Creation and registration of custom tools for LangChain agents
- Defining robust input schemas using Pydantic
- Real-world API integration (e.g., Open-Meteo for weather)
- Wikipedia search tool with error handling
- Formatting tools for OpenAI function calling and OpenAPI specs

### 5. Conversational Agent
- Building a multi-turn conversational agent using LangChain
- Integrating custom tools (weather, Wikipedia search) into the agent
- Using prompts, scratchpads, and output parsers for dynamic conversations
- Chaining user input, tool invocation, and agent responses

## 📋 Project Structure

```
langchain_Practical/
├── function-calling.ipynb             # Function calling implementations
├── openai_functions.ipynb             # OpenAI function calling examples
├── lcel.ipynb                        # LangChain Expression Language examples
├── tagging-and-extraction.ipynb       # Tagging and extraction examples
├── tools-routing-apis.ipynb           # Custom tools and API routing examples
└── functional_conversation-agent.ipynb # Conversational agent with tool integration
```

## 🚀 Getting Started

### Prerequisites
- Python 3.8+
- OpenAI API key
- Required Python packages (to be added to requirements.txt)

### Installation
```bash
# Clone the repository
git clone https://github.com/yourusername/langchain_Practical.git

# Navigate to the project directory
cd langchain_Practical

# Install dependencies
pip install -r requirements.txt
```

## 📝 Usage Examples

### Function Calling
```python
from pydantic import BaseModel, Field
from langchain.utils.openai_functions import convert_pydantic_to_openai_function
from langchain.chat_models import ChatOpenAI

class WeatherSearch(BaseModel):
    """Call this with an airport code to get the weather at that airport"""
    airport_code: str = Field(description="airport code to get weather for")

weather_function = convert_pydantic_to_openai_function(WeatherSearch)
model = ChatOpenAI()

# Invoke the model with the function
response = model.invoke("what is the weather in SF today?", functions=[weather_function])
print(response)
```

### LCEL Implementation
```python
from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser

# Simple chain: prompt -> model -> output parser
prompt = ChatPromptTemplate.from_template("tell me a short joke about {topic}")
model = ChatOpenAI()
output_parser = StrOutputParser()

chain = prompt | model | output_parser
result = chain.invoke({"topic": "bears"})
print(result)
```

### Tagging and Extraction
```python
from pydantic import BaseModel, Field
from langchain.utils.openai_functions import convert_pydantic_to_openai_function
from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.output_parsers.openai_functions import JsonOutputFunctionsParser

class Tagging(BaseModel):
    """Tag the piece of text with particular info."""
    sentiment: str = Field(description="sentiment of text, should be `pos`, `neg`, or `neutral`")
    language: str = Field(description="language of text (should be ISO 639-1 code)")

tagging_function = convert_pydantic_to_openai_function(Tagging)
model = ChatOpenAI(temperature=0).bind(functions=[tagging_function], function_call={"name": "Tagging"})
prompt = ChatPromptTemplate.from_messages([
    ("system", "Think carefully, and then tag the text as instructed"),
    ("user", "{input}")
])
tagging_chain = prompt | model | JsonOutputFunctionsParser()

result = tagging_chain.invoke({"input": "I love langchain"})
print(result)
```

### Tools & Routing APIs
```python
from langchain.agents import tool
from pydantic import BaseModel, Field

class OpenMeteoInput(BaseModel):
    latitude: float = Field(..., description="Latitude of the location")
    longitude: float = Field(..., description="Longitude of the location")

@tool(args_schema=OpenMeteoInput)
def get_current_temperature(latitude: float, longitude: float) -> str:
    """Fetch current temperature for given coordinates."""
    # ... API call logic ...
    return f"The current temperature is ...°C"
```

### Conversational Agent
```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain.tools.render import format_tool_to_openai_function
from langchain.agents.output_parsers import OpenAIFunctionsAgentOutputParser

# Define tools and format for OpenAI
functions = [format_tool_to_openai_function(f) for f in tools]
model = ChatOpenAI(temperature=0).bind(functions=functions)
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are helpful but sassy assistant"),
    ("user", "{input}"),
])
chain = prompt | model | OpenAIFunctionsAgentOutputParser()

result = chain.invoke({"input": "what is the weather in sf?"})
print(result)
```

## 🎯 Key Features

- **Modular Design**: Each implementation is self-contained and follows best practices
- **Comprehensive Examples**: Detailed examples with explanations and use cases
- **Best Practices**: Implementation of industry-standard patterns and practices
- **Error Handling**: Robust error handling and validation
- **Documentation**: Clear and concise documentation for each feature

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- LangChain team for their excellent documentation and framework
- OpenAI for providing the underlying language models
- The open-source community for their continuous support and contributions

## 📞 Contact

Feel free to reach out if you have any questions or suggestions!

---

⭐ Star this repository if you find it helpful! 