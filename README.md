# Objetivo
Este projeto tem como objetivo executar microserviços em kubernetes usando Rancher Descktop, controlado por GitOps com ArgoCD apartir de um repositorio publico
## Ferramentas
- Git
- GitHub
- Rancher Descketop
- Kubernetes
- ArgoCd
- Docker
- windows 10
# Passo a passo
**1. instalando o Rancher Descktop**
1) acesse https://rancherdesktop.io/
**2. Para este projeto foi necessario fazer um forck do repositório microservices-demo usando apenas o arquivo .yaml**
- realize o fork de https://github.com/GoogleCloudPlatform/microservices-demo
- Deixe apenas a pasta **release** com o documento kubernetes-manifests.yaml
- Renomeiea pasta release para **k8s** e o arquivo para **online-boutique.yaml**
**3. Instalando o argoCD**
   Para instalar o argocd é necessario utilizar o terminal Powershell
   e executar os seguintes comandos
   ```
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifest/install.yaml
   ```
   Em caso de erro status=Pending após o primeiro comando, espere, pois o kubertes pode estar iniciando os pods, use kubectl get pods -n argocd para ver o status, caso apareça 0/1 eles ainda não estão prontos
**4. Acessando argocd**
   1. para adquirir a senha de login do argoCD execute
```       [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String((kubectl
-n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"))) 
```
2. Para iniciar a interface do argoCD execute a porta https 433
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
3. acesse o localhost:8080 no seu navegador
- na sua tela aparecera uma mensagem de aviso
- clique em avançadas e em seguida em proseguir para localhost(não seguro)
- enquanto voce acessa a pagina aparecerão logs de conecxão no seu powershell
4. Aparecera a tela de login, para se conctar use a senha que foi dada pelo comando anterior
<img width="915" height="756" alt="image" src="https://github.com/user-attachments/assets/aa32cd8b-b3b2-4416-8f5c-fe1034d48ed2" />
Após preencher as configurações verifique a sincronização clicando no botão synchronize
Caso a aplicação esteja health volte ao prompt
execute `ctrol+C` para parar
execute
```
kubectl port-forward svc/frontend-external 8080:80
```
acesse `http://localhost:8080/`

# Conclusão
  Este projeto tem como objetivo de utilizar kubernesres para fazer deploy de forma automatizada gerenciando o vericionamento com a ferramenta Git.
