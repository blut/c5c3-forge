# CobaltCore Forge

Prototyping ground for CobaltCore.

## Overview

CobaltCore (C5C3) is a Kubernetes-native OpenStack distribution for operating Hosted Control Planes across
a multi-cluster topology (Management, Control Plane, Hypervisor, Storage). This repository delivers everything
needed for a fully self-contained Keystone deployment stack — from infrastructure deployment manifests through
the Keystone Operator to the c5c3-operator orchestration layer — built with Operator SDK (Go), controller-runtime,
and Kubebuilder.

The implementation follows a Keystone-first strategy: the Keystone Operator serves as the reference implementation
establishing patterns for all subsequent operators. The c5c3-operator is implemented with Keystone-only orchestration,
ready to be extended for additional services later.

The architecture is organized as a Go Workspace monorepo with a shared library (`internal/common/`), individual
operator modules (`operators/keystone/`, `operators/c5c3/`), container image builds (`images/`), declarative
infrastructure deployment manifests (`deploy/`), and comprehensive tests at every level (unit, envtest integration,
Chainsaw E2E).

## Roadmap

The full implementation plan is documented in [`.planwerk/PLAN.md`](.planwerk/PLAN.md) and spans 11 phases:

1. **Project Foundation & Test Infrastructure** — Go Workspace monorepo, build system, test harnesses, and CI pipeline
2. **Shared Library Packages** — Common types, conditions, config rendering, and Kubernetes helpers in `internal/common/`
3. **Keystone Container Image Build Pipeline** — Multi-stage Docker builds for the Keystone service image
4. **Infrastructure Deployment Stack** — FluxCD HelmReleases, OpenBao HA, ESO integration, MariaDB & Memcached
5. **Keystone CRD & Webhooks** — API type definitions with Kubebuilder markers, defaulting, and validation
6. **Keystone Reconciler** — Sequential sub-reconciler pattern with condition progression
7. **Keystone Dependencies & E2E Tests** — Detailed dependency interactions and Chainsaw E2E test suite
8. **Keystone Operator Packaging** — Operator image, Helm chart, and complete CI pipeline
9. **c5c3-operator** — ControlPlane CRD with Keystone-only orchestration and auxiliary CRDs
10. **End-to-End Integration Hardening** — Full-stack validation, failure recovery, and stress tests
11. **Release Preparation** — Release workflow, documentation, and CI gate validation
