# Wave Function Collapse

[![Version](https://img.shields.io/crates/v/wfc.svg)](https://crates.io/crates/wfc)
[![Documentation](https://docs.rs/wfc/badge.svg)](https://docs.rs/wfc)

Library for generating grids of values which are similar to a specified grid.
A typical use case for this is procedurally-generated images, though it generalizes
to any grid of values.

*Similar* is defined as both:
 - *strictly locally similar*: every small (typically 3x3) pattern in the output
   image appears somewhere in the input image.
 - *loosely globally similar*: the distribution of small patterns in the output
   image is roughly the same as the distribution of small patterns in the input
   image.

Grids are populated using a constraint solver. For each cell, we store a probability
distribution representing how likely that cell is to contain the top-left corner of
possible pattern. Initially the probability of each pattern is based on its frequency
in the sample image. Then, it repeatedly identifies the cell whose entropy is the lowest,
and decides (randomly, weighted by probability distribution) which pattern to assign to
the cell. This assignment may remove some candidate patterns from neighbouring cells,
so it then updates candidate cells. This process of choosing a cell, assigning it a
pattern, and propagating incompatible neighbours continues until either the entire grid
is populated with values, or all the candidates are removed from a cell.

## Example of Similar Images

![Flowers Input](/images/flowers.png)
->
![Flowers Output1](/images/flowers-output1.png)
![Flowers Output2](/images/flowers-output2.png)

For more image examples, see [wfc-image](https://github.com/stevebob/wfc/tree/master/wfc-image).

## Animation

This shows the process of generating an image based on the sample flowers image above.
The colour of each pixel is the average of all colours which could be assigned to it,
weighted by probability.

![Flowers Animation](/images/flowers-animate.gif)
