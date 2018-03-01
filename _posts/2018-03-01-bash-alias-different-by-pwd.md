---
layout: post
title: bash alias, different commands in different directories
excerpt_separator: <!--more-->
---

## Intro
A bash alias is a shortcut for a command, you decide on a few characters that you find meaningful enough to represent your command, and you add them to your `.bashrc` file.  
But why would you want to set bash alias to output different commands based on the current working directory?
<!--more-->
I work on both frontend and backend project, so I have two aliases for them in my `.bashrc` file.  
So I type `bac` short for backend, and `web` short for frontend.  

When I am in the backend project folder, I want to serve the project, I type `php artisan serve`,
 and when I am in the frontend project folder and want to serve the project, I type `gulp serve`.  

So the main idea is that I want to type `serve` in any of the two folders and get the correct command no matter which folder I am currently in.  

I'm on Ubuntu 16.04 LTS, and this is the command I ended up writing to get this working:  

```bash
serve ()
{
    if [[ "$PWD" == "/home/moafak/backend" ]]; then
       php artisan serve
    elif [[ "$PWD" == "/home/moafak/dashboard" ]]; then
       gulp serve
    fi;
}
```

Simple but pretty usefull,  
Enjoy...   