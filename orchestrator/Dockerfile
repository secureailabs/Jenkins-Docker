FROM ubuntu

ARG git_personal_token

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

EXPOSE 8080

RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get install -y libssl-dev
RUN apt-get install -y libcurl4-openssl-dev
RUN apt-get install -y uuid-dev
RUN apt-get install -y git

RUN apt-get install -y libpython3.8
RUN apt-get install  -y python3
RUN apt-get install  -y python3-pip

RUN pip3 install numpy
RUN pip3 install requests numpy==1.20 pandas
RUN pip3 install sklearn
RUN pip3 install shap==0.38.1
RUN pip3 install torch --no-cache-dir
RUN pip3 install xgboost

#
RUN apt-get install -y jupyter

WORKDIR /

#Bring down the SAILPyAPI lib
RUN git config --global url."https://$git_personal_token:@github.com/".insteadOf " https://github.com/" \
    && git clone https://$git_personal_token@github.com/secureailabs/Development

# Includes find/repl command to hot fix hardcodred IP for now
WORKDIR /Development/Milestone3/VirtualMachine/Orchestrator/
RUN sed -i 's/20.185.6.111/52.150.26.47/g' Sources/frontend.cpp
RUN make all -j

RUN pip3 install /Development/Milestone3/VirtualMachine/Orchestrator/sail

# Bring some generic orchestrator notebook
RUN mkdir /ScratchPad
WORKDIR /ScratchPad
RUN git init \
    && git config core.sparseCheckout true \
    && git config --global url."https://$git_personal_token:@github.com/".insteadOf " https://github.com/" \
    && git remote add origin -f https://$git_personal_token@github.com/secureailabs/ScratchPad \
    && echo "AdamHall/Orchestrator/sail/generic_notebook.ipynb" > .git/info/sparse-checkout \
    && git checkout master

RUN mkdir /demo
RUN cp /ScratchPad/AdamHall/Orchestrator/sail/generic_notebook.ipynb /demo
RUN mkdir /host
WORKDIR /demo

CMD /bin/bash
# CMD  jupyter notebook --ip=0.0.0.0 --port=8080 --allow-root
