
# Descrição

Este repositório contém a resolução de uma série de exercícios práticos realizados na plataforma Killercoda, abordando conceitos fundamentais do Kubernetes. As atividades incluem a criação de:
- Pods
- Deployments
- ConfigMaps
- Secrets
- PersistentVolumes
- Serviços de rede
- Jobs e Horizontal Pod Autoscalers

O objetivo é aplicar na prática as principais funcionalidades do Kubernetes para gerenciar e orquestrar aplicações em contêineres de forma eficiente.

# Tecnologias Utilizadas
- Kubernetes
- Killercoda
- kubectl
- Minikube
- NGINX
- BusyBox
- YAML
- Ferramentas de monitoramento e teste

## Atividade 1
Ao acessar a plataforma Killercoda, navegue até a área dedicada ao Kubernetes e faça as lições de cada sessão, seguindo as instruções apresentadas ao lado do terminal.

![Captura de tela 2025-01-13 004432](https://github.com/user-attachments/assets/cb2e626f-36c3-4e82-bf1b-c4c3da81acd5)



- O "Playground" é um ambiente interativo que oferece um terminal de acesso livre para você praticar, experimentar e testar seus conhecimentos em Kubernetes ou outros tópicos relacionados. 

### 1. Pod intro:
Criar um Pod um Pod chamado my-pod utilizando a imagem nginx:alpine.
- *Finalidade:* Um Pod é a menor unidade executável no Kubernetes, responsável por rodar um ou mais containers. Essa atividade demonstra como iniciar um container simples dentro de um cluster Kubernetes.

### 2. Deployment Basic:
Criar um Deployment chamado my-first-deployment usando a imagem nginx:alpine.
- *Finalidade:* Um Deployment gerencia a criação e o estado desejado de múltiplos Pods. Ele automatiza tarefas como escalonamento, atualizações e recuperação de falhas, garantindo alta disponibilidade das aplicações.

### 3. Kubernetes Dashboard:
Instalar e configurar o Kubernetes Dashboard para ser acessado via HTTP em todas as interfaces (0.0.0.0). Criamos um ServiceAccount com permissões administrativas e configuramos um port-forward na porta 9090.
- *Finalidade:* O Dashboard fornece uma interface gráfica que facilita o gerenciamento e monitoramento do cluster Kubernetes. Ele simplifica tarefas como visualização de recursos, execução de operações e análise de logs.

## Atividades práticas no terminal do Playground

- ### Criar um ConfigMap e usá-lo em um Pod
Questão: Crie um ConfigMap chamado "app-config" com uma variável de configuração personalizada. Monte o ConfigMap em um pod e verifique se o valor foi aplicado corretamente.


![Captura de tela 2025-01-13 013308](https://github.com/user-attachments/assets/703333d0-518b-4b34-8c9c-39638a18d171)

*Resumo:* Foi criado um ConfigMap chamado app-config para armazenar uma configuração personalizada. Esse ConfigMap foi aplicado no cluster e, em seguida, montado em um Pod chamado pod-with-config como uma variável de ambiente. O valor da variável foi verificado dentro do Pod utilizando o comando kubectl exec, confirmando que o ConfigMap foi aplicado corretamente.


- ### Criar um Secret
Questão: Crie um Secret chamado "app-secret" contendo informações sensíveis. Injete o Secret como uma variável de ambiente em um pod e teste se está acessível.

![image](https://github.com/user-attachments/assets/657cbc68-9b3a-46ab-a522-52afab25c9dd)

*Resumo:* Foi criado um Secret no Kubernetes chamado app-secret, contendo informações sensíveis como API_KEY e PASSWORD. Em seguida, configurou-se um Pod para injetar esses valores como variáveis de ambiente. Após iniciar o Pod, verificou-se dentro do container que as variáveis estavam acessíveis. Por fim, os recursos foram limpos.


- ### PersistentVolume (PV) e PersistentVolumeClaim (PVC)
Questão: Configure um PersistentVolume de 1Gi de armazenamento local e vincule-o a um PersistentVolumeClaim. Monte o volume em um pod e salve arquivos para verificar a persistência.

![image](https://github.com/user-attachments/assets/e0b6fa35-aa1e-433d-a7c0-d0263c895fd2)

*Resumo:* Foi criado um PersistentVolume (PV) de 1Gi com armazenamento local e vinculado a um PersistentVolumeClaim (PVC). Um Pod foi configurado para montar o volume e salvar arquivos. Após testes, confirmou-se que os dados persistem mesmo após a recriação do Pod.

- ### Criar um serviço ClusterIP
Questão: Crie um serviço do tipo ClusterIP para um Deployment chamado "backend" e teste a conectividade interna entre pods usando o nome do serviço.

![image](https://github.com/user-attachments/assets/9d18f2f6-c892-4589-8a43-f3075fdf63f0)

*Resumo:* Foi criado um serviço do tipo ClusterIP chamado "backend", que expõe um conjunto de Pods para comunicação interna dentro do cluster Kubernetes. O arquivo backend-service.yaml define o serviço com o seletor de Pods app: httpd e especifica que o serviço escuta na porta 80 (TCP). Após aplicar o arquivo com o comando kubectl apply -f backend-service.yaml, a conectividade foi testada usando kubectl get svc backend para verificar o serviço criado.

- ### Criar um Job
Questão: Implante um Job chamado "batch-job" que execute um comando simples e termine. Verifique os logs do Job para confirmar sua execução.

![image](https://github.com/user-attachments/assets/e9221795-81e2-4254-9a24-05c33f51990d)

![image](https://github.com/user-attachments/assets/f9555874-ae68-413d-a5c9-dab6c31e09d2)

*Resumo:* Foi criado um Job no Kubernetes para executar uma tarefa única com o comando echo "Hello from batch job!". O Job foi aplicado com sucesso, mas ao tentar visualizar os logs com kubectl logs job/batch-job, ocorreu um erro, pois o pod associado ao Job já havia terminado e foi removido. Para resolver, foi necessário usar o comando kubectl describe pod para verificar o status do pod e garantir que o Job tivesse sido concluído corretamente. Após a verificação, o Job foi recriado com sucesso, permitindo capturar novamente os logs.

- ### Horizontal Pod Autoscaler (HPA)
Questão: Crie um Horizontal Pod Autoscaler para um Deployment chamado "hpa-deployment" e configure-o para escalar com base no uso de CPU. Aumente a carga e observe o escalonamento.

![image](https://github.com/user-attachments/assets/68984f8a-8aee-406a-a51b-2683d819805f)

*Resumo:* Foi criado um Deployment chamado hpa-deployment, que executa um contêiner com um loop simples usando o busybox. Em seguida, foi configurado um Horizontal Pod Autoscaler (HPA) para escalar automaticamente o número de réplicas com base na utilização de CPU, ajustando o número de réplicas entre 1 e 5 quando a CPU atingir 50% de uso. Para testar o escalonamento, foi gerada carga de CPU dentro de um pod usando os comandos yes e dd, forçando o Kubernetes a aumentar o número de réplicas do Deployment.

- ### Serviço NodePort
Questão: Crie um serviço do tipo NodePort para expor externamente um Deployment chamado "webapp". Acesse o serviço usando o endereço IP do Minikube e a porta atribuída.

![image](https://github.com/user-attachments/assets/2982dbc2-73ea-4a8c-9a07-2d6ff45ecb56)

*Resumo:* Foi criado um Deployment chamado webapp com a imagem nginx e, em seguida, foi exposto externamente usando um serviço NodePort. O serviço foi configurado para mapear a porta 80 do container para a porta 32120 no nó, permitindo o acesso externo ao serviço. O acesso foi testado com sucesso usando o IP do nó e a porta do NodePort, retornando a página padrão do nginx, confirmando que o serviço está funcionando corretamente.

- ### Pod chamado "restart-pod"
Questão: Crie um pod chamado "restart-pod" com a política de reinício configurada como "OnFailure". Provoque uma falha no pod e observe seu comportamento.

![Captura de tela 2025-01-13 142601](https://github.com/user-attachments/assets/32e53e0e-fe1c-43a1-ab98-569997eac56e)

*Resumo:* Foi criado um pod chamado restart-pod com a política de reinício configurada como OnFailure. O contêiner foi configurado para falhar após imprimir a mensagem "Pod falhou", conforme esperado, e você pode observar esse comportamento ao monitorar o número de reinicializações.
