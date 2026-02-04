# Projeto 02: Orquestração de Contêineres com Docker, Vagrant e Ansible

Este projeto foi desenvolvido como parte da disciplina de **Administração de Sistemas Abertos** do **IFPB - Campus João Pessoa**.

O objetivo é implementar um provisionamento de servidor totalmente via código, utilizando ferramentas como **Vagrant**, **Ansible** e **Docker**.

## 📋 Descrição do Projeto

O projeto segue o seguinte fluxo de execução:

1. **Vagrant**: Cria uma máquina virtual e chama o Ansible para executar o arquivo `playbook_ansible.yml`.
2. **Ansible**: Realiza configurações no sistema operacional e chama o `docker-compose.yml`.
3. **Docker**: Cria a infraestrutura de containers, configurando e expondo a aplicação Wordpress utilizando o Nginx como proxy.

### Arquitetura da Aplicação

A aplicação é composta por 3 contêineres em uma rede chamada `wordpress`:

* **database**: Utiliza a imagem oficial do MySQL com volume persistente `my`.
* **webserver**: Utiliza a imagem oficial do Wordpress com volume persistente `app`.
* **webproxy**: Imagem personalizada do Nginx configurada para **LoadBalance de Camada 4**.
    * Recebe requisições na porta **8080** e encaminha para o `webserver` na porta 80.
    * Possui as ferramentas `ping` e `curl` pré-instaladas.

---

## 🛠️ Pré-requisitos

* **Provider**: VirtualBox.
* **Box**: `debian/bookworm64`.
* **Configuração da VM**: 1024 MB de RAM e guest additions desabilitado.

---

## 📂 Estrutura de Arquivos

```text
.
├── nginx-personalizado/        # Arquivos para build da imagem Docker
│   ├── Dockerfile
│   └── nginx.conf              # Configuração de Proxy Reverso L4
├── docker-compose.yml          # Definição dos containers, redes e volumes
├── playbook_ansible.yml        # Automação do provisionamento e Docker
├── Vagrantfile                 # Configuração da VM e chamada do Ansible
└── README.md                   # Documentação do projeto
```

---

## 🚀 Como Executar

### Passo 1: Provisionamento da Infraestrutura
Na raiz do projeto, execute o comando para iniciar a máquina virtual e o provisionamento:

```bash
vagrant up
```

O Vagrant irá configurar a rede privada e o IP `192.168.56.148`. O Ansible instalará o Docker e subirá os serviços automaticamente.

### Passo 3: Acessando a Aplicação
Após o término, acesse no navegador:

**[http://192.168.56.148:8080]**


---

## ⚙️ Detalhes da Configuração

### Credenciais e Volumes
* **Banco de Dados (MySQL):** Imagem oficial configurada via variáveis de ambiente.
* **Persistência:** Utiliza volumes nomeados `app` e `my` para garantir que os dados não sejam perdidos.

---

## ✒️ Autores

* **Luis Henrike Marinho da Costa** - *20241380040*
* **Marcelino Marcelo do Nascimento Camilo** - *20241380018*
