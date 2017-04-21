# DCBLAST

The Basic Local Alignment Search Tool (BLAST) is by far best the most widely used tool in for sequence analysis for rapid sequence similarity searching among nucleic acid or amino acid sequences. Recently, cluster, grid, and cloud environmentshave been are increasing more widely used and more accessible as high-performance computing systems. Divide and Conquer BLAST (DCBLAST) has been designed to perform run on grid system with query splicing which can run National Center for Biotechnology Information (NCBI) BLASTBLAST search comparisons  over withinthe cluster, grid, and cloud computing grid environment by using a query sequence distribution approach NCBI BLAST. This is a promising tool to accelerate BLAST job dramatically accelerates the execution of BLAST query searches using a simple, accessible, robust, and practical with extremely easy access, robust and practical approach.


## Requirement

Following basic softwares are needed to run

- Perl (Any version 5+)

```
$ which perl
$ perl --version
```

- NCBI-BLAST+ (Any version)

```
$ which blastn
```

- Sun Grid Engine (Any version)

```
$ which qsub
```

- Grid cloud or distributed computing system.

## Prerequisites

The following Perl modules are required:

- Path::Tiny
- Data::Dumper
- Config::Tiny

Install prerequisites with the following command:

```
$ cpan `cat requirement`
```
or
```
$ cpanm `cat requirement`
```
or 
```
$ cpanm Path::Tiny Data::Dumper Config::Tiny
```
We strongly recommend to use Perlbrew http://perlbrew.pl/ to avoid having to type sudo

We also recommend to use 'cpanm' https://github.com/miyagawa/cpanminus



## Installation

The program is a single file Perl scripts. Copy it into executive directories.

We recommend to copy it on scratch disk.


```
cd ~/scratch/  # We recommend to copy it on scratch disk.

git clone git://github.com/ascendo/DCBLAST.git

cd ~/scratch/DCBLAST

perl dcblast.pl

Usage : dcblast.pl --ini config.ini --input input-fasta --size size-of-group --output output-filename-prefix  --blast blast-program-name

  --ini <ini filename> ##config file ex)config.ini

  --input <input filename> ##query fasta file

  --size <output size> ## size of chunks usually a ll core x 2, if you have 160 core all nodes, you can use 320. please check it to your admin.

  --output <output filename> ##output folder name

  --blast <blast name> ##blastp, blastx, blastn and etcs.

  --dryrun Option will only split fasta file into chunks


```



## Configuration

Please edit config.ini before you run!!

```
[dcblast]
##Name of job (will use for SGE job submission name)
job_name_prefix=dcblast

[blast]
##BLAST options

##BLAST path (your blast+ path); $ which blastn; then remove "blastn"
path=~/bin/blast/ncbi-blast-2.2.30+/bin/

##DB path (build your own BLAST DB)
##example
##makeblastdb -in example/test_db.fas -dbtype nucl (for nucleotide sequence)
##makeblastdb -in example/your-protein-db.fas -dbtype prot (for protein sequence)
db=example/test_db.fas

##Evalue cut-off (See BLAST manual)
evalue=1e-05

##number of threads in each job. If your CPU is AMD it needs to be set 1.
num_threads=2

##Max target sequence output (See BLAST manual)
max_target_seqs=1

##Output format (See BLAST manual)
outfmt=6

##any other option can be add it this area

[sge]
##Grid job submission commands
##please check your job submission scripts
##Especially Queue name and Threads option will be different depends on your system
pe=SharedMem 1
M=your@email
o=log
q=common.q
j=yes
cwd=

```
If you need any other options for your enviroment please contant us.


## Usage


```
perl dcblast.pl

Usage : dcblast.pl --ini config.ini --input input-fasta --size size-of-group --output output-filename-prefix  --blast blast-program-name

  --ini <ini filename> ##config file ex)config.ini

  --input <input filename> ##query fasta file

  --size <output size> ## size of chunks usually a ll core x 2, if you have 160 core all nodes, you can use 320. please check it to your admin.

  --output <output filename> ##output folder name

  --blast <blast name> ##blastp, blastx, blastn and etcs.

  --dryrun Option will only split fasta file into chunks


```


## Examples

### Dryrun (--dryrun option will only split fasta file into chunks)
```
perl dcblast.pl --ini config.ini --input example/test.fas --output test --size 20 --blast blastn --dryrun
```
```
DRYRUN COMMAND : [qsub -M your@email -cwd -j yes -o log -pe SharedMem 1 -q common.q -N dcblast_split -t 1-20 dcblast_blastcmd.sh]
DRYRUN COMMAND : [qsub -M your@email -cwd -j yes -o log -pe SharedMem 1 -q common.q -hold_jid dcblast_split -N dcblast_merge dcblast_merge.sh test/results 20]
DRYRUN COMMAND : [qstat]
DONE


```
### Run

```
perl dcblast.pl --ini config.ini --input example/test.fas --output test --size 20 --blast blastn 
```

This run will splits file into 20 chunks, run on 20 cores and generated BLAST output file "test/results/merged" and chunked input file "test/chunks/"


## Citation
Won Cheol Yim and John Cushman (2017) Divide and Conquer BLAST: using grid engines to accelerate BLAST and other sequence analysis tools. PeerJ submitted.



## Copyright

The program is copyright by Yim, Won Cheol.
