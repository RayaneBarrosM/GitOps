# 🚀 GitOps com ArgoCD e Rancher Desktop

## Objetivo
Este projetodemonstra a execução de microserviços em um cluster Kubernetes local (Rancher Desktop), controlado por GitOps através do ArgoCD, utilizando um repositorio publico.

## Ferramentas
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Rancher](https://img.shields.io/badge/Rancher-0075A8?style=for-the-badge&logo=rancher&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Windows 10](https://img.shields.io/badge/Windows_10-0078D6?style=for-the-badge&logo=windows&logoColor=white)
  
# 🎯Passo a passo 
 📑 **Sumario**
1. [Instalando o Rancher Desktop](#1-instalando-o-rancher-desktop)
2. [Preparação do Repositório Git](#2-preparação-do-repositório-git)
3. [Instalando o ArgoCD no Kubernetes](#3-instalando-o-argocd-no-kubernetes)
4. [Acessando a Interface Web do ArgoCD](#4-acessando-a-interface-web-do-argocd)
5. [Configuração da Aplicação no ArgoCD](#5-configuração-da-aplicação-no-argocd)
6. [Acessando o Front-end da Aplicação](#6-acessando-o-front-end-da-aplicação)
7. [Conclusão](#7-conclusão)
   
## 1. Instalando o Rancher Descktop
1) acesse https://rancherdesktop.io/
## 2. Preparação do Repositório Git

1) realize o fork de https://github.com/GoogleCloudPlatform/microservices-demo
2) Deixe apenas a pasta **release** com o documento **kubernetes-manifests.yaml**
3) Renomeiea pasta release para **k8s** e o arquivo para **online-boutique.yaml**
   
```
gitops-microservices/ 
        └── k8s/ 
             └── online-boutique.yaml
```
   
## 3. Instalando o argoCD no Kubernetes
Para instalar o argocd é necessario utilizar o terminal Powershell e executar os seguintes comandos
```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifest/install.yaml
```
   Em caso de erro status=Pending após o primeiro comando, espere, pois o kubertes pode estar iniciando os pods, use kubectl get pods -n argocd para ver o status, caso apareça 0/1 eles ainda não estão prontos
   
  <img width="891" height="34" alt="image" src="https://github.com/user-attachments/assets/56f2af48-c9a9-4355-ae9b-5773053ceae1" />

## 4 Acessando a Interface Web do ArgoCD
1) Para adquirir a senha de login do argoCD execute
```       [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String((kubectl
-n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"))) 
```
<img width="938" height="64" alt="image" src="https://github.com/user-attachments/assets/d4aaa9ae-10b4-4488-9813-2c5eac3df9af" />

2) Para iniciar a interface do argoCD execute a porta https 433
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
3) Acesse o `localhost:8080` no seu navegador
- na sua tela aparecera uma mensagem de aviso
4) Clique em avançadas e em seguida em proseguir para localhost(não seguro)
- enquanto voce acessa a pagina aparecerão logs de conecxão no seu powershell
  
  <img width="677" height="708" alt="image" src="https://github.com/user-attachments/assets/00c3b573-ba29-4cfb-97fe-4f7f469b1c44" />
  
5) Na tela de login use a senha que foi dada pelo comando anterior

<img width="915" height="756" alt="image" src="https://github.com/user-attachments/assets/aa32cd8b-b3b2-4416-8f5c-fe1034d48ed2" />

## 5. Configuração da Aplicação no ArgoCD
1) Preencha os campos de criação da aplicação conforme as imagens
   
**Dicas para a criação:**
- Não utilize o nomes com letras maiúsculas
- Caso não queira utilizar o projectname como `default` crie antes de começar a configurar a aplicação
  
<img width="768" height="846" alt="image" src="https://github.com/user-attachments/assets/bcc231d2-b5a8-4344-8eb2-3d20ce2c02ee" />
<img width="780" height="742" alt="image" src="https://github.com/user-attachments/assets/17f4df63-9480-44ac-b177-af598ea82469" />

2) Clique em create no topo da tela
3) Após criar verifique a sincronização clicando no botão synchronize

### Erro CrashLoopBack
<img width="567" height="329" alt="image" src="https://github.com/user-attachments/assets/d4a57a95-8aee-4956-a42b-e2aea2af7acb" />

- Caso o status de sincronização fique Degraded abra outro terminal e verifique os pods com `kubectl get pods -n default`

<img width="664" height="224" alt="image" src="https://github.com/user-attachments/assets/67ff6ade-bd1b-4d71-bccf-208d7322e37a" />

❗ Para forçar a sincronização delete os pods problematicos e serão criados outros automaticamente

## 6. Acessando o Front-end da Aplicação
1. Execute `ctrol+C` para parar
2. execute o comando abaixo para adicionar o front-end a porta http 80
```
kubectl port-forward svc/frontend-external 8080:80
```
3. Acesse `http://localhost:8080/`
   
<img src="https://github.com/user-attachments/assets/158e0732-7a72-4e9b-9b5e-2857ed7ec8c6" alt="kubectl get pods status" style="display: block; margin: 0 auto; width: 400px;">


# Conclusão
  Este projeto foi desenvolvido como parte do Programa de Estágio da Compass UOL com o foco no aprendizado do GitOPs no deployment de microserviços
