# Deploy Go Application on Heroku

Guide for deploying the Go web app to Heroku from a Linux (Debian/Ubuntu) machine.
You have to create an account on Heroku, if you haven't already.
We will only use the free tier of Heroku, and we don't need to add any credit card.

## Set Up

### Install Git

Install `git`:

```bash
$ sudo apt install git
```

Configure `git`:

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

### Install Heroku CLI

Install Heroku CLI For Debian/Ubuntu:

```bash
$ curl https://cli-assets.heroku.com/install-ubuntu.sh | sh
```

Log in:

```bash
$ heroku login
```

You will be redirected to a browser page, where you will have to log in with your Heroku account.

## Prepare the app

### 1. Track your codebase in a Git repository

Already configured with a Git repository.

If your app is not, follow the guide [Deploying with Git - Tracking your app in Git](https://devcenter.heroku.com/articles/git#tracking-your-app-in-git):

```bash
$ cd myapp

$ git init
Initialized empty Git repository in .git/

$ git add .

$ git commit -m "My first commit"
Created initial commit 5df2d09: My first commit
 44 files changed, 8393 insertions(+), 0 deletions(-)
 create mode 100644 README
 create mode 100644 Procfile
 create mode 100644 app/controllers/source_file
...
```

### 2. Add a Heroku Git remote

You deploy your app by pushing its code to a special Heroku-hosted remote that’s associated with your app. The heroku create CLI command creates a new empty application on Heroku, along with an associated empty Git repository.

Run this command from your app’s root directory, the empty Heroku Git repository is automatically set as a remote for your local repository:

```bash
$ heroku create
Creating app... done, ⬢ pure-wildwood-51621
https://pure-wildwood-51621.herokuapp.com/ | https://git.heroku.com/pure-wildwood-51621.git
```

Check the new remote:

```bash
$ git remote -v
heroku  https://pure-wildwood-51621.herokuapp.com (fetch)
heroku  https://pure-wildwood-51621.herokuapp.com (push)
origin  git@github.com:thanoskoutr/url-shortener.git (fetch)
origin  git@github.com:thanoskoutr/url-shortener.git (push)
```

Deploy the code to `heroku` remote:

```bash
$ git push heroku main
```

- Use this same command whenever you want to deploy the latest committed version of your code to Heroku.
- Note that Heroku only deploys code that you push to master or main. Pushing code to another branch of the `heroku` remote has no effect.

## Deploy App

After the `git push` on the `heroku` branch, the application is now deployed.

Visit the app at the URL generated by its app name.

As a handy shortcut, you can open the website as follows:

```bash
$ heroku open
```

## Free Dyno Hours

Every personal accounts are given a base of 550 free dyno hours each month. If an app has a free web dyno, and that dyno receives no web traffic in a 30-minute period, it will sleep. In addition to the web dyno sleeping, the worker dyno (if present) will also sleep.

To check the amount of free dyno hours remaining by using the CLI:

```bash
$ heroku ps -a pure-wildwood-51621
Free dyno hours quota remaining this month: 550h 0m (100%)
Free dyno usage for this app: 0h 0m (0%)
```

You can also view this on Dashboard's [billing page](https://dashboard.heroku.com/account/billing), which is refreshed daily to display the updated amount.

## Define config vars

At runtime, config vars are exposed as environment variables to the application. Your application is already reading one config var, the `$PORT` config var. `$PORT` is automatically set by Heroku on web dynos.

A web dyno must bind to its assigned `$PORT` within 60 seconds of startup. If it doesn’t, it is terminated by the dyno manager.

## Rename Heroku App from CLI

You can rename the app's random name given by Heroku to the one you want (if available), with the `heroku apps:rename` command.

To rename this application:

```bash
$ heroku apps:rename url-shortener-thanoskoutr
Renaming pure-wildwood-51621 to url-shortener-thanoskoutr... done
https://url-shortener-thanoskoutr.herokuapp.com/ | https://git.heroku.com/url-shortener-thanoskoutr.git
 ▸    Don't forget to update git remotes for all other local checkouts of the app.
Git remote heroku updated
```

When you rename an app, it immediately becomes available at the new corresponding `herokuapp.com` subdomain and unavailable at the old one.

### Updating Git remotes

If you use the Heroku CLI to rename an app from inside its associated Git repository, your local Heroku remote is updated automatically. **However, other instances of the repository must update the remote’s details manually.**

If you have the repository in other places, update the remote, manually:

```bash
$ git remote rm heroku
$ heroku git:remote -a newname
```

## Links

- [Getting Started on Heroku with Go](https://devcenter.heroku.com/articles/getting-started-with-go)
- [Heroku Go Support](https://devcenter.heroku.com/articles/go-support)
- [The Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
- [Preparing a Codebase for Heroku Deployment](https://devcenter.heroku.com/articles/preparing-a-codebase-for-heroku-deployment)
- [Deploying with Git](https://devcenter.heroku.com/articles/git)
- [Free Dyno Hours](https://devcenter.heroku.com/articles/free-dyno-hours)
- [Renaming Apps from the CLI](https://devcenter.heroku.com/articles/renaming-apps)