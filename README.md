# FindFungi - Hackaton
FindFungi es un pipeline que permite la búsqueda de hongos presentes en muestras metagenómicas de Shotgun.
El siguiente pipeline es una modificación del desarrollado por [Donovan y col., 2018](https://doi.org/10.1371/journal.pone.0192898).

## Prerriquisitos
- gcc version 4.4.4 20100726 (Red Hat 4.4.4-13)
- coreutils 8.27
- skewer 0.2.2
- kraken 0.10.5-beta
- ncbi blast 2.2.30
- Rscript 3.3.3 (packages: wordcloud)
- graphviz 2.40.1
- python 2.7.13 (modules: sys, os, ete3, Bio, math, argparse, itertools, collections & re)
    
## Instalación
- Descarga todos los scripts de GitHub/GiantSpaceRobot y guardalos en una carpeta de Scripts. Necesitas darles el permiso (chmod 755 * ).
- El script FindFungi-withoutBSUB.sh y cambia las rutas absolutas que se indiquen dentro del script.
- De igual manera edita la ruta abosluta indicada en el script LowestCommonAncestor.sh.
- NOTA: puede ser necesario que incluyas la ruta absoluta para todos los scripts y herramientas dentro del script master FindFungi-withoutBSUB.sh, dependiendo de las preferencias del nodo del cluster.
- Descargar la base de datos de [Kraken y BALST](http://bioinformatics.czc.hokudai.ac.jp/findfungi/)
~~~
# Fungal Genome BLAST Databases
wget http://bioinformatics.czc.hokudai.ac.jp/findfungi/FungalGenomeDatabases_EqualContigs.tar.gz

# Fungal Genome Kraken Databases
for file in {1..32}
do
wget http://bioinformatics.czc.hokudai.ac.jp/findfungi/Kraken_$file.tar.gz
done
~~~
- Descromprime los archivos
~~~
ls *gz | sort -k2n -t_ | while read line
do 
tar -xvzf $line
done
~~~

## Datos de prueba
Descarga el conjuntos de datos ERR675624 de la European Nucleotide Archive. Este conjunto contiene lecturas de hongos.
~~~
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR675/ERR675624/ERR675624_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR675/ERR675624/ERR675624_2.fastq.gz
~~~
Descomprime los archivos y concatenalos
~~~
gunzip ERR675624_*.fastq.gz
cat ERR675624_*.fastq > ERR675624_both-pairs.fastq
~~~

## Ejecuta el pipeline
~~~
./FindFungi-0.23.sh /path/to/ERR675624_both-pairs.fastq ERR675624
~~~
El resultado de .csv debería mostrar lo siguiente:
~~~
#Taxon name,Taxid,Reads mapping to taxid,Reads mapping to children taxids,Pearson skewness score,Percent of pseudo-chromosomes with read hits
Candida sp. LDI48194,1759314,671,0,0.524623062587,100.0
Malassezia restricta,76775,378,0,0.496034792692,100.0
Candida tropicalis MYA-3404,294747,265,0,-0.265788716977,100.0
~~~

## Descargar e instalar NCBI SRA Toolkit
A través de herramientas en línea de comando se pueden obtener los datos necesarios para continuar con este pipeline.
~~~
# Descargar programa
 wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/3.0.0/sratoolkit.3.0.0-ubuntu64.tar.gz
# Instalar
tar -vxzf sratoolkit.tar.gz
export PATH=$PATH:/media/strain/datos/sratoolkit.3.0.0-ubuntu64/bin
which fastq-dump
vdb-config --interactive
~~~

## Datos obtenidos de NCBI SRA
Para obtener los archivos `fastq` a partir del [NCBI-SRA](https://www-ncbi-nlm-nih-gov.ezproxy.u-pec.fr/Traces/study/) usando el ID BioProject. Descarga SRR_Acc_List.txt de la sección "Accession List" de todos los archivos seleccionados. Con este archivo y con la herramienta `SRA toolkit`.
El formato SRR_Acc_List.txt es como sigue:
~~~
SRR11192680
SRR11192681
SRR11192682
SRR11192683
SRR11192684
~~~
Descargar archivos SRA
~~~
prefetch --option-file SRR_Acc_List.txt
~~~
Convertir SRA a fastq
~~~
for name in DIRECTORY
do
fasterq-dump --split-files $name/$name.sra
done
~~~
## Instalación de Kraken2
Antes de instalar kraken 2, necesitarás actualizar la versión de sudo con:
```
$ sudo apt-get update
```
Entonces puedes instalar el paquete
```
$ sudo apt-get install kraken2
```
El siguiente paso en la instalación podría tratar de instalar la paquetería manualmente por descargar con anaconda (el cual deberás tener previamente instalado https://docs.anaconda.com/anaconda/install/):
```
$ conda install -c bioconda kraken2
```
Una vez que hayas descargado los datos, necesitarás correr el script de bash:
```
$ bash Anaconda3.....2022.sh
```
Para seguir con el código anterior, presiona enter para continuar y acepta los términos de instalación. Para finalizar el proceso, necesitan configurar el PATH donde quieras ubicarlo, de lo contrario, deja las condiciones en default.






