PROTOC=$(shell which protoc)
PROTOC-GEN-GRPC-GATEWAY=$(shell which protoc-gen-grpc-gateway)

PROTO_FILE = $(shell find npool -name "*.proto")
PROTO_GW_FILE = $(shell grep -l 'google.api.http' -r npool)
PROTO_GO_FILE = $(patsubst %.proto,%.pb.go,$(PROTO_FILE))
PROTO_GO_GW_FILE = $(patsubst %.proto,%.pb.gw.go,$(PROTO_GW_FILE))

PROTO_INCLUDE += -I.
PROTO_INCLUDE += -I./google

proto: $(PROTO_GO_FILE) $(PROTO_GO_GW_FILE)
%.pb.go: %.proto
	$(PROTOC) $(PROTO_INCLUDE) \
		--go_out . --go_opt paths=source_relative \
		--go-grpc_out=. --go-grpc_opt paths=source_relative \
		--doc_out=$(dir $*) \
		--doc_opt=markdown,$(notdir $*).md $<

%.pb.gw.go: %.proto
	$(PROTOC) $(PROTO_INCLUDE) $< --plugin=protoc-gen-grpc-gateway=$(PROTOC-GEN-GRPC-GATEWAY) \
		--grpc-gateway_out=logtostderr=true:. \
		--grpc-gateway_opt paths=source_relative

clean:
	find ./ -name "*.go" | xargs rm -rf
	find ./ -name "*.md" | xargs rm -rf
