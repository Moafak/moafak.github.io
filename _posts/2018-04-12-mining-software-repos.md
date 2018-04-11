---
layout: post
title: Mining Software Repositories to learn a new framework
excerpt_separator: <!--more-->
---

So here is the idea: a framework is getting hype and you want to learn it. You consult the documentation, read a few short blog posts about how to do X in the cool framework Y.  
But what if you can get some insights on how to _really_ use the framework?  
This is where the idea the script in this blog post came from.  
<!--more-->
For a framework that gained some attention there would (likely) be some GitHub projects that utilize it and built on top of it. So why not see how those projects are actually using the framework?  
Git is not only a tool to make you feel safe that you got your past versions, or that you can blame someone for bad code. But it can also be a knowledge mine if you dig in it.  
The script I made for this purpose is actually simple, it gets the git log of a repository, then gets the changeset for each commit.  

How to use the script:
First you’ll have to clone a repository to your local machine, then run the script:
```bash
python pythonplayground.py
```
It will ask for the path of the git repository, paste it in.  
The output will be an html file that shows some information about the commits (commit sha, author, date, subject), and the files `A`dded or `M`odified in the commit.  

The following html is the output of the script running on cloned [TypiCMS](https://github.com/TypiCMS/Base) repository:  

![html-output](/public/images/mining-software-repositories/html-output.png)


My initial intention was to visualize the output with D3.js, but pandas [DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) makes it easy to write an html output of the DataFrame you built. So that’s what I ended up doing in the script for now.  

[The script](https://github.com/moafak/MSR/blob/22d7a1019280b215e4e32378160007179d20281a/gitplayground.py#L1) is well documented, and should be easy to modify.  

The python libraries used are:  
* [GitPython](https://github.com/gitpython-developers/GitPython) to interact with git  
* [Pandas](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html)  for dataframes  

In later posts we are going to visualize the output in a better way, to make it easier to go through the project history and the changes.  
  
Thanks for reading.  
Cheers.  

