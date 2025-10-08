# Pipeline de CI/CD para Aplicações Django com Docker, Nginx e GitHub Actions

Este repositório demonstra um pipeline de Integração Contínua e Entrega Contínua (CI/CD) completo para uma aplicação web Django. O objetivo é automatizar todo o processo de deploy: a cada `push` para a branch `main`, o GitHub Actions automaticamente constrói uma imagem Docker, envia para um registro e atualiza a aplicação em um servidor de produção (VPS) que roda por trás de um proxy reverso Nginx com HTTPS.

## Arquitetura do Pipeline

O diagrama abaixo ilustra o fluxo completo, desde o `push` do desenvolvedor até a atualização da aplicação no servidor para o usuário final.

```mermaid
graph TD
    subgraph "Seu Computador"
        A["💻<br>Desenvolvedor"]
    end

    subgraph "GitHub"
        B["🐙<br>Repositório<br>(Branch 'main')"] -- Dispara o Workflow --> C["🤖<br>GitHub Actions"]
    end

    subgraph "Registro de Imagem"
        D["🐳<br>Docker Hub"]
    end

    subgraph "Servidor de Produção (VPS)"
        E["🌐<br>Nginx<br>(Proxy Reverso)"] --> F["🐍<br>Contêiner Web<br>(Django/Gunicorn)"]
        F <--> G["🐬<br>Contêiner DB<br>(MySQL)"]
    end

    subgraph "Internet"
        H["👤<br>Usuário Final"]
    end

    A -- "1. git push" --> B
    C -- "2. Build & Push Image" --> D
    C -- "3. SSH & Deploy" --> E
    H -- "Requisição HTTPS" --> E