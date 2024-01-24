# SPDX-FileCopyrightText: OpenTalk GmbH <mail@opentalk.eu>
#
# SPDX-License-Identifier: EUPL-1.2

FROM alpine:3.17

RUN apk update --no-cache; \
    apk add --no-cache \
        ca-certificates \
        gcc \
        protoc \
        lld \
	libxml2-utils \
	musl-dev

ARG RUST_VERSION

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=$RUST_VERSION

COPY config.toml /usr/local/cargo/

RUN set -eux; \
    apkArch="$(apk --print-arch)"; \
    case "$apkArch" in \
        x86_64) rustArch='x86_64-unknown-linux-musl'; rustupSha256='7aa9e2a380a9958fc1fc426a3323209b2c86181c6816640979580f62ff7d48d4' ;; \
        aarch64) rustArch='aarch64-unknown-linux-musl'; rustupSha256='b1962dfc18e1fd47d01341e6897cace67cddfabf547ef394e8883939bd6e002e' ;; \
        *) echo >&2 "unsupported architecture: $apkArch"; exit 1 ;; \
    esac; \
    url="https://static.rust-lang.org/rustup/archive/1.26.0/${rustArch}/rustup-init"; \
    wget "$url"; \
    echo "${rustupSha256} *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path \
	--profile minimal --component clippy --component rustfmt --component llvm-tools-preview \
	--default-toolchain $RUST_VERSION --default-host ${rustArch}; \
    rm rustup-init; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup --version; \
    cargo --version; \
    rustc --version

RUN set -eux; \
    cargo install --locked grcov; \
    cargo install --locked cargo-machete; \
    cargo install --locked cargo-audit; \
    cargo install --locked cargo-auditable; \
    cargo install --locked cargo-deny; \
    cargo machete --version; \
    cargo audit --version; \
    cargo auditable --version; \
    cargo deny --version;
