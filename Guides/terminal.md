# Terminal

This document covers basic knowledge for working with the command line, in particular using the MacOS Terminal.

## The command line interface

A command-line interface (CLI) is a means of interacting with a computer program where the user (or client) issues commands to the program in the form of successive lines of text (command lines).

A program which handles the interface is called a shell. There are various Unix shells that provide a CLI to the operating system's services, including sh, ksh, csh, tcsh and bash.

## Terminal

Terminal is an application on MacOS that provides a CLI to the system. It uses the bash shell by default.
You can change the default shell in Terminal's General settings, by specifying the path to your shell. This can be one of the above named shells that are shipped with MacOS (all available under `/bin/`), or a different one such as [zsh](http://ohmyz.sh) or [fish](https://fishshell.com).

## Startup scripts

Most shells use a startup script to do additional configuration, set various environment variables and aliasses. This is done by reading a startup script, which is done every time the shell is started. 
In most cases, the startup file can be found in your home directory (`~`), as a dot file with the name of the shell, followed by "rc". For bash this is `.bashrc`, for zsh `.zshrc`. For bash you'll also find a `.bash_profile` startup script that's run [when bash is started as a login-shell](http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html).

You can source other files in your startup script, for example to isolate all your aliases in a separate file. The default `.bashrc` contains this bit of shell script:

