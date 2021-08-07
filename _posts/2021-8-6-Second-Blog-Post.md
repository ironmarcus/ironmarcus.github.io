---
layout: post
title: Connecting a Github repo to a R project, Dockerize R Shiny application, and Github actions
---

For this blog post, I plan on discussing the following topics:

1. Connecting a Github repository to a R project
2. Writing a Dockerfile for a R Shiny application
3. Setting up Github actions with the Docker image for the R shiny application

# Connecting a Github repository to a R project

In order to get the R project to Github, first go to Github and log in. Once logged in, click on the `+` icon and click New Repository. From here, make a repository name and click on "Add a README file". Then, click on the green code icon and copy the https. It should look something like this: `https://github.com/your-github-username/your-github-repository.git`. You should copy the https.

Now open up R studio. On the top left hand corner, click `File`, then `New Project`. There should be three options: `New Directory`, `Existing Directory`, and `Version Control`. Click on `Version Control`. Then hit `Git`. Now paste your https onto Repository URL and the Project directory name should prepopulate. Now if you don't like where the subdirectory is for where the R project is being stored, you can change the directory by clicking on browsing. Once that is done, click on `Create Project`.

If you can get through all the steps mentioned above, you have successfully connected the Github repository to the R project. 

## Adding R files to the R project and sending R files to the Github repository

If you want to add R files, you would first create the R files:

* R scripts(ui.R, server.R, app.R, etc.)
* R markdown
* etc.

From here, find the terminal tab and run the following commands to add your new files to the Github repository:

`git add -A` - Add the changed files

`git commit -m "Whatever message you want to put in here"` - Commit the changed files

`git push` - Push the changed files onto Github.

# Writing a Dockerfile for a R shiny application

# Setting up Github actions with the Docker image for the R shiny application

References:





