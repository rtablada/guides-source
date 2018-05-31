This 101 tutorial covers the fundamentals of Ember.js, building up a contact
manager app from scratch! We'll be covering all of the core concepts and tools
necessary for you to start using Ember to build ambitious web apps, including:

* The CLI
* Components
* Services
* Routes
* Styles
* and Tests

This tutorial is meant to build a minimal Ember.js app progressively, starting
small and adding concepts over time. If you're new to Single Page App
development, this is the best guide to start with. If you're comfortable
building apps in the browser and want to get up to speed with Ember quickly,
check out the [Super Rentals Tutorial](../../tutorial/ember-cli) for something more fast paced.

## What You'll Build

Here's a mockup of the design we'll be shooting for as we build the Rolodex
application:

This app is meant to be allow users to manage and search through their contact
information quickly and easily. We'll want this app be able to:

* Show a list of contacts
* Search through contacts
* Show a detail view of the selected contact
* Add, edit, and delete contacts

We'll build out each of the these features step-by-step using Ember's core
concepts. If you get stuck at any point or would like to jump ahead to the
finished product, checkout [the github repo]( https://github.com/ember-learn/ember-rolodex).
Each step of this guide is a single commit on the master branch of the repo,
allowing you to follow along

## Prerequisites

This guide is written with the assumption that you know some basic HTML, CSS,
and Javascript. If you haven't ever used any of these before, you should
probably start with guides for those (note: Add recommendations here)

You'll need to have Git, Node.js, and NPM installed on your computer for this
tutorial. Checkout the [Installing Ember](../../getting-started) guide for more
details.

You'll also need a modern web browser. We'll be making use of some of the latest
web APIs throughout this tutorial, like `fetch`. The latest version of Chrome,
Firefox, Microsoft Edge, or Safari should work just fine.

## Bootstrapping a New App

If you haven't already, install Ember-CLI:

```sh
npm install -g ember-cli
```

Ember-CLI is the Ember command line tool. It provides a ton of helpful features,
like generators and blueprints for new applications. It also wraps the Ember
build system which builds and packages your app for distribution. While it is
possible to set everything up on your own by including scripts or using other
build tools, its not recommended.

Once the CLI is installed, you can generate a new Ember application by running
`ember new` and providing an application name:

```sh
ember new ember-rolodex
```

A new project we be generated inside your current directory. Let's check it out:

```sh
cd ember-rolodex
```

## Directory Structure

The `new` command generates a project structure with the following files and
directories:

```text
├─ /app
├─ /config
├─ /node_modules
├─ /public
├─ /tests
├─ /vendor

<other files>

ember-cli-build.js
package.json
README.md
testem.js
```

Let's take a look at the folders and files Ember CLI generates.

**app**: This is where your app code is going to live. You'll see folders and
files here for things we'll be covering in this guide like:

```text
app
├─ /templates
├─ /components
├─ /services
├─ /routes
├─ /styles
```

as well as some files such as `index.html` and `app.js` which are necessary for
our app. There should also be some folders for other things that we _won't_ be
covering just yet:

```text
├─ /controllers
├─ /models
├─ /helpers
```

Ember CLI assumes that you're going to need these things eventually which is why
it adds these folders up front. If you feel like there's a lot going on in here,
it's because there is, and you can hide or remove the folders that we won't be
going over to clean things up and focus in for the time being. They're just
folders, and you can always add them back!

**config**: The config directory is the place for various types of
configurations, such as environment configs (development, testing, production,
etc) and deployment configs. We won't be diving into this area during the course
of this tutorial.

**node_modules / package.json**: This directory and file are from `npm`. `npm`
is the package manager for Node.js, which Ember-CLI to build Ember apps. Apps can
also include dependencies via Ember addons, which are packaged and distributed
through `npm`. The `package.json` file maintains the list of current `npm`
dependencies for the app. Packages listed in `package.json` are installed in the
`node_modules` directory.

**public**: This directory contains assets such as images and fonts.

**vendor**: This directory is where front-end dependencies (such as JavaScript
or CSS) that are not managed by NPM go.

**tests / testem.js**: Automated tests for our app go in the `tests` folder,
and Ember CLI's test runner **testem** is configured in `testem.js`.

**ember-cli-build.js**: This file describes how Ember CLI should build our app.
We'll be using the default configuration for this tutorial, but if you ever need
to customize your apps build process, this will be one of the places to start.

## The Development Server

Once we have a new project in place, we can confirm everything is working by
starting the Ember development server:

```sh
ember serve
```

or, for short:

```sh
ember s
```

If we navigate to [`http://localhost:4200`](http://localhost:4200), we'll see the default welcome screen.
When we edit the `app/templates/application.hbs` file, we'll replace that content with our own.

![default welcome screen](/images/ember-cli/default-welcome-page.png)

The first thing we want to do in our new project is to remove the welcome screen.
We do this by opening up the application template file located at `app/templates/application.hbs`.
You should see something like this:

```handlebars {data-filename="app/templates/application.hbs"}
{{!-- The following component displays Ember's default welcome message. --}}
{{welcome-page}}
{{!-- Feel free to remove this! --}}

{{outlet}}
```

The welcome page is being created by the `{{welcome-page}}` component that is in
this template. We'll return to components in a little bit, for now we can remove
this and the comments that surround it. You'll also notice an `{{outlet}}` tag -
this is how the Router yields to subroutes in an application. We'll be returning
to that later as well, go ahead and remove it as well for the time being.

Now we should have a completely blank canvas to build our application on!

```handlebars {data-filename="app/templates/application.hbs" data-diff="-1,-2,-3,-5"}
{{!-- The following component displays Ember's default welcome message. --}}
{{welcome-page}}
{{!-- Feel free to remove this! --}}

{{outlet}}
```
