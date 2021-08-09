---
title: "Disallow Host Namespaces"
category: Pod Security Standards (Baseline)
version: 
subject: Pod
policyType: "validate"
description: >
    Host namespaces (Process ID namespace, Inter-Process Communication namespace, and network namespace) allow access to shared information and can be used to elevate privileges. Pods should not be allowed access to host namespaces. This policy ensures fields which make use of these host namespaces are set to `false`.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//pod-security/baseline/disallow-host-namespaces/disallow-host-namespaces.yaml" target="-blank">/pod-security/baseline/disallow-host-namespaces/disallow-host-namespaces.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-host-namespaces
  annotations:
    policies.kyverno.io/category: Pod Security Standards (Baseline)
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      Host namespaces (Process ID namespace, Inter-Process Communication namespace, and
      network namespace) allow access to shared information and can be used to elevate
      privileges. Pods should not be allowed access to host namespaces. This policy ensures
      fields which make use of these host namespaces are set to `false`.
spec:
  validationFailureAction: audit
  background: true
  rules:
    - name: host-namespaces
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: >-
          Sharing the host namespaces is disallowed. The fields spec.hostNetwork,
          spec.hostIPC, and spec.hostPID must not be set to true.
        pattern:
          spec:
            =(hostPID): "false"
            =(hostIPC): "false"
            =(hostNetwork): "false"

```
