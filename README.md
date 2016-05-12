# tvnorm-nn
Total Variation Norm as Torch 7 `nn` module. Expose several modules to calculate Total Variation Norm (TODO: ref?) as nn layer or loss function regularizer.
Only support CUDA tensor, `cudnn` required. 

## Requirement
* Torch
* nn
* cunn
* cudnn

## Install
git clone this repo, cd to the directory, then type command
```
luarocks make
```

## Usage
We'll use the following data size notations hereafter:
```
B: batch size
C: #channels/#feature maps
H: image height
W: image width
```

#### `nn.SpatialTVNorm([kerType])`
Expect tensor size:
```
input: B, C, H, W
output: B
```
Calculate TV norm for each `H, W` sized image with `C` channels. 
Use kernel type `kerType`, which can be `sobel` (default) or `simple`.
Each result has been averaged by size `C*H*W`.
`Forward()` and `Backward()` routines are implemented. No parameters.

Examples: see `temp/timing_tvnorm.lua`.

#### `nn.SpatialTVNormCriterion()`
TV Norm as criterion. Convenient when used as regularizer. See also `nn.MultiCriterion`.
Expect tensor size:
```
input: B, C, H, W
output: 1 (lua number)
```
Calculate TV norm for each `H, W` sized image with `C` channels by calling `nn.SpatialTVNorm`, 
then average the results by size `B` to get the loss in `lua` number.
Use kernel type `kerType`, which can be `sobel` (default) or `simple`.
`Forward()` and `Backward()` routines are implemented. No parameters.

Examples: see `temp/timing_tvnormCri.lua`.

#### `nn.SpatialSobelFilter()`
Apply `3x3` Sobel filter to calculate x- and y- directional gradients for each of the `H, W` sized image. Expect tensor size:
```
input: B, 1, H, W
output: B, 2, H, W
```

Examples: see `temp/timing_sobel.lua`.

#### `nn.SimpleGradFilter()`
Calculate x- and y- directional gradients (consecutive pixels subtraction) for each of the `H, W` sized image. Expect tensor size:
```
input: B, 1, H, W
output: B, 2, H, W
```

Examples: see `temp/timing_simplegrad.luat`
