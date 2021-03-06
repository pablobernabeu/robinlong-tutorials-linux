**********************
Piping and redirecting
**********************

One of the great powers of the command line, and Linux, is the ability to seemlessly chain small commands together to do really useful things. In the previous chapters we have learnt how to use the command line to do various tasks, and run commands. In this chapter we look at how to save the output from those commands, and chain commands together.

Overview
===================

In this chapter you will learn:

* what *streams* are.
* how to redirect *stdout* and *stderr* to a file.
* how to direct a file into a command using *stdin*.
* how to chain the input and output of commands together with pipes.


Introduction to stdin, stdout, and stderr
==========================================
Every command that we run on the command line has three * streams* connected to it. These are:

* stdin(0) - Standard In(put), data fed into the program/command.
* stdout(1) - Standard Out(put), data received from the program/command. By default this prints out to the screen.
* stderr(2) - Standard Error, for error messages generated by the program/command. This is also sent to the screen by default.

We can redirect these streams into files, into each other, or into commands.  Piping allows us to chain many commands together, each using the output of the previous.

Redirecting to a file
======================

When we run a command it normally prints its output to the screen. Sometimes it is more convenient to save this to a file for use later, or as a record.  The greater than symbol, ``>``, is used to tell the command line we want the standard output (stdout) to be sent to a file instead of the screen. Here is an example

.. code-block:: bash
   :linenos:

   $ ls 
   Public     Private     Documents
   Thesis     Research    contacts.txt
   $ ls > output.txt
   $ ls
   Public     Private     Documents
   Thesis     Research    contacts.txt
   output.txt  
   $ cat output.txt
   Public
   Private
   Documents
   Thesis
   Research
   contacts.txt

Let's look at this line by line.

* **Line 1** - We use the ``ls`` to show the contents of our current directory.
* **Line 4** - Now we use the ``>`` to redirect the output to a file.
* **Line 5** - Using ``ls`` to show the directory contents again, we can see a new file called ``output.txt`` has been created.
* **Line 8** - Using ``cat`` we look at the contents of the file.

Different outputs
-----------------------------
You might have noticed that in the above example the formatting of the output when it was printed to screen, and when it was written to a file are different.  This is because the program knows the size of the screen, and can format the output to fit.  When we output to somewhere other than the screen, the safest thing is to place one entry on each line.


Redirecting to an existing file
---------------------------------
When we redirect to a file that does not exist, it will be created. However if we redirect to a file that does exist, its contents will be overwritten.

.. code-block:: bash
   :linenos:

   $ cat output.txt
   Public
   Private
   Documents
   Thesis
   Research
   contacts.txt
   $ wc -l contacts.txt > output.txt
   $ cat output.txt
   2 contacts.txt

if we wish the output to be appended to the file, we can use the double greater than operator, ``>>``.  

.. code-block:: bash
   :linenos:

   $ cat output.txt
   2 contacts.txt
   $ ls >> output.txt
   $ cat output.txt
   2 contacts.txt
   Public
   Private
   Documents
   Thesis
   Research
   contacts.txt

Redirecting standard error, stderr
===================================

We mentioned earlier that there are two output streams; standard out, and stand error. You will notice that these had numbers associated with them, which actually represent the stream. If we put a number infront of the redirect operator (``>``) then it will redirect that stream to a file, without a number it defaults to stream 1, stdout.

.. code-block:: bash
   :linenos:

   $ ls contacts.txt mycontacts.txt
   ls: cannot access 'mycontacts.txt': No such file or directory
   contacts.txt
   $ ls contacts.txt mycontacts.txt 2> error.txt
   contacts.txt
   $ cat error.txt
   ls: cannot access 'mycontacts.txt': No such file or directory

We can also redirect both stdout and stderr to a file. We do this by directing stdout to a file, and then directing stderr to stdout.  This is similar to redirecting to a file, however we need to put ``\&`` infront of ``1`` to identify it as a stream, and not a file.

.. code-block:: bash
   :linenos:

   $ ls contacts.txt mycontacts.txt > output.txt 2>&1
   $ cat output.txt
   ls: cannot access 'mycontacts.txt': No such file or directory
   contacts.txt

Redirecting from a file
==========================
Using the less than operator, ``<``, we can send data to a program from a file.  This reads in the data from a file and passed it to the standard in, STDIN, stream of a program.

.. code-block:: bash
   :linenos:

   $ wc -l contacts.txt
   2 contacts.txt
   $ wc -l < contacts.txt
   2

Notice that whilst both commands use the contents of the file as input to the program, the output is different.  This is because when we redirect the file to the input, the data is sent anonymously.  This can be useful sometimes if you do not want ancillary data sent to another command.

Pipes
===================

So far we have looked at how to send output to files, and read input from files.  Lets instead look at how to send data from one program to another.  We do this using the pipe operator, ``|``.  This takes the output from the command on the left and sends it as input to the command on the right.

.. code-block:: bash
   :linenos:

   $ cat contacts.txt | wc -l
   2 contacts.txt

We can chain as many of these as we want together. You may also want to combine pipes with a redirect so that the output can be saved.

.. code-block:: bash
   :linenos:

   $ cat contacts.txt | wc -l > words_contact.txt
   $ cat words_contact.txt
   2

Summary
===================


Concepts
--------
* Streams are flows of input or output data.
* There are two outputs, **standard output**, and **standard error**.
* Streams can be redirected to or from a file.

Commands
--------
* ``>`` Save output to a file.
* ``>>`` Append output to a file.
* ``<`` Read input from a file.
* ``2>`` Redirect error messages.
* ``2>\&1`` Redirect stderr to stdout.
* ``|`` Send output from one command as an input to another.

Exercises
===================
#. Using ``ls`` and redirection, create a file listing all files and directories in your home directory.
#. Add to the file you created in the previous question, the contents of ``/``. Do not overwrite your previous entry.
#. Using the command ``ls / /nonexistantdir``, redirect standard out to one file, and stderr to another.
#. This time send them to the same file.


Further Reading
===================

`Linux Documentation Project - All about redirection. <http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-3.html>`_
