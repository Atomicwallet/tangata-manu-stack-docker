FROM ubuntu:18.04

ENV PATH /root/.cargo/bin:$PATH
ENV PATH /root/.yarn/bin:/root/.config/yarn/global/node_modules/.bin:$PATH
ENV NODE_VERSION 10.19.0
ENV YARN_VERSION 1.22.0

COPY ./tangata-manu/ /opt/app

RUN apt-get update && apt-get install -y -q --no-install-recommends \
    && apt install -y build-essential curl wget \
    # Install RUST
    && curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y \
    # Install wasm-pack
    && curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh  \
    # Install Nodejs
    && wget -q https://deb.nodesource.com/node_10.x/pool/main/n/nodejs/nodejs_${NODE_VERSION}-1nodesource1_amd64.deb -P /tmp \
    && apt install -y /tmp/nodejs_${NODE_VERSION}-1nodesource1_amd64.deb \
    # Install Yarn
    && curl -o- -L https://yarnpkg.com/install.sh | bash -s -- --version ${YARN_VERSION} \
    # Clean
    && apt clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/* 

WORKDIR /opt/app 

RUN npm run preinstall && yarn install && yarn build \
    # Clean
    && rm -rf /root/.cargo  \
    && rm -rf /root/.npm \
    && rm -rf /root/.rustup

CMD ["node", "./dist/index.js"]