# FindFungi

Download files Fungi-Kraken and Fungi-Blast
http://bioinformatics.czc.hokudai.ac.jp/findfungi/

Terminal
# Fungal Genome BLAST Databases
wget http://bioinformatics.czc.hokudai.ac.jp/findfungi/FungalGenomeDatabases_EqualContigs.tar.gz
# Fungal Genome Kraken databases
for file in {1..32}
do
wget http://bioinformatics.czc.hokudai.ac.jp/findfungi/Kraken_$file.tar.gz
done

# Extract files Kraken_#.tar.gz
ls *gz | sort -k2n -t_ | while read line ; do tar -xvzf $line; done

# Download SRA files
# Firts create list SRAs.txt
prefetch --option-file SRAs.txt
# Convert SRA to fastq
for name in DIRECTORY
do
fasterq-dump --split-files $name/$name.sra
done

# For list Cuatro Cienegas SRAs and Pepper to search here
https://www-ncbi-nlm-nih-gov.ezproxy.u-pec.fr/Traces/study/
# SraAccList.txt is formatted like this:
SRR11192680
SRR11192681
SRR11192682
SRR11192683
SRR11192684

# Download files tests
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR675/ERR675624/ERR675624_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR675/ERR675624/ERR675624_2.fastq.gz
# Gunzip these files and concatenate them, as we have no need to read pair information.
gunzip ERR675624_*.fastq.gz
cat ERR675624_*.fastq > ERR675624_both-pairs.fastq

# Download and Install NCBI SRA Toolkit
# Download program
 wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/3.0.0/sratoolkit.3.0.0-ubuntu64.tar.gz
# Install
tar -vxzf sratoolkit.tar.gz
export PATH=$PATH:/media/strain/datos/sratoolkit.3.0.0-ubuntu64/bin
which fastq-dump
vdb-config --interactive


# Installing module with pip python
pip3 install module
