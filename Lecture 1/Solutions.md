# Solutions of Lecture 1

1. In short, installing a Unix shell environment.

    **Solution:** As for macOS and Linux system, you don't need to do anything. As for Windows system, you'd better install a virtual machine of Unix or use [Windows Subsystem of Linux](https://learn.microsoft.com/en-us/windows/wsl/).

2. Create a new directory called `missing` under `/tmp`

    **Solution:** In terminal, use the instruction `mkdir`to create a directory. The instruction of creating `missing` should be `mkdir /tmp/missing`.

3. Look up the `touch` program. The `man` program is your friend.

    **Solutuion:**: The `man` can help us know how to use a program, in this question, we should know how to use `touch`, so just type `man touch` to learn.

4. Use `touch` to create a new file called `semester` in `missing`.

    **Solution:** Just type `touch semester`(ps: you should make sure you are in the `missing` folder).

5. Write the following into that file, one line at a time:

    ```bash
    #!/bin/sh
    curl --head --silent https://missing.csail.mit.edu  
    ```

    The first line might be tricky to get working. It¡¯s helpful to know that `#` starts a comment in Bash, and `!` has a special meaning even within double-quoted (`"`) strings. Bash treats single-quoted strings (`'`) differently: they will do the trick in this case. See the Bash quoting manual page for more information.

    **Solution:** In this question, we should know that how to use the program `echo` and the symbol `>`. Typing these in your terminal:

    ```bash
    echo '#!/bin/sh' > semester
    echo 'curl --head --silent https://missing.csail.mit.edu' >> semester
    ```

6. Try to execute the file, i.e. type the path to the script (`./semester`) into your shell and press enter. Understand why it doesn¡¯t work by consulting the output of `ls` (hint: look at the permission bits of the file).

    **Solution:** Run the instruction `./semester`
    There would be a mistake:

    ```bash
    zsh: permission denied: ./semester
    ```

    Run the instruction `ls -l`, you will find that you don't have the permission to execute the file.

7. Run the command by explicitly starting the `sh` interpreter, and giving it the file `semester` as the first argument, i.e. `sh semester`. Why does this work, while `./semester` didn¡¯t?

    **Solution**: As I have mentioned in Q6, the current user don't have the permission to execute the file, so the command `./semester` won't work. But the `sh` command have another usage, it can communicate with kernal system.

8. Look up the `chmod` program (e.g. use `man chmod`).

    **Soulution:** Learn how to use the command `chmod`, using `man chmod` to learn.

9. Use chmod to make it possible to run the command ./semester rather than having to type sh semester. How does your shell know that the file is supposed to be interpreted using sh? See this page on the shebang line for more information.

    **Solution:** In order to make the `semester` executable, we can use the command `chmod +x semester` to solve.

10. Use `|` and `>` to write the ¡°last modified¡± date output by `semester` into a file called `last-modified.txt` in your home directory.

    **Solution:** The symbol `|` can make one command's output into another command's input.
    So, type the following into your terminal:

    ```bash
    ./semester | grep last-modified > last-modified.txt
    ```

11. Write a command that reads out your laptop battery¡¯s power level or your desktop machine¡¯s CPU temperature from `/sys`. Note: if you¡¯re a macOS user, your OS doesn¡¯t have sysfs, so you can skip this exercise.

    **Solution:** My OS doesn't have sysfs, skipping the exercise. Other OS can refer to the Internet.