# SPLAB Code Demonstration
* Each app serves at a unique `LAN_IP:PORT`
* Each app has a jupyterlab dir under `/data/splab_demo/`
```
# LAN_IP: PORT map to a notebook-dir
192.168.11.214:9991/lab :: /data/splab_demo/acoustic_scene_classification
192.168.11.214:9992/lab :: /data/splab_demo/speech_enhancement
```

## start the jupyter notebook/lab server
```
# --no-browser: dont automatically open webbrowser
# &: run backend so that terminal can be closed
jupyter notebook --port=9991 --no-browser --notebook-dir=/data/splab_demo/speech_enhancement &
```

## gateway
