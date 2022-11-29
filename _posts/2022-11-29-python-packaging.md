---
layout: post
title: How to deploy a python / hy package
tags: devops
---

I am using a bash script I called mkvenv to manage environments.

{% highlight bash %}
#/usr/bin/env bash
#
# Make a python venv and incorporate it with direnv
# Usage: mkvenv python3.X
# Works well with pyenv.

# --- python ---
# when pythonX.Y passed as an argument
# find python exec and create venv
python=$(which ${1})
if [[ -z ${python} ]]; then
    echo "Invalid python ${python} specified."
    exit 1
fi
alias python="${python}"
python_root=$(echo $(dirname "${python}") | sed -e 's:bin$::')
export PATH="$PATH:${python_root}/bin"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:${python_root}/lib"
export LD_RUN_PATH="$LD_RUN_PATH:${python_root}/lib"

# --- venv directory ---
pyver=$(${python} --version)
venv=".venv/${MACHTYPE}/${pyver}"
mkdir -p "${venv}"
# impure leakage may cause problems
#$python -m venv --system-site-packages ${venv}
"${python}" -m venv "${venv}"

echo "created venv at ${venv}"

# --- direnv magic ---
cat > .envrc << EOF
venv=".venv/\$MACHTYPE/${pyver}"

export VIRTUAL_ENV="\${venv}"
export PATH="\${venv}/bin:\$PATH:${python_root}/bin"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:${python_root}/lib"
export LD_RUN_PATH="$LD_RUN_PATH:${python_root}/lib"
if [[ -f "\${venv}/bin/python" ]]; then
    export PROMPT_PREFIX="\$(python --version)"
else
    export PROMPT_PREFIX="\$(which python)"
fi
EOF

direnv allow .
{% endhighlight %}

You can pip install from the local git repo by putting the following in requirements.txt:
```
git+ssh://git@company.ltd/company/repo.git
```
Notice the different format to what git itself uses to point to a remote repository.
It's also possible to install direct from the local source tree, e.g.:
```
python -m pip install /src/program
```
This last way may be the easiest, doing versioning on the git server and installing from /src.

[python-for-hpc](https://betterscientificsoftware.github.io/python-for-hpc/tutorials/python-pypi-packaging/) says:
> A Python project will consist of a root directory with the name of the project. Somewhere inside this will be included a directory which will constitute the main installable package. Most often this has the same name as the project (This is not compulsory but makes things a bit simpler. Inside that package directory, alongside your python files, create a file called `__init__.py`. This file can be empty, and it denotes the directory as a python package. When you pip install, this directory will be installed and become importable.

A simple project may have this structure:

```
graphics
├── LICENSE
├── graphics
│   ├── __init__.py
│   └── screen.hy
├── README.md
├── pyproject.toml
└── setup.cfg
```

{% highlight conf %}
#setup.cfg
[metadata]
name = graphics
author = Name
author_email = name@some.domain
version = 0.1.2
description = A simple ncurses UI class
url = https://some.domain

[options]
python_requires = >= 3.8
include_package_data = True
package_dir = 
packages = find:

# if a command-line utility is required
[options.entry_points]
console_scripts =
    cmd-line-util = graphics.some-module:some-module

[options.packages.find]
where = 

[options.package_data]
* = *.hy
{% endhighlight %}

{% highlight toml %}
# pyproject.toml
[build-system]
requires = [
  "setuptools >= 40.9.0",
  "wheel",
]
build-backend = "setuptools.build_meta"
{% endhighlight %}

This means pip will throw away the top-level stuff (README etc.).

The same source goes on to recommend setuptools to make the package installable, provide package metadata, dependencies etc.

The python docs tutorials recommend setup.cfg as a simpler static alternative.
However setup.py could allow using the git tag as a version.


### References

[installing packages](https://packaging.python.org/tutorials/installing-packages/)

[packaging packages](https://packaging.python.org/tutorials/packaging-projects/)
