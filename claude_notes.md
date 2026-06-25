Difference between Claude, Claude Code, and Claude Code Assistant:

|Claude|Claude Code|Claude Code Assistant| 
|----|----|----|
|AI Engine| The Developer Tool| The Agent| 
|Web based chat interface (Browser/Chatbot)| CLI tool, runs in the terminal| This is claudes capabailities as a coding helper
|Writing, summarizing and helping with code| high level agentic coding and can execute terminal commands,understands full code at once| using asst means using Claude Dev extention in VS code, the official Claude code CLI
|Gives the response but not able to work by itself and view files| Can read and write files and save changes to the code| Claude code u install but this refers to function the AI serves as an assistant to the developer and can do the work for you by itself|
|Like chatgpt| To create task like folder or files etc| To help in our coding work (low level- auto corrections/suggestions and all)|


## TASK:

  “Create an API to store user data in a database”
Claude Code will:
	•	Create files:
	•	app.py
	•	models.py
	•	database.py
	•	Write full working code
	•	Add dependencies
	•	Set up DB connection
	•	Maybe even run it  
## Claude Code Assistant:
  - You are already coding in app.py:
  - We ask: “How do I add a POST endpoint?”
  - Assistant: Explain what it does and fix error.

Install Claude using below command: under the project main directory

    npm install -g @anthropic-ai/claude-code

Now i have a project folder so go inside it and run (here it is nodejs)
    
    npm run setup    
  This will install all the dependencies related to the project
    
    npm run dev
  To run the project and start the UI for the claude code assistant
  
  Now it will show you the app url and can access it in the browser


- Now do /init - it will take look at the project and createclaude.md file
- We can write any question and say @ and give the path of the file and it will read that file and give you the response
- Esc - once will interrupt claude and will allow u to change the question
- Double Esc - it will take you to the previous point in the session
- /compact - summarize the conversation and helps claude to remember the important points in current session
- /clear - clear conversation and start new 

## Custom commands:

We can create our own commands especially used for any repetitive tasks. For this
  - Create a folder under .claude/commands/audit.md - audit is just the name of file
  - inside that we can write few commands waht all should run apart from the default
      
        Run 'npm audit'

        Run 'npm audit fix'
        
        Run tests and verify pdates didnt break anything

After all this restart the claude code and if typed /audit it will run all the commands we wrote and gives output

We can add new tools to the claude code via MCP servers and it will connect to the external database. These can be run on either local or remote the popular one is 'Playwright'
   
   To open browser
   to run code in browser
   to navigate to an address
   to click on the screen

```bash
claude mcp add playwright npx @playwright/mcp@latest
```
While doing this it may ask for permission so if we want dont want to ask it everytime we can mention it under -> commands > settings.local.json file allow : "mcp__playwright" double underscore 

For the mcp server to access to the browser we need to give few things under src > lib > prompts > generation.md

### Claud ecode with github

- We can install claude code in github and can run it in github itself

    /install-github-app



### HOOKS:

These allow to run commands before or after claude generates a response.

These gets executed before the tools execute so we have pre tool and post tool hooks

  - **Pre tool hook:** When we ask claude to do something, it will first check if there are any pre tool hooks. If it is there then it will do what have been mentioned under that hook, then only it will do the task
  - **Post tool hook:** After the task is done then it will check if there are any post tool hooks and do the work mentioned under that hook and do that task after the main task is done and then provide the response to the user

    All these hooks are written under .claude/hooks folder - this .claude folder is main folder where all the settings and commands are written for claude assistant. But this is not read by claude code but can be ready by assistant. 

```bash
When we ask claude to do something, the flow is like this:

Reads CLAUDE.md first for general or basic rules.

Searches for only the relevant files based on what you asked.

Decides to take action (use a tool).

Before that it goes to the .claude folder in which we have few files or hooks folder so checks there if any hook avaialble it first cehcks with that and then does the work.

CLI Intercepts to check for a Pre-Hook.

Once done before providing the response checks if there is any post hook and then does
```

Here matcher is something in the hooks code part where we mention which tool claude is used for pre or post 

like pre-hook for read_file tool, post-hook for write_file tool and all etc

Here exit code 0 means everything is good

      Exit code 2 means there is a block

So under hooks folder we write our conditions like ex: if calude is trying to read a file then block it we can even write an error message, so when user asks the claude to read it via terminal it should throw the error














---


## The Role of the Assistant

- The Claude Code assistant (the CLI tool) acts as the agent. It provides the LLM with a "toolkit."
- The LLM decides what needs to happen (e.g., "I need to read main.py to understand the error").
- The Assistant executes the action (e.g., actually opening main.py and feeding the text back to the LLM).

### The Concept of "Tools"
- You give the assistant permission to access your files when you run it.
- The Assistant tells the LLM: "Hey, I have these tools available (read_file, edit_file, run_terminal_command). Use them if you need to."
- The LLM responds with a special command: "I would like to use the read_file tool on index.js."
- The Assistant performs the task and shows the LLM the result.

### What happens if there is no Assistant?
- If you just go to a website and ask an LLM to "fix the bug in my local folder," it will likely say: "I can't see your folders. Please copy and paste the code here."

#### Without the assistant, the LLM is:
- Isolated: No access to your file system or terminal.
- Passive: It can only suggest text; it cannot take action.

#### With the assistant, the LLM becomes:
- Context-Aware: It can explore your whole codebase to find where a variable is defined.
- Active: It can save files, run tests, and check if the fix actually worked.

