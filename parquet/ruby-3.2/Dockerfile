FROM f3ddevelopers/aws-lambda-ruby-patched:3.2

RUN yum swap -y openssl-snapsafe-libs openssl-libs

RUN yum groupinstall -y "Development Tools" && \
    yum install -y \
    make gcc gcc-c++ \
    libcurl-devel \
    mariadb-devel \
    libffi-devel \
    xz \
    cmake3 \
    git \
    wget \
    boost-devel \
    python3-devel \
    epel-release \
    pkg-config \
    gobject-introspection \
    gobject-introspection-devel \
    arrow \
    arrow-libs \
    arrow-glib \
    arrow-glib-devel \
    parquet-libs \
    parquet-glib \
    parquet-glib-devel \
    gtk-doc \
    glib2-devel

RUN yum install -y python3-pip

RUN pip3 install meson ninja

# Download Apache Arrow source once and extract it
RUN cd /tmp && \
    wget https://github.com/apache/arrow/archive/refs/tags/apache-arrow-19.0.0.tar.gz && \
    tar xf apache-arrow-19.0.0.tar.gz

# Build Arrow C++ libraries
# These provide core Arrow functionality including Parquet support
RUN cd /tmp/arrow-apache-arrow-19.0.0/cpp && \
    mkdir build && \
    cd build && \
    cmake3 .. \
    -DARROW_PARQUET=ON \
    -DARROW_DATASET=ON \
    -DARROW_WITH_SNAPPY=ON \
    -DARROW_WITH_ZLIB=ON \
    -DARROW_WITH_ZSTD=ON \
    -DARROW_WITH_LZ4=ON \
    -DARROW_WITH_BROTLI=ON \
    -DARROW_COMPUTE=ON \
    -DARROW_CSV=ON \
    -DARROW_JSON=ON \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_LIBDIR=lib64 && \
    make -j$(nproc) && \
    make install && \
    /sbin/ldconfig

# Build Arrow GLib bindings
# These are required for Ruby to interface with the C++ libraries
RUN cd /tmp/arrow-apache-arrow-19.0.0/c_glib && \
    meson build --prefix=/usr --libdir=lib64 && \
    cd build && \
    ninja && \
    ninja install && \
    cd /tmp && \
    rm -rf arrow-apache-arrow-19.0.0 apache-arrow-19.0.0.tar.gz && \
    /sbin/ldconfig

ENV PKG_CONFIG_PATH=/usr/lib64/pkgconfig \
    LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH \
    ARROW_HOME=/usr

RUN yum install -y amazon-linux-extras && \
    amazon-linux-extras install epel -y
