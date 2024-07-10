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

### vLLM

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
  - <ingress-ip>.nip.io #E.g.: 141.147.54.51.nip.io

```

Usage:
```bash
# Note by default the resource limit is set to 1 GPU
helm install mistral-7b-instruct oracle-ai-charts/vllm \
  -f values-override.yaml
```

### NIM

Create a Kubernetes secret for authentication to NGC.

```bash
kubectl create secret docker-registry regcred --docker-server=nvcr.io --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

Create the `values-override.yaml` file:

```yaml
deploy:
  imagePullSecrets:
  - name: regcred
  env:
  - name: NGC_API_KEY
    value: <ngc-token>

ingress:
  enabled: true
  className: "nginx"
  hosts:
  - <ingress-ip>.nip.io #E.g.: 141.147.54.51.nip.io

```

Usage:
```bash
# Note by default the resource limit is set to 1 GPU
helm install llm-nim oracle-ai-charts/nim \
  -f values-override.yaml
```
