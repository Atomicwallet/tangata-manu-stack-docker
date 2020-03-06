FROM ubuntu:18.04 as builder

ENV PATH /root/.cargo/bin:$PATH

COPY ./tangata-manu/cardano-http-bridge /opt/app
WORKDIR /opt/app

RUN apt update \
    && apt install -y build-essential curl \
    && curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

RUN cargo run --release || true # Because always exit 1  \ 
    && ls ./target/release/cardano-http-bridge # Check file exists, exit 1 if not exists

FROM ubuntu:18.04

WORKDIR /opt/app

COPY --from=builder /opt/app/target/release/cardano-http-bridge .

CMD ["/opt/app/cardano-http-bridge", "start"]