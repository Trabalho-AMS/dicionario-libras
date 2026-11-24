# Equipe e atividades por membro

> Exigido pelas diretrizes finais ("o documento entregue no Moodle deve incluir um descritivo mencionando as
> atividades realizadas por cada membro da equipe") e pelo item 4 dos critérios de avaliação.

## Integrantes

| Integrante | GitHub |
|---|---|
| Gabriel Lima | [@GabrielLima2803](https://github.com/GabrielLima2803) |
| Eduardo Pasqualeto | [@edupasqualeto](https://github.com/edupasqualeto) |

## Responsável por etapa

| Etapa do trabalho | Responsável | Entregável |
|---|---|---|
| 1. Engenharia de Requisitos | Eduardo Pasqualeto | [02-engenharia-de-requisitos.md](02-engenharia-de-requisitos.md) |
| 2. Modelagem de Software (UML) | Eduardo Pasqualeto | [03-modelagem/](03-modelagem/README.md) |
| 3. Projeto de Software (arquitetura) | Gabriel Lima | [04-arquitetura.md](04-arquitetura.md) |
| 4. Repositório e documentação | Gabriel Lima | [README.md](../README.md) e esta página |
| 5. Código em funcionamento (protótipo) | Gabriel Lima e Eduardo Pasqualeto (em conjunto) | [backend](https://github.com/Trabalho-AMS/dicionario-libras-backend) e [frontend](https://github.com/Trabalho-AMS/dicionario-libras-frontend) |

## Descritivo de atividades

### Gabriel Lima
- **Proposta de Arquitetura:** escolha do padrão arquitetural (três camadas, SPA no frontend e monólito modular
  no backend) e justificativa em relação a microsserviços; definição dos sete módulos do backend (identidade,
  dicionário, submissões, mídia, privacidade, notificações e auditoria), das camadas internas de cada módulo
  e da comunicação entre módulos por eventos; organização do frontend por páginas e componentes compartilhados.
- **Repositório e documentação:** criação da organização [Trabalho-AMS](https://github.com/Trabalho-AMS) e deste
  repositório; estruturação do README e da pasta `docs/`; conversão dos documentos PDF para Markdown;
  templates de issue e pull request.
- **Código em funcionamento (em conjunto com Eduardo):** implementação do protótipo do fluxo de submissão e
  aprovação de um novo sinal, no backend em Java + Spring Boot e no frontend em Vue 3 + TypeScript.

### Eduardo Pasqualeto
- **Engenharia de Requisitos:** levantamento dos tipos de usuário e das histórias de usuário; requisitos
  funcionais (RF01–RF10) e diferenciais (RF11–RF14); requisitos não-funcionais com métricas (RNF01–RNF07);
  conformidade com a WCAG 2.2 AA e com a LGPD (dados sensíveis, consentimento e descarte); caso de uso UC01 –
  Submeter e aprovar novo sinal com vídeo.
- **Modelagem de Software (UML):** diagramas de Classes, Sequência e Atividades do fluxo de aprovação de um novo
  sinal com vídeo.
- **Código em funcionamento (em conjunto com Gabriel):** implementação do protótipo do fluxo de submissão e
  aprovação de um novo sinal, no backend em Java + Spring Boot e no frontend em Vue 3 + TypeScript.
