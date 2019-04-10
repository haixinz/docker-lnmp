FROM centos:7
ENV LANG C.UTF-8
WORKDIR /opt/hexo
RUN yum install -y git epel-release 
RUN yum install -y nodejs && npm config set registry https://registry.npm.taobao.org \
&& npm install hexo-cli -g && hexo init haixinz && cd haixinz && npm install
WORKDIR haixinz
RUN npm install --save hexo-deployer-git
RUN git config --global user.email "383504860@qq.com"
RUN git config --global user.name "haixinz"
EXPOSE 4000
ENTRYPOINT ["hexo","server"]
