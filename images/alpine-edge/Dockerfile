# SPDX-FileCopyrightText: OpenTalk GmbH <mail@opentalk.eu>
#
# SPDX-License-Identifier: EUPL-1.2

FROM alpine:edge

RUN apk update --no-cache; \
    apk upgrade --no-cache; \
    apk add --no-cache \
        ca-certificates \
        gcc \
        protoc \
        lld \
	libxml2-utils \
	musl-dev \
	rust cargo \
	rustfmt rust-clippy \
	cargo-audit cargo-auditable

ENV CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH

COPY config.toml /usr/local/cargo/

RUN set -eux; \
    chmod -R a+w $CARGO_HOME; \
    cargo install --locked cargo-deny; \
    cargo install --locked cargo-machete; \
    cargo install --locked grcov; \
    cargo install --locked sqlant; \
    cargo machete --version; \
    cargo audit --version; \
    cargo auditable --version; \
    cargo deny --version; \
    sqlant --help; \
