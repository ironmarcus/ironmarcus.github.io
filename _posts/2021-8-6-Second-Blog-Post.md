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

Docker is great for deploying and sharing your projects with others. Check out the following links to learn more(Links can be found in References):

1. [https://jsta.github.io/r-docker-tutorial/](https://jsta.github.io/r-docker-tutorial/)
2. [https://juanitorduz.github.io/dockerize-a-shinyapp/](https://juanitorduz.github.io/dockerize-a-shinyapp/)
3. [https://blog.sellorm.com/2021/04/25/shiny-app-in-docker/](https://blog.sellorm.com/2021/04/25/shiny-app-in-docker/)

Now assuming you have the following project folder structure below:

```
your-project.Rproj
your-script.R
README.md
```

You would need to create new R files for the R shiny application. Create one R file called `ui` and another one caleld `server`. The output of the files should look something like this:

```
ui.R
server.R
```

Once that is done, we need to create a Dockerfile.

## Docker

To create a docker image, we would need to create a Dockerfile. To do this, click on the plus icon on the top left corner on R studio, and click on Text File. Then name the Text File to Dockerfile and make sure it is in the same directory as the `ui.R` and the `server.R`.

Now your Dockerfile should contain something like this:

```
# get shiny serves plus tidyverse packages image
FROM rocker/shiny-verse:latest
# system libraries of general use
RUN apt-get update && apt-get install -y \
    sudo \
    pandoc \
    pandoc-citeproc \
    libcurl4-gnutls-dev \
    libcairo2-dev \
    libxt-dev \
    libssl-dev \
    libssh2-1-dev 
# install R packages required 
# (change it dependeing on the packages you need)
RUN R -e "install.packages('shiny', repos='http://cran.rstudio.com/')"
RUN R -e "install.packages('shinydashboard', repos='http://cran.rstudio.com/')"
RUN R -e "install.packages('DT', repos='http://cran.rstudio.com/')"
RUN R -e "install.packages('plotly', repos='http://cran.rstudio.com/')"
# copy the app to the image
COPY r-shiny-dashboard.Rproj /srv/shiny-server/
COPY ui.R /srv/shiny-server/
COPY server.R /srv/shiny-server/
# select port
EXPOSE 3838
# allow permission
RUN sudo chown -R shiny:shiny /srv/shiny-server
# run app
CMD ["/usr/bin/shiny-server.sh"]
```

Note: I am still in the process of getting my Dockerfile to work properly since I got an error

Now in order to build the docker image, we can run on the console: `docker build -t my-shiny-app .`. And to create a container just run: `docker run --rm -p 3838:3838 my-shiny-app`.

From here, find the terminal tab and run the following git commands from above to add your new files to the Github repository.

# Setting up Github actions with the Docker image for the R shiny application

Once you get your dockerfile with your R shiny application files onto Github, go to Action. From here, click on the "Set up this workflow" for Docker Image.

Note: I need to do some more research on the Docker Image and Containers.

Once that is done, click on `start commit`, and docker-image.yml file should be created. Don't worry too much about the details, since I plan on talking about this in another blog post. 

Now, if you go back to Actions, you should see a log saying `Create docker-image.yml`. If you click on that and click on the build, you can see that it is building the docker image.

# Sidenote

Other than that, I need to do more research on this Github Actions optins for docker image and docker containers. Also, I need to figure out why I am getting an error for running for a dockerfile.



# References:

1. https://jsta.github.io/r-docker-tutorial/
2. https://juanitorduz.github.io/dockerize-a-shinyapp/
3. https://blog.sellorm.com/2021/04/25/shiny-app-in-docker/





