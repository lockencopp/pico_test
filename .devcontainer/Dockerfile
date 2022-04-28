FROM debian:11.3

RUN apt-get update && apt-get upgrade -y

RUN apt-get install -y cmake gcc-arm-none-eabi libnewlib-arm-none-eabi libstdc++-arm-none-eabi-newlib build-essential
RUN apt-get install -y gdb
RUN apt-get install -y wget git python3
RUN ln -s /usr/bin/gdb /usr/bin/arm-none-eabi-gdb

RUN wget --post-data 'accept_license_agreement=accepted&non_emb_ctr=confirmed&submit=Download+software' -O jlink.tgz https://www.segger.com/downloads/jlink/JLink_Linux_V764c_x86_64.tgz
RUN tar xfz jlink.tgz
RUN mv JLink_Linux_V764c_x86_64 /opt/jlink
RUN rm jlink.tgz
RUN ln -s /opt/jlink/JLinkExe /usr/local/bin/jlinkexe

RUN apt-get install -y  automake autoconf texinfo libtool libftdi-dev libusb-1.0-0-dev pkg-config
RUN mkdir pico

WORKDIR /pico
RUN git clone https://github.com/raspberrypi/openocd.git --branch rp2040 --depth=1 --no-single-branch
WORKDIR /pico/openocd
RUN ./bootstrap
RUN ./configure --enable-picoprobe
RUN make -j4 && make install

WORKDIR /pico
RUN git clone -b master https://github.com/raspberrypi/pico-examples.git && git clone -b master https://github.com/raspberrypi/pico-sdk.git
WORKDIR /pico/pico-sdk
RUN git submodule update --init

#ENV PICO_SDK_PATH=/pico/pico-sdk