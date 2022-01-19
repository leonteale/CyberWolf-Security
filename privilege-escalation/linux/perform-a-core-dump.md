# Perform a core dump

reference: [https://linux-audit.com/understand-and-configure-core-dumps-work-on-linux/](https://linux-audit.com/understand-and-configure-core-dumps-work-on-linux/)



The plan is to execute the program, have it read the file into memory, and then purposefully crash the program. Causing a core dump will dump the contents of the applications memory to a file.

```
./count
Enter source file/directory name: /root/root.txt

Total characters = 33
Total words      = 2
Total lines      = 2
Save results a file? [y/N]: ^Z
[1]+  Stopped                 ./count


ps
    PID TTY          TIME CMD
   2213 pts/4    00:00:00 bash
   2373 pts/4    00:00:00 count
   2374 pts/4    00:00:00 ps

kill -SIGSEGV 2373


fg
./count
Bus error (core dumped)
```

The core dump files are located at `/var/crashes`, and they can be unpacked using `apport-unpack` to view the data.

```
apport-unpack /var/crashes/count.1000.crash /tmp/crash-report
```

You can use 'less' to view the core dump, but its a binary file and the data is hard to sift through. 'xxd' would be a good option, but since we’re looking for a flag string, using the 'strings' command is the best call.