#### The Restaurant Analogy
- You (The User): You are the Customer. You want a specific result (a meal / a bug fix).
- The LLM (The Brain): This is the Chef. The Chef is highly skilled and knows exactly how to cook every recipe in the world, but the Chef is stuck inside a kitchen with no windows. They can't see the dining room or know what ingredients are currently in the fridge unless someone tells them.
- Claude Code (The Assistant): This is the Waiter. The Waiter is the bridge. They go to the fridge (your files), tell the Chef what’s available, and carry the Chef's finished dishes back to your table.


## A Concrete Technical Example
Imagine you want to "Change the background color of my website to blue."

- Scenario A: LLM only (The Chef with no Waiter)
You: "Make my website background blue."

- LLM: "I'd love to! Please copy and paste your CSS file here so I can see it. Then, I'll give you the code, and you will have to save it yourself."
- Result: You have to do all the manual labor of finding the file, copying, and saving.
- Scenario B: Claude Code Assistant (The Chef + Waiter)
- You: "Make my website background blue."

LLM (to Assistant): "I need to find the styling file. List the files in this folder."

Assistant (The "Hands"): Runs ls and tells the LLM: "I see index.html and styles.css."

LLM (to Assistant): "Read styles.css for me."

Assistant: Opens the file and feeds the text to the LLM.

LLM (to Assistant): "Okay, I see the body tag. Please rewrite line 5 of styles.css to background-color: blue;."

Assistant: Actually overwrites the file on your hard drive.

Result: The task is finished without you lifting a finger.


## Claude with Tools

- Claude is very good at making use of the tools. It has few default tools (read_file, write_file, run_command) but you can also create your own custom tools.
- It can also make use of external tools like databases, APIs, or even browser automation (MCP Playwright).

- the .env file acts like a secure "vault" where you store your credentials, and your code acts like the "courier" that fetches those credentials to prove to Claude that you have permission to talk to it.


----
---





Now run the UI and use the following 

|commands| purpose|
|----|----|
|terminal-setup| gives new lines when we pressed shift+enter
|clear | clears the terminal screen
|exit | exits the terminal UI and start a new chat by again pressing claude
|clear | clears the terminal screen
|resume | resumes the last chat session
esc twice |reqind to previous point in session
|compact | small summary of chat and context
|init | initializes the project with claude.md file which contains all the notes of full project.
|#| to memorize the point that we have written
|@|provide the path 

- Promptinng commands for it to make sure it directly make changes in particualr folder instead of checking all
    
- For an Image we can write like can you update this image with some specific look and providign the path of the file that it needs to particualr
- Tools and permission it only takes cares of what tools to use in which place and reads it automaticyally
- When we provide yes and dont ask always for any particular question then it automatically creates settings.local.json file under claude

## Planning and Thinking:
It will write full plan for the project and how to proceed with it. We can write think or think harder and do the work then it thinks

## Slash commands
/ we can use for existing commands but we can also create a customized command 
First create a commands folder under .claude folder with some name.md file and write the command description there

 Whatever the file name we provided if we search with / we can see and enter it
 we this we can see a card componet
 now in terminal 
```bash
 /ui|comp card | description..
```

 ## MCP and MCP servers:
 In this it can connect to external database, sources or apis and get the work done

 So the flow is like

   - Claude code -->  MCP Server  (will have list of tables and all) --> Database or external source
   - MCP sever - Playwright - browser automation capabilities inclduign takign screenshots
   
## claude with github: Installing claude code on github
/install-github - to install github

Go to the github and add issue and add the text description of the issue and adding comment

      @claude - can u fix this
      !       - gives u to bash mode
      ?       - for shortcuts




## 1. The Role of the API Key
When you send a request, the API key serves two main purposes:

- Authentication: Proving who you are.

- Authorization: Confirming you have permission to access specific data or features (and often tracking how many requests you've made so they can bill you or rate-limit you).

## 2. Why the .env File?
You are 100% right about putting it in a .env file. This is a "Best Practice" for a few critical reasons:

- Security: If you "hard-code" the key directly into your script (e.g., const apiKey = "12345-ABC") and then push your code to GitHub, everyone in the world can see and use your key. They could run up a massive bill on your account.

- Environment Switching: You might use a "Test" key for development and a "Live" key for your actual website. The .env file makes it easy to swap them without changing the actual code.

- Standardization: Most modern frameworks (React, Node, Python, etc.) are built to automatically look for a .env file to load "secrets."

## How the Flow Actually Works
Here is a quick look at the "journey" of that key:

- Storage: You save API_KEY=your_secret_value in the .env file.

- Ignorance: You add .env to a .gitignore file so it never leaves your computer.

- Loading: Your code uses a library (like dotenv) to pull that value into memory.

- Request: Your code attaches that value to the "Header" or "Query Parameter" of your web request.

- Validation: The server receives the request, checks the key against its database, and says, "Cool, come on in."



## Claude commands

- We have to give correct guidance to the claude to make it work properly and give us the correct output. We can use some commands to make it work in a better way and get the output in a better way.

- /init - it will take look at all the project. (project level) - claude.md, can be shared to the team

  - Now it will finalize everything and put it in claude.md file and it will be like the main file for the project and it will have all the notes and everything about the project.
    - In this .md file it has 2 things - one is it has all details of project so that it is easier to go to the respective file and do a work. Second we can give some commands to claude using this file

- claude.local.md - not shared or not commited and we can put our personal instructions or notes for claude.
- claude/claude.md- global file common for whole project
- We can edit the claude.md file and add our instructions too using few symbols like # and @


[Next: [Interview Questions](interview.md)                                   [Back to Table of Contents](index.md)