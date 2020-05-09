# Ambiente de Desenvolvimento LEMP com Containers Bitnami

---

### Introdução

Muitos desenvolvedores e administratores de sistemas, estão migrando para os [Containers Docker](https://www.docker.com/what-docker), porque eles são portáveis e fáceis de manter e usar. Usando o mesmo ambiente Docker, os desenvolvedores e administradores podem garantir que não haja alteração no ambiente, mantendo a configuração e compatibilidade original. Além disso os containers Docker requerem menos manutenção e recursos de servidor, quando comparado com máquinas físicas ou máquinas virtuais.

Este guia, cobre a instalação de um ambiente completo baseado em **LEMP** (Linux, Nginx, MariaDB e PHP). Permitindo, que se possa ter um ambiente de desenvolvimento, sem a necessidade de instalação de pacotes adicionais no computador. O intuito é obter um ambiente seguro, consistente e atualizado, que seja fácil de reproduzir e compartilhar.

### Pré-requisitos

Este guia combina quatro imagens Bitnami, para executar uma aplicação PHP com suporte a banco de dados:

- [Bitnami’s MariaDB](https://github.com/bitnami/bitnami-docker-mariadb) - Vai fornecer o banco de dados.
- [Bitnami’s PHP-FPM](https://github.com/bitnami/bitnami-docker-php-fpm) - Vai permitir executar o PHP.
- [Bitnami’s NGINX](https://github.com/bitnami/bitnami-docker-nginx) - Vai servir as páginas da aplicação.
- [Bitnami’s PHPMyAdmin](https://hub.docker.com/r/bitnami/phpmyadmin) - Vai permitir administrar o banco de dados.

Este guida pressume, que o ambiente seja Linux e haja um [Ambiente Docker](https://www.docker.com/) com [Docker Compose](https://docs.docker.com/compose/) instalado.

### Configurando o ambiente

- Clone o repositório no servidor.

  > git clone git@github.com:FaBMak/docker-lemp.git meuapp.com

- Inclua **meuapp.com** no hosts do computador.

  > echo '127.0.0.1 meuapp.com' | sudo tee -a /etc/hosts

  **OBSERVAÇÃO: Não necessário, se seu domínio já esteja configurado, no computador.**

- Acesse o diretório do repositório clonado.

  > cd meuapp.com

- Criar os conteiners com o docker-compose, pode demorar um pouco, na primeira vez.

  > docker-compose up -d

### Acessando o ambiente

- Abrindo o navegador no endereço, _http://meuapp.com_, será possível ver a página do PHPInfo.

- Abrindo o navegador no endereço, _http://meuapp.com:8080_, será possível ver a página do PHPMyAdmin.

### Personalização

- Os arquivos que serão servidos/executados pelo _PHP_, devem ficar dentro do diretório _meuapp.com/app_.

### Persistência

O container usa o conceito de **persistência**, através de **volumes**. Sendo assim, no caso de necessidade, de parar ou recrair a um container, os dados são persistidos, desde que seja usado o comando _"docker-compose up -d"_.

**Não realizar o docker system prune, antes de fazer o backup do banco de dados.**

## Backup/Restore do MariaDB

### Backup

> docker exec mariadb-container mysqldump --opt -u root --password=bitnami --databases NOMEDOBANCO > NOMEDOBANCO.sql

### Restore

> cat app.sql | docker exec -i mariadb-container mysql -u root --password=bitnami

## Acesso ao Servidor MariaDB

> docker exec -it mariadb-container mysql -u root --password=bitnami
