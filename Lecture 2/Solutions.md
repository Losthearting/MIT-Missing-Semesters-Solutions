# Solutions of Lecture 2

1. Read `man ls` and write an `ls` command that lists files in the following manner

    - Includes all files, including hidden files
    - Sizes are listed in human readable format (e.g. 454M  instead of 454279954)
    - Files are ordered by recency
    - Output is colorized  

    A sample output would look like this

    ```bash
         -rw-r--r--   1 user group 1.1M Jan 14 09:53 baz
         drwxr-xr-x   5 user group  160 Jan 14 09:53 .
         -rw-r--r--   1 user group  514 Jan 14 06:42 bar
         -rw-r--r--   1 user group 106M Jan 13 12:12 foo
         drwx------+ 47 user group 1.5K Jan 12 18:08 ..
    ```

    **Solutions:** As for the **first command**, we need to include all the files(including hidden files), so we may use the `-a` to show the files;
    As for the **second command**, we need to list human readable format, so we use `-h` to show the readable size;  
    As for the **third command**, files are ordered by recency might be used as `-t`;  
    As for the **colorful output**, the command `--color=auto`.  
    So the `ls` command should be `ls -a -h -t --color=auto -l`.

2. Write bash functions `marco` and `polo` that do the following. Whenever you execute `marco` the current working directory should be saved in some manner, then when you execute `polo`, no matter what directory you are in, `polo` should `cd` you back to the directory where you executed `marco`. For ease of debugging you can write the code in a file `marco.sh` and (re)load the definitions to your shell by executing `source marco.sh`.

    **Solutions:** The shell script should be wirted as following:

    ```bash
        #!usr/bin/env bash
        macro(){
            tmp=$(pwd)
            echo "$tmp"`
        }

        polo(){
            cd "$tmp"
        }
    ```

    First of all, we should write the shabang(#!) to declare it is a script bash. Then there are two functions named `macro` and `polo`. When executing the `macro()`, there would be a variable named `tmp` to keep the present working directory. And when we executing the `polo()`, we will change directory into the floder where the `tmp` has already stored.

3. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run. Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.

    ```bash
        #!/usr/bin/env bash
        n=$(( RANDOM % 100 ))

        if[[n -eq 42]]; then
            echo "Something went wrong"
            >&2 echo "The error was using magic numbers"
            exit 1
        fi

        echo "Everthing went according to plan"
    ```

    **Solutions:** As we all know from the question that the shell script can be hardly make a mistake. So loop is the easiest way we can think about. We'd like to write another script that count down how many times the program runs on the one hand and document the information of mistake. The script is like the following:

    ```bash
        #!/usr/bin/env bash

        count=0  
        until [[ "$?" -ne 0 ]];
        do
            count=$((count+1))
            ./ques3 &> out.txt
        done

        echo "found error after $count times"
        cat out.txt
    ```

    Let's make a short explanation, the `count` is used for counting down the error times. And there is a `until condition, do...done` loop, the `$?` is used for documenting the failure, if there is not, it will be 0. So only when there is a mistake, the loop is going to an end. The output of the script will be stored in `out.txt` and last will output the result.

4. As we covered in the lecture `find`¡¯s `-exec` can be very powerful for performing operations over the files we are searching for. However, what if we want to do something with **all** the files, like creating a zip file? As you have seen so far commands will take input from both arguments and STDIN. When piping commands, we are connecting STDOUT to STDIN, but some commands like `tar` take inputs from arguments. To bridge this disconnect there¡¯s the `xargs` command which will execute a command using STDIN as arguments. For example `ls | xargs` rm will delete the files in the current directory. 

    Your task is to write a command that recursively finds all HTML files in the folder and makes a zip with them. Note that your command should work even if the files have spaces (hint: check `-d` flag for `xargs`).

    If you¡¯re on macOS, note that the default BSD `find` is different from the one included in GNU coreutils. You can use `-print0`on `find` and the `-0` flag on `xargs`. As a macOS user, you should be aware that command-line utilities shipped with macOS may differ from the GNU counterparts; you can install the GNU versions if you like by using brew.

    **Solutions:** I'm on Linux system, to finish the task, it's quiet simple. We just need to find out all the `html` files and then learn how to use `zip` and `xarg`.
    We just need to write on the terminal:

    ```bash
        find . -name "*.html" | xargs zip -q -r html.zip
    ```

5. **(Advanced)** Write a command or script to recursively find the most recently modified file in a directory. More generally, can you list all files by recency?

    **Solutions:** To find the files that most recently modified, we need to use the command `find`.
    
    ```bash
        find . -type f | ls -t | head -n -1
    ```

    This will show the lastest modified file. And in order to show all the files the time modified by clock, just use `ls -t -l` is easiest way to show the answer.