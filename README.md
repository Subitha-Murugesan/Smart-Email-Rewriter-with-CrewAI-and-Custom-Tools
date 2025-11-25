# Smart-Email-Rewriter-with-CrewAI-and-Custom-Tools
Professional Email Assistant with CrewAI and Custom Jargon Tool

**EmailAgent** is a Python project that uses **CrewAI** to rewrite informal or rough emails into professional, polished versions. This version adds a **custom tool to replace company-specific abbreviations and jargon**, making emails more readable for broader audiences.

---

The **EmailAgent** project leverages **Agentic AI** via **CrewAI** to:

* Rewrite rough emails into professional versions.
* Expand abbreviations and improve grammar.
* Replace company-specific jargon with clear explanations.

By combining **LLM-based agents** with **custom tools**, this project demonstrates how CrewAI can manage workflows for specialized communication tasks.

---

## Features

* Automatically rewrites emails in a professional tone.
* Detects and replaces company-specific abbreviations and jargon.
* Uses CrewAI for structured agent and task management.
* Verbose logging to track agent actions.
* Extensible for additional tools or tasks.

---


## Custom Tools

The project includes a **ReplaceJargonsTool** class, which:

* Detects internal abbreviations and jargon in an email.
* Suggests replacements with more descriptive terms.
* Can be integrated into CrewAI agents as a tool.

Example replacements:

* `PRX` - `Project Phoenix (internal AI project)`
* `TAS` - `technical architecture stack`
* `SDS` - `Smart Data Syncer`
* `SYNCBOT` - `internal standup assistant bot`
* `WIP` - `in progress`
* `POC` - `proof of concept`
* `ping` - `reach out`

---

## Usage

1. Import required libraries and load environment variables:

```python
from dotenv import load_dotenv
load_dotenv()

from crewai import LLM, Agent, Task, Crew
from crewai.tools import BaseTool
```

2. Define an LLM model:

```python
llm = LLM(
    model="gemini/gemini-2.0-flash",
    temperature=0.1
)
```

3. Create the **jargon replacement tool**:

```python
class ReplaceJargonsTool(BaseTool):
    name: str = "Jargon replacement tool"
    description: str = "Replaces jargon with more specific terms."

    def _run(self, email: str) -> str:
        replacements = {
            "PRX": "Project Phoenix (internal AI project)",
            "TAS": "technical architecture stack",
            "DBX": "client database cluster",
            "SDS": "Smart Data Syncer",
            "SYNCBOT": "internal standup assistant bot",
            "WIP": "in progress",
            "POC": "proof of concept",
            "ping": "reach out"
        }
        suggestions = []
        email_lower = email.lower()
        for jargon, replacement in replacements.items():
            if jargon.lower() in email_lower:
                suggestions.append(f"Consider replacing '{jargon}' with '{replacement}'")
        return "\n".join(suggestions) if suggestions else "No jargon or internal abbreviations detected."

jt = ReplaceJargonsTool()
```

4. Define the **Email Assistant Agent**:

```python
email_assistant = Agent(
    role="Email Assistant Agent",
    goal="Improve emails and make them sound professional and clear",
    backstory="A highly experienced communication expert skilled in professional email writing",
    tools=[jt],
    verbose=True,
    llm=llm
)
```

5. Define the email task:

```python
original_email = """
looping in Simon. TAS and PRX updates are in the deck. ETA for SDS integration is Friday.
Let's sync up tomorrow if SYNCBOT allows. ping me if any blockers.
"""

email_task = Task(
    description=f"""Take the following rough email and rewrite it into a professional and polished version.
Expand abbreviations:
'''{original_email}'''""",
    agent=email_assistant,
    expected_output="A professional written email with proper formatting and content."
)
```

6. Create a Crew and run the workflow:

```python
crew = Crew(
    agents=[email_assistant],
    tasks=[email_task],
    verbose=True
)

result = crew.kickoff()
print("Revised Email:\n", result)
```

---

## How It Works

1. **LLM Initialization** – Sets up the language model.
2. **Tool Integration** – Adds a custom tool for replacing company-specific jargon.
3. **Agent Creation** – Defines the Email Assistant with role, goal, backstory, and tools.
4. **Task Definition** – Specifies the email to rewrite and expected output.
5. **Crew Execution** – Orchestrates the agent and task workflow using CrewAI.
6. **Result** – Outputs a polished, professional email with jargon replaced.

---

### Output
<img width="851" height="440" alt="image" src="https://github.com/user-attachments/assets/8e031c82-9eb5-4d78-be57-f66bd758e724" />
<img width="845" height="515" alt="image" src="https://github.com/user-attachments/assets/5eb72fff-ab2b-4fd4-b9b9-d43a3b7715dd" />
<img width="1024" height="745" alt="image" src="https://github.com/user-attachments/assets/c87018ac-8247-42bb-bc18-53539e9878cd" />
<img width="835" height="429" alt="image" src="https://github.com/user-attachments/assets/a4790c23-e603-4dbe-a06a-9df2af64c39e" />





