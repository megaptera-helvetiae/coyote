# This is the repository where we calculate dNdS ratios for all shared genes in our lucinid clams.

## Unfortunately, the tutorial is pretty bad in a way that it does not tell you that you have to install plenty of tools before you can run it.

Original tutorial: https://www.protocols.io/view/introduction-to-calculating-dn-ds-ratios-with-code-qhwdt7e?step=4

# So here comes my private tutorial:



## Step 2 CLUSTALO:


To make AA sequence alignemnts.

How to install clustalo:
Instructions from here: https://www.biostars.org/p/128261/

```
# You might have to install Homebrew first:
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install argtable

# Download Clustalo from its website (manually):
http://www.clustal.org/omega/
# Unzip it:
tar zxvf clustal-omega-1.2.4.tar.gz
# cd into it
./configure CFLAGS="-I/usr/local/include" LDFLAGS="-L/usr/local/lib" --prefix="/usr/local"

make
sudo make install
which clustalo
/usr/local/bin/clustalo

# If you are working on a server:

# download this file from the Clustalo website: http://www.clustal.org/omega/
scp /Users/megaptera/Downloads/clustal-omega-1.2.4.tar.gz megaptera@barbera.genomecenter.ucdavis.edu:/share/eisenlab/megaptera/clam_alignments/

# You also need argtable2!
wget -qO- http://prdownloads.sourceforge.net/argtable/argtable2-13.tar.gz > argtable2-13.tar.gz
tar zxvf argtable2-13.tar.gz
cd argtable2-13
./configure --prefix="/share/eisenlab/megaptera/clam_alignments"

# Now go into Clustalo directory and run the following:
./configure CFLAGS="-I/share/eisenlab/megaptera/clam_alignments/clustal-omega-1.2.4/argtable2-13/src/" LDFLAGS="-L/share/eisenlab/megaptera/clam_alignments/lib/" --prefix="/share/eisenlab/megaptera/clam_alignments"

```

And once installed, we run:
```
clustalo -i 998674.ATTE01000001_gene588.faa -o cluster_1.aln.faa
(in this case in /share/eisenlab/megaptera/clam_alignments)
```


## Step 3:


PAL2NAL converts a multiple sequence alignment of proteins and the corresponding DNA (or mRNA) sequences into a codon alignment. Synonymous (Ks) and non-synonymous (Ka) substitution rates can be calculated.

How to get pal2nal.pl:
https://raw.githubusercontent.com/LANL-Bioinformatics/PhaME/master/src/pal2nal.pl
Throw it into your working directory.

And once installed, we run:
```
perl pal2nal.pl cluster_1.aln.faa 998674.ATTE01000001_gene588.fna -output paml -nogap > cluster_1.pal2nal
```


## Step 4:


To run codeml all we need to do is type 'codeml' in the same folder that the codeml.ctl file is in
I installed a GUI here: /Volumes/pamlX1.3.1-osx-x86_64/pamlX.app/Contents/MacOS
However, that does not help. I want to run it in terminal. (not ./pamlX)

Download source code from here: http://abacus.gene.ucl.ac.uk/software/paml.html
And download the latest archive and save on your hard disk. 
Unzip it.
scp /Users/megaptera/Downloads/paml4.8a.macosx.tgz megaptera@barbera.genomecenter.ucdavis.edu:/share/eisenlab/megaptera/clam_alignments/

```
cd src
make
ls -lF
mv baseml basemlg codeml pamp evolver yn00 chi2 ../bin
cd ..
bin/codeml 
```

Installing instructions can be found here: https://embnet.vital-it.ch/CoursEMBnet/PagesPHYL07/Exercises/day2/pamlDOC.pdf

And once installed, we run:
```
paml4.8/bin/codeml (basically step 3)
```



## Parse output:

```
python parse_codeml_output.py codeml.txt
```

All is now working!


But I actually do not want to run pairwise comparisons.
Should I change the value of 'runmode'? See thread here: https://www.biostars.org/p/48370/
PAML Manual is here: https://embnet.vital-it.ch/CoursEMBnet/PagesPHYL07/Exercises/day2/pamlDOC.pdf


## Try to clone Kevin's repository

I could not do it because I do not know his user name!!!
