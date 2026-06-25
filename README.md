# Projeto Prático: Emulação de Redes locais com Containerlab e Docker

Este repositório apresenta o desenvolvimento prático de um ambiente de rede local emulado utilizando **Containerlab** e containers **Docker**. O laboratório consiste na criação de uma topologia ponto a ponto conectando dois nós Linux para testes de conectividade e monitoramento de pacotes de rede.

---

## 1. Topologia da Rede

A rede foi estruturada conectando duas máquinas de forma direta através de suas interfaces customizadas. A topologia segue o modelo abaixo:

* **Nó A (node-a):** Host Linux configurado com a imagem utilitária `nicolaka/netshoot`.
* **Nó B (node-b):** Host Linux configurado com a imagem utilitária `nicolaka/netshoot`.
* **Link:** Conexão direta interligando a interface `eth1` do `node-a` à interface `eth1` do `node-b`.

---

## 2. Arquivo de Configuração (lab.clab.yml)

O mapeamento da infraestrutura foi definido utilizando a sintaxe YAML aceita pelo Containerlab:

name: lab

topology:
  nodes:
    node-a:
      kind: linux
      image: nicolaka/netshoot:latest
    node-b:
      kind: linux
      image: nicolaka/netshoot:latest

  links:
    - endpoints: ["node-a:eth1", "node-b:eth1"]

---

## 3. Implementação e Resultados

### 3.1 Deploy da Topologia
A execução e inicialização do ambiente foram efetuadas na pasta do projeto por meio do comando de deploy do Containerlab. O gerenciador validou a sintaxe, provisionou a rede virtual interna do Docker e colocou os containers em execução, gerando a tabela de nós ativos.

<img width="1280" height="596" alt="print1" src="https://github.com/user-attachments/assets/3df3a18d-7950-4622-9289-2f679041f261" />

---

### 3.2 Configuração de Endereçamento IP
Com os nós ativos, as interfaces de comunicação ponto a ponto foram configuradas de forma manual com endereços pertencentes à mesma sub-rede IP (10.0.0.0/24).

* Comandos executados no Node-A:
  ip addr add 10.0.0.1/24 dev eth1
  ip addr show dev eth1

<img width="979" height="235" alt="print2" src="https://github.com/user-attachments/assets/5a88210d-0b9e-4bd8-8c10-a347525f81a6" />

* Comandos executados no Node-B:
  ip addr add 10.0.0.2/24 dev eth1
  ip addr show dev eth1

  <img width="978" height="238" alt="print3" src="https://github.com/user-attachments/assets/7d2226cf-e2f3-4246-89d7-2698d42a9640" />

---

### 3.3 Conectividade e Monitoramento de Tráfego
Para validar o canal de comunicação, foi iniciado o monitoramento de pacotes ICMP no **Node-A** utilizando o utilitário `tcpdump`. Simultaneamente, o **Node-B** efetuou o disparo de 5 pacotes de teste (`ping`) direcionados ao endereço do Node-A.

Os resultados obtidos comprovam o perfeito funcionamento da infraestrutura:
* O `ping` registrou **0% de perda de pacotes**, atestando o tráfego regular na camada de rede.
* O `tcpdump` interceptou perfeitamente as requisições (`ICMP echo request`) e as respostas (`ICMP echo reply`) associadas aos endereços MAC e IPs corretos de cada interface em tempo real.

<img width="1757" height="834" alt="print4" src="https://github.com/user-attachments/assets/a762f894-3741-41f2-b0ae-31c08fe7fc00" />

---

## 4. Conclusão

A atividade prática demonstrou a eficiência do Containerlab combinado ao ecossistema Docker para fins de estudo e homologação de redes de computadores. A abordagem baseada em containers permite simular cenários realistas de topologias locais com baixíssimo consumo de hardware, isolamento de processos e total controle das interfaces de comunicação.
