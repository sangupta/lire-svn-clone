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
I�d like to thank the numerous people having contributed
code to Lire or having published their work as open source
allowing integration into LIRe: Savvas A. Chatzichristofis,
Manuel Oraze, Lukas Esterle, Roman Kern, Roman Divotkey,
Katharina Tomanec, Fabrizio Falchi, Bastian H�sch, Janine
Lachner, Marko Keuschnig, Christian Penz and Benjamin
Sznajder.

- Mathias Lux, Klagenfurt, 2008-06-08
  http://www.SemanticMetadata.net
  

Changes from the original code at GoogleCode Repository
=======================================================

The following changes have been made from the original repo
on Google Code (http://code.google.com/p/lire). Hopefully,
these will make into the original repo some day.

With github commit: 799b2a09f6846f711bde3bd0108e0e5b65f28cca

 * Mavenization of the project
 * Removed all warnings of resource leaks
 * Implemented type-safety by using generics than raw types
 * Upgraded Lucene code to not use deprecated methods

- Sandeep Gupta (twitter.com/sangupta)
