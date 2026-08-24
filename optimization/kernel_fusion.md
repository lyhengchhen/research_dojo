# Kernel Fusion 
It is an optimization technique used to make the computation faster and much more memory efficient by combining multiple sequential operations into a single execuation step to reduce memory overhead and speed up processing. 

**Problem:** so conventionally, we have to launch separate instructions for every single math operations, the compiler fusese them together so the hardware can process them all at once. aka memory bottleneck. AI hardware like the GPU and TPUs are really fast in computing the math but having to move from data to and from memory (VRAM) is relatively slow.

**For example:** an expression like `y = (x * 2) + 1` requires the hardware to:

1. Load `x` from main memory.
2. Multiply by 2.
3. Write the temporary result back to main memory.
4. Load that temporary result back into the processor.
5. Add 1.
6. Write the final result `y` back to main memory.

while Kernel fusion eliminates the middle steps. The compiler rewrites the execution code so the processor loads `x` exactly once, multiplies it by 2, immediately adds 1 while the data is still sitting inside its ultra-fast local registers, and writes out the final result.

Common practical example of fused operation:
 - Fused Multiply-Add (FMA): Combines (A * B) + C into one operation 
 - Activation Fusion: Merges a convolution or linear layer directly with its activation function (like ReLU or GeLU)

 and for the implement, we dont really have to manually write the fusion by hand. Modern AI compiler handle this automatically under the hood:
 - PyTorch: triggered natively via torch.compile()
 - XLA (Accelerated Linear Algebra): the default compiler for google TPUs and JAX that aggressively uses fusion to achieve high speed. 
 - Triton / CUDA: Used by advanced developers to write custom fused kernels for NVIDIA GPUs.

