FROM p21jgitlin/ruby-rpm:amzn2
LABEL MAINTAINER="Josh Gitlin <jgitlin@pinnacle21.com>"

RUN yum -y upgrade && \
    yum -y groupinstall "Development Tools" && \
    yum -y install curl wget ca-certificates iproute rsync openssh-clients java-11-openjdk-headless sudo man procps-ng bzip2 openssl-devel libyaml-devel libffi-devel readline-devel zlib-devel gdbm-devel ncurses-devel gmp-devel && \
    curl -o /tmp/ruby.rpm -L "https://github.com/jgitlin-p21/ruby-rpm/releases/download/2.6.5/ruby-2.6.5-1.amzn2.x86_64.rpm" && \
    /usr/bin/yum -y install /tmp/ruby.rpm && \
    # Eventually, we will install cinc
    curl -L https://www.chef.io/chef/install.sh | bash -s -- -v 14 && \
    mkdir -p /var/chef/cache /var/chef/cookbooks && \
    wget -qO- https://supermarket.chef.io/cookbooks/omnibus/download | tar xzC /var/chef/cookbooks && \
    for dep in windows mingw build-essential chef-sugar chef-ingredient git homebrew remote_install seven_zip wix; do \
        wget -qO- https://supermarket.chef.io/cookbooks/${dep}/download | tar xzC /var/chef/cookbooks; done && \
    mkdir -p /etc/chef && \
    echo 'node_name "omnibus-amzn2.internal"' > /etc/chef/client.rb && \
    chef-solo -o 'recipe[omnibus]' && \
    sed -i -e 's/^env.*//' /home/omnibus/load-omnibus-toolchain.sh && \
    yum -y remove chef && rm -rf /var/chef/ /opt/chef/embedded/ /etc/chef && \
    yum clean all && rm -rf /var/cache/yum
