# compare_vals
Compare pixel values of raster objects to the values of adjacent pixels. This may be helpful when using the result of a classification (where the classes were written in a raster object as discrete numeric values) for further analysis, e.g. to exclude pixels at the borders of patches that belong to different classes under the assumption that such pixels cover most likely not only areas of the assigned class.
The function provided here was written in R version 3.6.2 and uses the R raster package v3.0-12.
Note that this function may require a large amout of memory, depending on the size of the raster object.
Input arguments are:
x = Raster Object
mode = Number of adjacent pixels to compare with. Possible values are 4 or "rook", 8 or "queen" and 16 or "bishop".
assign = Value to assign to pixels with neighbours that differ in their value. Default: NA
