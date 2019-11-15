
# Putting squirrels in a container
In today's lab we'll package an rmarkdown document into a Docker container shared on Binder. The squirrels.Rmd document is similar to the report from lab 6 (but now also has a topic modeling analysis). Note that you do *not* need to knit this document or install the required libraries locally in order to proceed.

## Install holepunch
Holepunch is not on CRAN (yet) so we need to install it from GitHub instead. To do this, first install the "remotes" package, and then run in the R console:
```{r}
remotes::install_github("karthik/holepunch")
```
Then run `library(holepunch)` to import the functions we'll need.

## Create Description file
Create a template Description file using holepunch (filling in the strings with something more meaningful):
```{r}
write_compendium_description(package = "Your compendium name", 
                             description = "Your compendium description")
```
Open the new Description file and inspect it. Note that the dependencies in the Rmd file (from library() calls) have automatically been added. Fill in the other fields which are currently filled with only placeholder information, e.g. title and author (you don't have to include your ORCID if you don't want to).

At this point we could actually skip right to the Binder section below (Binder only requires a Description file) but we're going to go farther and define a full Docker image.

## Create Dockerfile
Use holepunch to generate a Dockerfile using:
```{r}
write_dockerfile(maintainer = "Your name") 
```
This creates a Dockerfile inside a hidden directory .binder/. Inspect this Dockerfile to see what it looks like (you may need to use terminal to get inside this directory). Note the first FROM line - what is its purpose? The rest of the Dockerfile contains instructions to copy all the files in this directory into the image, and also to install all dependencies in the Description file.

If you have Docker installed on your computer, you can build a Docker image from this Dockerfile using the command `docker build -t image_name .`. To simplify this lab, we'll instead have Binder build this image for us in a couple steps.

## Generate badge, push, make public
To make it easier for us and others to run this Docker image on Binder, we can generate a "badge" link. Use `generate_badge()` to create a badge that can be pasted into this Readme.
Paste generated code here:



Now add all the files in this directory to your git repository (remember to include the .binder/Dockerfile as well - use `git status` to confirm you got everything) and push to GitHub. Then in your browser go into the Settings of your GitHub repo and make it public, so that it will be visible to Binder.

## Build Docker image on Binder
View this Readme in your browser. The bade code you pasted in should look like a button - click it! This will start building your Docker image on Binder. It will be a slow process the first time, so click the "Show" option to see the log of what is happening internally. In the future, this image will be pre-cached and just need to be started, which will be much faster. Once it is done, this will open up what looks like RStudio (but in your browser). Open squirrels.Rmd and Knit! Note that you do *not* need to have R installed on your computer at all for this to work - R is running inside the Docker container on Binder's servers.