```
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

Which means that when you have a file `.bash_aliases`, it'll be included when your `.bashrc` is executed.

A suggested approach can be to create your own custom `.yournamerc` file (and perhaps a `.yourname_aliases` too that is sourced by your rc file), that you can source from every shell-specific rc file, allowing you to easily change shell when needed. This way you can keep all your personal startup logic out of `.bashrc`, `.zshrc` etc, and you won't have to copy-paste that when you choose to switch to `fish` or something else.

### PATH

The [`PATH`](https://en.wikipedia.org/wiki/PATH_(variable)) environment variable is specified as a list of one or more directory names separated by colon (:) characters. Each directory name is used as a search path for executable programs.
It typically contains `/bin`, `/usr/bin`, and `/usr/local/bin`. For superusers, `/sbin` and `/usr/sbin` are often also included. A command executable is searched for in every directory from left to right until a match is found.

Extending the PATH with new search paths can be done in your startup script, as follows (in this example to add executables from [Fastlane](http://fastlane.tools)):

```
export PATH="$HOME/.fastlane/bin:$PATH"
```

This exports a new PATH environment variable that includes `/.fastlane/bin` in the `HOME` directory (usually the same as `~`), as well as whatever was already in `PATH`.

### Aliases

An alias is a useful shorthand for a longer command. For example:

```
alias l='ls -lAht'
```

will allow you to just type `l` in your command line, and it'll execute `ls -lAht`.

If you want to quickly know the contents of an alias, you can execute:

```
type l
```

which in this case will output

```
l is an alias for ls -lAht
```

## Useful commands

There are quite a lot of different commands available to you, but you will use a number of them more often than others.

### Directories and Files

##### cd

`cd` will change the directory. 

When you do not specify anything, it'll change to your home directory.

You can specify either a full path or a relative path  after `cd` to change to the specified directory. A full path begins with `/`. A relative path begins with the name of a subdirectory of the working directory. This includes `..` and `.`, which are the parent and current directory respectively.

Any space in a directory name should be escaped with a backslash, or the path should be put inside quotes:

```
cd My\ files
cd 'My files'
```

##### ls

`ls` lists the contents of the current directory. Without any arguments specified, this will list only the names of visible files and folders.

###### Useful arguments:

<table>
<thead>
<tr>
<th>argument</th>
<th>description</th>
</tr>
<thead>
<tbody>
<tr>
<td>A</td>
<td>will also list hidden files and directories, but will not include <code>.</code> and <code>..</code> (where <code>a</code> will).</td>
</tr>
<tr>
<td>l</td>
<td>will list in long format, including permissions, size and last modified date.</td>
</tr>
<tr>
<td>h</td>
<td>when used in combination with <code>l</code> argument, will print out file size in human-readable format, using unit suffixes.</td>
</tr>
<tr>
<td>t</td>
<td>sort by time modified (most recently modified first).</td>
</tr>
</tbody>
</table>

Arguments can be combined as follows:

```
ls -Alht
```

##### mkdir

`mkdir` makes directories. The command should be followed by the required directory name.

###### Useful arguments:

<table>
<thead>
<tr>
<th>argument</th>
<th>description</th>
</tr>
<thead>
<tbody>
<tr>
<td>p</td>
<td>will create intermediate directories as required.</td>
</tr>
</tbody>
</table>

```
mkdir -p my/new/directories
```

##### rmdir

`rmdir` removes directories.

##### pwd

`pwd` will print out the full path of the current working directory.

##### cp

`cp` copies files. The command should be followed by `source_file`, then `destination_file` (but any arguments first).

###### Useful arguments:

<table>
<thead>
<tr>
<th>argument</th>
<th>description</th>
</tr>
<thead>
<tbody>
<tr>
<td>R</td>
<td>If <code>source_file</code> designates a directory, <code>cp</code> copies the directory and the entire sub-
           tree connected at that point.  If the <code>source_file</code> ends in a <code>/</code>, the contents of the
           directory are copied rather than the directory itself.</td>
</tr>
<tr>
<td>n</td>
<td>Do not overwrite an existing file.</td>
</tr>
</tbody>
</table>

##### mv

`mv` moves files. The command should be followed by `source`, then `target` (but any arguments first).

###### Useful arguments:

<table>
<thead>
<tr>
<th>argument</th>
<th>description</th>
</tr>
<thead>
<tbody>
<tr>
<td>n</td>
<td>Do not overwrite an existing file.</td>
</tr>
</tbody>
</table>

##### rm

`rm` removes directory entries.

```
rm foo
```

will remove the file named foo from the current working directory.

###### Useful arguments:

<table>
<thead>
<tr>
<th>argument</th>
<th>description</th>
</tr>
<thead>
<tbody>
<tr>
<td>r</td>
<td>Removes directories and their contents recursively.</td>
</tr>
<tr>
<td>i</td>
<td>will ask confirmation for every deletion.</td>
</tr>
<tr>
<td>f</td>
<td>will ignore confirmation prompts as well as non-existent files. Use with care (<code>rm -rf /</code> will silently remove EVERYTHING 😱)</td>
</tr>
</tbody>
</table>

##### chmod

`chmod` changes access permissions to files or directories. This can be done in either octal (e.g. `0664`) or symbolic (e.g. `a+rx`) notation. For an extensive overview of this, please see the [chmod Wikipedia page](https://en.wikipedia.org/wiki/Chmod).

##### chown

`chown` changes the owner and/or group of files or directories. This may only be done by a super-user.

##### touch

`touch` is actually used to change file access and modification times of files, but it is also very useful to create a new file with default permissions, as follows:

```
touch foo.txt
```

##### nano / emacs / vim

These are all editors with which you can edit files inside the CLI.

##### cat / head / tail / less

These commands will print out (in the CLI) the contents of a file.


###### cat

`cat` will print the entire contents of a file.

###### head

`head` will print out a number of lines (specified with the `n` argument) starting from the beginning (the head) of the file.

```
head -n 10 access.log
```

###### tail

`tail` is similar to head, but will print lines starting from the end (the tail) of the file. Another useful property with tail is `f`, particularly when inspecting log files.

```
tail -f access.log
```

This will wait for additional data to be appended and output that as well. So in case of the example you'll see new access log messages being appended in real time.

###### less

`less` will print out exactly enough to fill the cli window. You can move up and down using the arrow keys. Close it by pressing `q`.

### Finding things

##### grep

`grep` will search for a specified string in a specific file, or all files within a specified directory, and print out any line that includes that string.

###### Useful arguments:

<table>
<thead>
<tr>
<th>argument</th>
<th>description</th>
</tr>
<thead>
<tbody>
<tr>
<td>r</td>
<td>will search subdirectories recursively.</td>
</tr>
<tr>
<td>v</td>
<td>will invert-match, so returns any line that does NOT contain the specified string.</td>
</tr>
</tbody>
</table>

```
grep -r 'foo' .
```

will search for "foo" in every file in the current working directory and all subdirectories.

`grep` is way more powerful than this brief description. Please see the `man` page and other resources on `grep` for further documentation.

##### sed

`sed` is a stream editor, and as such can be used for search/replace actions (but, as with `grep`, also for much more).

The most basic search/replace is done as follows:

```
sed 's/regexp/replacement/g' inputFileName > outputFileName
```

See for example the `sed` [Wikipedia page](https://en.wikipedia.org/wiki/Sed) for many more things that can be done using `sed`.

##### find

`find` will walk through a file hierarchy, listing any file or directory that matches specified criteria.

For example:

```
find . -name 'foo'
```

Will list anything that is named foo, found in the current working directory (the `.`) and all its subdirecties.

If you want to find a specific type, you can specify that using the `type` argument:

```
find . -name '*.md' -type f
```

Will list any regular file that ends with ".md", again in the current working directory and all its subdirecties. Use `-type d` for directories.

##### wc

`wc` will print out word, line, character and byte count.

```
cat access.log | grep '11/Dec/2017' | wc -l

