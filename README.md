# MicroServices
We offer simple functions, in Python and R, which provide programmatic access to remote computation and remote data.
The microservices architecture is described by [Martin Fowler](http://martinfowler.com/articles/microservices.html).
He argues that software systems are most flexibly, robustly and adaptively constructed via <i>composition</i>,
in which each of the component parts performs a single, well-defined task.  

Current services include:

 * [getDNAService](https://github.com/PriceLab/getDNAService): currently a lightweight wrapper around an
    established, public UCSC DAS server.  The reference sequence for the specified genome, and chromosomal location is returned,
    reverse-complemented if requested.

 * [fimoService](https://github.com/PriceLab/fimoService): Invoke the [MEME Suite's](http://meme-suite.org/)
   [FIMO](http://meme-suite.org/tools/fimo) application, which scores the match of transcription factor binding
   motifs to sequences you submit.  The FIMO client runs on your computer, the FIMO service (application)
   runs on <i>whovian</i>, an ISB linux server visible only within the firewall.  The target motifs,
   expressed as PWMs, is obtained from JASPAR with some additions.   Client/server communication is
   accomplished with the [ZMQ](http://zeromq.org/) protocol, the same one used by [Jupyter](http://jupyter.org) notebooks.
   

These first two services are complementary:  the getDNAService provides the DNA sequence - typically short --
which the fimoService requires.  Once the software is installed (see each respective package for language-specific instructions),
these operations become possible, as demonstrated here with the loss of binding motifs due to a
SNP [rs146894928](http://www.ncbi.nlm.nih.gov/projects/SNP/snp_ref.cgi?rs=146894928), `chr1:172883225 C/T` on forward strand.
These coordinates are hg38.  (A programmatic "liftover" service would come in handy.)

```
library(getDNAClient)
dnaClient<- getDNAClient("hg38")
start <- 172883225 - 5
end <- 172883225 + 5
seq.wt <- getSequenceByLoc(dnaClient, "chr1", start, end)
seq.snp <- sprintf("%s%s%s", substr(seq.wt, 1, 5), "T", substr(seq.wt, 7, 11))
```

We now have the wild-type and variant sequence.  Submit each to fimo.  Does this SNP affect predicted TF binding?

```
fimo <- FimoClient("whovian", 5558)
print(requestMatch(fimo, list(wt=seq.wt)))     # an empty table
print(requestMatch(fimo, list(mut=seq.snp)))   # four motifs reported
```
