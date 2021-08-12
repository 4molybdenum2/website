---
title: "Disallow Privileged Containers"
category: Pod Security Standards (Baseline)
version: 
subject: Pod
policyType: "validate"
description: >
    Privileged mode disables most security mechanisms and must not be allowed. This policy ensures Pods do not call for privileged mode.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//pod-security/baseline/disallow-privileged-containers/disallow-privileged-containers.yaml" target="-blank">/pod-security/baseline/disallow-privileged-containers/disallow-privileged-containers.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-privileged-containers
  annotations:
    policies.kyverno.io/category: Pod Security Standards (Baseline)
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      Privileged mode disables most security mechanisms and must not be allowed. This policy
      ensures Pods do not call for privileged mode.
spec:
  validationFailureAction: audit
  background: true
  rules:
    - name: priviledged-containers
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: >-
          Privileged mode is disallowed. The fields spec.containers[*].securityContext.privileged
          and spec.initContainers[*].securityContext.privileged must not be set to true.
        pattern:
          spec:
            =(initContainers):
              - =(securityContext):
                  =(privileged): "false"
            containers:
              - =(securityContext):
                  =(privileged): "false"

```
