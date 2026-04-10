CONTRIBUTIONS

TODO: write a brief summary of how each team member contributed to
the project.

Edmund Tsou and Evan Batten worked collaboratively on all parts.

REPORT

TODO: add your report according to the instructions in the
"Experiments and analysis" section of the assignment description.

Experiments and analysis:
Output:
bash-5.2$ ./run_experiments.sh
rm -f *.o parsort is_sorted gen_rand_data
gcc -g -Wall -c parsort.c -o parsort.o
gcc -o parsort parsort.o
gcc -g -Wall -c is_sorted.c -o is_sorted.o
gcc -o is_sorted is_sorted.o
gcc -g -Wall -c gen_rand_data.c -o gen_rand_data.o
gcc -o gen_rand_data gen_rand_data.o
Wrote 16777216 bytes to '/tmp/etsou2/data_16M.in'
Test run with threshold 2097152

real    0m0.399s
user    0m0.385s
sys     0m0.009s
Test run with threshold 1048576

real    0m0.240s
user    0m0.413s
sys     0m0.033s
Test run with threshold 524288

real    0m0.156s
user    0m0.410s
sys     0m0.054s
Test run with threshold 262144

real    0m0.129s
user    0m0.442s
sys     0m0.055s
Test run with threshold 131072

real    0m0.138s
user    0m0.460s
sys     0m0.058s
Test run with threshold 65536

real    0m0.118s
user    0m0.481s
sys     0m0.079s
Test run with threshold 32768

real    0m0.138s
user    0m0.522s
sys     0m0.101s
Test run with threshold 16384

real    0m0.135s
user    0m0.528s
sys     0m0.167s

Analysis:
Running the experiment on the 16 MB randomly generated input file the observed real running time was:
Threshold 2097152: real 0m0.399s
Threshold 1048576: real 0m0.240s
Threshold 524288:  real 0m0.156s
Threshold 262144:  real 0m0.129s
Threshold 131072:  real 0m0.138s
Threshold 65536:   real 0m0.118s
Threshold 32768:   real 0m0.138s
Threshold 16384:   real 0m0.135s

This shows that the the general trend is that decreasing the threshold improves performance since more child subprocesses are created allowing for more parallelism. 
With a very large threshold, most of the work was done sequentially, so the total running time was higher. As the threshold got smaller, the work was divided across multiple CPU cores.
However, the speedup peaked at threshold 65536 at 0.118 seconds and lower threshold values actually increased time, decreasing performance. 
After the threshold becomes too small, creating more child processes introduces overhead from fork and waitpid, and eventually there are more parallel tasks than available CPU cores.
At that point, the extra process-management cost outweighs the benefit of parallelism. 
This suggests that 65536 gave a good balance between parallel execution and process overhead for the CPU cores we had available. 