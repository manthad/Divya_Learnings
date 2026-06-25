# Claude vs Open Code


|Claude |Open Code|
|---|---|
|Claude use Anthropic LLM i.e. it uses its own| Open Code uses multiple LLMs like OpenAI, Anthropic, Google Gemini, etc.|
|Claude is a paid service| Open Code is an open-source project|
|it will does all the work automatically and fixes if there is a bug too| This is a tool but we have to do all the work by ourself|
|This is like a readymade sandwich| This is like a kitchen we can make whatever we want|


## The Files

|Claude |Open Code|
|---|---|
|Claude.md  | Agents.md / opencode.json |
| 1. We tell AI what we want and how to behave | 
| 2. Simple text file and can edit |
| 3. it is right under the project folder |
| 4. Can commit this to GIT and can share with others | 



- Install the opencode (this is just the software)
- Run the packages of opencode using npm command (here with this we get tools, agents and plugins)
- Install ollama 
- Write a json file under .opencode folder which connects to ollama(we write what provider, its url and port- if local then localhost else add the mini ip address:11434)
- Now opencode and directly type a command

|Agents |Skills|
|---|---|
|Agents are the ones who do the work| Skills are the ones which agents use to do the work|
|Agents are like workers| Skills are like tools|
|They are combination of LLM and tools| They are the set of instructions and tools used to do a task, its a folder SKILL.md It has yml code which tells opencode how to use them|
|Claude 3.5 and specific set of permissions like what files to access| These can execute and perform tasks|
|Different agents for different tasks like frontend and security| We can use same skill for different agents|
|Chef - We have pastry and biryani chef, each person can only make what he knows| Recipie book, this tells what to do and he does it is under .opencode/skills/ Tool is the physical object|


- Gen AI means if we provide a query it will generate a response for us.
- Agentic AI - Instead of just generating a response it can actually work for us, execute, test and fix the code for us etc.

## The Real-World Workflow
- The Agent (The Brain): You talk to Claude (your Agent). You give it a high-level goal, like "Prepare a production release for this project."

- Skill Discovery (The Recall): Claude looks at the descriptions of all the skills in your .opencode/skills/ folder. It realizes, "Oh, I have a skill called git-release that knows exactly how we do this."

- Skill Injection (The Instructions): Claude "loads" that SKILL.md file. This doesn't run code yet; it just gives Claude the specific "Recipe" (e.g., "Step 1: Run build, Step 2: Check linting, Step 3: Tag git").

- Tool Execution (The Hands): Now that Claude has the recipe, it sees that it needs to run a command (like npm run build).

- Claude doesn't just "pass the query" to the tool.

- Instead, Claude issues a command back to the OpenCode environment: "I want to use the Bash Tool to run npm run build."

- Response Loop: The OpenCode terminal runs the npm command, grabs the output (success or error), and feeds it back to Claude. Claude looks at the result and decides if it can move to the next step of the skill or if it needs to fix an error.


- Defaultly opencode uses claude but to connet to it we need to generate API key. So and we have to pay for it. 
- here thats why we have ollama which is local provider so we connected that to the opencode and using it for free.


## Recap

1. The Local Engines: Ollama & LM Studio
These are the "Power Plants" that run the AI on your hardware without an internet connection.

Ollama: A CLI-first tool that is the industry standard for running local models. It is extremely efficient and designed to be a "set it and forget it" background service.

LM Studio: A GUI-first (visual) application that is great for testing different models quickly and checking if they fit in your RAM.

Link to OpenCode: OpenCode doesn't have its own "brain." You tell OpenCode in its opencode.json file: "Instead of calling Claude in the cloud, talk to my local Ollama server at localhost:11434."

2. The Universal Connector: MCP (Model Context Protocol)
This is a new open standard created by Anthropic. It’s like a USB port for AI.

Functionality: Traditionally, to give an AI a tool (like "Search my Database"), you had to write custom code. With MCP, you just run an "MCP Server," and any AI that speaks MCP (like OpenCode or Claude) can instantly use that tool.

Link to OpenCode: OpenCode can act as an MCP Host. You can add local MCP servers to OpenCode to give your offline AI the ability to read your local files, query a local SQLite database, or even control your local Spotify app.

