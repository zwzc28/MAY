# This Makefile is meant to be used by people that do not usually work
# with Go source code. If you know what GOPATH is then you probably
# don't need to bother with make.

.PHONY: MAY android ios geth-cross evm all test clean
.PHONY: MAY-linux geth-linux-386 geth-linux-amd64 geth-linux-mips64 geth-linux-mips64le
.PHONY: MAY-linux-arm geth-linux-arm-5 geth-linux-arm-6 geth-linux-arm-7 geth-linux-arm64
.PHONY: MAY-darwin geth-darwin-386 geth-darwin-amd64
.PHONY: MAY-windows geth-windows-386 geth-windows-amd64

GOBIN = ./build/bin
GO ?= latest
GORUN = env GO111MODULE=on go run

MAY:
	$(GORUN) build/ci.go install ./cmd/MAY
	@echo "Done building."
	@echo "Run \"$(GOBIN)/MAY\" to launch geth."

all:
	$(GORUN) build/ci.go install

android:
	$(GORUN) build/ci.go aar --local
	@echo "Done building."
	@echo "Import \"$(GOBIN)/MAY.aar\" to use the library."
	@echo "Import \"$(GOBIN)/MAY-sources.jar\" to add javadocs"
	@echo "For more info see https://stackoverflow.com/questions/20994336/android-studio-how-to-attach-javadoc"
	
ios:
	$(GORUN) build/ci.go xcode --local
	@echo "Done building."
	@echo "Import \"$(GOBIN)/Geth.framework\" to use the library."

test: all
	$(GORUN) build/ci.go test

lint: ## Run linters.
	$(GORUN) build/ci.go lint

clean:
	env GO111MODULE=on go clean -cache
	rm -fr build/_workspace/pkg/ $(GOBIN)/*

# The devtools target installs tools required for 'go generate'.
# You need to put $GOBIN (or $GOPATH/bin) in your PATH to use 'go generate'.

devtools:
	env GOBIN= go get -u golang.org/x/tools/cmd/stringer
	env GOBIN= go get -u github.com/kevinburke/go-bindata/go-bindata
	env GOBIN= go get -u github.com/fjl/gencodec
	env GOBIN= go get -u github.com/golang/protobuf/protoc-gen-go
	env GOBIN= go install ./cmd/abigen
	@type "npm" 2> /dev/null || echo 'Please install node.js and npm'
	@type "solc" 2> /dev/null || echo 'Please install solc'
	@type "protoc" 2> /dev/null || echo 'Please install protoc'

# Cross Compilation Targets (xgo)

MAY-cross: geth-linux geth-darwin geth-windows geth-android geth-ios
	@echo "Full cross compilation done:"
	@ls -ld $(GOBIN)/MAY-*

MAY-linux: geth-linux-386 geth-linux-amd64 geth-linux-arm geth-linux-mips64 geth-linux-mips64le
	@echo "Linux cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-*

MAY-linux-386:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/386 -v ./cmd/MAY
	@echo "Linux 386 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep 386

MAY-linux-amd64:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/amd64 -v ./cmd/MAY
	@echo "Linux amd64 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep amd64

MAY-linux-arm: geth-linux-arm-5 geth-linux-arm-6 geth-linux-arm-7 geth-linux-arm64
	@echo "Linux ARM cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep arm

MAY-linux-arm-5:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/arm-5 -v ./cmd/MAY
	@echo "Linux ARMv5 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep arm-5

MAY-linux-arm-6:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/arm-6 -v ./cmd/MAY
	@echo "Linux ARMv6 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep arm-6

MAY-linux-arm-7:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/arm-7 -v ./cmd/MAY
	@echo "Linux ARMv7 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep arm-7

MAY-linux-arm64:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/arm64 -v ./cmd/MAY
	@echo "Linux ARM64 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep arm64

MAY-linux-mips:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/mips --ldflags '-extldflags "-static"' -v ./cmd/MAY
	@echo "Linux MIPS cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep mips

MAY-linux-mipsle:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/mipsle --ldflags '-extldflags "-static"' -v ./cmd/MAY
	@echo "Linux MIPSle cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep mipsle

MAY-linux-mips64:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/mips64 --ldflags '-extldflags "-static"' -v ./cmd/MAY
	@echo "Linux MIPS64 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep mips64

MAY-linux-mips64le:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=linux/mips64le --ldflags '-extldflags "-static"' -v ./cmd/MAY
	@echo "Linux MIPS64le cross compilation done:"
	@ls -ld $(GOBIN)/MAY-linux-* | grep mips64le

MAY-darwin: geth-darwin-386 geth-darwin-amd64
	@echo "Darwin cross compilation done:"
	@ls -ld $(GOBIN)/MAY-darwin-*

MAY-darwin-386:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=darwin/386 -v ./cmd/MAY
	@echo "Darwin 386 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-darwin-* | grep 386

MAY-darwin-amd64:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=darwin/amd64 -v ./cmd/MAY
	@echo "Darwin amd64 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-darwin-* | grep amd64

MAY-windows: geth-windows-386 geth-windows-amd64
	@echo "Windows cross compilation done:"
	@ls -ld $(GOBIN)/MAY-windows-*

MAY-windows-386:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=windows/386 -v ./cmd/MAY
	@echo "Windows 386 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-windows-* | grep 386

MAY-windows-amd64:
	$(GORUN) build/ci.go xgo -- --go=$(GO) --targets=windows/amd64 -v ./cmd/MAY
	@echo "Windows amd64 cross compilation done:"
	@ls -ld $(GOBIN)/MAY-windows-* | grep amd64
