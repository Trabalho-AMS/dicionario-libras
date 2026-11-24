# Expansão do Dicionário de Libras

Trabalho final da disciplina **AMS0002 – Análise e Modelagem de Sistemas**
(Prof.ª Carla Diacui Medeiros Berkenbrock).

O sistema expande o [Dicionário de Libras do INES](https://www.ines.gov.br/dicionario-de-libras/) para permitir
criar, alterar e excluir todos os campos de um verbete, registrar variações regionais de um mesmo sinal
(ex.: "Pai" no padrão nacional, no RJ e no RS) e enviar o vídeo do sinal. Toda contribuição passa por um
curador antes de ser publicada.

## Equipe

| Integrante | GitHub | Responsável por |
|---|---|---|
| Gabriel Lima | [@GabrielLima2803](https://github.com/GabrielLima2803) | Proposta de Arquitetura, repositório, documentação e código (em conjunto) |
| Eduardo Pasqualeto | [@edupasqualeto](https://github.com/edupasqualeto) | Engenharia de Requisitos, diagramas UML e código (em conjunto) |

O descritivo completo das atividades de cada membro está em [docs/05-equipe.md](docs/05-equipe.md).

## Apresentação

Roteiro da apresentação para a professora, seguindo os critérios de avaliação: **[apresentacao.md](apresentacao.md)**

## Como rodar o sistema completo (Docker)

O `docker-compose.yml` deste repositório sobe tudo de uma vez: banco PostgreSQL, bucket S3, backend e frontend.
Requer apenas o Docker. Clone os três repositórios lado a lado:

```bash
git clone https://github.com/Trabalho-AMS/dicionario-libras.git
git clone https://github.com/Trabalho-AMS/dicionario-libras-backend.git
git clone https://github.com/Trabalho-AMS/dicionario-libras-frontend.git

cd dicionario-libras
docker compose up --build
```

| Serviço | Endereço |
|---|---|
| Frontend | http://localhost:3000 |
| API | http://localhost:8080/api/status |
| Swagger | http://localhost:8080/swagger-ui.html |
| PostgreSQL | `localhost:5433` (`dicionario` / `dicionario`) |
| Bucket S3 | http://localhost:8333 |

O frontend (Nginx) repassa as chamadas de `/api` para o backend. Para parar: `docker compose down`;
para apagar também os dados: `docker compose down -v`.

> Se o banco e o bucket do `docker-compose.yml` do backend estiverem rodando, pare-os antes
> (`docker compose down` na pasta do backend), porque usam as mesmas portas.

## Sumário

1. [Enunciado do trabalho](docs/01-enunciado.md)
2. [Engenharia de Requisitos](docs/02-engenharia-de-requisitos.md)
   - Tipos de usuário e histórias
   - Requisitos funcionais e não-funcionais (com métricas)
   - Acessibilidade (WCAG 2.2 AA) e LGPD
   - Caso de uso UC01 – Submeter e aprovar novo sinal com vídeo
3. [Modelagem de Software (UML)](docs/03-modelagem/README.md)
   - Diagramas de Classes, Sequência e Atividades do fluxo de aprovação de um novo sinal
4. [Projeto de Software – Proposta de Arquitetura](docs/04-arquitetura.md)
5. [Equipe e atividades por membro](docs/05-equipe.md)

## Estrutura do repositório

```
.
├── README.md
├── apresentacao.md              # roteiro da apresentação
├── docker-compose.yml           # sobe o sistema completo
├── docs/
│   ├── 01-enunciado.md
│   ├── 02-engenharia-de-requisitos.md
│   ├── 03-modelagem/             # diagramas UML (classes, sequência, atividades)
│   ├── 04-arquitetura.md
│   ├── 05-equipe.md
│   ├── assets/                   # imagens e diagramas
│   └── pdf/                      # versões originais em PDF
└── .github/                      # templates de issue e pull request
```

## Código do protótipo

| Repositório | Stack |
|---|---|
| [dicionario-libras-backend](https://github.com/Trabalho-AMS/dicionario-libras-backend) | Java 17 + Spring Boot (monólito modular) |
| [dicionario-libras-frontend](https://github.com/Trabalho-AMS/dicionario-libras-frontend) | Vue 3 + TypeScript (SPA) |

## Documentos originais (PDF)

- [Enunciado do trabalho final](docs/pdf/enunciado-trabalho-final.pdf)
- [Engenharia de Requisitos](docs/pdf/engenharia-de-requisitos.pdf)
- [Proposta de Arquitetura](docs/pdf/proposta-de-arquitetura.pdf)
- [Diagramas UML](docs/pdf/diagramas-uml.pdf)

## Critérios de avaliação

| Etapa | Pontuação | Onde está | Situação |
|---|---|---|---|
| 1. Engenharia de Requisitos | 3,0 | [docs/02-engenharia-de-requisitos.md](docs/02-engenharia-de-requisitos.md) | ✅ Concluída |
| 2. Modelagem de Software | 2,0 | [docs/03-modelagem](docs/03-modelagem/README.md) | ✅ Concluída |
| 3. Projeto de Software | 1,0 | [docs/04-arquitetura.md](docs/04-arquitetura.md) | ✅ Concluída |
| 4. Repositório e Documentação | 1,0 | este README + [docs/05-equipe.md](docs/05-equipe.md) | ✅ Concluída |
| 5. Código em funcionamento | 2,0 | [backend](https://github.com/Trabalho-AMS/dicionario-libras-backend) e [frontend](https://github.com/Trabalho-AMS/dicionario-libras-frontend) | 🚧 Em andamento |
| 6. Participação em palestra | 1,0 | 24/11/2026 às 13:30 | ⏳ Agendada |
