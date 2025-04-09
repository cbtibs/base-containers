FROM debian:bookworm-slim

ARG STEAM_UID=1000
ARG STEAM_GID=1000
ARG PROTON_VERSION=9-25

ENV DEBIAN_FRONTEND=noninteractive \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    HOME=/home/steam \
    STEAMCMD_DIR=/home/steam/steamcmd \
    PATH="/home/steam/steamcmd:${PATH}"

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
      ca-certificates \
      curl \
      dbus-bin \
      file \
      locales \
      lib32gcc-s1 \
      lib32stdc++6 \
      libfreetype6 \
      winbind && \
    rm -rf /var/lib/apt/lists/* && \
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen && \
    rm -f /etc/machine-id && \
    dbus-uuidgen --ensure=/etc/machine-id && \
    groupadd -r -g ${STEAM_GID} steam && \
    useradd -r -u ${STEAM_UID} -g steam -m steam

USER steam
WORKDIR /home/steam

RUN mkdir -p "$STEAMCMD_DIR" \
    /home/steam/.steam/steam/compatibilitytools.d \
    /home/steam/.steam/steam/steamapps && \
    cd "$STEAMCMD_DIR" && \
    curl -sL https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz \
      | tar -zxvf - && \
    chmod +x steamcmd.sh && \
    ln -s steamcmd.sh steamcmd && \
    curl -sL "https://github.com/GloriousEggroll/proton-ge-custom/releases/download/GE-Proton${PROTON_VERSION}/GE-Proton${PROTON_VERSION}.tar.gz" \
      | tar -zxvf - -C /home/steam/.steam/steam/compatibilitytools.d

CMD ["/bin/bash"]