---
title:  "Setting up a Neovim and pipenv based Python development environment"
date:   2018-04-10 17:00
categories: [development]
tags: [development]
---
I think everybody has been there after some time:

 * multiple python venvs for dozends of projects
 * huge `requirements.txt` files containing all dependencies of dependencies
 * JuPyter notebooks everywhere, including their dependecies

For the start of my PhD I decided to try to bring some order in the chaos of environments and dependecies by switching to `pipenv`. Furthermore, I show how to implement __jupyter notebook__ style programming in a Neovim()/Oni() development environment. 

## pipenv

Pipenv is a tool that allows to manage project dependent virtual environments, while additonally enhancing reproducability by using checksums of installed packages (`Pipfile.lock`). It is the recommended package manager by http://www.python.org, is straightforward to install and also supports loading project specific environment variables using an `.env` file.

Virtual environments in pipenv are not stored in the repository of the project, also there are no additional files besides the Pipfile and the Pipfile.lock (these are actually good to have to ensure reproducability). The stategy is to avoid installing packages outside of pipenv (for example using pip), which automatically ensures that all project dependencies are tracked and up to date. Overall pretty neat in my oppinion.

### But how can I avoid the countless jupyter dependecies?

Basically the only package that is required for a jupyter interface (notebook, qtconsole, console) to communicate with an environment is `ipykernel`, which can be installed as an development dependecy in pipenv (`pipenv install --dev ipykernel`).
For kernels of environments to be useable from the interfaces, they need to be registered.
In order to make the whole process easier, I wrote a small bash function to create a python3 environment, install ipykernel as a development dependency and register the new kernel for usage.

```bash
init_python3_pipenv () {
   echo "Setting up pipenv environment"
   pipenv install --three
   echo "Installing ipython kernel"
   pipenv install --dev ipykernel
   # get name of environment and remove checksum for pretty name
   venv_name=$(basename -- $(pipenv --venv))
   venv_prettyname=$(echo $venv_name | cut -d '-' -f 1)
   echo "Adding ipython kernel to list of jupyter kernels"
   $(pipenv --py) -m ipykernel install --user --name $venv_name \
   --display-name "Python3 ($venv_prettyname)"
}
```

This allows allows the appropriate kernel to be launched from a jupyter notebook installed in the global environment.

## JuPyter notebook style programming in Oni/Neovim

For the vim users out there, I will explain how you can convert vim into an interactive developing environment similar to working in a jupyter notebook or using an ide like spyder.
This setup involves launching an IPython kernel in a QTConsole, establishing a remote connection to the kernel using the `nvim-ipy` plugin and configuring the QTConsole such that it outputs the results of remote commands. The result is quite accepatble:

![Screenshot of working environment](/assets/2018-04-10_Oni-and-QtConsole.png)

### Running QtConsole from vim using correct kernel
The benefit of the QTConsole is that the output of a command is diectly visible, allowing interactive programming with intermediate plots and variable inspection.

On macOS, the dependencies (mainly QT) of the QTConsole can be installed via brew allowing it to be launched using `jupyter qtconsole`.
To allow the QTConsole to easily be launched using the correct kernel and from within vim, I defined the following vim functions in my `init.vim`

```vimscript
function! GetKernelFromPipenv()
    return tolower(system('basename $(pipenv --venv)'))
endfunction

function! StartConsolePipenv(console)
    let a:flags = '--kernel ' . GetKernelFromPipenv()
    let a:command=a:console . ' ' . a:flags
    echo a:command
    call jobstart(a:command)
endfunction
```

And added corresponding commands

```vimscript
command! -nargs=0 RunQtPipenv call StartConsolePipenv('jupyter qtconsole')
command! -nargs=0 RunPipenvKernel terminal pipenv run python -m ipykernel
command! -nargs=0 RunQtConsole call jobstart("jupyter qtconsole --existing")
```


