# MicroServices
A collection of simple functions, written in Python and R, providing access to remote computation and remote data.
The microservices architecture is persuasively described by [Martin Fowler](http://martinfowler.com/articles/microservices.html),
claiming that software systems are most flexibly, robustly and adaptively created through composition,
in which each of the component parts each perform a single, well-defined task.

Current contents include:

 * [getDNAService](https://github.com/PriceLab/getDNAService): currently a lightweight wrapper around a
    UCSC DAS server.  The reference sequence for the specified genome, and chromosomal location is returned,
    reverse-complemented if requested.

 * [fimoService](https://github.com/PriceLab/fimoService): Invoke the [MEME Suite's](http://meme-suite.org/)
   [FIMO](http://meme-suite.org/tools/fimo) application, which scores the match of transcription factor binding
   motifs to sequences you submit.  The FIMO client runs on your computer, the FIMO service (application)
   runs on <i>whovian</i>, an ISB linux server visible only within the firewall.  The target motifs,
   expressed as PWMs, is obtained from JASPAR with some additions.
   
 
 
