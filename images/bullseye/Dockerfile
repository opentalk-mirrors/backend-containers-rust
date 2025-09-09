# SPDX-FileCopyrightText: OpenTalk GmbH <mail@opentalk.eu>
#
# SPDX-License-Identifier: EUPL-1.2

FROM buildpack-deps:bullseye

ARG RUST_VERSION

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=$RUST_VERSION

COPY config.toml /usr/local/cargo/

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        libxml2-utils \
        lld \
        protobuf-compiler \
        ; \
    dpkgArch="$(dpkg --print-architecture)"; \
    case "${dpkgArch##*-}" in \
        amd64) rustArch='x86_64-unknown-linux-gnu'; rustupSha256='6aeece6993e902708983b209d04c0d1dbb14ebb405ddb87def578d41f920f56d' ;; \
        armhf) rustArch='armv7-unknown-linux-gnueabihf'; rustupSha256='3c4114923305f1cd3b96ce3454e9e549ad4aa7c07c03aec73d1a785e98388bed' ;; \
        arm64) rustArch='aarch64-unknown-linux-gnu'; rustupSha256='1cffbf51e63e634c746f741de50649bbbcbd9dbe1de363c9ecef64e278dba2b2' ;; \
        i386) rustArch='i686-unknown-linux-gnu'; rustupSha256='0a6bed6e9f21192a51f83977716466895706059afb880500ff1d0e751ada5237' ;; \
        *) echo >&2 "unsupported architecture: ${dpkgArch}"; exit 1 ;; \
    esac; \
    url="https://static.rust-lang.org/rustup/archive/1.27.1/${rustArch}/rustup-init"; \
    wget "$url"; \
    echo "${rustupSha256} *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path --profile default --default-toolchain $RUST_VERSION --default-host ${rustArch}; \
    rm rustup-init; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup component add \
        rustfmt \
        llvm-tools \
        ; \
    rustup toolchain install nightly; \
    cargo install --locked grcov; \
    cargo install --locked cargo-machete; \
    cargo install --locked cargo-audit; \
    cargo install --locked cargo-auditable; \
    cargo install --locked cargo-deny; \
    cargo install --locked taplo-cli; \
    cargo install --locked sqlant; \
    cargo install --locked diesel_cli --version ~2 --no-default-features --features="postgres"; \
    cargo install --locked just; \
    cargo install --locked cargo-nextest; \
    rustup --version; \
    cargo --version; \
    rustc --version; \
    cargo +nightly --version; \
    grcov --version; \
    cargo machete --version; \
    cargo audit --version; \
    cargo auditable --version; \
    cargo deny --version; \
    taplo --version; \
    sqlant --help; \
    diesel --version; \
    just --version; \
    cargo nextest --version; \
