---
layout: post
title: 'how to virtualenv'
date: 2017-11-08 23:00:00 +07:00
categories: [doc]
tags: [etc]
---

### Introduction

In this guide, we will be setting up a simple Python application using the Flask micro-framework on Ubuntu 16.04. The bulk of this article will be about how to set up the uWSGI application server to launch the application and Nginx to act as a front end reverse proxy.

### Prerequisites
Before starting on this guide, you should have a non-root user configured on your server. This user needs to have sudo privileges so that it can perform administrative functions. To learn how to set this up, follow our initial server setup guide.

To learn more about uWSGI, our application server and the WSGI specification, you can read the linked section of this guide. Understanding these concepts will make this guide easier to follow.

When you are ready to continue, read on.

Install the Components from the Ubuntu Repositories
Our first step will be to install all of the pieces that we need from the repositories. We will install pip, the Python package manager, in order to install and manage our Python components. We will also get the Python development files needed to build uWSGI and we'll install Nginx now as well.

We need to update the local package index and then install the packages. The packages you need depend on whether your project uses Python 2 or Python 3.

If you are using Python 2, type:

```no-highlight
 sudo apt-get update`
 sudo apt-get install python-pip python-dev nginx
 ```
If, instead, you are using Python 3, type:

```no-highlight
sudo apt-get update
sudo apt-get install python3-pip python3-dev nginx
```
Create a Python Virtual Environment
Next, we'll set up a virtual environment in order to isolate our Flask application from the other Python files on the system.

Start by installing the virtualenv package using pip.

If you are using Python 2, type:

```no-highlight
nsudo pip install virtualenv
```
If you are using Python 3, type:

```no-highlight
sudo pip3 install virtualenv
```
Now, we can make a parent directory for our Flask project. Move into the directory after you create it:

```no-highlight
mkdir ~/myproject
cd ~/myproject
```
We can create a virtual environment to store our Flask project's Python requirements by typing:

```no-highlight
virtualenv myprojectenv
```
This will install a local copy of Python and pip into a directory called myprojectenv within your project directory.

Before we install applications within the virtual environment, we need to activate it. You can do so by typing:

```no-highlight
 source myprojectenv/bin/activate 
```
Your prompt will change to indicate that you are now operating within the virtual environment. It will look something like this > `(myprojectenv)user@host:~/myproject$.`