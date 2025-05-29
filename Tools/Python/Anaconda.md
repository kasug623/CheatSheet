# TOC
- [install](#install)

# install
After install, without `conda init`,  
an error happens when you run `conda activate`.
```
$ cd ~
$ mkdir tmp
$ cd tmp
$
$ wget https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh
$ 
$ bash Anaconda3-2021.11-Linux-x86_64.sh
$ 
$ rm -rf tmp
$ 
$ conda -V
$
$ conda init zsh
```
- ref.  
[https://kondeneenen.com/wsl2_anaconda_jupyterlab/](https://kondeneenen.com/wsl2_anaconda_jupyterlab/)  

## the added content by `conda init`
This is added on `.zshrc`.  
After `conda init`, "virtual environment `base`" is automatically activated when you start `zsh`.  
It's not on my taste so I will add `deactivate` command to `.zshrc`.
```
# .zshrc

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/user/anaconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/user/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/home/user/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/user/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```
After above scripts added by `conda init`,  
I would like to add this to `.zshrc`.
```
# .zshrc

# for avoid auto activate "base"
conda deactivate
```