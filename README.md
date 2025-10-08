graph TD
    subgraph "Ambiente Local"
        Dev[💻 Desenvolvedor]
    end

    subgraph "Plataforma de Versionamento"
        GitHub[🐙 GitHub Repo<br>(Branch 'main')]
    end

    subgraph "CI/CD"
        GHA[🤖 GitHub Actions]
        DockerHub[🐳 Docker Hub<br>(Registro de Imagens)]
    end

    subgraph "Servidor de Produção (VPS)"
        Nginx[🌐 Nginx<br>(Proxy Reverso)]
        
        subgraph Docker
            Compose[📄 Docker Compose]
            Web[🐍 Contêiner Web<br>(Django/Gunicorn)]
            DB[🐬 Contêiner DB<br>(MySQL)]
        end
    end
    
    User[👤 Usuário Final]

    %% Fluxo de Deploy
    Dev -- "1. git push" --> GitHub
    GitHub -- "2. Dispara o Workflow" --> GHA
    GHA -- "3. Constrói a Imagem (Dockerfile)" --> GHA
    GHA -- "4. Envia Imagem para o Registro" --> DockerHub
    GHA -- "5. Conecta via SSH e executa comandos" --> Compose

    %% Fluxo no Servidor
    Compose -- "6. Baixa a nova Imagem" --> DockerHub
    Compose -- "7. Orquestra os contêineres" --> Web & DB
    Web <--> DB

    %% Fluxo do Usuário
    User -- "Requisição HTTPS" --> Nginx
    Nginx -- "Serve Arquivos Estáticos/Mídia" ---> Nginx
    Nginx -- "Encaminha para a Aplicação" --> Web