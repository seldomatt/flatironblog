---
layout: post
title: "BASH and The One True Path"
date: 2012-10-08 14:04
comments: true
categories: 
---

<p>As a nuby programmer, you're going to be cozying up to your command line in ways you never before imagined.  You'll be cd'ing, ls'ing, grep'ing, and (carefully) rm -rf'ing your way around your filesystem, and, for Linux and OSX users like me, you'll be using BASH to do it.  BASH is a command processing program that reads the input you type into your command line and does things with it.  You can read more about it <a href="http://en.wikipedia.org/wiki/Bash_(Unix_shell)" target="_blank">here</a>, and <a href="http://www.linuxfromscratch.org/blfs/view/svn/postlfs/profile.html" target="_blank">here</a>, but the real topic of this post is the special little beast known as the <strong>PATH</strong>, so let's get to it.</p>

<img src="../images/path_post_img.jpg" alt="hiker on the $PATH">

<!--more-->
<p>When BASH starts, it creates an environment for your shell session by executing commands from a few different scripts.  These include ~/.bash_profile, ~/.bash_login, and ~/.bash_rc.  Since BASH looks in your .bash_profile first, you should be setting most of your environment variables and methods there (though read up on .bash_login/.bash_rc, because they'll be invoked under certain conditions and, reading is good for you).  Let's take a look in our bash_profile (I'm using SublimeText, hence the 'subl' command, but use whatever editor you've got):</br>
</br>
<strong>$ cd ~</strong></br>
<strong>$ subl .bash_profile</strong></br>
</br>
What you'll likely see (among other things we're going to ignore in this post) is a line that reads PATH = $PATH.  Those of you quicker than myself will instantly recognize that this is setting the variable named PATH.  You can call that variable with $PATH (this is the syntax BASH uses, like var in Javascript).  Jump back into your command line and type in $PATH.  What you'll see printed to the screen is something like this:</br>
</br>
<strong>$ $PATH</strong></br>
<strong>-bash: /bin:/usr/local/bin:/usr/bin:/Users/Username/bin:</strong></br>
</br>
This is your PATH environment variable.  It's a list of directories that BASH will look in for commands, scripts, and programs.  The filepaths to those directories are separated by ':', and the directories are checked in order.  So, when you type 'git' to access the git revision control program from your command line, BASH will first check your ~/bin directory, then your ~/usr/local/bin directory, then ~/usr/bin, and so on.  As soon as BASH finds what it's looking for (in this case, 'git'), it'll execute the command and stop travelling down the PATH.  The PATH environment variable is like a map that BASH consults every time you tell it do something.  The map says 'Go here first, and if you don't find what you're looking for, go here, or else go here, etc., etc...'</p> 

<p>Seems simple enough, and it is, but there are a few things to keep in mind.</p>

<p>BASH, like all programs, is pretty dumb - it only knows what you teach it.  So if you want to execute a script or program from your command line, you're going to need to make sure it's in your $PATH, or else BASH won't know where to look.  I recently ran into an issue when trying to run Postgres.app from the command line.  The documentation told me to add the /Applications/Postgres.app/Contents/MacOS/bin directory to my PATH, so I did.  I was then supposed to be able to run 'psql' from the command line to startup Postgres, but it didn't work.  In what I would like to tell you was a very short amount of time spent running the same command and pulling my hair out, I realized that Postgres.app was still in my /Downloads directory, not in /Applications. But the directory I'd added to my PATH was supposed to be in /Applications, not /Downloads, and as mentioned, BASH will only check the directories in your PATH, and nowhere else.  So I moved Postgres from my /Downloads dir to /Applications, and voila - everything worked, my blood pressure came down, and I learned a little bit about how the PATH works.</p>

<p>It's also important to remember that BASH will stop looking for a program or script once it finds a match, so the sequence of directories in your PATH is important.  If you have two different versions of a program saved in different places, (say, one version in /usr/bin, and one version in usr/local/bin), BASH will use the version located in the directory that appears first in your PATH.  So if you think you've got a more recent version of a program installed, but for some reason BASH keeps loading the older version, check your PATH against the locations of your programs/scripts.  You can add a directory to the front of your path by opening your bash_profile and adjusting your PATH = line like so:</br></br>
<strong>PATH = "path/to/new/dir:$PATH"</strong></p>

<p>And that's the PATH.  An environment variable that provides BASH with instructions on where to look for things.  If you're ever running into an issue with BASH not recognizing a command or program, check your PATH first; it may be as easy as telling BASH where to look.  Now, go forth and wander.</p>