# SpikeTools

## spike-package.exe
This is designed to be super easy to use, no installation needed. And I restricted where the compiled code (.mpy) can go (only to /projects directory) to avoid accidents by kids or students.

To use, just download the `spike-package.exe` file and run it under command prompt:
> spike-package.exe -h

**IMPORTANT!** Before generating the package project (`Xense.llsp`) please remember to close the project from Spike App if you have previously generated and opened the project!

### What's behind the scene?

What's behind the scene (when you do `spike-package.exe myprogram.llsp`):
- create a temporary directory in your system
- extract the python code from `myprogram.llsp` to the temporary directory as `myprogram.py`
  - it will check for some basic requirements, like: it has to be a python-based project, the name of the program should not contain spaces, etc
- compile `myprogram.py` into `myprogram.mpy`
- create `Xense.py`:
  - add a simple python loader
  - add `xsloader.mpy` (this contain all the nice handling of loading files into the hub)
  - add `myprogram.mpy`
- create `Xense.llsp`
- clean up temporary directory

What's behind the scene (when you load `Xense.llsp` to Spike App and run it):
- the simple python loader will look for `xsloader.mpy` by reading itself and read its content, then it will write `xsloader.mpy` to `/xsloader.mpy` in your HUB filesystem
- it will use `__import()__` to dynamically load the `xsloader` module and use its function to read the rest of the content. It has better handling of reading `.py`, `.mpy`, or even any type of files, with some bells-and-whistles on UI (light and sound)

### FAQ
- Q: What is happening on the Spike when you want to update the library? Does the new one replace the old one? 
  - A: Every time you run `Xense.llsp` from Spike App it will update the library in the HUB filesystem. Yes, new one replaces the old one.
- Q: How can you know if the library is already loaded on the Spike?
  - A: if you run the following program and it is not complaining then your library (let's call it `your_library`) is loaded:
    ```python
    from projects import your_library
    ```
- Q: A help file should give a real world small example with a description step by step.
  - A: TBD
