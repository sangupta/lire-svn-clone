              LIRE - Lucene Image REtrieval
      (A Homage to Lots of Great Holidays in Italy)
==========================================================

The LIRE library allows the creation of a Lucene Index for
content based image retrieval (CBIR). Furthermore  methods
for searching the index are provided.

This library is part of the Caliph & Emir project and aims
to provide the CBIR features to other Java projects  in an
easy and light weight way.

For more information on the usage of LIRE see the online
docs:
http://www.semanticmetadata.net/wiki/doku.php?id=lire:lire

Acknowledgements
I'd like to thank the numerous people having contributed
code to Lire or having published their work as open source
allowing integration into LIRe: Savvas A. Chatzichristofis,
Manuel Oraze, Lukas Esterle, Roman Kern, Roman Divotkey,
Katharina Tomanec, Fabrizio Falchi, Bastian Hösch, Janine
Lachner, Marko Keuschnig, Christian Penz and Benjamin
Sznajder.

- Mathias Lux, Klagenfurt, 2008-06-08
  http://www.SemanticMetadata.net
  
  
Difference in this FORK
-----------------------

This fork is a little different from the original codebase
found at Google Code. The changes include:

* Complete Mavenization of the project
* Type parameterization to make calls strongly-typed
* Minor performance optimizations by caching computations that have already been done
* Latest Maven-based dependencies
* Remove unused imports
* Minor code cleanup

This fork is maintained by Sandeep Gupta (http://sangupta.com) in
the GitHub repo at https://github.com/sangupta/lire.

For questions, feel free to reach me at my email address: sandy.pec@gmail.com
