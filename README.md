# Desafio: Gerenciamento de Instâncias EC2 na AWS

Este repositório documenta a prática e os aprendizados adquiridos no desafio do bootcamp sobre Gerenciamento de Instâncias EC2 na AWS.

O objetivo foi aplicar os conceitos de provisionamento, escalabilidade e gerenciamento de servidores virtuais em um cenário prático: **uma arquitetura para um sistema de hotelaria**.

## 1. Arquitetura Proposta: Sistema de Hotelaria

Para consolidar os conhecimentos, desenhei uma arquitetura escalável e de alta disponibilidade, focada no gerenciamento de instâncias EC2. A arquitetura garante que o sistema de hotelaria (para reservas, painel administrativo, etc.) possa lidar com falhas e picos de demanda.

<img width="649" height="629" alt="image" src="https://github.com/user-attachments/assets/e2224192-c331-47ab-a817-1a42eb85e53d" />


### Componentes da Arquitetura

Cada peça do diagrama tem um papel crucial no gerenciamento das instâncias EC2:

* **Usuário (Administrador do Hotel):** O usuário que acessa a aplicação. Ele não precisa saber quantos servidores existem; ele apenas acessa um link (URL) único.

* **Elastic Load Balancer (ELB):**
    * **Função:** Atua como a "porta de entrada" principal do sistema.
    * **Por que usar?** Ele recebe 100% do tráfego do usuário e o distribui de forma inteligente para as instâncias EC2 que estão saudáveis. Se uma instância falhar, o ELB para de enviar tráfego para ela automaticamente, garantindo que o usuário não perceba a falha.

* **Auto Scaling Group (ASG):**
    * **Função:** É o "cérebro" do gerenciamento das EC2.
    * **Por que usar?** Este é o componente-chave. Ele garante que o número desejado de instâncias EC2 esteja sempre rodando.
        * **Alta Disponibilidade:** Se uma instância (como a "Instância EC2") falhar, o ASG detecta a falha e automaticamente sobe uma nova (como a "Instância EC2 Reserva") para substituí-la.
        * **Escalabilidade:** Em dias de alta demanda (ex: Black Friday de reservas), podemos configurar o ASG para *adicionar* mais instâncias EC2 automaticamente. Quando a demanda cair, ele as *remove* para economizar custos.

* **Instâncias EC2 (Instância EC2 e Instância EC2 Reserva):**
    * **Função:** São os servidores virtuais que rodam a aplicação do sistema de hotelaria (o código em Python, C#, Node.js, etc.).
    * **Gerenciamento:** Elas são gerenciadas pelo ASG. No diagrama, ter duas instâncias já garante que o sistema não tenha um ponto único de falha.

* **Banco de Dados Postgree (RDS):**
    * **Função:** Armazena todos os dados persistentes (reservas, quartos, clientes, disponibilidade).
    * **Por que usar?** É crucial que todas as instâncias EC2 leiam e escrevam no *mesmo* banco de dados centralizado. Isso garante que, não importa qual servidor EC2 responda ao usuário, os dados serão sempre consistentes.

## 2. Anotações e Insights (Aprendizados do Desafio)

Documentar este processo gerou vários insights importantes:

1.  **EC2 não é só "ligar um servidor":** O verdadeiro poder do EC2 está no ecossistema ao seu redor. Usar uma EC2 sozinha é arriscado (ponto único de falha). O "gerenciamento" real vem do uso conjunto com **ELB** e **ASG**.

2.  **Diferença de Serviços:** Ficou claro o papel de cada serviço:
    * **EC2:** Para poder de computação e processamento (rodar o código).
    * **RDS:** Para dados estruturados (tabelas, linhas, colunas).
    * 
## 3. Conclusão

Este desafio foi fundamental para entender que "gerenciamento de instâncias" na nuvem não é sobre cuidar de servidores individuais, mas sobre projetar um sistema automatizado (com ASG) e resiliente (com ELB e múltiplas instâncias) que se gerencia sozinho.
