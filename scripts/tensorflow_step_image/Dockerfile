FROM ubuntu:18.04

# The default source archive.ubuntu.com is busy and slow. We use the following source makes docker build running faster.
RUN echo '\n\
deb http://cn.archive.ubuntu.com/ubuntu/ bionic main restricted universe multiverse \n\
deb http://cn.archive.ubuntu.com/ubuntu/ bionic-security main restricted universe multiverse \n\
deb http://cn.archive.ubuntu.com/ubuntu/ bionic-updates main restricted universe multiverse \n\
deb http://cn.archive.ubuntu.com/ubuntu/ bionic-proposed main restricted universe multiverse \n\
deb http://cn.archive.ubuntu.com/ubuntu/ bionic-backports main restricted universe multiverse \n\
' > /etc/apt/sources.list

RUN apt-get -qq update && apt-get install -y openjdk-8-jdk maven wget unzip > /dev/null

RUN wget -q http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/119096/cn_zh/1557995455961/odpscmd_public.zip && \
unzip -qq odpscmd_public.zip -d /usr/local/odpscmd && \
ln -s /usr/local/odpscmd/bin/odpscmd /usr/local/bin/odpscmd && \
rm -rf odpscmd_public.zip

ADD step /usr/bin/
ADD parser/* /opt/sqlflow/parser/

ENV DEBIAN_FRONTEND=noninteractive
RUN ln -sf /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
apt-get install -y tzdata > /dev/null && \
dpkg-reconfigure --frontend noninteractive tzdata

RUN apt-get install -y build-essential libmysqlclient-dev > /dev/null

RUN apt-get install -y python3 python3-pip
RUN ln -s /usr/bin/python3 /usr/bin/python
RUN ln -s /usr/bin/pip3 /usr/bin/pip

RUN bash -c "pip install numpy==1.16.1 tensorflow==2.0.0b1 mysqlclient==1.4.4 impyla==0.16.0 pyodps==0.8.3"
ADD sqlflow_submitter $SQLFLOWPATH/python/
ADD sqlflow_models $SQLFLOWPATH/python/
