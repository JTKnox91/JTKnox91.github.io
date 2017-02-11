---
layout: post
title:  "Setting up Ghost/Buster"
date:   2015-07-26 00:00:00 -0800
categories: jekyll update
---

## What it Does

In short, this article will show you how to set use the
[Ghost](https://ghost.org/) blogging platform's user interface to make
static html pages upload to a github.io (see [github
pages](https://pages.github.com/)) repository.

**Note:** This tutorial is for OSX (I'm using Mavericks). Not all steps
may work for Windows.

**Update:** As you may have noticed, I moved my blog to jekyll. There's a funny story behind the switch, but fear not, Ghost still works. And technically this content was written in a Ghost editor before being moved.

## What you Need

####Ghost

[Ghost](https://ghost.org/)

###### Also:

* [Node.js](https://nodejs.org/)

#### Buster

[Buster](https://github.com/axitkhurana/buster)

###### Also:

* [pip](https://bootstrap.pypa.io/get-pip.py)
* [homebrew](http://brew.sh/)
* wget
* git

## Step by Step

### Step 0

#### The Terminal

*If you have done any work with the command line, like ever, feel free to skip this.*

###### The Terminal {#theterminal}

* [This](http://blog.teamtreehouse.com/introduction-to-the-mac-os-x-command-line) article provides teaches some great command lines basics, and
* [this](http://osxdaily.com/2013/02/05/improve-terminal-appearance-mac-os-x/) article teaches some ways to cusomize the terminal display. I'll just cover the commands we'll be using for the project.
* The Default^[1](index.html#fn:1)^ OSX Terminal can be found in "Applications" &gt; "Utilities" &gt; "Terminal". Open it.
![img](http://blog.teamtreehouse.com/wp-content/uploads/2012/09/Screen-Shot-2012-09-25-at-12.57.00-PM.png)
* Congratulations! You are now a command line warrior.

### Step 1

#### Ghost Pre-reqs: (Node.js)

*This step will only cover the installation of Node.js, so again, feel free to skip.*

![img](http://nodejs.org/images/logos/nodejs.png)

* [Here](https://nodejs.org/) is the Node.js website
* Download and install. Follow their directions... yada yada.

### Step 2

#### Ghost

*If you already have and are comfortable using the ghost platform on a local machine, skip to Section 3*

![img](http://www.webdesignerhub.com/wp-content/uploads/ghost-logo.jpg)

###### What is Ghost?

* If you're still confused about what "Ghost" is. Its an elegantly simple blogging platform, used to make content like what you're reading right now.
* [Here](https://www.youtube.com/watch?v=6rP5R5bIJk0) is their promo video.

###### Installing

If you want more detail, I'm paraphrasing [these](http://support.ghost.org/installation/) steps. (Note that you already have Node.js if you followed step 1).

* [Download](https://ghost.org/download/) Ghost.
* Unzip the "ghost-\#.\#.\#" folder you downloaded.
* (I would suggest moving the unzipped version outside of your downloads folder, into another folder for your blog, since you will need to access it whenever you want to write or edit a post.)
* Open a terminal window, and navigate to the unzipped ghost folder.
* Enter $`npm install --production`.

###### Launching Ghost

* (From the same folder as above) Enter $`npm start`.
* (Note: the `npm start` command from that folder is how you will start up the Ghost platform whenever you want to write or edit a post. Get used to it.)
* You can now visit "your blog" at http://localhost:2368 (or http://127.0.0.1:2368)
* The nice folks at Ghost give you a starting post that is literally a tutorial on Ghost markdown styling. Once you log in, you can delete it, and just use [this](http://support.ghost.org/markdown-guide/) guide for reference.

###### Setup

* Add a "/ghost" to your url, so [http://localhost:2368/ghost](../ghost).
* You now have a login menu to enter your Ghost account info.
* Now you can make and edit posts. Spend as much time as your want playing with the markdown or making tests posts. I would suggest bookmarking [http://localhost:2368/ghost](../ghost) for convenience.
* When you're done, you can type Ctrl+C in the same terminal window you started ghost to end it.

### Step 3

#### Buster Pre-reqs: (pip, wget, homebrew)

*If you already have these packages, skip to step 4*

###### So many Letters! What are these things?

Good question! To elaborate:

* **We will use Pip to install Buster.** Pip is a package manager for python (the language). Buster is a python application. We will also need a python interpreter, but I will explain how to get that too.
* **We will use Homebrew to install wget.** Hombrew is a package manager for OSX. Its useful to have in other circumstances, but today we'll just use it to install wget (and git, if you don't have that already).
* **Buster will use wget to retrieve information from Ghost.** Wget is a program for retrieving information though http, and also just a useful thing to have for future projects.

###### Homebrew

* Visit <http://brew.sh/>
* There will be long command line argument that you need to paster into a terminal window.
* Follow the directions from the Homebrew Installer

###### wget and git

* After installing homebrew, use these two command line arguments in a terminal:
  * $`brew install wget`
  * $`brew install git` If you don't already have it

###### pip

* If you don't already have Python Installed on your machine:
  * Vistit <https://www.python.org/>
  * Under "Downloads" pick a version and install it.
  * (For this project either version 2 or 3 works).
* Go to <https://bootstrap.pypa.io/get-pip.py>
* Paste that text into a python editor and run it (F5).
* You now have pip.

### Step 4

#### Buster

For reference here is the Buster [githubrepo](https://github.com/axitkhurana/buster).

###### Installing {#installing}

* In a terminal, $`pip install buster`
* Done.

###### Setup {#setup}

* If you haven't already, make a [github page](https://pages.github.com/) (not to be confused with your github account's page).
  * **Note:** The github page directions might have you make a remote to your \[name\].github.io. However Buster will make its own remote to that repo (in a folder called "static"). You may want to delete your original remote to keep things simple.
* We also need to configure a few things in Ghost, so that it makes internal links to your github page, rather than localhost.
  * In your Ghost folder (the one from inside you do `npm start`) look for a "config.js", and open in in a text editor.
  * In `Config{}`, change `http://my-ghost-blog.com` in `production{}` and `http://localhost:2368` in `development{}` to your github page name. Example for this page:

    config = {
            // some comments
            production: {
                url: 'http://jtknox91.github.io'
                ... //other config settings
            },
            development: {
                //more comments
                url: 'http://jtknox91.github.io',
                ... //more config settings
            },         
       },

* Folder Organization
  * Make a folder, "my-blog" (or whatever name you want).
  * Move the "Ghost" folder into "my-blog".
  * Create an empty folder called "static" (spelled exactly like that, buster will make it if its not there).
  * I'll explain using this organization during Step 5.
* Finally we can configure buster
  * From "my-blog" run $`buster setup`
  * Enter your url, ie:`http://www.github.com/jtknox91.jtknox91.github.io`

![img](https://camo.githubusercontent.com/8c5dba0b1e86a90d708cf3fc3436cc0198b9ff7e/687474703a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f656e2f632f63372f47686f7374627573746572735f636f7665722e706e67)

### Step 5

#### Who ya gonna call?

###### Making content

* From a terminal you can navigate to "my-blog." $`cd ghost-#.#.#` and $`npm start`.
* In a browser, visit [http://localhost:2368/ghost](../ghost). Edit your blog, make new posts, etc.
* Make sure to "Publish" all of your posts instead of just saving as drafts.

###### Generating static html

* From a new terminal window (with Ghost still running), again navigate to "my-blog" (the parent folder, not the "Ghost" folder).
* Do $`buster generate` (Buster well then do a massive wget and copy/paste script).
  * Note: By default buster will look for html at http://localhost:2368. If you configured your Ghost to serve at different port, see Buster's documentation on github.

###### Previewing (optional) {#previewingoptional}

* From the same folder, run \$`buster preview`
* In a web broswer, visit <http://localhost:9000>
* If you want to change something, do in in the ghost editor, then run $`buster generate` again. Otherwise...

###### Publishing

* From the same parent folder, run $`Buster deploy`
* (Same as `cd static && git push origin master`)

###### Manual edits {#manualedits}

* Buster is still a work in progress. I've had a few issues with it (mostly with meta-data), but I am overall very satisfied.
* To manually edit your blog content (without Ghost or Buster) you can navigate to "my-blog/static", make file edits in a text editor, then $`git push origin master`.

#### Good luck, have fun.