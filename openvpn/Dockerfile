FROM registry.cn-hangzhou.aliyuncs.com/huati/centos
MAINTAINER Leelmes "i@chengxuan.li"

RUN yum install -y openvpn && yum clean all

CMD ["/usr/sbin/openvpn", "--status", "/run/openvpn_server.status", "10", "--cd", "/data1/openvpn", "--config", "/etc/openvpn/server.conf", "--writepid", "/run/openvpn_server.pid"]
