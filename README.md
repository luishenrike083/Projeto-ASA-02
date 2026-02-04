# [cite_start]Projeto 02: Orquestração de Contêineres com Docker, Vagrant e Ansible [cite: 2]

[cite_start]Este projeto foi desenvolvido como parte da disciplina de **Administração de Sistemas Abertos** do **IFPB - Campus João Pessoa**[cite: 1, 25, 52].

[cite_start]O objetivo é implementar um provisionamento de servidor totalmente via código, utilizando ferramentas como **Vagrant**, **Ansible** e **Docker**[cite: 11].

## [cite_start]📋 Descrição do Projeto [cite: 10]

O projeto segue o seguinte fluxo de execução:
1. [cite_start]**Vagrant:** Cria uma máquina virtual e chama o Ansible para executar o arquivo `playbook_ansible.yml`[cite: 13].
2. [cite_start]**Ansible:** Realiza configurações no sistema operacional e chama o `docker-compose.yml`[cite: 14].
3. [cite_start]**Docker:** Cria a infraestrutura de containers, configurando e expondo a aplicação Wordpress utilizando o Nginx como proxy[cite: 15].

### [cite_start]Arquitetura da Aplicação [cite: 54]
[cite_start]A aplicação é composta por 3 contêineres em uma rede chamada `wordpress`[cite: 44, 50]:
* [cite_start]**`database`**: Utiliza a imagem oficial do MySQL com volume persistente `my`[cite: 47, 49].
* [cite_start]**`webserver`**: Utiliza a imagem oficial do Wordpress com volume persistente `app`[cite: 46, 48].
* [cite_start]**`webproxy`**: Imagem personalizada do Nginx configurada para **LoadBalance de Camada 4**[cite: 41, 45].
    * [cite_start]Recebe requisições na porta **8080** e encaminha para o `webserver` na porta 80[cite: 41, 54].
    * [cite_start]Possui as ferramentas `ping` e `curl` pré-instaladas[cite: 42].

---

## 🛠️ Pré-requisitos

* [cite_start]**Provider:** VirtualBox[cite: 19].
* [cite_start]**Box:** `debian/bookworm64`[cite: 20].
* [cite_start]**Configuração da VM:** 1024 MB de RAM e verificação de guest additions desabilitada[cite: 29, 30].

---

## 📂 Estrutura de Arquivos

.
├── nginx-personalizado/        # Arquivos para build da imagem Docker
│   ├── Dockerfile
│   └── nginx.conf              # Configuração de Proxy Reverso L4
[cite_start]├── docker-compose.yml          # Definição dos containers, redes e volumes [cite: 59]
[cite_start]├── playbook_ansible.yml        # Automação do provisionamento e Docker [cite: 58]
[cite_start]├── Vagrantfile                 # Configuração da VM e chamada do Ansible [cite: 57]
└── README.md                   # Documentação do projeto

---

## 🚀 Como Executar

### Passo 1: Configuração da Imagem Docker (Opcional)
[cite_start]*Nota: A imagem deve ser pública no DockerHub*[cite: 38].

1. Entre na pasta da imagem e faça o build/push:
   - `docker build -t luish083/nginx-personalizado:latest .`
   - `docker push luish083/nginx-personalizado:latest`

### Passo 2: Provisionamento da Infraestrutura
[cite_start]Na raiz do projeto, execute o comando para iniciar o processo[cite: 60]:

`vagrant up`

[cite_start]O Vagrant irá configurar o Hostname e o IP privado `192.168.56.1XY`[cite: 22, 23]. [cite_start]O Ansible então atualizará o SO e instalará o Docker/Docker-compose[cite: 33, 34].

### Passo 3: Acessando a Aplicação
[cite_start]Após o provisionamento, abra o navegador e acesse a URL da aplicação[cite: 60]:

**http://192.168.56.1XY:8080**

[cite_start]*(Substitua `XY` pelos últimos dígitos das matrículas dos integrantes)*[cite: 22].

---

## ⚙️ Detalhes da Configuração

### Credenciais e Volumes
* **Banco de Dados (MySQL):** Imagem oficial configurada via variáveis de ambiente.
* [cite_start]**Persistência:** Utiliza volumes nomeados `app` e `my` para garantir que os dados não sejam perdidos[cite: 48, 49].

---

## ✒️ Autores

* **Luis Henrike Marinho da Costa** - *Matrícula*
* **Marcelino Marcelo do Nascimento Camilo** - *Matrícula*
