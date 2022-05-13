---
title: File descriptor
excerpt: concept

toc: true
toc_label: About
toc_sticky: true

categories: os

date: 2022-05-13
last_modified_at: 2022-05-13
---
# What is File descriptor
File descriptor is a number that identifies an open file in a operating system. It is a index of the data. The descriptor is identified by a unique non-negative intege. At least one file descriptor exists for every open file on the system. File descriptors were first used in Unix, and are used by modern operating systems including Linux, macOS. <br><br>

# When does the File descriptor create
When a process makes a successful request to open a file, the kernel returns a file descriptor which points to an entry in the kernel's global file table. The file table entry contains information such as the inode of the file, byte offset, and the access restrictions for that data stream. <br><br>

![Header](/assets/images/filedescriptor.PNG)<br><br>

# Stdin, Stdout, Stderr
On UNIX-like os, there are three file descriptors.<br>
![Header](/assets/images/3fd.PNG)<br><br>
