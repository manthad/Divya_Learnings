Install Claude using below command: under the project main directory
npm install -g @anthropic-ai/claude-code
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

- @claude can u fix this

- ! gives u to bash mode

- ? for shortcuts



