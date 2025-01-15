# mapseq_processing_jacobs
MAPseq processing code based on previous works and designed to be used with the CSHL python pipeline.

Any code found here is a work in progress with no guarantee that it will work until otherwise stated.

**Before you run:**
 Setup a conda environment and the listed dependencies. All should be available in bioconda, conda-forge, and the default.

Run --help to see arguments.

**-0** = path to your output directory

**-d** = path to your nbcm.tsv

**-s** = prefix for your saved files

**-l** = list of your columns in the tsv (Example:"area,area,area,neg,area") You must use 'neg' for any columns containing negative controls. You can use whatever names you want for samples but avoid spaces and characters. The code will try to sort samples if you have repeat values (visp1,visp2,visp3,audp1,audp2...)

**alpha** will default but you can set it if you like, see the script for comments.

**target_umi_min** = filter for low counts in the matrix eg some_row_[0,1,0,35,12,1,0,120,1,0] will be filtered with the default value of 2 to some_row_[0,0,35,12,0,0,120,0,0].

If you get the upsetplots out then you got all the way through.


**BUGS**
There are a few bugs presently. 

1.The plots are not all in a format that I love.

2. [334]&[723] order_partial is not dynamically defined

3. [215] Stats relying on two specific areas being found in the labels. Still Broken.
