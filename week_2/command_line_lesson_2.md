# Introduction to the command line

This introduction shows some other commands that you will likely find helpful.

## Searching for and within files:


Firstly let's get back to the Desktop:

```sh
cd ~/Desktop
```

Now you'll notice that I used the `~` directory. Now this isn't quite a local or an absolute path, it's customized for the end user and is a link to their home directory. This means nothing more than where they would be able to find `/Desktop` or `/Downloads`. it's the main directory that you get when you open up the command line.

### find
Now once we're there, a common exercise will be trying to find files. There are only two real ways that you'll want to find files. You'll either search for them by name, or search for them by contents.

Searching by name is done with the `find` command. For example:

```sh
# from our Desktop
find . -name "*file*"
```

Should print out:

```
./test_creation/my_file.txt
./test_creation/second_file.txt
```

You'll notice my use of stars in the above example as well. This is a nice little piece of functionality. a `*` is just a wildcard and could be any character. You can get very expressive with this miniature langauge called regular expressions and we'll be diving into that later on in this course.


In addition to the `-name` parameter that we passed, there are plenty of different parameters that you can pass into the find command, you can specify whether it should search deeply first or search the top-levels first, when the file was created and more. Remember you can use

`man find` to see all the options.

### grep
Another command you should know about is `grep`. Grep is a command that I use all the time because rather than finding files by name, it searches by the contents of the file. We can use it to specify a specific file or a whole group of files.

For examples
```sh
grep "Hello" test_creation/my_file.txt
```

would return

```
/test_creation/my_file.txt:Hello From the Command Line
```

If I didn't know which file it was in, I can just use the `-r` flag to make a recursive search of a directory.

```sh
grep -r "second" .
```

Would recursively search within my current directory (Desktop). I could equally just search within our test directory.

```sh
grep -r "second" test_creation/
```

Now there are a ton of flags that you can pass to grep, this is just a sample. Now we're going to move on to a more destructive section.

## The Danger Zone: Removing Files

This section is called the danger zone for a reason. You can do **serious** damage to you system with these commands. I can tell you that likely hundreds of thousands of dollars of damage have been done with less that 10 keystrokes. This means that you *really* need to think about what you're doing before executing these commands. They are how you can destroy directories and files.

make sure that you're in our test_creation folder.

```sh
cd Desktop/test_creation
```

Now let's make a new directory and some files to test some stuff on.

```sh
mkdir test_destruction
touch test_destruction/1.txt
touch test_destruction/2.txt
touch test_destruction/3.txt
```

Now we're going to start removing files from these directories. We've created a directory and three files. Let's delete the last one.

```sh
rm test_destruction/3.txt
ls test_destruction
```

```
test_destruction/1.txt
test_destruction/2.txt
```

You'll see that it deleted the last file. However this isn't your grandma's "oh I'll just go look in the recycle bin". This file is gone. It's not here anymore, it's not in some little recovery bin. The file. is. gone. Only doing some sort of forensic extraction could you get it back - something that you don't want to do. So it's worth checking twice that you're deleting the right thing when you go to use the `rm` command.

For better or for worse, remember when we used the `*` in `find`? Well that wild card works with all commands. Want to delete everything in test destruction?

```sh
rm test_destruction/*
ls test_destuction
```

you'll likely get a prompt before you can do this but once it's done, you should see the power of this tool. With 7 keystrokes, you can destroy everything on your computer. Literally everything. We are not responsible for anything that you delete that you don't mean to, use this tool wisely, because you can do serious damage if you're not careful.

Now let's use `rmdir` to remove our test destruction folder. You'll notice that

```sh
rm test_destruction
```

will fail. This is because rm is intended for files, not folders (although can be use for such). You'll need `rmdir` to remove the directory.

```sh
rmdir test_destruction
ls
```

Should show you that the folder is destroyed. Again, be careful with what you delete using this tool. I use it because I am comfortable with it, but make sure that you triple check everytime you think you should delete something.

## Connecting Commands: Piping and Passing Output

In out introduction we covered a lot of these but I wanted to show you one last tip. This is the `|` character. Try this from your Desktop

```sh
ls | grep "test"
```

What we're doing is piping the output from one command to another. We're taking the output of `ls` and outputting in into the `grep` command. If you've run this correctly, you should see the `test_creation` directory listed in this output.

Another things you can do is something like

```sh
echo "Does this contain a ?" | grep -o "?"
```

Which will output one `?` because the `-o` flag signifies that it should only return matching characters. Remove the `-o` and you'll see the entire `echo` statement. This piping capability is very powerful and sits at the base of the `>` and `>>` character combinations that we covered in previous lessons.

Another combination that you'll come across is `<` which basically allows you to pipe a file or command another way.

```sh
grep "echo" < test_creation/my_file.txt
```

Will basically force that file into `grep`. While this is used less, it's not uncommon to see it come up in other people's code.

## Conclusion
This was a crash course introduction and is not intended to be rigorous - it's meant to give you the basics of what you're going to need to know to succeed as a data scientist and programmer. There is by no means an introduction to everything that you can do at the command line. In fact, the command line has it's own complete programming language called Bash.

Zed Shaw, an amazing teacher, has an excellent cheatsheet that is absolutely worth your time looking at. It will show you a lot more of the commands that you should become familiar with at the command line. [You can find that cheat sheet here: http://cli.learncodethehardway.org/bash_cheat_sheet.pdf](http://cli.learncodethehardway.org/bash_cheat_sheet.pdf).

One of the reasons that you'll find this so useful is once you start getting a sense for the commands you write a lot. For example, I find myself looking for files at the command line a fair amount, so I wrote a function `ff`.

```sh
function ff { find . -name $1; }
```

All that this function does is make it extremely easy for me to search for files. I just type `ff "fname"` and it searches within the current directory.

It's great when I know what the file is like because I can just write `ff "*file*"` and it will find everything that contains `file` in the file name. Now you shouldn't write these or use these if you don't need them but it should show you the power of the shell. With time I hope you come to love it as much as I do.
