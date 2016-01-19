# Mogon_HPC_course
Introductory course to the cluster system of the JGU

## Getting help
The HPC provides help in profiling programs and also improving (paralellizing) your own tools. Contact them via the mailling-list to have a quick answer. There is also the possibility of meeting with them (Thursday workshop) and discuss your problems.

They also provide help on writting grants, specifically the HPC details - if needed. Gratis. 

When asking for help with an error, prepare a minimum example of the code and a small set of data. Also confer access to the relevant folders.
They already provide manz courses, and will provide many more courses, including C++, python, openMP.

## Software instalation
Go to the new webpage, software instalation, and fill the form.

## Mogon
Name derived from the Roman name for Mainz, *Mongutia*.
- 500 nodes
- 64 cores, with at least 128GiB RAM
- Nodes connected with QDR (2.7 GB/s)
- Parallel gtsf file system.

## Logging in
Enbale ssh fowarding in your computor
ssh mogon

Check the wiki for instructions.

Once logged in, use the login node only to copy data and submit jobs. It is however ok to make simple tests, even in parallel.
firefox --no-remote will load a webpage for Linux Computing with programs to download and manuals.

## Directory structure
Home directory should be used to store programs/scripts, shared data and binaries. Clean data regularly and tidy up.
There is a regular backup in `/home/username` and `/home/username` and folders `temp` and `scratch`.

*Bad ideas:*
- use a million files
- access many many files at the same time in the same directory

Save save space and time with tar. `tar cf archive.tar directory` to pack, and `tar czf archive.tar directory` to pack and compress. To unpack use `c` instead of `z`.

mogon.fs.zdv.uni-mainz is the dedicated file server. Use sFTP is faster and secure transfer - better than scp or rsync.


## Scheduler and submmiting jobs
- LSF, IBM's
- with Mogon2 it will chance.

Test a jobs with `bsub -q hpckurs date` and the job output should be sent to your email. `bqueues -l short` will give details of the requirements for that queue. More than 64 cores we shoudl use `nodeshort` and `nodelong`. The maximum time limite is 5h in long, bz default and and 300MB by default. With `bsub -W 5:00` will reserve a specific time and can be longer than the 5 hours.

`bsub -n 8 -R "span[tile=8]"` makes sure that all 8 cores are from the same computer node. 

Some hands on examples:

```bash
module load mpi/intelmpi/5.1.079
mkdir mogon-intro
cd mogon-intro

cp -r /cluster/hpc_course/mogon-intro/* .

# let's calculate pi
cd mpi_example
bsub -q hpckurs -n 4 -R 'span[ptile=2]' -Is time mpirun -np 4 ./mpi_pi 1000000000
```

Other options:
- `-o joboutput.txt` will save stder/sdout and with `-N` it will still trigger an email. 
- `-u some@mail.de` will send an email to another email address.
- `-J jobname` is usefull to know what the job is when listed with `bjobs`.

```bash
cd mogon-intro
cd pipe_bsub

./pipe_and_compress.sh fastq
```


## Memory
Memory is requested per job slot. The simplest way is `bsub -app Reserve10G`and `bapp` will list all the available pre-set memory reservations. Memory reserved per core. `-app` is just a short cut to putting memory requirements in the `bsub` command.


## Useful commands
Do not type `bjobs` very frequently in the command line because it will send a request to the entire system everytime. `bpeek jobid` will let you know the out.err of the system. Btw, I checked all jobs being run with `bjobs -u all | wc -l` and it's ~40000. This is a normal day at mogon. `bhosts i0001` will tell you the status of a particular host. `lshosts | head` for another overview. `lsload a0001` for the current usage of a host. *Important*, `btop jobid` and `bbot jobid` will put a job at the top of bottom of your job queue. It should not be used very frequentely though since it takes resources. `bkill jobid` will kill the job (probably). `bmod` is a fairly complex command that allows changing of pending commands (alter memory requirements for instance).  


## RAM disk


```bash
cd mogon-intro
cd ramdisk
bsub < concat_remote.sh
```

In this script some files are copied to RAM and executed there.
