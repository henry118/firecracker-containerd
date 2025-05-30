# Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License"). You may
# not use this file except in compliance with the License. A copy of the
# License is located at
#
# 	http://aws.amazon.com/apache2.0/
#
# or in the "license" file accompanying this file. This file is distributed
# on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either
# express or implied. See the License for the specific language governing
# permissions and limitations under the License.

SOURCES := $(shell find mmds . -name '*.go')
GOMOD := $(shell go env GOMOD)
GOSUM := $(GOMOD:.mod=.sum)

REVISION=$(shell git rev-parse HEAD)

all: credential-helper

credential-helper: docker-credential-mmds

docker-credential-mmds: $(SOURCES) $(GOMOD) $(GOSUM)
ifneq ($(STATIC_DOCKER_CREDENTIAL_MMDS),)
	CGO_ENABLED=0 go build -o docker-credential-mmds $(EXTRAGOARGS) -ldflags "-X main.revision=$(REVISION)"
else
	go build -o docker-credential-mmds $(EXTRAGOARGS) -ldflags "-X main.revision=$(REVISION)"
endif

test:
	go test ./... $(EXTRAGOARGS)

integ-test:

# installation is handled by the rootfs building targets in the main Makefile
install:

clean:
	- rm -f docker-credential-mmds

distclean: clean

.PHONY: all credential-helper clean distclean test integ-test install

