---
title: Pwn - Naughty List - HTB Cyber Santa CTF 
tags:
  - Pwn 
  - Stack
  - ROP
---

# Naughty List Pwn Challenge

## TL;DR;
Getting the flag in this challenge involves exploiting a stack based buffer
overflow and defeating ASLR and DEP to get code execution. We use a 
well established technique of leaking `puts@GOT` to get the base address of `libc`. 
After establishing this base address, we can use a "one shot" gadget 
to get RCE and retrieve the flag.

## Recon:
The download link provides us with a challenge binary, `naughty_list`,  
and it's associated `libc`. We can do some basic recon with `file` and 
`checksec`. 

 
![checksec_file](naughty_list_img/file_checksec.png)

From this we get a good idea of what protections are in place. With `NX` enabled,
we won't be able to execute our own shell code on the stack. Furthermore, we see the 
binary is dynamically linked. 

### Challenge Binary Functionality
Now that we know what protections are in place, lets check out the binary's functionality.
Running it, we see that it asks for our firstname, last name, age, and what gift we want.


![checksec_file](naughty_list_img/functionality.png)


### Vulnerability
Reversing here isn't really needed, as we could just manually fuzz each input.



