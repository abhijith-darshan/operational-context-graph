# SPDX-FileCopyrightText: 2026 SAP SE or an SAP affiliate company and Operational Context Graph contributors
#
# SPDX-License-Identifier: Apache-2.0

SHELL := /usr/bin/env bash

IMG ?= ctx/ocg:local

.PHONY: build start docker-build

build: ## Build the ocg service
	pnpm build

start: ## Run the ocg service
	pnpm start

docker-build: ## Build the Docker image locally
	docker build -t $(IMG) -f $(CURDIR)/Dockerfile $(shell git -C $(CURDIR) rev-parse --show-toplevel)