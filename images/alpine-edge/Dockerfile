# SPDX-FileCopyrightText: OpenTalk GmbH <mail@opentalk.eu>
#
# SPDX-License-Identifier: EUPL-1.2

FROM alpine:edge

RUN apk update --no-cache; \
    apk add --no-cache \
        ca-certificates \
        curl \
        gcc \
        g++ \
        cmake \
        clang-dev \
        git \
        gpg \
        grep \
        protoc \
        shellcheck \
        lld \
        libxml2-utils \
        musl-dev \
        libpq-dev \
        openssl-libs-static \
        perl \
        make \
        libva-dev \
        libdrm-dev

ARG RUST_VERSION

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=$RUST_VERSION \
    RUSTFLAGS="-lpgcommon_shlib -lpgport_shlib -lssl -lcrypto"

COPY config.toml /usr/local/cargo/

RUN set -eux; \
    apkArch="$(apk --print-arch)"; \
    case "$apkArch" in \
        x86_64) rustArch='x86_64-unknown-linux-musl'; rustupSha256='1455d1df3825c5f24ba06d9dd1c7052908272a2cae9aa749ea49d67acbe22b47' ;; \
        aarch64) rustArch='aarch64-unknown-linux-musl'; rustupSha256='b1962dfc18e1fd47d01341e6897cace67cddfabf547ef394e8883939bd6e002e' ;; \
        *) echo >&2 "unsupported architecture: $apkArch"; exit 1 ;; \
    esac; \
    url="https://static.rust-lang.org/rustup/archive/1.27.1/${rustArch}/rustup-init"; \
    wget "$url"; \
    echo "${rustupSha256} *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path \
	--profile minimal --component clippy --component rustfmt --component llvm-tools-preview \
	--default-toolchain $RUST_VERSION --default-host ${rustArch}; \
    rm rustup-init; \
    rustup toolchain install nightly --component llvm-tools-preview --component rustfmt; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup --version; \
    cargo --version; \
    rustc --version; \
    cargo +nightly --version

RUN set -eux; \
    cargo install --locked grcov; \
    cargo install --locked cargo-machete; \
    cargo install --locked cargo-audit; \
    cargo install --locked cargo-auditable; \
    cargo install --locked cargo-deny; \
    cargo install --locked olpat-cli; \
    ln -sf olpat /usr/local/cargo/bin/taplo; \
    cargo install --locked sqlant; \
    cargo install --locked diesel_cli --version ~2 --no-default-features --features="postgres"; \
    cargo install --locked just; \
    cargo install --locked cargo-nextest; \
    cargo install cargo-llvm-cov; \
    cargo machete --version; \
    cargo audit --version; \
    cargo auditable --version; \
    cargo deny --version; \
    taplo --version; \
    sqlant --help; \
    diesel --version; \
    just --version; \
    cargo nextest --version; \
    cargo llvm-cov --version; \
