# Copyright 2021 The Kubernetes Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

ARG GOLANG_VERSION

FROM --platform=$BUILDPLATFORM golang:${GOLANG_VERSION} AS builder

ARG BUILDPLATFORM
ARG TARGETARCH


WORKDIR /workspace
COPY . .
RUN go mod tidy && CGO_ENABLED=0 GOOS=linux GOARCH=${TARGETARCH} go build -a -o kube-webhook-certgen main.go

FROM --platform=$BUILDPLATFORM gcr.io/distroless/static:nonroot
ARG BUILDPLATFORM
ARG TARGETARCH

LABEL org.opencontainers.image.source=https://github.com/kubernetes/ingress-nginx

WORKDIR /
COPY --from=builder /workspace/kube-webhook-certgen /kube-webhook-certgen
USER 65532:65532
ENTRYPOINT ["/kube-webhook-certgen"]
