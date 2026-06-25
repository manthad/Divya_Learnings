Gemini said
To make this practical, I’ve mapped out the popular industry-standard tools (the "names") you’ll likely encounter or use for each of these layers.
The AI Infrastructure Tech Stack

|Layer|	Component|	What we use (Tools/Names)|
|------|-----------|--------------------------|
|User Plane|	Conversational Interface	|LibreChat, Open WebUI, Vercel AI SDK, or custom Streamlit apps.|
|Admin Plane|	Role & Skill Management	|Keycloak (for roles), PostHog (for feature flags), or LangSmith (for skill governance).|
|Orchestration|	The "Manager" Logic	|LangChain, LlamaIndex, PydanticAI, or CrewAI.|
|Resource Access|	The "Hands" (MCP/APIs)	|MCP (Model Context Protocol), Supabase (DB), Tavily (Search API), Stripe (Payments).|
|Inference/Routing|	The "Brain" Host	|Ollama (Local), vLLM (Production), Groq (Fast Cloud), LiteLLM (The Router).|
|Models|	The "Brain" Weights	|Llama 3.1 (Meta), Qwen 2.5 (Alibaba), Mistral, or Claude/GPT-4.|
|Safety/Isolation|	The Sandbox	|Docker, E2B (specifically for AI code execution), or gVisor.|
|Observation|	Telemetry	|LangSmith, Arize Phoenix, OpenTelemetry, or Weights & Biases.|


## How a single request flows through these "Names"
User Plane: A Student logs into Open WebUI.

Admin Plane: The system checks Keycloak and sees they are a "Student," so it only enables the "Tutor" skill.

Orchestration: LangChain receives the prompt. It decides: "I need to look at the textbook."

Resource Access: It uses an MCP Server to fetch a PDF from a folder.

Inference/Routing: LiteLLM (the router) sends the prompt and the PDF text to Ollama, which is running Llama 3.

Telemetry: LangSmith records that this entire process took 2 seconds and cost 500 tokens.

User Plane: The student gets their answer.