FROM ubuntu:20.04
RUN apt-get update && DEBIAN_FRONTEND="noninteractive" apt-get install -y build-essential cmake ninja-build libboost-dev git libc6-dbg gcc-multilib g++-multilib
WORKDIR /build
RUN ./checkout.sh && ./build.sh
CMD ["/build/libbw64/build/examples @@ /dev/null"]
