For GIT HUB pages, we need to create a repository named `<username>.github.io`, where `<username>` is your GitHub username. This repository will host your Jekyll site.
To create the repository, follow these steps:
1. Log in to your GitHub account.
2. Click on the "+" icon in the upper right corner and select "New repository".
3. In the "Repository name" field, enter `<username>.github.io`.
4. Optionally, add a description for your repository.
5. Choose the repository visibility (public or private).
6. Click on the "Create repository" button. 
Now do the following to set up Jekyll:
   brew install ruby
   gem install bundler jekyll
   jekyll -v - to check the version
Crete a new Jekyll site:
   jekyll new myblog
   cd myblog
   bundle exec jekyll serve
Now open your browser and go to `http://localhost:4000` to see your new Jekyll site running locally.
To deploy your Jekyll site to GitHub Pages, follow these steps:
1. Initialize a git repository in your Jekyll site directory:
   git init
2. Add your GitHub repository as a remote:
   git remote add origin https://github.com/<username>/<username>.github.io.git 
3. Add all files to the staging area:
    git add .
4. Commit the changes:
    git commit -m "Initial commit"
5. Push the changes to the GitHub repository:
    git push -u origin main
Your Jekyll site should now be live at `https://<username>.github.io`.  
Make sure to replace `<username>` with your actual GitHub username in the commands above.