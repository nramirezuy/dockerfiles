FROM nixos/nix:2.24.12 AS builder

ARG EMANOTE_VERSION

RUN nix --extra-experimental-features "nix-command flakes" \
        profile install github:srid/emanote/${EMANOTE_VERSION}

RUN mkdir /tmp/nix-store-closure \
    && emanote_path=$(readlink -f $(which emanote)) \
    && cp -R $(nix-store -qR $emanote_path) /tmp/nix-store-closure

RUN mkdir /empty

FROM scratch

COPY --from=builder /tmp/nix-store-closure /nix/store
COPY --from=builder /root/.nix-profile/bin/emanote /usr/local/bin/emanote
COPY --from=builder /empty /data/dist
COPY --from=builder /empty /tmp

WORKDIR /data
ENTRYPOINT ["emanote"]
