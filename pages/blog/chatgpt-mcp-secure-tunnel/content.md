# Conectando um MCP local ao ChatGPT com o Secure MCP Tunnel

> **⚠️ Nota sobre o escopo**
>
> Este guia apresenta um setup voltado principalmente para **uso pessoal e desenvolvimento local**. Ao transformar uma integração MCP em um produto para terceiros, é mais adequado considerar uma arquitetura própria de autenticação e autorização, como **OAuth**, em vez de simplesmente reproduzir este setup local.
>
> O `tunnel-client` também contempla runtimes gerenciados e cenários de infraestrutura, mas **não aprofundei essas possibilidades neste artigo**. Portanto, não estou propondo aqui uma arquitetura específica para produção, AWS ou Bedrock — esta é apenas uma observação baseada no funcionamento e na documentação do projeto.

> **Objetivo:** executar um servidor MCP na sua própria máquina e permitir que o ChatGPT o utilize sem expor o servidor MCP diretamente à Internet.

O [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) permite que aplicações de IA utilizem ferramentas e recursos externos.

O **Secure MCP Tunnel**, disponibilizado pela OpenAI, permite conectar um MCP que continua rodando localmente ao ChatGPT. O `tunnel-client` mantém uma conexão de saída com a infraestrutura da OpenAI e encaminha as chamadas recebidas para o processo MCP local.

```text
ChatGPT
   │
   ▼
OpenAI Secure MCP Tunnel
   │
   │ conexão de saída
   ▼
tunnel-client
   │
   │ stdio
   ▼
MCP local
````

Assim, não é necessário expor uma porta do computador à Internet ou utilizar serviços como ngrok.

> **Nota:** este guia apresenta o processo utilizando **Linux**. O conceito é o mesmo em outros sistemas, mas os comandos de instalação e configuração podem variar.

---

## 1. Pré-requisitos

Você precisará de:

* ChatGPT com **Developer Mode** habilitado;
* acesso ao OpenAI Platform;
* uma máquina Linux;
* um servidor MCP compatível com `stdio`;
* `tunnel-client`.

O Developer Mode permite criar e utilizar aplicativos MCP personalizados no ChatGPT.

Para habilitá-lo, acesse:

**ChatGPT → Settings → Apps → Advanced Settings → Developer Mode**

A disponibilidade do recurso pode depender do plano ou das configurações do workspace.

---

## 2. Criando o Tunnel

Acesse:

[OpenAI Platform — Tunnels](https://platform.openai.com/settings/organization/tunnels)

Crie um novo **Tunnel** e copie seu **Tunnel ID**.

Ele terá um formato semelhante a:

```text
tunnel_<YOUR_TUNNEL_ID>
```

O Tunnel ID não é uma credencial secreta.

---

## 3. Criando uma API Key

Acesse:

[OpenAI Platform — API Keys](https://platform.openai.com/settings/organization/api-keys)

Crie uma **Restricted API Key** com a permissão:

```text
Tunnels → Read
```

Guarde a chave somente na sua máquina.

> **Nunca publique a API Key, coloque-a no Git ou envie-a para terceiros.**

Uma Admin API Key não é necessária para conectar um runtime a um Tunnel que já foi criado.

---

## 4. Instalando o `tunnel-client`

O `tunnel-client` é o software distribuído pela OpenAI que executará na máquina onde o MCP está rodando.

Baixe o release correspondente à sua arquitetura em:

[OpenAI — tunnel-client Releases](https://github.com/openai/tunnel-client/releases)

Para Linux x86_64, utilize o pacote `linux-amd64`.

Depois de extrair o arquivo:

```bash
mkdir -p ~/.local/bin

cp tunnel-client ~/.local/bin/
chmod +x ~/.local/bin/tunnel-client

tunnel-client --version
```

Se o comando não for encontrado, certifique-se de que `~/.local/bin` está no seu `PATH`.

---

## 5. Configurando as credenciais

Defina o Tunnel ID:

```bash
export CONTROL_PLANE_TUNNEL_ID='tunnel_SEU_ID'
```

Para informar a API Key sem deixá-la visível no terminal:

```bash
read -s CONTROL_PLANE_API_KEY
export CONTROL_PLANE_API_KEY
```

Agora o `tunnel-client` poderá utilizar essas variáveis sem que a chave precise ser colocada diretamente nos comandos.

---

## 6. Exemplo: MCP do LinkedIn

Para demonstrar o processo, utilizaremos o [`mcp-server-linkedin`](https://github.com/stickerdaniel/linkedin-mcp-server).

> ⚠️ Esse é um **MCP de terceiros**, não desenvolvido ou verificado pela OpenAI ou pelo LinkedIn. O autor deste post não possui vínculo com os responsáveis pelo projeto e **não se responsabiliza por eventuais problemas, danos ou consequências decorrentes do uso desse software**. Recomenda-se revisar o projeto e utilizá-lo por sua própria conta e risco.

Por isso, antes de executar qualquer MCP de terceiros, revise seu código, dependências, ferramentas disponíveis e credenciais utilizadas.

Neste exemplo, o MCP será executado pelo [`uvx`](https://docs.astral.sh/uv/guides/tools/):

```bash
uvx mcp-server-linkedin@latest
```

O `tunnel-client` pode iniciar esse processo automaticamente:

```bash
tunnel-client runtimes connect \
  --alias linkedin \
  --tunnel-id "$CONTROL_PLANE_TUNNEL_ID" \
  --runtime-api-key env:CONTROL_PLANE_API_KEY \
  --mcp-command "uvx mcp-server-linkedin@latest"
