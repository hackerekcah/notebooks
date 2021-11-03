# env `torch`
```
conda create -n torch python=3 ipython matplotlib pandas tqdm pyyaml pytorch torchvision torchaudio cudatoolkit=10.2 scikit-learn librosa jupyterlab tensorboard -c pytorch -c conda-forge
```

# to supoort 3090
```
conda install pytorch==1.8.1 torchvision==0.9.1 torchaudio==0.8.1 cudatoolkit=11.1 -c pytorch -c conda-forge -c nvidia
```

## issue with llvmlite, make sure install llvmlite from numba channel
```
conda install --channel=numba llvmlite
```
