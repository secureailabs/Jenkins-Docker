FROM ubuntu

ARG git_personal_token

RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get -y dist-upgrade
RUN apt-get install -y libssl-dev
RUN apt-get install -y libcurl4-openssl-dev
RUN apt-get install -y uuid-dev

RUN apt-get install -y python3
RUN apt-get install -y python3-pip
RUN pip3 install numpy
RUN pip3 install requests numpy==1.20 pandas sklearn shap==0.38.1
RUN pip3 install torch --no-cache-dir
RUN pip3 install xgboost

#
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true
RUN apt-get install -y jupyter
RUN apt-get install -y git

WORKDIR /
# RUN git init \
#     && git config core.sparseCheckout true \
#     && git config --global url."https://$git_personal_token:@github.com/".insteadOf " https://github.com/" \
#     && git remote add origin -f https://$git_personal_token@github.com/secureailabs/Development \
#     && echo "Milestone3/VirtualMachine/Orchestrator/" > .git/info/sparse-checkout \
#     && echo "Milestone3/SharedCommonCode" >> .git/info/sparse-checkout \
#     && echo "Milestone3/WebService/RestApiPortal" >> .git/info/sparse-checkout \
#     && echo "Milestone3/VirtualMachine/SharedComponents" >> .git/info/sparse-checkout \
#     && echo "Milestone3/Common.mk" >> .git/info/sparse-checkout \
#     && git checkout main


# RUN rm /Orchestrator/Milestone3/SharedCommonCode/Sources/CompressionHelperFunctions.cpp

RUN git config --global url."https://$git_personal_token:@github.com/".insteadOf " https://github.com/" \
    && git clone https://$git_personal_token@github.com/secureailabs/Development

WORKDIR /Development/Milestone3/VirtualMachine/Orchestrator/
RUN make .

WORKDIR /Development/Milestone3/VirtualMachine/Orchestrator/sail
RUN pip3 install .

RUN mkdir /host
WORKDIR /host

ENV PYTHONPATH "${PYTHONPATH}:/Development/Milestone3/VirtualMachine/Orchestrator/sail"

CMD /bin/bash
# CMD  jupyter notebook --notebook-dir=/home/SAIL --ip=0.0.0.0 --port=8080 --allow-root
