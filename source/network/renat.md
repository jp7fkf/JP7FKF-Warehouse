# RENAT install battle (# 2018-10-26 16:40:31 YudaiHashimoto)
```
yum -y install centos-release-scl-rh
yum -y install python27
python --version

yum install -y epel-release
yum install -y gettext gcc net-snmp net-snmp-devel net-snmp-utils czmq czmq-devel python27-tkinter xorg-x11-server-Xvfb ghostscript firefox-60.2.1-1.el6.centos.x86_64 httpd vim
pip install numpy pyte PyYAML openpyxl Jinja2 pandas paramiko lxml requests pdfkit pyvmomi PyVirtualDisplay
# yum search python | grep -i devel
# yum install -y python-devel (when errors occored at next step)
pip install netsnmp-py==0.3 

groupadd techno -o -g 1000
useradd robot -g techno
passwd  robot
```