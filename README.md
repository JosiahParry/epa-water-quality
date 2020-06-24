
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Deploying R Content to Connect

There are three ways to publish R content to RStudio Connect:
push-button deployment from the RStudio IDE, from git, or from the R
console.

The deployment of R content uses the functionality in the
[`rsconnect`](https://github.com/rstudio/rsconnect) package.

The way that RSC works is by mimicking the R environment that the
application or piece of content was developed in. In order to do this
RSC needs to know what the environment looked like. `rsconnect` creates
a `manifest.json` file. This manifest document enumerates all of the
packages used, their versions, and their sources. With this information
RSC will install the packages if needed and create a sandboxed R process
that mirrors that of the development environment. This ensures that
there will be no package version conflicts.

Now, when we deploy content using the IDE, RStudio infers the
application files (or we specify them manually using the GUI), then
builds a `manifest.json` for us, and then packages the R files and any
other associated files into a *bundle*. The bundle is received by RSC
and the piece of content is recreated and deployed.

## Manual Deployment

To deploy content manually from the R command line, we will use the
function `rsconnect::deployApp()`. If you’ve connected your account to
RSC via `Tools > Global Options > Publishing`, you will not need to
manage credentials through the package. `deployApp()`, at minimum
requires three things: a `manifest.json` file, the `server` name as a
scalar character, and the files needed for the application supplied as a
character vector to the argument `appFiles`.

We build the manifest using `rsconnect::writeManifest(appFiles =
c("app.R"))`. Depending on how many files are used and their relative
size, this can take anywhere from a few seconds to a few minutes.

Once the manifest has been created we can deploy the piece of content
using `deployApp()`. `deployApp()` deploys from the current working
directory. If you are deploying from a different directory than your
working directory specify that in the `appFiles` paths or set the
working directory with the `appDir` argument.

``` r
rsconnect::deployApp(appFiles = c("app.R", "data/water_quality.csv"), server = "server-name")
```

## Git backed deployment

For a thorough description of Git backed deployment visit the [user
guide](https://docs.rstudio.com/connect/user/git-backed/).

Deploying from git requires RSC v1.7.6+ (I always recommend being on the
most up to date version of the product). Again, a manifest file is
required at the root of the application directory—i.e. the
`manifest.json` must be at the same level as the app files. Once the
manifest has been created, navigate to the RSC content page. Click the
blue publish button and press `Import from Git`.

![](https://docs.rstudio.com/connect/user/images/screenshots/import-from-git.png)

Following you will be prompted to specify which branch and piece of
content you’d like to deploy (assuming there is more than one
`manifest.json` file present), et voila\! The deployment process has
began.

See this [blog
post](http://josiahparry.com/post/2019-08-09-epa-waterquality/) for a
walk through of the code.
