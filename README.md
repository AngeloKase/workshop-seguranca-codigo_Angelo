# workshop-seguranca-codigo_2025-08
Conteúdos do Workshop "Implementando Segurança em Código na prática: de aplicações à infraestrutura com GitHub Actions!", realizado durante o Esquenta MVPConf em São Paulo-SP no dia 30/08/2025.

## Implementando Segurança em Código na prática: de aplicações à infraestrutura com GitHub Actions!

Aprenda neste workshop como implementar soluções para análise de código em aplicações e em scripts de infraestrutura na prática, automatizando a execução destas checagens com o GitHub Actions, Linux e diversas ferramentas open source!

A seguir estão as instruções passo a passo para a execução das atividades práticas.

---

Para os testes com o Job que será publicado no cluster Kubernetes utilizamos uma das APIs públicas catalogadas em:
https://github.com/public-apis/public-apis

Foi a API Bacon Ipsum 😂:
https://baconipsum.com/json-api/

Através do endpoint:
https://baconipsum.com/api/?type=meat-and-filler

Nosso cluster de testes foi criado via kind, um emulador de Kubernetes muito útil para testes:
https://kind.sigs.k8s.io/

---

### Corrigindo problemas com o arquivo YAML do Kubernetes

Versão do arquivo com problemas:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: exemplo-job
spec:
  template:
    spec:
      containers:
      - name: exemplo
        image: busybox
        command: ["echo", "Hello Kubernetes Job!"]
      restartPolicy: Never
```

Definir no arquivo YAML (**/.devops/job-teste.yaml**) a **configuraçallowPrivilegeEscalation = false**. Gravar as alterações e observar uma nova execução do workflow:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: exemplo-job
spec:
  template:
    spec:
      containers:
      - name: exemplo
        image: busybox
        command: ["echo", "Hello Kubernetes Job!"]
        securityContext:
          allowPrivilegeEscalation: false
      restartPolicy: Never
```

Referência sobre este tópico: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
