---
title: "Develop a Slack-bot using Golang - Part 1"
date: 2022-11-13T19:51:39+01:00
draft: true
author: "Ahmed"
tags:
  - Markdown syntax
  - Sample
  - example
image: /images/goslackbot.jpg
description: ""
toc: 
---


In this first part of the blog we will explore how to create a simple slack bot which send scheduled updates to a slack channel only using the net/http package from the go standard library. 
<!--more-->


## Setup a slack workspace

First, you will need to create a workspace on Slack. visit [Slack](https://slack.com/) and press ***Create a new workspace***
<br></br>

<img src="/images/slackbotblog/createWorkSpace.png" alt="create slack workspace" style="max-width: calc(100% - 20px);"/>
<br></br>
Once you finish creating a work space, we can now create a slack app. 

## Create a slack app
Go to the [Slack api website](https://api.slack.com/apps/) and click <mark> create new app</mark> then choose <mark> From scratch </mark> from the dialog menu
<br></br>
<img src="/images/slackbotblog/createapp.png" alt="create slack app" style="max-width: calc(100% - 20px);"/>
<br></br>
Give your app a name and choose your workspace and click <mark> Create App </mark>
<br></br>
<img src="/images/slackbotblog/03.png" alt="app name and workspace" style=" display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;"/>
<br></br>
## Set Oauth scopes

In <mark>Add features and functionality</mark> click on <mark>Bots</mark> after that click on <mark> Review scopes to add </mark> 
Scopes govern what the app can do. If our Bot, for example, needs to be able to read messages from the workspace, we need to explicitly give it a permission to do so. For now, we only need the following two scopes 

* chat:write.public
* chat:write

<img src="/images/slackbotblog/04.png" alt="Oauth scopes" style=" display: block;
  margin-left: auto;
  margin-right: auto;
  width: 80%;"/>

the <mark> chat:write </mark> scope allows our Bot to send messages to channels. the the <mark> chat:write.public </mark> scope allows it to send messages to channels the bot isn't member of.

Now that everything is setup we can proceed by installing the app to our workspace. 

<img src="/images/slackbotblog/05.png" alt="Install app to workspace" style=" display: block;
  margin-left: auto;
  margin-right: auto;
  width: 80%;"/>

After clicking on <mark> Install to Workspace </mark> you will be redirected to a page to choose which workspace you would like to install the app to. choose the workspace and clock allow. Now in  **OAuth & Permissions** you should be able to see a ***Bot User OAuth Token***

Now we actually have everything we need in order to start writing our bot program in Go. 

## Connecting to Slack from Golang

We first create a new golang package and and install the godotenv library which allows us to load envrionment varaibles from a .env file 

``` sh
mkdir slackgobot && cd slackgobot
go mod init github.com/adel-habib/slackgobot
go get -u github.com/joho/godotenv 
```
We now create a file and name it .env
``` sh
touch .env
```
in the file we store both the token and id of the channel we want to send messages to
the file should look like this
``` text
API_TOKEN = xoxb-4123701175729-438sofjko32640-kDPSSdKAig899238OrBOGUibQH
CHANNELID = C0436368FTP
```


### Code block with Hugo's internal highlight shortcode

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use *Markdown syntax* within a blockquote.


### Blockquote with attribution


> Don't communicate by sharing memory, share memory by communicating.</p>
> — <cite>Rob Pike[^1]</cite>


[^1]: The above quote is excerpted from Rob Pike's [talk](https://www.youtube.com/watch?v=PAAkCSZUG1c) during Gopherfest, November 18, 2015.

## Tables

Tables aren't part of the core Markdown spec, but Hugo supports supports them out-of-the-box.

   | Name  | Age |
   | ----- | --- |
   | Bob   | 27  |
   | Alice | 23  |

### Inline Markdown within tables

| Inline&nbsp;&nbsp;&nbsp; | Markdown&nbsp;&nbsp;&nbsp; | In&nbsp;&nbsp;&nbsp;                | Table  |
| ------------------------ | -------------------------- | ----------------------------------- | ------ |
| *italics*                | **bold**                   | ~~strikethrough~~&nbsp;&nbsp;&nbsp; | `code` |

## Code Blocks

### Code block with backticks

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
```
### Code block indented with four spaces

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Example HTML5 Document</title>
    </head>
    <body>
      <p>Test</p>
    </body>
    </html>

### Code block with Hugo's internal highlight shortcode
{{< highlight html >}}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
{{< /highlight >}}

## List Types

### Ordered List

1. First item
2. Second item
3. Third item

### Unordered List

* List item
* Another item
* And another item

### Nested list

* Item
  1. First Sub-item
  2. Second Sub-item

## Headings

The following HTML `<h1>`—`<h6>` elements represent six levels of section headings. `<h1>` is the highest section level while `<h6>` is the lowest.

# H1
## H2
### H3
#### H4
##### H5
###### H6

## Other Elements — abbr, sub, sup, kbd, mark

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd><kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Delete</kbd></kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.