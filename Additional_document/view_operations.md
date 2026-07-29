# View Operation using PyTorch 
### .permute() 
Use it to reorder the dimension
.permute(keep, swap, swap)

### .view()
Use it to manipulate the tensor shape
.view(row, column)

**Example:** 
```
x = torch.randn(32, 3, 28, 28)   # batch of 32 images, 3 channels, 28x28
x = x.view(32, -1)               # flatten each image -> (32, 3*28*28) = (32, 2352)
```


### .contiguous
so basically the .view() and .permute() does respectively resharpe the tensor and manipulate the axes, but it does not appear to be reordered to (row by row) in the memory (i.e., tensor (2, 5, 4) -> so in the memory (2, 5, 4) not row by row in a straight line). so with the `.contiguous()`,the tensor's memory is rearranged so that reading straight through memory, in order, top to bottom, gives you the elements in exactly the same order as reading the tensor row by row according to its current shape.