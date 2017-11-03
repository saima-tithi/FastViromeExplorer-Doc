FastViromeExplorer
===================
Indentify the viruses/phages and their abundance in the viral metagenomics data. FastViromeExplorer is freely available at: <a href="https://code.vt.edu/saima5/FastViromeExplorer">https://code.vt.edu/saima5/FastViromeExplorer</a>

# Installation
FastViromeExplorer requires JAVA (JDK) 1.8 or later, Samtools 1.4 or later, and Kallisto 0.43.0 or later installed in the user's machine.
## Download FastViromeExplorer
You can download FastViromeExplorer directly from github (Github link: <a href="https://code.vt.edu/saima5/FastViromeExplorer">https://code.vt.edu/saima5/FastViromeExplorer</a>) and extract it. You can also download it using the following command:
```bash
git clone git@code.vt.edu:saima5/FastViromeExplorer.git
```
From now on, we will refer the FastViromeExplorer directory in the user's local machine as project directory. The project directory will contain 5 folders src, bin, test, tools-linux, and tools-mac. It will also contain two text file ncbi-viruses-list and imgvr-viruses-list.txt.
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
Download the kallisto index file for NCBI RefSeq database "ncbi-virus-kallisto-index-k31.idx" and save it. From the project directory, run the following command:
```bash
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -i /path-to-index-file/ncbi-virus-kallisto-index-k31.idx -o $outputDirectory
```

# Run FastViromeExplorer using IMG/VR database
Download the kallisto index file for IMG/VR database "imgvr-virus-kallisto-index-k31.idx" from <a href="http://bench.cs.vt.edu/FastViromeExplorer/">http://bench.cs.vt.edu/FastViromeExplorer/</a> and save it. From the project directory, run the following command:
```bash
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -i /path-to-index-file/imgvr-virus-kallisto-index-k31.idx -l imgvr-viruses-list.txt -o $outputDirectory
```
For running FastViromeExplorer using IMG/VR database, we need to specify the kallisto index file and the list of viruses in the database along with their genome length, which is given in the file "imgvr-viruses-list.txt".

After running FastViromeExplorer using IMG/VR as reference database, the abundance output will be saved in FastViromeExplorer-final-sorted-abundance.tsv. The 1st column of this file contains the ids of the mVCs. An example id is: 7000000498.a:SRS017521_Baylor_scaffold_5189. Here, the 1st part of the id (before .a:) represents a genome id from IMG/VR database and the 2nd part (after .a:) represents the scaffold id from IMG/VR database. To retrieve the annotation of a genome or of a scaffold, you need to go to the IMG/VR webpage (<a href="https://img.jgi.doe.gov/cgi-bin/vr/main.cgi">https://img.jgi.doe.gov/cgi-bin/vr/main.cgi</a>). You can find annotation of a genome by searching for the genome id in the "Quick Genome Search" search box. Similarly, you can find annotation of a scaffold by searching for the scaffold id in the "Scaffold Search" section of IMG/VR (<a href="https://img.jgi.doe.gov/cgi-bin/vr/main.cgi?section=ScaffoldSearch">https://img.jgi.doe.gov/cgi-bin/vr/main.cgi?section=ScaffoldSearch</a>)

# Run FastViromeExplorer using Global Ocean Virome (GOV) database
Download the kallisto index file for GOV database "GOV_viral_contigs_EPI_MES.idx" and the list of viruses in the database "GOV_viral_contigs_EPI_MES_list.txt" from <a href="http://bench.cs.vt.edu/FastViromeExplorer/">http://bench.cs.vt.edu/FastViromeExplorer/</a> and save those files. From the project directory, run the following command:
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

The text file should have four columns seperated by tab. Those four columns are virus-id, virus-name, virus-taxonomy, and genome-length. The 1st and last column must have values. For 2nd and 3rd columns, you can put "N/A" if those values are not available.
Two list of viruses are already given with the source of FastViromeExplorer, the default virus list (ncbi-viruses-list.txt) containg all NCBI RefSeq viruses and the IMG/VR virus list (imgvr-viruses-list.txt). You need to create a similar list for your custom database.

An example of the virus list from "ncbi-viruses-list.txt":

NC_033618.1    Pea leaf distortion betasatellite clone N36-54, complete sequence    Unclassified;Unclassified;Unclassified;Unclassified;Unclassified;Unclassified;Pea leaf distortion betasatellite    1347

NC_028989.1    Pepper yellow leaf curl Thailand virus isolate KON-KG5 segment DNA-A, complete sequence    Unclassified;Unclassified;Unclassified;Unclassified;Geminiviridae;Begomovirus;Pepper yellow leaf curl Thailand virus    2742

...

After preparing the reference database and the list of viruses file, you can run FastViromeExplorer using the following command:
```bash
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -db /path-to-reference-database/$reference-database.fa -l /path-to-virus-list/$virus-list.txt -o $outpurDirectory
```

# Usage
java -cp bin FastViromeExplorer -1 $read1File -2 $read2File -i $indexFile -o $outputDirectory

The full parameter list of FastViromeExplorer:
<ol>
<li>-1: input .fastq file for read sequences (paired-end 1), mandatory field.</li>
<li>-2: input .fastq file for read sequences (paired-end 2).</li>
<li>-i: kallisto index file, mandatory field.</li>
<li>-db: reference database file in fasta/fa format.</li>
<li>-o: output directory, default option is the project directory.</li>
<li>-l: virus list containing all viruses present in the reference database along with their length.</li>
<li>-cr: the value of ratio criteria, default: 0.3.</li>
<li>-co: the value of coverage criteria, default: 0.1.</li>
<li>-cn: the value of number of reads criteria, default: 10.</li>
</ol>

You can also run FastViromeExplorer with more flexible or more strict criteria by specifying different values for parameters "-cr", "-co", and "-cn".

# Support
If you are having issues, please contact us at saima5@vt.edu
# License
This project is licensed under the BSD 2-clause "Simplified" License.
# Citation
If you are using our tool, please cite us:

Saima Sultana Tithi, Roderick V. Jensen, and Liqing Zhang. "FastViromeExplorer: A Pipeline for Virus and Phage Identification and Abundance Profiling in Metagenomics Data." bioRxiv (2017): 196998.
