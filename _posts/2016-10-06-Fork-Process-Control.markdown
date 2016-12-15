---
layout: post
title:  "fork(), vfork() and control of processes"
date:   2016-10-06 13:28:24 -0600
categories:
---
<style>
p {
  text-align: justify
}</style>
<div style="text-align:center"><img src =""/></div>

<h1> Now what is a "process" in a Linux System? </h1>

A process is any application/program that runs or gets started inside a Linux system. The term "process" usually refers to the instance of this application. The definition is as simple as that! To have an idea of a what are the processes that are running in your Linux system, we can use the command `ps aux` or more specifically `ps auf grep | application name`. After using this command, you could get an output like this one:
{% highlight ruby %}

USER      PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root      2111  0.0  0.0  20016  2052 tty1     Ss+  Oct04   0:00 /sbin/getty -8 38400 tty1
root      1044  0.0  0.0  20016  2064 tty6     Ss+  Oct04   0:00 /sbin/getty -8 38400 tty6
root      1040  0.0  0.0  20016  2128 tty3     Ss+  Oct04   0:00 /sbin/getty -8 38400 tty3

{% endhighlight %}

Notice - here <b>PID</b> stands for Process ID. You can use the Process ID to manipulate the processes that are running. One of the operations you can perform is a <b>kill</b>. One could kill a process using `kill -9 PID`. This would kill that particular process.

<h2>fork()</h2>

This is a system call that is used to create  a new subprocess or a child for a current process. This new process created, would have a new Process ID. Usually the address space for the new process is copied from the parent.

<h2> Use wait()</h2>

When a replication of the parent process occurs through the fork() command, there is every chance that the pointers that represent some files and other parts of memory, are also replicated. When this happens, the wait() command comes in handy. This can be used to make the other processes that use the same files or memory segment to wait. This would make sure that there is no no race conditions between the parent and the child processes.

<h2>File locks and termination </h2>
There can also be issues related to file locks. When the parent process exists - there can be file locks that can be set by the parent. This is often not inherited by the child processes. This can be a sure problem.

<h2> vfork()</h2>

There is a subtle difference between fork() and vfork(). We have seen that the fork() command would make a copy of the parents' address space. The vfork() command, on the other hand, would not make the copy of a address space - instead vfork() shares the parents' address space. Also, the vfork() command would suspend the parent process until the child process terminates. The memory for the operation is also shared with the parent until this happens (termination of the child process).

<h2> clone()</h2>

The clone() command is quite similar to the fork() command. The  major difference is that the  the child processes would die if the parent process is terminated.

These are some of the  important operations and concepts relating to fork() and processes.

<br>
<br>
<b>Do you find any typos or disagree with something on this page? Let me know!</b>
