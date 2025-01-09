

FROM rockylinux:8.9

# ARG USERNAME
# ARG PASSWORD

RUN dnf -y update && \
    dnf -y install libpq tzdata && \
    dnf -y install python3.11 python3.11-pip && \
    dnf clean all


### we need the epel-release for some libraries
RUN dnf install -y epel-release && \
    dnf -y update

### for g++ and other stuff
# RUN dnf -y groupinstall "Development Tools" 
RUN dnf -y install \
            git \
            ## replacing libjemalloc-dev
            jemalloc-devel \
            wget \
            cmake \
            # is gcc-toolset a valid replacement to g++?
            # gcc-toolset-11 \
            # gcc-toolset-11-gcc-c++ \ 
            zlib-devel \
            ## below for autoscaler
            tmux \
            screen \
            rsync \
            # netbase \ # what is it?
            # is openssh-clients replacement to openssh-client?
            openssh-clients \
            gnupg && \
    dnf clean all

### you can run python as python3, python3.11 or python
### the system default python is 3.6 (too old), and is not user linked
RUN ln -s /usr/bin/python3.11 /usr/bin/python

RUN pip3.11 install --no-cache-dir \
    "psycopg2-binary==2.9.9" \
    "sqlalchemy==2.0.32" \
    "kubernetes==30.1.0" \
    "ray[all]==2.34" \
    "redis>=3.5.0,<4.0.0" \
    "six==1.13.0" \
    "boto3==1.26.76" \
    "pyOpenSSL==24.2.1" \
    "cryptography==42.0.4" \
    "google-api-python-client==1.7.8" \
    "google-oauth==1.0.1" \
    "azure-cli-core==2.63.0" \
    "azure-identity==1.17.1" \
    "azure-mgmt-compute==23.1.0" \
    "azure-mgmt-network==19.0.0" \
    "azure-mgmt-resource==20.0.0" \
    "msrestazure==0.6.4"
## remove PIP

### testing odbcs
RUN dnf -y install unixODBC postgresql-odbc && \
    dnf clean all

# Development
RUN dnf -y   groupinstall "Development Tools"

RUN dnf -y install iproute

RUN dnf -y install sudo

RUN dnf -y install gcc-toolset-11

RUN dnf -y install postgresql

RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

RUN install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

RUN adduser chris

RUN usermod -aG wheel chris

RUN echo chris:chris | chpasswd

RUN echo root:root | chpasswd

RUN echo [user] > /etc/wsl.conf

RUN echo default=chris >> /etc/wsl.conf

USER chris
