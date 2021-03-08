FROM golang:1.14.7

WORKDIR /go/src/app
RUN git config --global http.https://gopkg.in.followRedirects true
RUN git clone https://github.com/dalareo/open-ethereum-pool.git

WORKDIR /go/src/app/open-ethereum-pool
RUN make

COPY config.json .

CMD ["./build/bin/open-ethereum-pool", "config.json"]
