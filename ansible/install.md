## aws ec2 amazon linux에 설치

    sudo su
    yum update
    yum install -y git
    yum -y install git python-jinja2 python-paramiko PyYAML make MySQL-python openssl* python-cffi
    git clone git://github.com/ansible/ansible.git
    cd ansible
    git submodule update --init --recursive
    make install
