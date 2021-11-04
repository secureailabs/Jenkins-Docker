# 1. Preparing the Notebook

## 1.0 Preparing the Dockerfile

The Dockerfile makes a change to the current Orchestrator code where a Back-end API portal ip is hard coded. The dockerfile changes the Back-end API portal ip to be: ```40.76.22.246```. Should you wish to point the Orchestrator at another location please edit in Dockerfile ```RUN sed -i 's/20.185.6.111/40.76.22.246/g' Sources/frontend.cpp``` replacing ```40.76.22.246``` with the ip you wish to use.

## 1.1 Building the Image

First you need to build the docker image from the dockerfile in this directory. You'll need an access token for the secureailabs github. Build your docker image by running:

```bash
docker build --build-arg git_personal_token=<your git access token> -f Dockerfile -t orchestrator .
```

## 1.2 Containerising the Image

Next you need to containerise your image. We mount our a local volume to the container with the -v command. You need to write the absolute path to your current directory. If you don't know your absolute path, try the pwd command. This'll give you access to the sail packages and demo files held here from within the notebook container. The rest of the tutorial will be shown as if you mounted to this directory on your host. We're also going to map ports used by the notebook on our container to port 8080.

```bash
docker run --name ubuntu_orchestrator -dit -p 8080:8080 orchestrator
```

## 1.3 Running the Notebook

Run the jupyter notebook command below once you've started your container. You should be able to access your notebook on your host device on your browser at this address: http://X.X.X.X:8080. You'll need the token from your terminal output to access the page.

```bash
docker exec ubuntu_orchestrator jupyter notebook --notebook-dir=. --ip=0.0.0.0 --port=8080 --allow-root
```


![notebook](images/2.png)
![terminal output](images/1.png)