```

O parâmetro `--mcp-command` pode ser substituído pelo comando necessário para iniciar qualquer outro MCP compatível com `stdio`.
### Automatizando o processo

Como esse comando depende de credenciais e precisa ser executado novamente quando o runtime é iniciado, criei um pequeno script para automatizar o gerenciamento desse runtime.

O script recupera as credenciais utilizando o `pass` e disponibiliza dois comandos: `start`, para iniciar o runtime, e `stop`, para desconectá-lo.

<script src="https://gist.github.com/FranciscoOssian/f95331a33740908165b20c8d84ba337d.js"></script>

Para utilizá-lo diretamente:

```bash
# iniciar o runtime
curl -sL https://gist.githubusercontent.com/FranciscoOssian/f95331a33740908165b20c8d84ba337d/raw | bash -s -- start

# desconectar o runtime
curl -sL https://gist.githubusercontent.com/FranciscoOssian/f95331a33740908165b20c8d84ba337d/raw | bash -s -- stop
```

O script é apenas uma camada de automação sobre os comandos do `tunnel-client`. As credenciais não ficam armazenadas no script: elas são recuperadas localmente pelo `pass`.


---

## 7. Verificando o runtime

Depois da conexão, verifique o estado do runtime:

```bash
tunnel-client runtimes status linkedin
```

Um runtime funcionando deverá aparecer como `ready`:

```text
linkedin    ready    tunnel_<tunel_id>
```

Nesse momento, o MCP continua sendo executado localmente, enquanto o Tunnel permite que o ChatGPT encaminhe chamadas para ele.

---

## 8. Conectando o Tunnel ao ChatGPT

No ChatGPT, abra:

**Settings → Apps**

Com o Developer Mode habilitado, adicione um aplicativo MCP personalizado e utilize a conexão por **Tunnel**.

Selecione o Tunnel criado anteriormente.

Depois, abra um chat e habilite o aplicativo MCP para testar suas ferramentas.

---

## 9. O que acontece na prática?

O fluxo completo é:

```text
┌──────────────────┐
│     ChatGPT      │
└────────┬─────────┘
         │
         ▼
┌──────────────────────────┐
│ OpenAI Secure MCP Tunnel │
└────────┬─────────────────┘
         │
         │ conexão de saída
         ▼
┌──────────────────────────┐
│      tunnel-client       │
│       sua máquina        │
└────────┬─────────────────┘
         │
         │ stdio
         ▼
┌──────────────────────────┐
│    mcp-server-linkedin   │
└──────────────────────────┘
```

O ponto importante é que o servidor MCP não precisa possuir um endpoint HTTP público.

---

## 10. Parando o runtime

Para consultar os comandos disponíveis na versão instalada:

```bash
tunnel-client runtimes --help
```

Nas versões que possuem o comando `stop`:

```bash
tunnel-client runtimes stop linkedin
```

Isso interrompe o runtime local.

O Tunnel criado no OpenAI Platform e a API Key continuam existindo. Eles precisam ser removidos separadamente caso não sejam mais necessários.

---

## Segurança

O Secure MCP Tunnel resolve principalmente o problema de **conectividade**. Ele não transforma automaticamente um MCP de terceiros em um software confiável.

Um MCP malicioso ou comprometido ainda pode apresentar riscos, incluindo **prompt injection** e uso indevido das ferramentas ou credenciais às quais ele tenha acesso.

Por isso:

* não compartilhe sua API Key;
* prefira MCPs cujo código possa ser auditado;
* revise as ferramentas disponibilizadas pelo MCP;
* conceda somente as credenciais necessárias;
* evite executar MCPs de origem desconhecida;
* trate MCPs que acessam contas externas como qualquer outro software de terceiros.

O Tunnel evita a necessidade de expor diretamente o servidor MCP à Internet, mas **não elimina os riscos do próprio MCP**.

---

## Referências

* [OpenAI — tunnel-client](https://github.com/openai/tunnel-client)
* [OpenAI — tunnel-client Releases](https://github.com/openai/tunnel-client/releases)
* [OpenAI Help Center — Developer Mode e MCP Apps no ChatGPT](https://help.openai.com/pt-br/articles/12584461-developer-mode-and-mcp-apps-in-chatgpt)
* [Model Context Protocol](https://modelcontextprotocol.io/)
* [mcp-server-linkedin](https://github.com/stickerdaniel/mcp-server-linkedin)