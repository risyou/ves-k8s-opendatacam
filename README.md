# Volterra demo - OpenDataCam

## About OpenDataCam

[OpenDataCam](https://raw.githubusercontent.com/risyou/ves-k8s-opendatacam/master/README.md) is a project based on Yolo (Darknet) to detect moving object real time. For more information, please check origin git and below video

| 👉 [UI Walkthrough (2 min, OpenDataCam 3.0)](https://vimeo.com/432747455) | 👉 [UI Walkthrough (4 min, OpenDataCam 2.0)](https://vimeo.com/346340651) | 👉 [IoT Happy Hour #13:  OpenDataCam 3.0](https://youtu.be/YfRvUeSLi0M?t=1000 ) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [![OpenDataCam 3.0](https://i.vimeocdn.com/video/914771794_640.jpg)](https://vimeo.com/432747455) | [![Demo OpenDataCam](https://i.vimeocdn.com/video/805477718_640.jpg)](https://vimeo.com/346340651) | [![IoT](https://img.youtube.com/vi/YfRvUeSLi0M/hqdefault.jpg)](https://youtu.be/YfRvUeSLi0M?t=1000) |


## Volterra
This project enables you to run opendatacam on a GPU enabled Volterra CE. We assume you have basic knowledge of how to use Volterra

## Prepare

### 1.0 Build GPU enabled Volterra node
Create a fleet with the following config

<img src="/images/fleet-GPU.png" alt="drawing" width="600"/>

Build your volterra site and attach above fleet

<img src="/images/GPU-label.png" alt="drawing" width="600"/>

Download kubeconfig and use it for the following command

### 2.0 Install

```bash
git clone https://github.com/risyou/ves-k8s-opendatacam.git

cd ves-k8s-opendatacam
```

### 3.0 Run

```bash
#Create configmap from config.json, it will be used in later deployment
kubectl create configmap opendatacam --from-file=config.json --dry-run -o yaml | kubectl apply -f -

#Create mongodb to store data
kubectl apply -f 0001-mongodb-deployment.yaml

#Create opendataacm deployment
kubectl apply -f 0002-opendatacam-deployment.yaml
```
You will have the following service and deployments

```
[shali@:~]$kubectl get service
NAME                  TYPE        CLUSTER-IP        EXTERNAL-IP   PORT(S)                    AGE
mongo                 ClusterIP   192.168.205.208   <none>        27017/TCP                  2d17h
opendatacam           ClusterIP   192.168.22.123    <none>        80/TCP,8071/TCP,8090/TCP   2d17h
[shali@:~]$kubectl get deployment
NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
opendatacam            1/1     1            1           6h1m
opendatacam-mongo      1/1     1            1           2d17h
```


### 4.0 Build access from Volterra

Create origin pool and loadbalancer, see ves-example.json for example

Access your loadbalancer, by default it will shows you an demo provided by opendatacam, just drag and drop your video there!

### 5.0 Customize

config.json contains the parameter you can customeize, for example
- VIDEO_INPUT: Chose from VIDEO_INPUTS_PARAMS{key} and input parameter in the {value}
- DISPLAY_CLASSES: Chose from yolo object store

After update the config.json you need to rebuild configmap and rebuild pod

```bash
kubectl create configmap opendatacam --from-file=config.json --dry-run -o yaml | kubectl apply -f -
```
