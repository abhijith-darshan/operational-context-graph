# SPDX-FileCopyrightText: 2026 SAP SE or an SAP affiliate company and Operational Context Graph contributors
#
# SPDX-License-Identifier: Apache-2.0

# syntax=docker/dockerfile:1

FROM node:24-slim AS builder
WORKDIR /app

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable

COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

COPY . .
RUN CI=true cd services/ocg && pnpm build

FROM node:24-slim AS deps
WORKDIR /app

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable

COPY --from=builder /app/package.json /app/pnpm-lock.yaml ./
COPY --from=builder /app/pnpm-workspace.yaml ./
COPY --from=builder /app/services/ocg/package.json ./services/ocg/
COPY --from=builder /app/services/ocg/node_modules ./services/ocg/node_modules
RUN CI=true pnpm --filter ./services/ocg deploy --prod /prod

FROM gcr.io/distroless/nodejs24-debian12:nonroot@sha256:14d42e2511532589a7c7e01a753667a74fcc96266e137e8125006b87b0c32d0a
WORKDIR /app

COPY --from=builder /app/services/ocg/dist ./dist
COPY --from=deps /prod/node_modules ./node_modules

EXPOSE 3000
USER nonroot:nonroot
CMD ["dist/main"]