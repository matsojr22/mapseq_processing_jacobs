# MAPseq Analysis Script
MAPseq processing code based on previous works and designed to be used with the CSHL python pipeline.

Code found here is generally a work in progress until publication.

## **Before you run:**
- All dependencies are known to work on debian based linux distributions. Mac is likely to work as well. Windows is currently untested, but if all the dependencies have windows versions things should work.
- Be sure that you have processed your fastq files using the [CSHL mapseq-processing Python Pipeline](https://github.com/ZadorLaboratory/mapseq-processing).
- This script **process-nbcm-tsv.py** uses the spike in normalized tsv produced by that pipeline (sample.nbcm.tsv). If you want to run a full analysis, you will need to ensure that the fastq processing has included: your samples, your negative control, and your injection columns in the output. Partial analysis is also possible at your discretion; there is a provided truncated sample dataset and associated labels which you can check out for guidance.
- Setup a new conda environment, repos, and dependencies as shown below.
- Run analysis.
<br/>

## Installation

1. Install mini-conda for your operating system. [mini-conda quick command line install](https://docs.anaconda.com/miniconda/install/#quick-command-line-install)

2.  With conda installed create a new environment preloaded with pip

 ```
conda create -n mapseq_processing python==3.9 pip
```

3. Activate your new environment

```
conda activate mapseq_processing
```

4. Install additional repositories

```
conda config --add channels conda-forge
conda config --add channels bioconda
```

5. Browse to your git directory and clone this project

```
cd /home/your_user/git/
git clone https://github.com/matsojr22/mapseq_processing_jacobs.git
```

6. Browse into the project directory and install dependencies

```
cd /mapseq_processing_jacobs/
pip install -r requirements.txt
```

7. Run the script on your sample.nbcm.tsv (command below shown using included sample dataset)

```
python process-nbcm-tsv.py -o /home/mwjacobs/git/mapseq_processing_jacobs/jr0375_out/ -s JR0375 -d /home/mwjacobs/git/mapseq_processing_jacobs/sample_data/JR0375.nbcm.tsv -u 2 -l "RSP,PM,AM,A,RL,AL,LM,neg,inj"
```

<br/>

## **REQUIRED Arguments**

**-o** = path to your output directory

**-d** = path to your sample.nbcm.tsv which was produced by the [CSHL mapseq-processing Python Pipeline](https://github.com/ZadorLaboratory/mapseq-processing)

**-s** = prefix for your saved files

**-l** = A list of your human readable column names in the tsv (Example:"area,area,area,neg,area,inj") 
- Your list must use 'neg' for any columns containing negative controls and 'inj' for any injection site column.
- Your list can use whatever names you want for the target areas but avoid spaces and characters.
- The code will try to sort target areas if you have repeat values (visp1,visp2,visp3,audp1,audp2...).
- I do not know if you can use more than one neg and and inj in a matrix. My data does not look like that and I havent tested.

<br/>
<br/>

## **OPTIONAL Arguments**

**-u** = Changes the threshold filter for target area UMI counts where very small values (noise) will be set to zero. (default: 2) You may want to set this to the maximum value seen in your negative control.

```
For example the default setting is 2 meaning that for every rown in your matrix the following logic will be applied

some_row_[0,1,0,35,12,1,0,120,1,0]

will be filtered with the default value of 2 to

some_row_[0,0,0,35,12,0,0,120,0,0].

Used for potential noise reduction of single UMI values in targets, but you can change this if you would like.
```

**-i** = Sets a threshold value for filtering barcodes by minimim injection site UMI.

**-f** = Enable outlier filtering of barcodes. Where any target value in a row is greater than the mean of all target values in the dataset plus two standard deviations, drop that barcode. We include this argument for microdissections which neighbor the injection site and there is no good way to know if very large UMI counts are from some kind of contamination.

**-a** = Value for alpha. This is the signifigance threshold (default 0.05) for Bonferroni correction, False Discovery Rate correction, and the Binomial Test.

<br/>

## **BUGS**
There are a few bugs presently. 

1. The plots may experience formatting issues

2. The variables order_full and order_partial are not dynamically defined and their use is not currently implemented correctly. You may see these strings in the cli output, but they can be ignored.

<br/>

## **Old Arguments not yet removed**

**-A** = Label from your labels to match for the first "important area" (e.g., 'AL') Must match something in your labels! (updated to dynamically calculate using all labeled areas)

**-B** = Label from your labels to match for the second "important area" (e.g., 'PM') Must match something in your labels! (updated to dynamically calculate using all labeled areas)

