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

RUN pip3 install requests numpy==1.20 pandas
RUN pip3 install sklearn
RUN pip3 install shap==0.38.1
# RUN pip3 install torch --no-cache-dir
RUN pip3 install xgboost
RUN pip3 install matplotlib==3.4.3

#
RUN apt-get install -y jupyter

WORKDIR /

#Bring down the Dev lib
RUN git config --global url."https://$git_personal_token:@github.com/".insteadOf " https://github.com/" \
    && git clone https://$git_personal_token@github.com/secureailabs/Development 

# make the SAILPyAPI 
WORKDIR /Development/Milestone3/VirtualMachine/Orchestrator/
# RUN git checkout jingwei_3
RUN make all -j

#install the orchestrator lib
RUN pip3 install /Development/Milestone3/VirtualMachine/Orchestrator/sail

#bring in the orchestrator script
WORKDIR /Development/Milestone3/orchestrator_scripts

CMD /bin/bash