```
will print out the number of lines in `access.log` that contain "11/Dec/2017".

###### Useful arguments:

<table>
<thead>
<tr>
<th>argument</th>
<th>description</th>
</tr>
<thead>
<tbody>
<tr>
<td>l</td>
<td>Will print out the number of lines.</td>
</tr>
<tr>
<td>w</td>
<td>Will print out the number of words.</td>
</tr>
<tr>
<td>m</td>
<td>Will print out the number of characters.</td>
</tr>
<tr>
<td>c</td>
<td>Will print out the number of bytes.</td>
</tr>
</tbody>
</table>

### Networking

##### curl

`curl` is an incredibly powerful and versatile tool for transferring data from or to a server. In its simplest form, you can use it to make HTTP(S) requests to a specific server.

```
curl "https://www.apple.com"
```

Will print out the contents of that website. This is just the tip of the tip of the iceberg. You can specify request method (GET / POST / PUT ...), request headers, request body, and much, much more.

##### ssh

`ssh` allows you to log into a remote machine, on which you can then execute commands. When connecting, you specify the hostname and optionally any other relevant parameters such as port or username.

```
ssh username@yourserver.com -p 1234
```

In the above the `-p` argument specifies the port to be 1234. There are a lot more arguments that can be specified, see the `man` page for `ssh` for more info.

You can specify common remotes in a file called `config`, located in `~/.ssh/`. An example of an entry for this file:

```
Host website_production
Hostname 123.45.67.89
Port 1234
User admin
```

This specifies a shortcut name `website_production` that can be used to connect to the specified hostname using the additional attributes. You can now simply call:

```
ssh website_production
```

When you make use of [SSH keys for authentication](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process), your generated keys will also reside in `~/.ssh/`. You can specify which so-called `IdentityFile` should be used per entry in your `config` file (put this directly after the example specification above):

```
IdentityFile ~/.ssh/id_rsa
```

##### scp

`scp` allows you to securely copy files, using `ssh`. It makes use of the same patterns as `ssh` to specify the remote machine to connect to (the `config` shortcuts are available too). You can use `scp` to download or upload files between two machines (both source and target machinge can be remote).

In the following examples we use the `config` host shortcut `your_remote_host`.

###### Download file `htdocs/example.html` from remote to local:

```
scp your_remote_host:htdocs/example.html /your/local/dir
```

###### Upload file `example.html` from local to remote (into folder `htdocs`):

```
scp example.html your_remote_host:htdocs 
```

###### Download directory `htdocs/content` from remote to local:

```
scp -r your_remote_host:htdocs/content ./content 
```

###### Upload directory `content` from local to remote (into folder `htdocs`):

```
scp -r content your_remote_host:htdocs/content 
```

##### dig

`dig` can be used to lookup DNS entries. The default call with a specified hostname will return the `A` type records:

```
dig touchwonders.com
```

If you need different types of records, you can simply append these:

```
dig touchwonders.com AAAA
dig touchwonders.com TXT
dig touchwonders.com MX
```

##### ping

Use `ping` to send packets to a network host to test its responsiveness. The command will continue to send packets until you terminate using Ctrl+C, upon which a small summary is provided.

```
ping touchwonders.com
```

will give you something like:

```
PING touchwonders.com (188.226.167.242): 56 data bytes
64 bytes from 188.226.167.242: icmp_seq=0 ttl=58 time=6.295 ms
64 bytes from 188.226.167.242: icmp_seq=1 ttl=58 time=5.980 ms
64 bytes from 188.226.167.242: icmp_seq=2 ttl=58 time=5.833 ms
^C
--- touchwonders.com ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 5.833/6.036/6.295/0.193 ms
```

### System

##### df

`df` simply shows you the free disk space on your machine.

##### top

`top` displays information about running processes. Useful for identifying CPU hogs. The output is comparable to Activity Monitor on MacOS.

### Misc

##### man

`man` shows a manual page on the requested command. The page informs you about intended use and available parameters.

```
man grep
```

will show you the manual page for the `grep` command. And yes, there's also manual page for `man`.

##### which

`which` will show you where a program file resides on your machine. For example:

```
which man
```

Will most likely output something like `/usr/bin/man`. However, not every command directly calls a single binary executable. Some commands might be an alias for calling an executable with specific parameters, or might be a small shell script.

##### sudo

`sudo` allows you to execute commands as another user. You need to have proper privileges to use `sudo`. Handle with care.

##### history

`history` prints out your command line history.

##### cal

`cal` prints out a calendar of the current month.

##### date

`date` prints out the current date and time.

##### echo

`echo` writes something to the standard output. This is particularly useful if you wish to inspect certain environment variables, such as the earlier mentioned `PATH`:

```
echo $PATH
echo "$HOME"
```

System variables can be inspected using the following commands:

##### env / printenv

`env` and `printenv` print out system variables. For a full set you can also use `set`.

##### export

As shown in the section on `PATH`, the `export` command can be used to define or update an environment variable.

```
export MY_VARIABLE=42
```

##### exit

`exit` terminates a process. For example, you can exit an `ssh` connection using `exit`, or you can terminate your current shell session using `exit`.


### Zsh plugins

When you use zsh and have enabled the `autojump` and `sublime` plugins, you willl also have the following very useful commands:

##### j

`j` can be used as shorthand for autojump, allowing you to jump to a directory that you have previously `cd`'d into. You can specify just part of the name and autojump will try and find the appropriate directory.

##### st

Will of course require that you have [Sublime Text](http://www.sublimetext.com) installed. With `st` you can easily open one or more files in Sublime Text. You can also specify `.`, opening the entire current working directory in Sublime Text. When you specify a filename with a wildcard, every match will be opened. For example:

```
st *.txt
```

Will open all txt files found in the current working directory.

## I/O Redirection

Several operators can be used to redirect output from one command to another command, or to a file, instead of to the screen (the `stdout`).

<table>
<thead>
<tr>
<th>Operator</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>&gt;</td>
<td>Redirects <code>stdout</code> to a file. Creates the file if not present, otherwise <strong>overwrites</strong> it.<br />
<pre>sed 's/searchterm/replacement/' input.txt > output.txt</pre>
</td>
</tr>
<tr>
<td>&gt;&gt;</td>
<td>Redirects <code>stdout</code> to a file. Creates the file if not present, otherwise <strong>appends</strong> to it.</td>
</tr>
<tr>
<td>&lt;</td>
<td>Accepts input from a file. For example useful with <code>grep</code>.</td>
</tr>
<tr>
<td>|</td>
<td>The "pipe" can be used to chain commands, redirecting the output of one to the input of another. For example, <code>tail</code> only the last couple of lines of a file, and pipe these into <code>grep</code> to search for a specific term:
<pre>tail -n100 access.log | grep "123.45.67.89"</pre></td>
</tr>
</tbody>
</table>

There are a few more operators that can be used for redirecting error messages (<code>stderr</code>).

<table>
<thead>
<tr>
<th>Operator</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>2&gt;</td>
<td>Redirects <code>stderr</code> to a file. Creates the file if not present, otherwise <strong>overwrites</strong> it.</td>
</tr>
<tr>
<td>2&gt;&gt;</td>
<td>Redirects <code>stderr</code> to a file. Creates the file if not present, otherwise <strong>appends</strong> to it.</td>
</tr>
<tr>
<td>2&gt;&amp;1</td>
<td>Redirects <code>stderr</code> to <code>stdout</code>.</td>
</tr>
</tbody>
</table>

## Terminal keyboard shortcuts

#### Moving the cursor

<table>
<thead>
<tr>
<th>Key combination</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ctrl <span style="color:#999">+</span> a</td>
<td>Moves the cursor to the beginning of the line</td>
</tr>
<tr>
<td>Ctrl <span style="color:#999">+</span> e</td>
<td>Moves the cursor to the end of the line</td>
</tr>
<tr>
<td>Option <span style="color:#999">+</span> click on current line</td>
<td>Jump in the line to where you click</td>
</tr>
<tr>
<td>Option <span style="color:#999">+</span> &larr;</td>
<td>Moves the cursor back one word</td>
</tr>
<tr>
<td>Option <span style="color:#999">+</span> &rarr;</td>
<td>Moves the cursor forward one word</td>
</tr>
<tr>
<td>&uarr; (or Ctrl <span style="color:#999">+</span> p)</td>
<td>Shows the previous command</td>
</tr>
<tr>
<td>&darr; (or Ctrl <span style="color:#999">+</span> n)</td>
<td>Shows the next command</td>
</tr>
<tr>
<td>Option <span style="color:#999">+</span> click on current line</td>
<td>Jump in the line to where you click</td>
</tr>
<tr>
<td>Option <span style="color:#999">+</span> &uarr;</td>
<td>Moves the cursor back one word</td>
</tr>
<tr>
<td>Option <span style="color:#999">+</span> &darr;</td>
<td>Moves the cursor forward one word</td>
</tr>
<tr>
<td>TAB</td>
<td>Auto-completion for file/directory names. When multiple options are available, hit TAB again to step through those options.</td>
</tr>
<tr>
<td>Ctrl <span style="color:#999">+</span> u</td>
<td>Deletes the current line</td>
</tr>
<tr>
<td>Ctrl <span style="color:#999">+</span> k</td>
<td>Deletes the current line after the cursor</td>
</tr>
<tr>
<td>Ctrl <span style="color:#999">+</span> t</td>
<td>Swaps the last two characters before the cursor</td>
</tr>
<tr>
<td>Esc <span style="color:#999">+</span> t</td>
<td>Swaps the last two words before the cursor</td>
</tr>
<tr>
<td>Ctrl <span style="color:#999">+</span> _</td>
<td>Undo</td>
</tr>
</tr>
<tr>
<td>Ctrl <span style="color:#999">+</span> l</td>
<td>Clears the screen</td>
</tr>
<tr>
<td>Ctrl <span style="color:#999">+</span> c</td>
<td>Interrupts / kills whatever you are doing (running a process, typing a command line ...)</td>
</tr>
</tbody>
</table>
