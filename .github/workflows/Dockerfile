FROM archlinux:base-devel as builder

RUN useradd -m builder && echo "\nbuilder ALL=(ALL) NOPASSWD: ALL" | tee /etc/sudoers && mkdir /packages /build && chown builder /packages /build
USER builder

WORKDIR /build
COPY xf86-video-amdgpu-ps4 ./xf86-video-amdgpu-ps4
RUN cd xf86-video-amdgpu-ps4 && makepkg -si && cp *.pkg.* /packages/ && rm -rf xf86-video-amdgpu-ps4

COPY libdrm-ps4 ./libdrm-ps4
RUN cd libdrm-ps4 && makepkg -si && cp *.pkg.* /packages/ && rm -rf libdrm-ps4

COPY mesa-ps4 ./mesa-ps4
RUN cd mesa-ps4 && makepkg -si && cp *.pkg.* /packages/ && rm -rf mesa-ps4

COPY lib32-libdrm-ps4 ./lib32-libdrm-ps4
RUN cd lib32-libdrm-ps4 && makepkg -si && cp *.pkg.* /packages/ && rm -rf lib32-libdrm-ps4

COPY lib32-mesa-ps4 ./lib32-mesa-ps4
RUN cd lib32-mesa-ps4 && makepkg -si && cp *.pkg.* /packages/ && rm -rf lib32-mesa-ps4

FROM scratch
COPY --from=builder /packages /
