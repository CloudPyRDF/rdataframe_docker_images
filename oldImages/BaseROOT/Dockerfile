FROM amazonlinux:2
ENV roothome=/cern_root/root
ENV PATH=/cern_root/chroot/usr/local/sbin:/cern_root/chroot/usr/local/bin:/cern_root/chroot/usr/sbin:/cern_root/chroot/usr/bin:/cern_root/chroot/sbin:/cern_root/chroot/bin:$PATH
ENV LD_LIBRARY_PATH=/cern_root/chroot/usr/lib64:/cern_root/chroot/usr/lib:/usr/lib64:/usr/lib:$LD_LIBRARY_PATH
RUN python -c 'import yum, json; yb = yum.YumBase(); print json.dumps(yb.conf.yumvar, indent=2)'
RUN find /etc/yum/vars -type f -print -exec cat {} \;
RUN curl -L https://root.cern/download/root_v6.22.02.Linux-centos7-x86_64-gcc4.8.tar.gz -o rootbin.tar.gz
RUN mkdir -p /cern_root/chroot/etc/ && cp -r /etc/yum /cern_root/chroot/etc/yum \
    && yum install -y tar gzip libXft-devel make cmake gcc-c++ gcc \
    binutils libXft libXft-devel libXpm-devel libXext-devel python3 python3-devel libzstd ncurses-libs tbb tbb-devel \
    openssl11 --installroot=/cern_root/chroot --releasever=/ \
    && tar xvf rootbin.tar.gz -C /cern_root > /dev/null

RUN for dir in tutorials README cmake js icons LICENSE man geom fonts emacs; do \
        rm -rf ${roothome}/${dir}; \
    done \
    && ls -la $roothome \
    && ln -s /cern_root/chroot/usr/lib64/libtinfo.so.6  /cern_root/chroot/usr/lib64/libtinfo.so.5 

CMD . ${roothome}/bin/thisroot.sh && root