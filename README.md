# fun-grpcgo

## Ref
https://developers.google.com/protocol-buffers/docs/proto3

### protobuf
sudo apt install protobuf-compiler

### protoc-gen-go
go get -u github.com/golang/protobuf/protoc-gen-go

## make command
protoc --go_out=. *.proto

## Stringer
https://github.com/golang/protobuf/blob/master/proto/text.go
