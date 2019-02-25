DSH 
===

DHS (dancer's shell or distributed shell) is tool for executing multiple remote 
sell commands (through ssh/rsh ...). For details [see the docs](https://www.netfort.gr.jp/~dancer/software/dsh.html)

Example of use:
1. Check NVIDIA devices GPU usage on several PCs listed on ``machines.lst``
On this example ``bsh`` runs the command ``nvidia-smi ...`` on all machines listed 
on ``machines.lst`` file. Then, output is formatted by ``dshbak`` in order to be
displayed. For details about ``bshbak`` [see the docs](https://www.ibm.com/support/knowledgecenter/en/ssw_aix_72/com.ibm.aix.cmds2/dshbak.htm)

```bash
dsh -f machines.lst -M -c "nvidia-smi --query-gpu=memory.total,memory.used,memory.free --format=csv" | dshbak'
```

Search in large directories
===========================
If you are trying to call a program with too many arguments you will see an 
error like ``ls argument list too long`` or whatever command are you using.
To see the reason behind [see the docs](https://www.in-ulm.de/~mascheck/various/argmax/).

In most of the cases the error is related to some command like ``command *`` or
``comand /foo/bar/*/*``. To prevent this error we can use ``find`` command.

Examples:
1. Count the number of files in a dir
```bash
find -type f -name '*.txt'  | wc -l
```

2. Run some command to all files that match
```bash
find -type f -name '*.txt' -exec <some_command> {} \;
```


Copy directories between machines
=================================

SCP is very useful to copy files and directories from one machine to another,
but it will dereference symbolic links (i.e. copy the file the link is pointing 
out instead the link itself). To prevent this behavior we can use ``tar`` and
``ssh`` commands.

```bash
tar cf - <dir_to_copy> | ssh <username>@<some_machine_address> tar xf -
```


