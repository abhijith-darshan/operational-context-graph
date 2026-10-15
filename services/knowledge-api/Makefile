# SPDX-FileCopyrightText: 2026 SAP SE or an SAP affiliate company and Operational Context Graph contributors
#
# SPDX-License-Identifier: Apache-2.0

SHELL := /usr/bin/env bash

IMG ?= ctx/knowledge-api:local

.PHONY: run docker-build

run: ## Run the knowledge-api service
	uv run uvicorn knowledge_api.main:app --host 0.0.0.0 --port 8080

docker-build: ## Build the Docker image locally
	docker build -t $(IMG) .