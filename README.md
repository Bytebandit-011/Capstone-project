# TutorAgent - AI-Powered Learning Assistant

An interactive AI tutoring system built with Google's Agent Development Kit (ADK) that provides personalized learning experiences through a structured teach-learn-quiz-review cycle.

## 🎯 Overview

TutorAgent is an intelligent tutoring assistant that helps users learn coding topics through a multi-agent system. It guides learners through:
1. **Learning** - Clear, structured explanations of topics
2. **Assessment** - Interactive quizzes to test understanding
3. **Reinforcement** - Teach-back exercises to solidify knowledge

## ✨ Features

- **Multi-Agent Architecture**: Four specialized agents working in sequence
- **Interactive Learning**: Conversational approach to education
- **Immediate Feedback**: Real-time validation and explanations
- **Adaptive Teaching**: Adjustable depth based on user needs
- **Web UI**: User-friendly interface via ADK web interface

## 🏗️ Architecture

The system consists of four specialized agents:

### 1. **Startup Agent**
- Greets users and introduces the TutorAgent system
- Explains available learning features
- Captures user's learning goals
- Routes to appropriate learning path

### 2. **Learn Agent**
- Teaches coding topics in clear, structured language
- Provides explanations in 10-20 lines
- Offers deeper or simpler explanations based on user preference
- Confirms understanding before proceeding
- Outputs: `quiz_topic`

### 3. **Quiz Agent**
- Tests understanding with 2-3 questions (multiple-choice or short-answer)
- Provides immediate feedback after each question
- Explains correct answers briefly
- Summarizes user performance
- Uses Google Search tool for enhanced accuracy
- Outputs: `teach_back_topic`

### 4. **Teach Back Agent**
- Asks users to explain the topic in their own words (2-3 sentences)
- Provides constructive feedback
- Offers corrective pointers if needed
- Validates knowledge retention
- Outputs: `learn_back_output`

## 🚀 Getting Started

### Prerequisites

- Python 3.x
- Kaggle account (for running in Kaggle Notebooks)
- Google API Key (Gemini API)

### Installation

1. **Install required packages:**
```python
!pip install google-adk
```

2. **Set up your Google API Key:**

In Kaggle, add `GOOGLE_API_KEY` to your secrets:
- Go to Kaggle Notebook Settings → Secrets
- Add a new secret with key `GOOGLE_API_KEY` and your API key as the value

3. **Run the setup cell:**
```python
import os
from kaggle_secrets import UserSecretsClient

try:
    GOOGLE_API_KEY = UserSecretsClient().get_secret("GOOGLE_API_KEY")
    os.environ["GOOGLE_API_KEY"] = GOOGLE_API_KEY
    print("✅ Gemini API key setup complete.")
except Exception as e:
    print(f"🔑 Authentication Error: {e}")
```

### Usage

1. **Initialize the agent system:**
```python
from google.adk.agents import Agent, SequentialAgent
from google.adk.models.google_llm import Gemini
from google.adk.runners import InMemoryRunner

# Create the root agent (already defined in the notebook)
root_agent = SequentialAgent(
    name="BlogPipeline",
    sub_agents=[startup_agent, learn_agent, quiz_agent, teach_back_agent],
)
```

2. **Run the agent:**
```python
runner = InMemoryRunner(agent=root_agent)
response = await runner.run_debug("")
```

3. **Launch the Web UI:**
```python
# Get the proxy URL
url_prefix = get_adk_proxy_url()

# Start the web interface
!adk web --url_prefix {url_prefix}
```

4. **Access the UI:**
- Run the cell that displays the "Open ADK Web UI" button
- Wait for the web server to start (cell will show "Running")
- Click the button to open the interactive interface

## 🔧 Configuration

### Retry Configuration
The system includes robust error handling with retry logic:
```python
retry_config = types.HttpRetryOptions(
    attempts=5,              # Maximum retry attempts
    exp_base=7,              # Delay multiplier
    initial_delay=1,         # Initial delay in seconds
    http_status_codes=[429, 500, 503, 504],  # Retry on these errors
)
```

### Model Selection
Currently uses `gemini-2.5-flash-lite` for optimal performance. You can modify the model in each agent's configuration:
```python
model=Gemini(
    model="gemini-2.5-flash-lite",
    retry_options=retry_config
)
```

## 📝 Example Workflow

1. **User starts conversation** → Startup Agent greets and asks what they want to learn
2. **User requests topic** (e.g., "Python loops") → Learn Agent teaches the concept
3. **Learning complete** → Quiz Agent tests understanding with questions
4. **Quiz complete** → Teach Back Agent asks user to explain in their own words
5. **Cycle complete** → User receives comprehensive feedback

## 🛠️ Technical Details

### Dependencies
- `google-adk` - Google's Agent Development Kit
- `google.genai` - Gemini AI integration
- `IPython` - For Jupyter notebook integration
- `jupyter_server` - For web server functionality

### Agent Communication
Agents communicate through output keys:
- `startup_agent` → `topics` → `learn_agent`
- `learn_agent` → `quiz_topic` → `quiz_agent`
- `quiz_agent` → `teach_back_topic` → `teach_back_agent`
- `teach_back_agent` → `learn_back_output` → End

### Tools Available
- `google_search` - Integrated into Quiz Agent for enhanced accuracy

## 🎓 Use Cases

- **Self-Directed Learning**: Students learning coding concepts independently
- **Concept Reinforcement**: Practice and validation of understood topics
- **Interview Preparation**: Quick reviews of technical concepts
- **Teaching Assistant**: Supplement to classroom instruction

## 🔒 Security Notes

- Never commit your API keys to version control
- Use Kaggle secrets or environment variables for API keys
- The system uses the Kaggle proxy for secure web UI access

## 🤝 Contributing

This is a capstone project demonstrating AI agent orchestration. Suggestions and improvements are welcome!

## 📄 License

This project is created for educational purposes as part of a capstone project.

## 🙏 Acknowledgments

- Built with [Google ADK](https://developers.google.com/adk)
- Powered by [Gemini AI](https://deepmind.google/technologies/gemini/)
- Designed for [Kaggle Notebooks](https://www.kaggle.com/code)

## 📞 Support

For issues or questions:
1. Check the Google ADK documentation
2. Verify your API key is correctly configured
3. Ensure all required packages are installed
4. Check that the web server is running before accessing the UI

---

**Note**: This project is designed to run in Kaggle Notebooks environment. Some modifications may be needed for local execution.
