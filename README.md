# Passado a passo

Execute o comando abaixo para copiar o arquivo de configuração do cluster para o diretório do `kubectl`
```bash
mv ~/Downloads/k8s-1-21-5-do-0-nyc1-1635587949189-kubeconfig.yaml ~/.kube/config
```


## Developer
Execute os comandos abaixo para implantar a aplicação no ambiente de `desenvolvimento`
```
kubectl create namespace dev
kubectl apply -f config/dev/ -n dev
kubectl apply -f k8s/ -n dev
```

## Stage
Execute os comandos abaixo para implantar a aplicação no ambiente de `stage`
```
kubectl create namespace stage
kubectl apply -f config/stage/ -n stage
kubectl apply -f k8s/ -n stage
kubectl get all -n stage
```

## Production
Execute os comandos abaixo para implantar a aplicação no ambiente de `production`
```
kubectl create namespace prod
kubectl apply -f config/prod/ -n prod
kubectl apply -f k8s/ -n prod
kubectl get all -n prod
```

## Info
Execute os comandos abaixo para verificar o `configmap` e a `secret` criada por namespace
```
 kubectl get configmap -n <namesapce>
 kubectl get secrets -n <namesapce>
```

Para acessar a aplicação abra a url abaixo no seu browser:
```
http://<your-external-ip>/swagger/index.html
```

# Anotações
## Config Map

```yaml
apiVersion:1
kind: ConfigMap
metadata:
    name: config-map-name
data:
    VAR_NAME:var_value
```

```bash
kubectl apply -f configmap-file.yaml
kubectl get configmap
kubectl describe configmap configmap-name
kubectl delete configmap configmap-name
```


Importando todo o config map para evitar a necessidade de declarar todas variáveis no manifesto

```yaml
envFrom:
    - configMapRef:
        name: config-map-name
```

Importando um variável individual do config map

```yaml
env:
- name: var__Name
  valueFrom:
     configMapKeyRef:
        key: VAR_NAME_FROM_CONFIG_MAP
        name: CONFIG_MAP_NAME
```

## Secrets

```yaml

apiVersion: v1
kind: Secret
metadata:
    name: secret-name
type: Opaque
data:
    SECRETE_NAME: SECRET_VALUE_BASE64
```
Para converter algum valor em base64 pelo terminal
```bash
$ echo -n "<some-value>" | base64
```

```bash
kubectl apply -f secrets-file.yaml
kubectl get secrets
kubectl describe secret configmap-name
```

Importando todas as secrets de uma vez.
```yaml
envFrom:
    - secretRef:
        name: secret-name
```
Importando secrets individualmente.
```yaml
env:
- name: var__Name
  valueFrom:
     secretKeyRef:
        key: VAR_NAME_FROM_SECRECT
        name: SECRECT_NAME
```

# Criando ConfigMap e Secrets por linha de commando

```shell
kubectl create configmap literal-configmap-name --from-literal=Mongo_Host=mongo-services
kubectl create configmap file-configmap --from-file=fila-name.yaml
kubectl create secret generic literal-secret-name --from-literal=MONGO_PWD=mongopwd
kubectl create secret generic file-secret --from-file=fila-name.yaml
```