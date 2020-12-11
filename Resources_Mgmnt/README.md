## Resources Management in a Pod

- Create a Pod using the simple polinux/stress image
  This Pod has resources which have limits and requests
  And it is running a command that stresses the system
  regarding the memory, not the cpu in this example.

- Enter the worker node that it runs and issue 'top' 
  command. In a worker node with 2GB RAM it goes about
  to 50% usage of memory ..
