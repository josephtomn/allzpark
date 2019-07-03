## Assumptions

In order to leverage Allspark, these are the assumptions it makes about your setup.

1. Project Packages MUST be of type `FileSystemRepository`
1. Allspark MUST distinguish between a Project package and other packages
1. Allspark MAY distinguish between an Application package and other packages

Project packages should reside in a single directory; the contents of this folder is what your artists are able to see from Allspark.

<br>

## Project

Allspark associates software and applications with a project via a Rez package.

```powershell
mkdir myproject
cd myproject
"name = `"myproject`"" | Add-Content package.py
"version = `"1.0`"" | Add-Content package.py
"build_command = False" | Add-Content package.py
```

With a project created, point allspark to it.

```powershell
cd ..
rez env allspark --root $(pwd)
```

With this in mind, it is recommended you separate your Project Packages from other packages.

```
PS> tree $(rez config release_packages_path)
├───proj
│   └───myproject
└───other
```

<br>

## Application

Allspark visualises applications relative a given project. A project can specify relevant applications using `~weak` references in its `requires = []`.

```python
name = "myproject"
version = "1.0"
build_command = False
requires = ["~maya-2018", "~nuke-11", "~zbrush-2019"]
```

This will result in `maya`, `nuke` and `zbrush` being visualised in Allspark, at these particular versions. Because they are `~weak`, Rez can resolve an environment with or without either one, which enables you to specify requirements that conflict across different applications; such as `ilmbase-1.1` for Maya and `ilmbase-3.6` for Nuke.