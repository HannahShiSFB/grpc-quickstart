FROM php:7.2-fpm-stretch

ENV GRPC_RELEASE=v1.23.0

RUN set -eux; \
    apt-get update; \
    apt-get install -y git build-essential automake autoconf libtool pkg-config zip unzip

RUN set -eux; \
    pecl install grpc protobuf; \
    mkdir -p /work; \
# Clone grpc source
    cd /work; \
    git clone https://github.com/grpc/grpc; \
    cd /work/grpc; \
    git checkout $GRPC_RELEASE; \
    git submodule update --init; \
# # Build and install the gRPC C core library \
    # cd /work/grpc; \
    # make -j10; \
    # make install;  \
# # Build and install gRPC PHP extension \
    # cd /work/grpc/src/php/ext/grpc; \
    # phpize; \
    # ./configure; \
    # make; \
    # make install; \
# PHP Protoc Plugin \
    cd /work/grpc; \
    make grpc_php_plugin; \
# Install Protobuf compiler \
#    cd /work/grpc/third_party/protobuf; \
#    ./autogen.sh && ./configure && make; \
#    make install; \
    cd /work; mkdir -p tmp; cd tmp; \
    curl -sLO https://github.com/protocolbuffers/protobuf/releases/download/v3.9.1/protoc-3.9.1-linux-x86_64.zip; \ 
    unzip protoc-3.9.1-linux-x86_64.zip; \
    mv /work/tmp/bin/protoc /usr/local/bin/; \
    rm -r /work/tmp; \
# Install composer \
    cd /work; \
    curl -sS https://getcomposer.org/installer | php; \
    mv composer.phar /usr/local/bin/composer;

# RUN set -eux; \
#     echo "extension=grpc.so" > /usr/local/etc/php/conf.d/grpc.ini; \
#     echo "extension=protobuf.so" > /usr/local/etc/php/conf.d/protobuf.ini;

RUN set -eux; \
    cd /work/grpc/examples/php; \
    ./greeter_proto_gen.sh; \
    composer install; \
    sed -i "s/'localhost:50051'/'node-greeter-server:50051'/" greeter_client.php; \
    mv /work/grpc/examples /work; \
    rm -r /work/grpc

CMD ["/work/examples/php/run_greeter_client.sh"]
