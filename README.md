Introduction to RDML
--------------------

The RDML file format is developed by the RDML-Consortium and can be used free of 
charge. The RDML file format was created to encourage the exchange, publication, 
revision and reanalysis of raw qPCR data. The core of an RDML file is an experiment, 
not a PCR run. Therefore all the information is collected which is required to 
understand an experiment. The structure of the file format was inspired by a 
database structure. In the file are several master elements, which are then referred 
to in other parts of the file. This structure allows to reduce the amount of 
redundant information and encourages the user to provide useful information.

The master elements are:
- experimenter (contact information of the experimenter)
- documentation (pieces of information to be used at many places)
- dye (dye type, used by targets)
- sample (description of one sample)
- targets (description of targets)
- thermal cycling conditions (description of the cycling conditions)
- experiment (a collection of several qPCR runs)

Each master element has an unique id attribute, by which it can be referenced by 
other elements of the file. This allows to describe the samples and targets only 
once in the file and then reference the reactions in a qPCR plate to these 
information using the respective ids. We suppose that a laboratory will research 
only some targets and therefore will be able to provide this information once. The 
software should allow to extract all the master elements (except the experiment 
element) from one other file when creating a new file and thereby does not require 
the user to fill in all this information by hand. 

In an RDML file the concept of a sample is an absolute identical solution at the 
moment of adding it to the qPCR reaction. Therefore almost all samples require 
different ids. The only exception would be technical replicates, which would be 
considered one sample because they are all made using the solution from one stock. 
Also different dilutions would require different ids, because the solutions are at 
the moment of adding to the qPCR reaction of different concentration. We suggest 
adding the concentration to the id and making it thereby unique.

The concept of a target is the solution added to the sample to form the qPCR 
reaction mix. Targets need to refer to dyes and same targets using different dyes 
would be considered different and require different target ids.

Thermal cycling conditions allow to describe the steps a themocycler goes through 
to amplify the DNA. It is a very flexible way to model all programs. It can also 
be used to describe a regular PCR or a cDNA program.

The experiment contains the data collected during an qPCR run. It can contain a 
number of runs and each run will have several qPCR reactions. Each reaction is made 
using one sample (which is referred to by its id) and several data elements, one for 
each target (also referred by its id). Also a reaction can have several data 
elements, this would be only more than one on multiplex machines recording in one 
run several colors / targets at once. If the reactions are on different plates the 
data should be stored as separate runs. The data elements should contain the recorded 
fluorescence values. The fluorescence data should include all corrections until the 
baseline correction, which should not be included (to allow reevaluation of the 
data). That requires that for example correction for empty wells and corrections for 
the loading (as for example ROX) are calculated into this values according to the 
qPCR machine producers algorithm (which is trusted to be good, because errors it 
creates can not be fixed by reevaluating the RDML file). Here the RDML file focuses 
to contain data which are comparable instead of matching machine specific correction 
approaches and storing all necessary data.  

The documentation elements are the multi-purpose documentation system in an RDML 
file. Its basically a text which has an unique id. What makes it so versatile is 
that from many places in the RDML file a reference can be made to this ids, 
allowing to document almost everything. If for example some samples were treated 
differently, the difference can be described in an documentation element and in the 
samples the id of this element is referred. If other samples thawed by accident, 
a documentation element can be linked to this samples. Each sample can have an 
unlimited amount of these elements and because they are referenced by id, even a 
long description which is used for all samples does not make the file over 
proportionally big. The use of documentation elements is not only limited to 
samples, it can be used at many places in an RDML file. Some elements have also 
description strings. These strings are unique to the element and should be used if 
the description is only required to a few elements.

Please be aware that the strings in an RDML file should be in the unicode format 
utf-8.

RDML File Compression and File extensions
The xml file containing the RDML compliant data should be stored in a file named 
rdml_data.xml. This file should be compressed using pkzip compatible compression 
into an archive. The archive can have any name, but instead of having the extension 
.zip it should have the extension .rdml (preferably) or .rdm.

Archives ending in .rdml or .rdm must contain the file rdml_data.xml.

Software should be able to read compressed .rdml or .rdm files as well
as uncompressed .xml files.

If third party information needs to be stored in the RDML file, it may be added in a 
machine specific format to the archive. We request that the used file name starts 
with the company name.
 
