FROM public.ecr.aws/lambda/ruby:3.4

RUN dnf update -y && \
    dnf install -y \
        ruby-devel \
        make gcc gcc-c++ \
        openssl-devel zlib-devel libffi-devel \
        git patch tar && \
    dnf clean all

COPY Gemfile .
COPY Gemfile.lock .

RUN bundle config set deployment true && \
    bundle config set without 'test development'

RUN BUNDLE_FORCE_RUBY_PLATFORM=true bundle
