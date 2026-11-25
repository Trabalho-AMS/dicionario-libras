# Proposta de Arquitetura

> Original: [PDF](pdf/proposta-de-arquitetura.pdf)

## 1. Visão geral
O sistema usa **três camadas** com frontend e backend separados. O frontend é uma **SPA** simples, organizada por
páginas. O backend é um **monólito modular**: roda como um único processo, mas é dividido em módulos independentes,
cada um dono das suas regras e dos seus dados.

![Visão geral da arquitetura](assets/arquitetura-visao-geral.png)

## 2. Justificativa
- **Microsserviços seriam complexos demais agora.** Para uma equipe pequena e um sistema desse tamanho, o custo de
  rede, deploy e transações distribuídas não compensa. Seguimos a recomendação de começar com um monólito.
- **Módulos isolados evitam o monólito bagunçado.** Um módulo só usa outro pela interface pública dele e nunca
  acessa tabelas alheias. Assim o sistema cresce sem virar um bloco único difícil de manter.
- **Eventos desacoplam os módulos.** Quando uma submissão é aprovada, um evento avisa os interessados (publicar o
  sinal, processar o vídeo, notificar e auditar), sem um módulo chamar todos os outros.
- **A SPA deixa a interface responsiva.** Player, gravação pela webcam e mapa funcionam sem recarregar a página, e a
  mesma API pode atender um app móvel no futuro.
- **Pronto para crescer aos poucos.** Se o processamento de vídeo virar gargalo, o módulo de mídia é extraído como
  microsserviço sem reescrever o resto, porque já funciona por eventos e com dados próprios.

## 3. Módulos do backend

| Módulo | Responsabilidade |
|---|---|
| `identidade` | Cadastro, login e perfis |
| `dicionario` | Verbetes, sinais, variações regionais e busca |
| `submissoes` | Envio de contribuições e curadoria |
| `midia` | Upload, validação e processamento de vídeo |
| `privacidade` | Consentimento e descarte de dados (LGPD) |
| `notificacoes` | E-mails ao usuário |
| `auditoria` | Registro das ações |

### Camadas de cada módulo

Cada módulo tem as mesmas quatro pastas, uma abaixo da outra:

```mermaid
flowchart TB
    http["<b>http</b><br/>Recebe as requisições da API<br/><i>SubmissaoController</i>"]
    aplicacao["<b>aplicacao</b><br/>Casos de uso: o que o sistema faz<br/><i>SubmissaoService</i>"]
    dominio["<b>dominio</b><br/>Regras de negócio e interfaces<br/><i>Submissao, SubmissaoRepository</i>"]
    infra["<b>infra</b><br/>Banco de dados e bucket de arquivos<br/><i>SubmissaoRepositoryJpa, S3ArmazenamentoDeArquivos</i>"]

    http ---|"chama o caso de uso"| aplicacao
    aplicacao ---|"usa as entidades e as interfaces"| dominio
    dominio ---|"interfaces implementadas por"| infra
```

| Pasta | O que significa | O que tem no módulo `submissoes` | Fala só com |
|---|---|---|---|
| `http` | Porta de entrada: recebe a requisição, valida o formato dos dados e devolve a resposta em JSON. Não tem regra de negócio. | `SubmissaoController`, `FormularioSubmissao` | `aplicacao` |
| `aplicacao` | Casos de uso: coordena os passos de cada operação (criar, listar, alterar, excluir), chamando o domínio. | `SubmissaoService`, `NovaSubmissao`, `SubmissaoResposta` | `http` (acima) e `dominio` (abaixo) |
| `dominio` | Coração do módulo: entidades e regras de negócio, e as interfaces do que ele precisa de fora (banco e arquivos). Não depende de nenhuma tecnologia. | `Submissao` (regras do vídeo e do consentimento), `SubmissaoRepository`, `ArmazenamentoDeArquivos` | `aplicacao` (acima) e `infra` (abaixo) |
| `infra` | Detalhes técnicos: implementa as interfaces do domínio com PostgreSQL (JPA) e o bucket S3. | `SubmissaoRepositoryJpa`, `S3ArmazenamentoDeArquivos` | `dominio` |

**Regra:** cada pasta fala **somente com a vizinha de cima ou de baixo**. O `http` nunca acessa o domínio
ou o banco direto, e a `aplicacao` não conhece o PostgreSQL nem o S3: ela usa as interfaces do domínio, e a
`infra` as implementa. Assim dá para trocar o banco ou o armazenamento sem mexer nas regras. A regra é
verificada automaticamente pelo teste `CamadasTest` do backend.

**Exemplo, enviar um sinal:** `SubmissaoController` recebe o formulário → `SubmissaoService.criar()` →
`Submissao` valida o vídeo e o consentimento → `SubmissaoRepository.salvar()` → `SubmissaoRepositoryJpa`
grava no PostgreSQL.

O pacote `compartilhado` fica fora dessa regra: tem só configuração comum (CORS, Swagger) e o tratamento de
erros usado por todos os módulos.

## 4. Frontend
SPA organizada por páginas — busca, verbete, enviar sinal, minhas submissões, curadoria e administração — com
alguns componentes compartilhados: player, gravador de webcam, mapa de variações e busca por parâmetros.
Toda comunicação com o backend passa por um único serviço de API.
