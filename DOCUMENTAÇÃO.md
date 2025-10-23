
---

### 📁 `documentacao.md`

```markdown
# 🚀 Pipeline de CI/CD para Aplicações Django com Docker, Nginx e GitHub Actions

Este repositório demonstra um pipeline completo de **Integração Contínua e Entrega Contínua (CI/CD)** para uma aplicação web Django. A cada `push` para a branch `main`, o GitHub Actions constrói uma imagem Docker, envia para um registro e atualiza a aplicação em um servidor de produção (VPS) com Nginx e HTTPS.

---

## 🛠️ Tecnologias Utilizadas

- **Backend:** Django, Django Rest Framework  
- **Servidor de Aplicação:** Gunicorn  
- **Banco de Dados:** MySQL 8.0  
- **Containerização:** Docker, Docker Compose  
- **Servidor Web / Proxy Reverso:** Nginx  
- **CI/CD:** GitHub Actions  
- **Certificado SSL:** Certbot (Let's Encrypt)  
- **Servidor:** VPS com Ubuntu  

---

## ✅ Pré-requisitos

Para replicar este projeto, você precisará de:

- Um servidor (VPS) com acesso root/sudo e pelo menos 2GB de RAM  
- Um nome de domínio apontando para o IP do seu servidor  
- Uma conta no GitHub  
- Uma conta no Docker Hub  

---

## 📋 Guia de Configuração Passo a Passo

### 🔐 Fase 1: Preparação do Servidor

- Criação de usuários com privilégios adequados  
- Instalação de Docker, Docker Compose, Nginx e Certbot  
- Configuração de permissões para o usuário `deployer`  
- Autenticação via chave SSH  
- Configuração do Nginx como proxy reverso  
- Geração de certificado SSL com Certbot  

### ⚙️ Fase 2: Configuração do Projeto Django

- Separação de settings em `base.py`, `development.py` e `production.py`  
- Uso de variáveis de ambiente com `os.getenv()` e `python-dotenv`  
- Configuração de arquivos estáticos e de mídia  

### 🐳 Fase 3: Containerização com Docker

- Dockerfile com instalação de dependências e `collectstatic`  
- `docker-compose.yml` com serviços `web` e `db`  
- Script `mysql.txt` para inicialização do banco  

### 🔄 Fase 4: Pipeline de CI/CD com GitHub Actions

- Armazenamento de segredos no GitHub  
- Workflow com gatilho em `push` na branch `main`  
- Build da imagem Docker e push para Docker Hub  
- Deploy via SSH com criação de arquivos, atualização de contêineres e execução de comandos Django  

---

## 🚀 Como Usar

Após configurar tudo, basta executar:

```bash
git push origin main

---

## 📚 Lições Aprendidas e Problemas Comuns
Caminhos inconsistentes: Padronize STATIC_URL, volumes e aliases do Nginx para evitar conflitos.

Banco de Dados não pronto: Use um loop de espera inteligente em vez de sleep para garantir que o MySQL esteja disponível antes de executar migrate.

Diferenças entre Dev e Prod: Configure corretamente DJANGO_SETTINGS_MODULE nos comandos de produção para evitar erros.

Sensibilidade de nomes de arquivos: Sistemas Linux são sensíveis a maiúsculas/minúsculas, diferente do Windows. Atenção ao nome dos arquivos.

Permissões de escrita: Corrija permissões com sudo chown para garantir que o contêiner tenha acesso aos diretórios de mídia.

Dependências e versões: Mantenha o requirements.txt atualizado e compatível com a versão do Python usada no Dockerfile.

Recursos do servidor: VPS com pouca memória pode travar durante o deploy. Considere fazer upgrade para planos com mais RAM.
