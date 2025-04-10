GITCOMMIT ?= $(shell git rev-parse HEAD)
GITDATE ?= $(shell git show -s --format='%ct')
VERSION ?= v0.0.0

LDFLAGSSTRING +=-X main.GitCommit=$(GITCOMMIT)
LDFLAGSSTRING +=-X main.GitDate=$(GITDATE)
LDFLAGSSTRING +=-X main.Version=$(VERSION)
LDFLAGS := -ldflags "$(LDFLAGSSTRING)"

# Use the old Apple linker to workaround broken xcode - https://github.com/golang/go/issues/65169
ifeq ($(shell uname),Darwin)
	FUZZLDFLAGS := -ldflags=-extldflags=-Wl,-ld_classic
endif

op-challenger:
	env GO111MODULE=on GOOS=$(TARGETOS) GOARCH=$(TARGETARCH) go build -v $(LDFLAGS) -o ./bin/op-challenger ./cmd

fuzz:
	go test $(FUZZLDFLAGS) -run NOTAREALTEST -v -fuzztime 10s -fuzz FuzzKeccak ./game/keccak/matrix

clean:
	rm bin/op-challenger

test:
	go test -v ./...

visualize:
	./scripts/visualize.sh

.PHONY: \
	op-challenger \
	clean \
	test \
	visualize
