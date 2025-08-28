FROM f3ddevelopers/aws-lambda-ruby-patched:3.2

RUN yum swap -y openssl-snapsafe-libs openssl-libs

RUN yum update -y && \
    yum install -y make gcc gcc-c++ ruby-devel && \
    yum clean all

COPY Gemfile .
COPY Gemfile.lock .

RUN bundle config set deployment true && \
    bundle config set without 'test development'
#     bundle config build.ffi -- --with-cflags="-Wno-implicit-function-declaration" && \
#     bundle config set retry 5 && \
#     bundle config set jobs $(getconf _NPROCESSORS_ONLN) && \
#     if [[ "$ARCHITECTURE" == "arm64" ]]; then \
#     bundle config set force_ruby_platform true && \
#     bundle config build.ffi --enable-libffi-alloc; \
#     fi && \
#     BUNDLE_FORCE_RUBY_PLATFORM=true bundle

RUN BUNDLE_FORCE_RUBY_PLATFORM=true bundle
