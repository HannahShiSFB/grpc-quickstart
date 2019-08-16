FROM node:10.16.2-stretch

RUN set -eux; \
    cd /; mkdir -v work; 

COPY examples /work/examples  

WORKDIR /work/examples/node

RUN set -eux; \
    npm install


CMD ["node", "dynamic_codegen/greeter_server.js"]
