# TOC
- [install](#install)
- [start](#start)

# install
There are several patterns.  
Famous ones are to use `pip` or use `Anaconda`.

## pip with VirtualEnv
1. create "virtual env" for `jupiterlab`
2. activate the virtual env
3. just `pip`
```
$ pip install jupyterlab
```

# start
## with token
```
$ jupyter lab
$ jupyter server list
// take token from the shown URL
```

## without token
```
$ jupyter lab --NotebookApp.token=''
```
