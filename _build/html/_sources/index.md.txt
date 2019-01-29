FastViromeExplorer
===================
Indentify the viruses/phages and their abundance in the viral metagenomics data. 
The paper describing FastViromeExplorer is available from here: <a href="https://peerj.com/articles/4227/">https://peerj.com/articles/4227/</a>. 
FastViromeExplorer is freely available at: <a href="https://code.vt.edu/saima5/FastViromeExplorer">https://code.vt.edu/saima5/FastViromeExplorer</a>

# Installation
FastViromeExplorer requires JAVA (JDK) 1.8 or later, Samtools 1.4 or later, and Kallisto 0.43.0 or 0.43.1 installed in the user's machine. As in later versions of Kallisto, the output format of pseudoalignments is different, please use Kallisto version 0.43.0 or 0.43.1.
## Download FastViromeExplorer
You can download FastViromeExplorer directly from VT github (VT Github link: <a href="https://code.vt.edu/saima5/FastViromeExplorer">https://code.vt.edu/saima5/FastViromeExplorer</a>) in zipped format using the "Download" button <img src="../../_static/download-button.png" alt="" align="middle"> and extract it. You can also download it from "Releases" page of FastViromeExplorer github repository (<a href="https://github.com/saima-tithi/FastViromeExplorer/releases">https://github.com/saima-tithi/FastViromeExplorer/releases</a>).

From now on, we will refer the FastViromeExplorer directory in the user's local machine as `project directory`. The `project directory` will contain 6 folders: src, bin, test, tools-linux, tools-mac, and utility-scripts. It will also contain two text files: ncbi-viruses-list and imgvr-viruses-list.txt.
## Install Java
If Java is not already installed, you need to install Java (JDK) 1.8 or later from the following link: <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html">http://www.oracle.com/technetwork/java/javase/downloads/index.html</a>. From this link, download the appropriate jdk installation file (for linux or macOS), and then install Java by double-clicking the downloaded installation file.
After installing Java, you can check it by running the following command in terminal:
```bash
java -version
```
It should show you something similar to the following output:
```bash
java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
Java HotSpot(TM) 64-Bit Server VM (build 25.144-b01, mixed mode)
```
## Install Kallisto and Samtools
If Kallisto or Samtools is not installed, you can install it from the executables distributed with FastViromeExplorer.
In terminal, go into the project directory. Then go into the `tools-linux` folder if you are using a linux machine or go into the `tools-mac` folder if you are using macOS. Copy the kallisto and samtools executables from this directory to the /usr/local/bin directory.

```bash
cd /path-to-FastViromeExplorer/tools-linux
sudo cp kallisto /usr/local/bin/
sudo cp samtools /usr/local/bin/
```
Or

```bash
cd /path-to-FastViromeExplorer/tools-mac
sudo cp kallisto /usr/local/bin/
sudo cp samtools /usr/local/bin/
```
After installing Kallisto and Samtools, you can check it by running the following commands to check their version.
```bash
kallisto version
samtools
```
It will show you outputs like this in terminal:
```bash
kallisto, version 0.43.1
```
```bash
Program: samtools (Tools for alignments in the SAM format)
Version: 1.5 (using htslib 1.5)
...
```

If the executables given for kallisto or samtools is not working properly, you need to download and install them from source from their respective websites. 

You can download samtools source from this link <a href="https://sourceforge.net/projects/samtools/files/">https://sourceforge.net/projects/samtools/files/</a> or from this link <a href="https://github.com/samtools/samtools/releases">https://github.com/samtools/samtools/releases</a> and then install it by following their installation guideline. 

You can download kallisto executables from this link <a href="https://pachterlab.github.io/kallisto/download">https://pachterlab.github.io/kallisto/download</a>.

## Install Kallisto and Samtools without sudo access
If you do not have sudo access, you can install them locally by updating the .bashrc file in your home directory. You need to add the following line in your .bashrc file:
```bash
export PATH=$PATH:/path-to-FastViromeExplorer-project-directory/tools-linux
```
Or 

```bash
export PATH=$PATH:/path-to-FastViromeExplorer-project-directory/tools-mac
```

## Install FastViromeExplorer
In terminal, go into the project directory, which should contain `src` and `bin` folders. From the project directory, run the following command:
```bash
javac -d bin src/*.java
```
After installing FastViromeExplorer, you can check it by running the following command from the same project directory:
```bash
java -cp bin FastViromeExplorer
```
It will print the usage and parameter list of FastViromeExplorer in terminal.

# Run FastViromeExplorer using test data
From the project directory, run the following commands:
```bash
mkdir test-output
java -cp bin FastViromeExplorer -1 test/reads_1.fq -2 test/reads_2.fq -i test/testset-kallisto-index.idx -o test-output
```
The test input files are given in the `test` folder. Here, the input files are:
<ol>
<li><i>reads_1.fq</i> and <i>reads_2.fq</i> : paired-end reads in fastq format</li>
<li><i>testset-kallisto-index.idx</i> : kallisto index file generated for a small set of NCBI RefSeq viruses</li>
</ol>
The output files will be generated in the `test-output` directory. The output files are:
<ol>
<li><i>FastViromeExplorer-reads-mapped-sorted.sam</i> : aligned/mapped reads in sam format</li>
<li><i>FastViromeExplorer-final-sorted-abundance.tsv</i> : virus abundance result in tab-delimited format</li>
</ol>
In a similar manner, we can run FastViromeExplorer for single-end reads without specifying the "-2" parameter. An example of running FastViromeExplorer for single-end reads:

```bash
mkdir test-output
java -cp bin FastViromeExplorer -1 test/reads_1.fq -i test/testset-kallisto-index.idx -o test-output
```

# Run FastViromeExplorer using NCBI RefSeq database
Some pre-computed kallisto index files are given in the following link: <a href="http://bench.cs.vt.edu/FastViromeExplorer/">http://bench.cs.vt.edu/FastViromeExplorer/</a>.
Download the kallisto index file for NCBI RefSeq database "ncbi-virus-kallisto-index-k31.idx" and save it. In terminal, go into the project directory. From the project directory, run the following command:
```bash
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -i /path-to-index-file/ncbi-virus-kallisto-index-k31.idx -o $outputDirectory
```

# Run FastViromeExplorer using IMG/VR database
Download the kallisto index file for IMG/VR database "imgvr-virus-kallisto-index-k31.idx" from <a href="http://bench.cs.vt.edu/FastViromeExplorer/">http://bench.cs.vt.edu/FastViromeExplorer/</a> and save it. In terminal, go into the project directory. From the project directory, run the following command:
```bash
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -i /path-to-index-file/imgvr-virus-kallisto-index-k31.idx -l imgvr-viruses-list.txt -o $outputDirectory
```
For running FastViromeExplorer using IMG/VR database, we need to specify the kallisto index file and the list of viruses in the database along with their genome length, which is given in the file "imgvr-viruses-list.txt".

After running FastViromeExplorer using IMG/VR as reference database, the abundance output will be saved in FastViromeExplorer-final-sorted-abundance.tsv. The 1st column of this file contains the ids of the mVCs. An example id is: 7000000498.a:SRS017521_Baylor_scaffold_5189. Here, the 1st part of the id (before .a:) represents a genome id from IMG/VR database and the 2nd part (after .a:) represents the scaffold id from IMG/VR database. To retrieve the annotation of a genome or of a scaffold, you need to go to the IMG/VR webpage (<a href="https://img.jgi.doe.gov/cgi-bin/vr/main.cgi">https://img.jgi.doe.gov/cgi-bin/vr/main.cgi</a>). You can find annotation of a genome by searching for the genome id in the "Quick Genome Search" search box. Similarly, you can find annotation of a scaffold by searching for the scaffold id in the "Scaffold Search" section of IMG/VR (<a href="https://img.jgi.doe.gov/cgi-bin/vr/main.cgi?section=ScaffoldSearch">https://img.jgi.doe.gov/cgi-bin/vr/main.cgi?section=ScaffoldSearch</a>)

# Run FastViromeExplorer using Global Ocean Virome (GOV) database
Download the kallisto index file for GOV database "GOV_viral_contigs_EPI_MES.idx" and the list of viruses in the database "GOV_viral_contigs_EPI_MES_list.txt" from <a href="http://bench.cs.vt.edu/FastViromeExplorer/">http://bench.cs.vt.edu/FastViromeExplorer/</a> and save those files. In terminal, go into the project directory. From the project directory, run the following command:
```bash
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -i /path-to-index-file/GOV_viral_contigs_EPI_MES.idx -l /path-to-virus-list/GOV_viral_contigs_EPI_MES_list.txt -o $outputDirectory
```

# Run FastViromeExplorer using custom database
You can run FastViromeExplorer using any custom database.

To run FastViromeExplorer using a custom database, you need to prepare two files:
<ol>
<li>A fasta file containing the reference genomes</li>
<li>A text file containing the list of reference genomes along with the genome length</li>
</ol>

The text file should have four columns seperated by tab. Those four columns are virus-id, virus-name, virus-taxonomy, and genome-length. The 1st and last column must have values. For 2nd and 3rd columns, you can put "N/A" if those values are not available. This file should not have any header.
Two list of viruses are already given with the source of FastViromeExplorer, the default virus list (ncbi-viruses-list.txt) containg all NCBI RefSeq viruses and the IMG/VR virus list (imgvr-viruses-list.txt). You need to create a similar list for your custom database.

An example of the virus list from "ncbi-viruses-list.txt":

NC_033618.1    Pea leaf distortion betasatellite clone N36-54, complete sequence    Unclassified;Unclassified;Unclassified;Unclassified;Unclassified;Unclassified;Pea leaf distortion betasatellite    1347

NC_028989.1    Pepper yellow leaf curl Thailand virus isolate KON-KG5 segment DNA-A, complete sequence    Unclassified;Unclassified;Unclassified;Unclassified;Geminiviridae;Begomovirus;Pepper yellow leaf curl Thailand virus    2742

...

A quick way to generate this virus-list.txt file is using the bash script named <i>generateGenomeList.sh</i> provided in `utility-scripts` folder. Go to the `utility-scripts` folder and run the following command to generate virus-list.txt file for the reference database:
```bash
bash generateGenomeList.sh /path-to-reference-database/$reference-database.fa /path-to-virus-list-file/$virus-list.txt
```
For this bash script, the 1st parameter is the fasta file for reference database and the 2nd parameter is the output file name. After running this script, you will have a FastViromeExplorer compatible virus-list.txt file, which will have four tab separated columns. The 1st and 4th column will have genome-id and genome-length, the 2nd and 3rd column will just have "N/A".

After preparing the reference database and the list of viruses file, you can run FastViromeExplorer using the following command:
```bash
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -db /path-to-reference-database/$reference-database.fa -l /path-to-virus-list-file/$virus-list.txt -o $outpurDirectory
```

If you are going to use the same custom database many times, it is better to generate a kallisto index file for this database and save it, and then use this index file for running FastViromeExplorer. You can generate a kallisto index file using the following command:
```bash
kallisto index -i kallisto-index.idx /path-to-reference-database/$reference-database.fa
```
It will generate a file named "kallisto-index.idx". Then you can run FastViromeExplorer using this index file using the following command:
```bash
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -i /path-to-index-file/kallisto-index.idx -l /path-to-virus-list-file/$virus-list.txt -o $outpurDirectory
```

# Usage
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -i $indexFile -o $outputDirectory

The full parameter list of FastViromeExplorer:
<ol>
<li>-1: input .fastq file or .fastq.gz file for read sequences (paired-end 1), mandatory field.</li>
<li>-2: input .fastq file or .fastq.gz file for read sequences (paired-end 2).</li>
<li>-i: kallisto index file, mandatory field.</li>
<li>-db: reference database file in fasta/fa format.</li>
<li>-o: output directory, default option is the project directory.</li>
<li>-l: virus list containing all viruses present in the reference database along with their length.</li>
<li>-cr: the value of ratio criteria, default: 0.3.</li>
<li>-co: the value of coverage criteria, default: 0.1.</li>
<li>-cn: the value of number of reads criteria, default: 10.</li>
<li>-salmon: use salmon instead of kallisto, default: false. To use salmon pass '-salmon true' as parameter.</li>
</ol>

You can also run FastViromeExplorer with more flexible or more strict criteria by specifying different values for parameters "-cr", "-co", and "-cn". Increasing the values for parameters "-cr", "-co", and "-cn" will output more "high confidence" or more specific set of viruses. On the other hand, decreasing the values will increase sensitivity.

# Support
If you are having issues, please contact us at saima5@vt.edu
# License
This project is licensed under the BSD 2-clause "Simplified" License.
# Citation
If you are using our tool, please cite us:

Saima Sultana Tithi, Frank O. Aylward, Roderick V. Jensen, and Liqing Zhang. "FastViromeExplorer: a pipeline for virus and phage identification and abundance profiling in metagenomics data." PeerJ 6 (2018): e4227.
