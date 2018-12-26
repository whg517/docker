FROM centos:7
ENV container docker
VOLUME [ "/sys/fs/cgroup" ]
RUN mkdir /etc/systemd/system/console-getty.service.d/;\
cp /lib/systemd/system/console-getty.service /etc/systemd/system/;\
sed -i '/ExecStart/ s/noclear/noclear --autologin root/' /etc/systemd/system/console-getty.service;\
ln -s ../console-getty.service /etc/systemd/system/console-getty.service.d/console-getty.service
CMD ["/usr/sbin/init"]
