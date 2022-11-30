---
layout: post
title: Python environments and direnv
categories: [devops, python, hy]
---

I am using a bash script I called mkvenv to manage python virtual environments (venvs).
It uses [direnv](https://direnv.net) to activate the venv automatically.

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

