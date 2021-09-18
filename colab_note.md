## Watch usage gpu

```
watch -n0.1 nvidia-smi
```

## Clear gpu memory on colab wihtout restart session
First: 
```
sudo fuser -v /dev/nvidia*
```

Second: replace the x with the PID the PID from first step:
```
sudo kill -9 x
```

Third: From the task Ram/Disk, chosing connect to the hosted runtime.
