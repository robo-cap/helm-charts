# AI Helm Charts
A collection of helm charts for LLM inferencing using:
- vLLM
- NIM

## Enable the Helm repo
```bash
helm repo add oracle-ai-charts https://robo-cap.github.io/helm-charts/
helm repo update oracle-ai-charts
```

## Usage guides

### vLLM/NIM

Create the `values-override.yaml` file:

```yaml
deploy:
  env:
    - name: HF_TOKEN
      value: <token>
  extraArgs:
    - "--model"
    - "mistralai/Mistral-7B-Instruct-v0.3"
    - "--max-model-len"
    - "16384" #Required for the A10 shapes
    - "--api-key"
    - "my-secure-api-key"

ingress:
  enabled: true
  className: "nginx"
  hosts:
  - <lb-ip>.nip.io #E.g.: 141.147.54.51.nip.io

```

Usage:
```bash
# Note by default the resource limit is set to 1 GPU
helm install mistral-7b-instruct oracle-ai-charts/vllm \
  -f values-override.yaml
```
