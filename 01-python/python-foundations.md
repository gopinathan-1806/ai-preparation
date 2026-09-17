# Python Foundations

## How to use this note

Keep the original classroom wording for revision, then use the additions for interview-quality understanding. The original note is intentionally preserved even where terminology can be made more precise.

## Original Class Notes

### Python VE

Virtual Environment —> The primary reason to create a virtual environment in Python is dependency isolation. If am working on different projects called A and B. If A need a version 1.0 and B need a version 2.0 then using VE will fix this issue.

### Python Modules

Created a function, if that needs to be used in another py file, then we need to use python module. Say example, I have created a function called addition under module folder. If this needs to be called in another code, it will be starting with

`from module.addition import add`

So automatically, that function will be imported into our code. We need to create `_init_.py` file under modules, which will convert our function into package.

### Python File handling

w —> write / overwrite  
r —> read  
a —> append the existing file

`with open(“myfile.txt” , “w”) as file`

## Additional Study Details

### Virtual environments — practical view

A virtual environment gives each project an isolated Python package environment. Typical workflow: create a venv, activate it, install dependencies, and capture dependencies in a requirements file.

### Modules and packages

A module is normally a `.py` file that can be imported. A package is a directory containing Python modules; `__init__.py` can be used to define/initialize a traditional package. Modern Python also supports namespace packages, so `__init__.py` is not universally mandatory.

### File handling essentials

Use `with open(...)` so the file is closed automatically. Common modes include `r`, `w`, and `a`; also know `rb`/`wb` for binary files and `x` for exclusive creation. Consider encoding explicitly for text files and handle `FileNotFoundError` and permission errors.

### Practice checklist

Be able to create a venv, install a package, freeze dependencies, import a local module, read/write a text file, and explain why dependency isolation matters in multi-project environments.
