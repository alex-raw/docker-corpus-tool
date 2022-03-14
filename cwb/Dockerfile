FROM ubuntu:impish as builder

# Install cwb dependencies
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install --no-install-recommends -y \
    apt-utils \
    autoconf \
    bison \
    flex \
    gcc \
    libc6-dev \
    libglib2.0-0 \
    libglib2.0-dev \
    libncurses5 \
    libncurses5-dev \
    libpcre3-dev \
    libreadline8 \
    libreadline-dev \
    make \
    pkg-config \
    subversion \
    && rm -rf /var/lib/apt/lists/*

# Download latest cwb source
RUN svn co http://svn.code.sf.net/p/cwb/code/cwb/trunk /cwb

# Run install script and Move to unified location
WORKDIR /cwb
RUN ./install-scripts/install-linux
RUN mv /usr/local/cwb-* /usr/local/cwb

###########################################
# Actual Image without build dependencies #
###########################################
FROM ubuntu:impish

# Install dependency libraries
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install --no-install-recommends -y \
    bison \
    flex \
    libglib2.0-0 \
    readline-common \
    libreadline8 \
    && rm -rf /var/lib/apt/lists/*

# Add new binaries to PATH
COPY --from=builder /usr/local/cwb /usr/local/cwb
ENV PATH="/usr/local/cwb/bin/:${PATH}"

# To mount data storage
VOLUME /data

CMD ["bash"]
