---
title: "Check deprecated APIs"
category: Best Practices
version: 
subject: Kubernetes APIs
policyType: "validate"
description: >
    Kubernetes APIs are sometimes deprecated and removed after a few releases. As a best practice, older API versions should be replaced with newer versions. This policy validates for APIs that are deprecated or scheduled for removal. Note that checking for some of these resources may require modifying the Kyverno ConfigMap to remove filters.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/check_deprecated_apis.yaml" target="-blank">/best-practices/check_deprecated_apis.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: check-deprecated-apis
  annotations:
    policies.kyverno.io/title: Check deprecated APIs
    policies.kyverno.io/category: Best Practices
    policies.kyverno.io/subject: Kubernetes APIs
    policies.kyverno.io/description: >-
      Kubernetes APIs are sometimes deprecated and removed after a few releases.
      As a best practice, older API versions should be replaced with newer versions.
      This policy validates for APIs that are deprecated or scheduled for removal.
      Note that checking for some of these resources may require modifying the Kyverno
      ConfigMap to remove filters.
spec:
  validationFailureAction: audit
  background: true
  rules:
  - name: validate-v1-22-removals
    match:
      resources:
        kinds:
        - admissionregistration.k8s.io/v1beta1/ValidatingWebhookConfiguration
        - admissionregistration.k8s.io/v1beta1/MutatingWebhookConfiguration
        - apiextensions.k8s.io/v1beta1/CustomResourceDefinition
        - apiregistration.k8s.io/v1beta1/APIService
        - authentication.k8s.io/v1beta1/TokenReview
        - authorization.k8s.io/v1beta1/SubjectAccessReview
        - authorization.k8s.io/v1beta1/LocalSubjectAccessReview
        - authorization.k8s.io/v1beta1/SelfSubjectAccessReview
        - certificates.k8s.io/v1beta1/CertificateSigningRequest
        - coordination.k8s.io/v1beta1/Lease
        - extensions/v1beta1/Ingress
        - networking.k8s.io/v1beta1/Ingress
        - networking.k8s.io/v1beta1/IngressClass
        - rbac.authorization.k8s.io/v1beta1/ClusterRole
        - rbac.authorization.k8s.io/v1beta1/ClusterRoleBinding
        - rbac.authorization.k8s.io/v1beta1/Role
        - rbac.authorization.k8s.io/v1beta1/RoleBinding
        - scheduling.k8s.io/v1beta1/PriorityClass
        - storage.k8s.io/v1beta1/CSIDriver
        - storage.k8s.io/v1beta1/CSINode
        - storage.k8s.io/v1beta1/StorageClass
        - storage.k8s.io/v1beta1/VolumeAttachment
    validate:
      message: >-
        {{ request.object.apiVersion }}/{{ request.object.kind }} is deprecated and will be removed in v1.22.
        See: https://kubernetes.io/docs/reference/using-api/deprecation-guide/
      deny: {}
  - name: validate-v1-25-removals
    match:
      resources:
        kinds:
        - batch/v1beta1/CronJob
        - discovery.k8s.io/v1beta1/EndpointSlice
        - events.k8s.io/v1beta1/Event
        - policy/v1beta1/PodDisruptionBudget
        - policy/v1beta1/PodSecurityPolicy
        - node.k8s.io/v1beta1/RuntimeClass
    validate:
      message: >-
        {{ request.object.apiVersion }}/{{ request.object.kind }} is deprecated and will be removed in v1.25.
        See: https://kubernetes.io/docs/reference/using-api/deprecation-guide/
      deny: {}

```
