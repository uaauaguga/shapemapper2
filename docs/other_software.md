<!---
NOTE:
If you're reading this, instead try opening README.html in a web browser 
or view this file from within the github repository website.

This is a github-flavored markdown file not meant to be easily readable.
-->

Other software
==============

RNA secondary structure modeling
--------------------------------
ShapeMapper does not perform structure modeling. To model RNA
structures with SHAPE reactivity constraints, we recommend 
using the `Fold` module of [RNAstructure](https://rna.urmc.rochester.edu/RNAstructure.html) 
for short sequences. `Fold` accepts a `.fa` sequence file and a `.shape` reactivity 
file as input, and produces a `.ct` file containing one or more modeled structures.
For sequences longer than a few hundred nucleotides, 
we recommend using 
[Superfold](https://weeks.chem.unc.edu/software.html),
which automates the process of running RNAstructure over subsequence
windows and merging the resulting structures. Superfold accepts a `.map`
file as input, and produces a `.ct` file containing 
a single predicted minimum free energy structure and a `.dp` file 
containing estimated base pairing probabilities.

RNA structure visualization
---------------------------

### Arc diagrams

#### IGV
We recommend [IGV](https://software.broadinstitute.org/software/igv/) 
for exploratory visualization of reactivity profiles
and structure models for RNAs longer than a few hundred nucleotides. 
Once a `.fa` file is loaded as the reference sequence 
(<kbd>Genomes</kbd> → <kbd>Load Genome from File</kbd>), 
IGV can directly open `.shape` or `.map` reactivity profiles and 
`.ct` and `.dp` files generated by Superfold. To open these
files, click <kbd>File</kbd> → <kbd>Load from File</kbd>, and
click <kbd>Continue</kbd> if a dialog box pops up.

### Traditional secondary structure diagrams

#### VARNA
[VARNA](http://varna.lri.fr/index.php) is a user-friendly option
for RNAs up to a few hundred nucleotides.

_Pros:_

- Can directly open `.ct` files for visualization and adjustment.
- Default layouts for short RNAs often require no manual adjustment
- Intuitive layout adjustment

_Cons:_

- Large structures often result in unmanageable layout overlaps.


#### Ribosketch
[Ribosketch](https://rnastructure.cancer.gov/ribosketch/) is a relatively new
software package for RNA secondary structure visualization. 

_Pros:_

- Supports `.ct` files
- Initial layouts for large RNAs often very good
- Attractive publication-quality output images

_Cons:_

- Manually editing larger structures
can be somewhat cumbersome, especially if there are 
layout overlaps.


#### RNAstructure Structure Editor
Recent releases of [RNAstructure](https://rna.urmc.rochester.edu/RNAstructure.html) 
include a fairly sophisticated graphical structure viewer/editor (the executable is
named `StructureEditor`).

_Pros:_

- Supports `.ct` files
- Direct support for `.shape` reactivity coloring
- Customizable nucleotide coloring for different
  publication styles

_Cons:_

- Initial layouts are not very good
- May require compiling from source


#### XRNA
For large RNAs, [XRNA](http://rna.ucsc.edu/rnacenter/xrna/)
is often the only practical option to generate and edit
publication-quality figures.

_Pros:_

- Most comprehensive interface for manual layout adjustment

_Cons:_

- Has not been updated since 2009
- Non-intuitive interface
- Does not support the `.ct` file format
- Does not handle pseudoknotted structures well
- May crash for very large RNAs
- Vector output may be broken

_*How to load a `.ct` file:*_

As a workaround for XRNA's lack of `.ct` file support, 
[RNAstructure](https://rna.urmc.rochester.edu/RNAstructure.html)
can be used to open
a `.ct` file and convert it to a helix text file that can be imported
into XRNA. Run the graphical interface `RNAstructureScript`.
Click <kbd>File</kbd> → <kbd>Draw</kbd>, and select a `.ct` file.
Click <kbd>Draw</kbd> → <kbd>Write Helix (Text) File</kbd>.

In XRNA, click the <kbd>Format</kbd> tab. 
Click <kbd>Format New Structure</kbd>. Click <kbd>New Primary Structure</kbd>,
and copy-paste in the RNA sequence, or click 
<kbd>Read</kbd> and select a text file containing only the RNA sequence. Close the
dialog. Click <kbd>Set New Helices</kbd> → <kbd>Read</kbd> and select the 
helix text file created above. Close the dialog. Click <kbd>Run Format</kbd> to generate
an initial layout, then move to the <kbd>Edit</kbd> tab for manual layout
adjustment.

Alternatively, the `pvclient.py` wrapper script included
with [Superfold](https://weeks.chem.unc.edu/software.html)
can be used to query the 
[Pseudoviewer](http://pseudoviewer.inha.ac.kr/) web service and
generate a `.xrna` file that can be directly loaded by XRNA.


### Coloring by SHAPE reactivity
Coloring secondary structure diagrams by SHAPE reactivity in
an aesthetically pleasing, publication-quality format still
often requires _ad-hoc_ scripting. Some of these scripts may be included
in future ShapeMapper releases. In the meantime, here we list
steps to import ShapeMapper-generated reactivity profiles into several 
software packages and render with reasonably aesthetic color scales.

#### VARNA coloring
Right click → <kbd>Color map</kbd> → <kbd>Show color map</kbd>

Right click → <kbd>Color map</kbd> → <kbd>Style</kbd> → <kbd>red</kbd>

Right click → <kbd>Color map</kbd> → <kbd>Load values</kbd> → <kbd>Choose file</kbd>,
and select a simplified reactivity profile 
(output by ShapeMapper) with a filename such as `*_varna_colors.txt`. 

These reactivity profiles have the same values as `.map` profiles, but 
have been simplified to replace values above 0.85 with 0.85, 
values below 0 with 0, and missing data with 0.

Reactivities will be shown as a smooth color gradient from
white at 0 to red at 0.85. Note that VARNA does
not have a concept of missing data or excluded positions, so positions
with low read depth or high background reactivity will show up as white,
the same as lowly reactive nucleotides.


#### Ribosketch coloring
Click <kbd>Load colors</kbd> and select a file output by ShapeMapper
such as `*_ribosketch_colors.txt`. 

These reactivity profiles have the same values as `.map` profiles, but 
have been simplified to replace values above 0.85 with 0.85, 
values below 0 with 0, and missing data with 0.

Reactivities will be shown as a smooth gradient
from white at 0 to red at 0.85, with nucleotides missing data shown 
as white. 

#### RNAstructure Structure Editor coloring
Click <kbd>Format</kbd> → <kbd>Color-Annotate Bases</kbd> → <kbd>Chemical Modification </kbd> → <kbd>Load File</kbd> and
select a `.shape` file output by ShapeMapper.

Click any combination of <kbd>Text</kbd>, <kbd>Fill</kbd>, and <kbd>Outlines</kbd>
to apply reactivity colors to nucleotides. The reactivity legend is displayed in
the `Color Annotations` dialog in `Value Ranges and Colors`. By default, 
missing data is shown in black (the same as lowly reactive positions). To change
this,
change the numeric limit for `Black` from "`-INF`" to "`-990`", click 
<kbd>Add rule</kbd>, and set the numeric limit for `Gray` to "`-INF`".


#### XRNA coloring
A `.shape` or `.map` file can be passed to the `pvclient.py`
script (included with 
[Superfold](https://weeks.chem.unc.edu/software.html)) 
using the <kbd>--shape</kbd> parameter along 
with a `.ct` file to generate a `.xrna` file with SHAPE reactivity coloring.
The `pvclient.py` script requires an active internet connection and relies
on the [Pseudoviewer](http://pseudoviewer.inha.ac.kr/) web service.

Reactivities >= 0.85 are colored
red, >= 0.4 orange, < 0.4 black, and missing data gray. 
The generated `.xrna` file can be loaded into XRNA for manual layout adjustment
and rendering.

&nbsp;&nbsp;&nbsp;&nbsp;

[← back to README](../README.md)
