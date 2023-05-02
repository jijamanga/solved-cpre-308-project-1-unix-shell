Download Link: https://assignmentchef.com/product/solved-cpre-308-project-1-unix-shell
<br>
5/5 - (1 vote)




In this project you will be creating your own version of a UNIX shell. It will not be as sophisticated as bash, the shell you have been using in lab, but it will perform similar functions. A UNIX shell is simply an interactive interface between the OS and the user. It repeatedly accepts input from the user and initiates processes on the OS based on that input.

2.1 Requirements

<ol>

 <li>Your shell should accept a -p &lt;prompt&gt; option on the command line when it is initiated. The &lt;prompt&gt; option should become the user prompt. If this option is not specified, the default prompt of “308sh&gt; ” should be used. Read section 3.1 on command line options for more information.</li>

 <li>your shell must run an infinite loop accepting input from the user and running commands until the user requests to exit.</li>

 <li>Your shell should support two types of user commands: (1) built-in commands, and (2) program commands.(a) (5 pts) Built-in commands are commands to interact with your shell program, such as cd, cd &lt;dir&gt;, etc. See section 2.2 for a list of the builtin commands your shell should support.</li>

</ol>

1

(b) Program commands require your shell to spawn a child process to execute the user input (exactly as typed) using the execvp() call. This means a user command will be entered either using an absolute path, a path relative to the current working directory, or a path relative to a directory listed in the PATH environment variable. (Read manual page of execvp, especially the “Special semantics for execlp() and execvp()” section. Note that execvp will search PATH for you.)

<ol start="4">

 <li> The shell should notify the user if the requested command is not found and cannot be run.</li>

 <li>When executing a program command (not a built-in command), print out the creation and exitstatus information for child process (see section 4 for examples):

  <ol>

   <li>(a)  (5 pts) When a child process is spawned, print the process ID (PID) before executing the specified command. It should only be printed once and it must be printed before any output from the command. You can include other information (name of program, command number, etc) if you find it useful.</li>

   <li>(b)  By default, the shell should block (wait for the child to exit) for each command. Thus the prompt will not be available for additional user input until the command has completed. Your shell should only wake up when the most recently executed child process completes. See waitpid.</li>

   <li>(c)  (5 pts) When a child process is finished, print out its PID and exit status. Revisit Lab 2 for a refresher on how to handle exit status.</li>

  </ol></li>

 <li>(10 pts) Your shell should support background commands using the suffix ampersand (&amp;), i.e. the child should run in the background, meaning the shell will not wait for the child to exit before prompting the user for further input. You still need to print the creation and exist status of these background processes.An example is, if the user entered “/usr/bin/emacs” then the shell would wait for the process to complete before returning to the prompt. However, if the user entered “/usr/bin/emacs &amp;” then the prompt would return as soon as the process was initiated.HINT: to evaluate and print the exit status of a background child process call waitpid with -1 for the pid and WNOHANG set in the options. This checks all children and doesn’t block – see the man page. To do this periodically, a check can be done every time the user enters a command.</li>

</ol>

2.2 Builtin Commands

The following commands are special commands (also called built-in commands) that are not to be treated as programs to be launched. No child process should be spawned when these are entered.

<ul>

 <li>exit – the shell should terminate and accept no further input from the user</li>

 <li>pid – the shell should print its process ID</li>

 <li>ppid – the shell should print the process ID of its parent</li>

 <li>cd &lt;dir&gt; – change the working directory. With no arguments, change to the user’s home directory (which is stored in the environment variable HOME)</li>

 <li>pwd – print the current working directory</li>

</ul>

2

2.3

•

•

3 3.1

Extra Credit (5 pts)

Add a built-in command jobs that outputs the names and PID of your child processes (i.e. “process pid (procnam) ”). For this, you need a data structure which can maintain a list of currently executing background tasks – we suggest using a list.

Make sure you clearly describe your implementation in your lab report. Your summary may be longer than two paragraphs in this case.

Useful Information Command line options

Command line options are options that are passed to a program when it is initiated. For example, to get ls to display the size of files as well as their names the -l option is passed to it. (e.g. ls {l /home/me). In this example, /home/me is also a command line option which specifies the desired directory for which the contents should be listed. From within a C program, command line options are handled using the parameters argc and argv which are passed into main.

<pre>int main(int argc, char **argv)</pre>

The parameter argc is a value that contains the number of command line arguments. This value is always at least 1, as the name of the executable is always the first parameter. The parameter argv is an array of string values that contain the command line options. The first option, as mentioned above, is the name of the executed program. The program below simply prints each command line option passed in on its own line.

<pre>int main(int argc, char **argv) {    int i;</pre>

<pre>    for(i=0; i&lt;argc; i++)        printf("Option %d is "%s"
", i, argv[i]);</pre>

return 0; }

If you are unfamiliar with how command line options work, enter the above program and try it with different values. (e.g. ./myprog I guess these are options)

3.2 Useful system and library calls

The following system calls will be useful in this project. Read the corresponding man pages for those you are unfamiliar with.

• fork – create a child process• execvp – replace the current process with that of the specified program • waitpid – wait for a child to exit (or get exit status)• exit – force the current process to exit, with the given return value• chdir – change working directory• getcwd – get current working directory• getenv/setenv – retrieve and set environment variables.

3

• perror – display error messages based on the value of errno • strcmp, strcpy, strcat,strtok – string manipulation.

4 Example and Test Cases

This is an example test case and output for your progrem. Your output does not have to look exactly like this; it is simply meant to give you some idea of how the shell should work and how output looks. A $ prompt indicates a bash prompt, used to execute your shell, and 308sh&gt; indicates your shell prompt. Also, you may want to prefix all your output messages with some sort of identifier, such as “&gt;&gt;&gt;”, to more easily determine which lines are yours and which came from the executed program.

You should test all of the builtin commands in your shell, in addition to several commands that are not builtin. You should test any error handling you use. You do not have to turn in any test code that you use, but don’t expect to find errors without testing!

Note that you do not have to output the process name on the “exit” line unless you are doing the extra credit; just the PID and status is sufficient.

Shell command line:

<pre>$ ./shell -p "hello&gt; "hello&gt; exit$</pre>

Simple execution:

<pre>308sh&gt; /bin/ls[977801] lsshell.c  shell.o  shell[977801] ls Exit 0308sh&gt; pwd/home/danield/308/308sh&gt; cd308sh&gt; pwd/home/danield/308sh&gt; cd 308308sh&gt; pwd/home/danield/308/308sh&gt; ps[977819] ps</pre>

<pre>   PID TTY  3590 pts/2977797 pts/2977819 pts/2</pre>

<pre>    TIME CMD00:00:01 bash</pre>

<pre>00:00:00 shell</pre>

<pre>                00:00:00 ps[977819] ps Exit 0</pre>

<pre>308sh&gt;308sh&gt; nonexistant[977850] nonexistantCannot exec nonexistant: No such file or directory[977850] nonexistant Exit 255308sh&gt; test 2 = 3[978229] test[978229] test Exit 1</pre>

4

308sh&gt;

Background processes:

<pre>308sh&gt; sleep 1 &amp;[977999] sleep308sh&gt;[977999] sleep Exit 0308sh&gt;</pre>

<pre>308sh&gt; sleep 1 &amp;[977864] sleep308sh&gt; sleep 2[977865] sleep[977865] sleep Exit 0[977864] sleep Exit 0308sh&gt;</pre>

<pre>308sh&gt; sleep 10 &amp;[977943] sleep308sh&gt; kill 977943[977945] kill[977945] kill Exit 0[977943] sleep Killed (15)308sh&gt;</pre>