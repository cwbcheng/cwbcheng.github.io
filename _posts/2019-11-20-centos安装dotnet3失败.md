# 现象
安装失败，提示
Error downloading packages:
  aspnet xxxxx(忘了）: [Errno 256] No more mirrors to try.

# 解决方案
切换epel源后重新安装。
yum -y install epel-release
yum clean all
yum makecache
