---
tags:
  - system_design
---

# Remote Procedure Calls
A Remote Procedure Call (RPC) is when a program causes a procedure or subroutine to execute in a separate address space

### Main Idea
- RPCs are an abstraction intended to make it seem like you are making a local function call, but are actually calling and processing the data in a separate address space
- It's just like calling a function, but the function itself is executed on a different instance

### When to Use
- When you want to call a function that may be too performance intense to run on the current machine 

### How it Works
![[RPC.png]]
- The stubs are the function calls made in each address space
- From client perspective they are just calling the stub method
- Actual remote calls and stuff happen in the backend 
## Example
- Software A needs to process a large dataset, DATASET
- Processing DATASET takes a long time and requires specific functions on a different serve