3. The Library: RAG (Retrieval-Augmented Generation)
Note: You likely meant RAG (Retrieval) rather than RSG.

Functionality: RAG is a technique, not a single software. It allows the AI to "look things up" in a private library before answering. It converts your PDFs/docs into "Vectors" (mathematical coordinates) so the AI can find the right page instantly.

Link to OpenCode: In an offline project, you use RAG to feed the AI your company’s private documentation. You might use an MCP Server that provides "RAG-as-a-service" to OpenCode, allowing the agent to answer questions based on 1,000 local PDF manuals without getting confused.

4. The Orchestrators: Dify & n8n
These are the "Project Managers" that build complex workflows.

Dify: An AI-Native platform. It’s best when the "Brain" is the center of the app. It has built-in RAG features and an easy way to create "Chatbots" that use your local models.

n8n: A Workflow-Native platform. It’s a "Swiss Army Knife" for automation. It uses a visual node-based editor to connect 400+ different apps.

Link to OpenCode:

n8n is often used to trigger OpenCode or vice-versa. For example: "When a local file is saved (n8n), tell OpenCode to review the code (Agent)."

Dify is often used to build the user interface or the "Knowledge Base" that OpenCode queries via an API.

### Summary Recap Table

|Component|	Role	|Analogy	 |Why it's vital for YOU
|---------|-------|---------|------------------
|Ollama|	Model Provider	|The Engine	|Runs the AI offline on your Mac/PC.
|LM Studio|	Model Provider	|The Dashboard	|Best for visually testing which local model is smartest.
|MCP (Tools)|	Protocol	|The USB Port	|Lets your offline AI use local tools (Files, DBs).
|RAG (Library)|	Technique	|The Library	|Lets the AI "read" your offline project docs.
|Dify|	Platform	|The Brain-Box	|Easiest way to build a UI + RAG for your local project.
|n8n|	Platform	|The Nervous System	|Connects the AI to your local system (Files → Email → Logs).

RAG will only read and retrieve but 
MCP will help you to write, check the local database and run terminal commands - So it needs to connect to the computer and take actions on the computer. So it is more powerful than RAG.


### Difference b/n n8n and DIfy

In n8n we create Nodes like Read local-> wait for 5 min-> HTTP request etc we are giving the flow what to do after what.
- This i used to connect to 400+ different apps and create a workflow.
- Ex; You want to build a Pipeline. For example: "When a new file is added to my local folder, tell the AI to summarize it, then save that summary into a database."
- here MCP is used as a Tool

Dify is like a specialized toolbox to test complex things like LLM
Here we are wiring directly RAG and Model
- This can be used if we need just answers from a large data
- here MCP is used as a Skill (I guess it have capability to call the actual tool and do the work)


###

Since you are working offline and local, here is exactly how you link them:

Ollama (The Battery): You start this first. It provides the "power" (the LLM) to everyone else.

Dify (The Library): You use Dify to upload your local PDFs/Docs. Dify creates the RAG system that makes your docs searchable.

n8n (The Conveyor Belt): You use n8n to monitor your local folders. When you save a new code file, n8n grabs it and sends it to your AI for a checkup.

OpenCode (The Command Center): You sit in your terminal using OpenCode. When you need help, OpenCode can "call" your Dify RAG to ask a question or "trigger" an n8n workflow to run a test.

## LLMFIT

- It is a terminal tool that tells which model is suitable based on the RAM, CPU and GPU we have. Detects your hardware, scores each model across quality, speed, fit, and context

```bash
brew install llmfit


```
press f or / and write whatever we want like searching reasoning, coding etc but with the word erfect after that do enter
press d and download the model with ollama or any provider

Either can press d or below

```bash
llmfit run deepseek-ai/DeepSeek-R1-Distill-Qwen-7B
```
t to change the themes - so interesting


| Task | Recommended Model| RAM Usage|
|-----|-----|----|
|Search/RAG| nomic-embed-text| ~0.5 GB |
|Main Coding,Qwen2.5-Coder-3B| ~2.2 GB |
|Logic/Debug,DeepSeek-R1-Distill-Qwen-1.5B | ~1.1 GB|
|System/OS,macOS / Windows / Apps| ~3.0 GB|
|TOTAL||~6.8 GB (Safe)|

- We need to prioritize which model should have more and then add tehm accordingly all the models can run together too