# agent-model

Meus subagentes do Claude Code, empacotados como plugin para reaproveitar em qualquer projeto.

## Agentes

| Agente | Modelo | Edita código? | Quando usar |
|---|---|---|---|
| `planner` | opus | não | Antes de implementar algo que mexe em vários arquivos ou não é óbvio. Gera um plano passo a passo. |
| `coder` | sonnet | sim | Implementar o plano (ou um pedido pequeno e claro). Roda testes antes de terminar. |
| `reviewer` | sonnet | não | Depois do coder ou antes de commitar. Aponta bugs, riscos e problemas com `arquivo:linha` e gravidade. |
| `committer` | haiku | não | Só quando você pedir commit, push ou PR. Cria branch se estiver na main, segue Conventional Commits. |
| `test-writer` | sonnet | só testes | Quando faltar cobertura. Não mexe no código de produção; reporta bugs que achar. |
| `debugger` | sonnet | não | Quando algo quebrou e a causa não é óbvia. Reproduz, acha a causa raiz e sugere a correção. |

## Fluxo

```
planner → coder → reviewer → committer
              ↘ test-writer
debugger → coder   (quando algo quebra)
```

## Instalação

Você só precisa instalar **uma vez**. Por padrão o plugin fica no escopo do usuário e os agentes aparecem em todos os projetos.

Dentro do Claude Code (no prompt):

```
/plugin marketplace add MateusQ7/agent-model
/plugin install agent-model@mateusq7
```

Ou no terminal normal, fora do Claude Code:

```
claude plugin marketplace add MateusQ7/agent-model
claude plugin install agent-model@mateusq7
```

Para instalar só num projeto específico, use `--scope project` (fica registrado no `.claude/settings.json` daquele projeto).

O plugin é baixado do GitHub, então só o que está na branch `main` vale para a instalação.

Os agentes aparecem com o prefixo do plugin, por exemplo `agent-model:reviewer`. Para chamar um direto: `@agent-agent-model:reviewer`.

## Atualizar

Depois de mudar algum agente aqui:

1. Suba a `version` em `.claude-plugin/plugin.json`.
2. Faça push.
3. Em qualquer sessão do Claude Code: `/plugin marketplace update mateusq7` e depois atualize o plugin pela aba **Installed** do `/plugin` (ou `claude plugin update agent-model@mateusq7` no terminal).
4. Se a sessão já estava aberta: `/reload-plugins`.

## Estrutura

```
.claude-plugin/
  plugin.json        # manifesto do plugin
  marketplace.json   # este repo também é o marketplace
agents/
  *.md               # um arquivo por agente
```

Para validar antes de dar push: `claude plugin validate .`
