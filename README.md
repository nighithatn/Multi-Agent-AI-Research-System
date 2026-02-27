# 🤖 Multi-Agent AI Research System using CrewAI

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![CrewAI](https://img.shields.io/badge/CrewAI-MultiAgent-orange)
![LLM](https://img.shields.io/badge/LLM-Automation-green)
![Status](https://img.shields.io/badge/Status-Active-success)

---

## 📌 Project Overview

This project demonstrates a **Multi-Agent AI System** built using the CrewAI framework.  
Multiple AI agents collaborate sequentially to research a topic and generate a structured markdown report automatically.

Instead of a single LLM handling everything, this system creates:

- 🔬 Research Agent  
- 📊 Reporting Analyst Agent  

The system dynamically injects runtime inputs (topic and year), executes tasks in order, and generates a final report file.

---

## 🧠 System Architecture

Sequential Workflow:

Research Agent  
⬇  
Reporting Analyst  
⬇  
Final `report.md` Output  

Process Type: Sequential Execution

---

## 📂 Project Structure

```
multi-agent-ai-research-system/
│
├── ai_research/
│   ├── crew.py
│   ├── config/
│   │   ├── agents.yaml
│   │   ├── tasks.yaml
│
├── main.py
├── requirements.txt
└── README.md
```

---

## 🧩 Core Implementation

### 🔹 crew.py

```python
from crewai import Agent, Crew, Process, Task
from crewai.project import CrewBase, agent, crew, task
from crewai.agents.agent_builder.base_agent import BaseAgent
from typing import List

@CrewBase
class AiResearch():
    """AiResearch crew"""

    agents: List[BaseAgent]
    tasks: List[Task]

    @agent
    def researcher(self) -> Agent:
        return Agent(
            config=self.agents_config['researcher'],
            verbose=True
        )

    @agent
    def reporting_analyst(self) -> Agent:
        return Agent(
            config=self.agents_config['reporting_analyst'],
            verbose=True
        )

    @task
    def research_task(self) -> Task:
        return Task(
            config=self.tasks_config['research_task'],
        )

    @task
    def reporting_task(self) -> Task:
        return Task(
            config=self.tasks_config['reporting_task'],
            output_file='report.md'
        )

    @crew
    def crew(self) -> Crew:
        return Crew(
            agents=self.agents,
            tasks=self.tasks,
            process=Process.sequential,
            verbose=True,
        )
```
---

### 🔹 main.py

```python
#!/usr/bin/env python
import sys
import warnings
from datetime import datetime
from ai_research.crew import AiResearch

warnings.filterwarnings("ignore", category=SyntaxWarning, module="pysbd")

def run():
    inputs = {
        "topic": "AI LLMs",
        "current_year": str(datetime.now().year)
    }

    try:
        AiResearch().crew().kickoff(inputs=inputs)
    except Exception as e:
        raise Exception(f"An error occurred while running the crew: {e}")

if __name__ == "__main__":
    run()
```

---

## ⚙️ Configuration Files

### 🔹 agents.yaml

```yaml
researcher:
  role: >
    AI Research Specialist
  goal: >
    Conduct in-depth research about {topic} in {current_year}
  backstory: >
    An expert AI researcher with strong analytical skills.

reporting_analyst:
  role: >
    Reporting Analyst
  goal: >
    Create a structured markdown report based on the research findings.
  backstory: >
    Experienced analyst skilled at converting research into readable reports.
```

---

### 🔹 tasks.yaml

```yaml
research_task:
  description: >
    Research the topic: {topic}.
    Focus on trends, advancements, and future outlook in {current_year}.
  expected_output: >
    A detailed research summary.

reporting_task:
  description: >
    Create a well-structured markdown report based on the research findings.
  expected_output: >
    A clean markdown report saved as report.md.
```

---

## 📦 requirements.txt

```
crewai
openai
python-dotenv
pyyaml
```

---

## 🚀 How to Run

### 1️⃣ Clone Repository

```
git clone https://github.com/YOUR_USERNAME/multi-agent-ai-research-system.git
cd multi-agent-ai-research-system
```

### 2️⃣ Create Virtual Environment

```
python -m venv venv
venv\Scripts\activate
```

### 3️⃣ Install Dependencies

```
pip install -r requirements.txt
```

### 4️⃣ Set OpenAI API Key

Windows:
```
set OPENAI_API_KEY=your_api_key_here
```

Mac/Linux:
```
export OPENAI_API_KEY=your_api_key_here
```

### 5️⃣ Run the Project

```
python main.py
```

---

## 📄 Output

After execution, a file named:

```
report.md
```

will be automatically generated in your project directory.

---

## 🧠 Concepts Demonstrated

- Multi-Agent AI System Design  
- Sequential Agent Orchestration  
- YAML-Based Agent Configuration  
- LLM Workflow Automation  
- Modular AI Architecture  

---

## 🔮 Future Improvements

- Add Web Search Tools  
- Switch to Hierarchical Process  
- Integrate Streamlit UI  
- Deploy as REST API  
- Add Memory Persistence  

---

## 👩‍💻 Author

Nighitha T N   

GitHub: https://github.com/nighithatn

---

⭐ If you found this project useful, consider giving it a star!
