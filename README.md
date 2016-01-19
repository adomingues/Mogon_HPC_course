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


## Scheduler
- LSF, IBM's
- with Mogon2 it will chance.

Test a jobs with `bsub -q hpckurs date` and the job output should be sent to your email.
