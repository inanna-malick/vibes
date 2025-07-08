# Pattern: Kubernetes YAML Configuration

## VIBES Rating
`<🙈🧶🌊>`

## Description
Kubernetes YAML represents a perfect storm of poor LLM ergonomics: no conceptual framework, hidden dependencies across files, and runtime-only validation that cascades failures across clusters.

## Example
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: production  # String reference to another file
spec:
  replicas: 3
  selector:
    matchLabels:
      app: fronted  # Typo undetected until runtime
  template:
    spec:
      containers:
      - name: web
        image: myapp:latest  # Mutable reference
        env:
        - name: API_URL
          valueFrom:
            configMapKeyRef:
              name: app-config  # Reference to external ConfigMap
              key: api.url      # String key, fails if missing
```

## Why This Rating

### Expressive Power: 🙈
- Any string is valid YAML
- No conceptual model guides construction
- Schema validation optional and external
- Field names are magic strings

### Context Flow: 🧶
- Cross-file references via strings
- ConfigMaps, Secrets, Services all interconnected
- Namespace isolation is partial
- Order of creation matters but isn't enforced

### Error Surface: 🌊
- Typos deploy successfully then crash pods
- Missing references only found at runtime
- Cascading failures across deployments
- `kubectl apply` succeeds, pods crash loop

## Common Occurrence
- Kubernetes clusters everywhere
- Helm charts (YAML generating YAML!)
- GitOps pipelines
- Platform engineering

## Impact
- LLMs confidently generate invalid manifests
- No feedback until deployment
- Debugging requires cross-file correlation
- Simple tasks require extensive context

## Related Patterns
- [magic-string-soup.md](./magic-string-soup.md) - Similar string-based chaos
- [global-state-mutation.md](./global-state-mutation.md) - Similar cascading failures

## Consensus Data
- Model Agreement: 95%
- Disputed Aspects: Some models rate 🌀 for context due to circular service dependencies
- Test Date: July 2025
