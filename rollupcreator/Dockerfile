FROM node:20-bookworm-slim
RUN apt-get update && \
    apt-get install -y git docker.io python3 make gcc g++ curl jq
ARG NITRO_CONTRACTS_BRANCH=main
WORKDIR /workspace  
RUN git clone --no-checkout https://github.com/OffchainLabs/nitro-contracts.git ./
RUN git checkout ${NITRO_CONTRACTS_BRANCH}
RUN git submodule update --init --recursive
RUN yarn install && yarn cache clean
RUN curl -L https://foundry.paradigm.xyz | bash
ENV PATH="${PATH}:/root/.foundry/bin"
RUN foundryup --install 1.0.0
RUN touch scripts/config.ts

RUN echo '\nsolc_version = "0.8.28+commit.7893614a"' >> foundry.toml
RUN echo '\nextra_solc_versions = ["0.8.17+commit.8df45f5f"]' >> foundry.toml

RUN yarn build:all
ENTRYPOINT ["yarn"]
