FROM amazonlinux:2
ENV UID 1000
ENV AWS_CLI_URL "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip"
ENV SAM_CLI_URL "https://github.com/aws/aws-sam-cli/releases/latest/download/aws-sam-cli-linux-x86_64.zip"
ENV NODE_VER 16
ENV NODE_MINOR 9.1
ENV TZ Asia/Tokyo
RUN yum update -y \
  && yum install -y \
    curl \
    tzdata \
    zip \
    unzip \
    sudo \
    wget \
    tar \
    openssh \
    git \
  && amazon-linux-extras install -y docker \
  && rm -rf /var/cache/yum/* \
  && yum clean all
RUN useradd -u ${UID} -m vscode \
  && echo "vscode:vscode" | chpasswd \
  && echo 'vscode ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
# awscli
RUN cd /usr/local/ \
  && curl ${AWS_CLI_URL} -o "awscliv2.zip" \
  && unzip awscliv2.zip \
  && rm -f awscliv2.zip \
  && ./aws/install \
  && rm -rf aws
# sam
RUN cd /usr/local/ \
  && wget ${SAM_CLI_URL} \
  && unzip aws-sam-cli-linux-x86_64.zip -d sam-installation \
  && rm -f aws-sam-cli-linux-x86_64.zip \
  && ./sam-installation/install \
  && rm -rf sam-installation
# node
RUN cd /usr/local \
  && curl https://nodejs.org/dist/v${NODE_VER}.${NODE_MINOR}/node-v${NODE_VER}.${NODE_MINOR}-linux-x64.tar.xz -o "node.tar.xz" \
  && mkdir node && tar xfv node.tar.xz -C node --strip-components 1 \
  && rm -f node.tar.xz \
  && chown -R vscode:vscode node \
  && ln -s /usr/local/node/bin/node /usr/bin/node \
  && ln -s /usr/local/node/bin/npm /usr/bin/npm \
  && ln -s /usr/local/node/bin/npx /usr/bin/npx