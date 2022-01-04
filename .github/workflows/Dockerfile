FROM archlinux:base-devel as builder

SHELL /bin/bash
RUN useradd -m builder && echo "builder ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    mkdir /packages && chown builder /packages && \
    echo "MAKEFLAGS=-j$(nproc)" >> /etc/makepkg.conf && \
    echo "[multilib]\nInclude = /etc/pacman.d/mirrorlist" >> /etc/pacman.conf && \
    pacman -Syuy lib32-gcc-libs
USER builder

WORKDIR /build
COPY --chown=builder libdrm-ps4 ./libdrm-ps4
RUN cd libdrm-ps4 && makepkg -si --log --noconfirm && cp *.pkg.* /packages/ && rm -rf libdrm-ps4

COPY --chown=builder mesa-ps4 ./mesa-ps4
RUN cd mesa-ps4 && makepkg -s --log --noconfirm && cp *.pkg.* /packages/ && rm -rf mesa-ps4

COPY --chown=builder xf86-video-amdgpu-ps4 ./xf86-video-amdgpu-ps4
RUN cd xf86-video-amdgpu-ps4 && makepkg -si --log --noconfirm && cp *.pkg.* /packages/ && rm -rf xf86-video-amdgpu-ps4

COPY --chown=builder lib32-libdrm-ps4 ./lib32-libdrm-ps4
RUN cd lib32-libdrm-ps4 && makepkg -si --log --noconfirm && cp *.pkg.* /packages/ && rm -rf lib32-libdrm-ps4

COPY --chown=builder lib32-mesa-ps4 ./lib32-mesa-ps4
RUN cd lib32-mesa-ps4 && makepkg -s --log --noconfirm && cp *.pkg.* /packages/ && rm -rf lib32-mesa-ps4

FROM scratch
COPY --from=builder /packages /
