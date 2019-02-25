Lattice char2word High Entropy Samples Time Bound
=================================================

In some cases if the entropy of the confidence matrix of some sample is too
high the lattice will be huge, hence normal pruning could be NOT enough or the 
process painfully slow. In order to circumvent this issue is better process each
line separately and use a max time bound to process it.

## Proposed solution
1. Run it on parallel using ``N`` workers.
2. Always use pruning (``--beam=<>``).
3. If process is taking too much kill it and reduce the ``beam``.

Sample code:

> For info about parallel execution using ``RunLOCKED`` see [FIFO Bash Parallel](bash_parallel.md)

```bash
work_folder=<SOME FOLDER>;
lat_file=<LAT FILE which contains all the char level lattices>;

max_time=<MAX time allowed per line-job, in minutes>;
def_beam=<DEFAULT BEAM>;
min_beam=<MINIMUM_BEAM>;

#--- Create temp files (one lattice/file per text line)
tmp_dir=$work_folder/`basename "${0%.*}"`_$(((RANDOM%99999) +1 ))
mkdir -p $tmp_dir/word
lattice-copy "ark:gunzip -c $lat_file |" ark,t:- | 
awk -v out=$tmp_dir -v RS= '{print > (out"/tmp-" NR ".lat")}';
#--- for each text line gen a word lattice
for l in $tmp_dir/tmp-*.lat; do
    ToRUN(){
        #--- OTHER processing steps before gen the lattice
        #---                .
        #---                .

        #--- check if job is taking more that $max_time; if so 
        #--- kill and re-run using $min_beam.
        timeout -s 9 $max_time \
            lattice-char-to-word --beam=$def_beam <OTHER_ARGUMENTS> || \
            lattice-char-to-word --beam=$min_beam <OTHER_ARGUMENTS>;
            
        #--- OTHER steps over the generated word lattice 
        #---                .
        #---                .

        #--- remove temp files
        rm ${tmp_dir}/${lat_name}* ;
    }   
    RunLOCKED ToRUN
done
wait;
```
