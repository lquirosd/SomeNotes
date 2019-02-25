FIFO bash Parallel
=========================

Sometimes is useful to run some jobs in parallel, assigned by a FIFO
queue. This method will execute each "sub-job" one after the other.

> Details in this [blog](https://blog.garage-coding.com/2016/02/05/bash-fifo-jobqueue.html)

Code Example:

```bash

#!/bin/bash

#------------------------------------------------------------------------------#
#                          PARALLELIZATION FUNCTIONS 
#------------------------------------------------------------------------------#
OpenFIFO(){
    mkfifo pipe-$$
    exec 5<>pipe-$$
    rm pipe-$$
    local i=$1
    for((;i>0;i--)); do
        printf %s 000 >&5
    done
}

RunLOCKED(){
    local x
    read -u 5 -n 3 x && ((0==x)) || exit $x
    (
    "$@"
    printf '%.3d' $? >&5
    )&
}

#--- USE:
#--- first open a FIFO  queue of "n" elements i.e. "n" workers
OpenFIFO $n
#--- when parallel, define an in-place function to be executed 
for <some iterator>; do
    ToRUN(){
        <CODE TO BE EXECUTED>
    }
    RunLOCKED ToRUN
done
#--- wait for all jobs to end
wait;

```
