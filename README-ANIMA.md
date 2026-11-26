# Ânima Vault: fork do `passbolt_browser_extension`

Fork **só visual** da [extensão do Passbolt](https://github.com/passbolt/passbolt_browser_extension) (AGPL-3.0) com a marca Ânima Vault.
Criptografia, autenticação, chamadas de API e lógica de negócio **não são alteradas**.

A orquestração (build, empacotamento, textos de marca e atualização a partir do upstream) fica em [Anima-Solutions/anima-vault](https://github.com/Anima-Solutions/anima-vault). Este README cobre só o que existe **dentro** deste fork.

- Branch: `anima/v5.x`, rebaseada sobre as tags do upstream (`upstream` = passbolt, com push desativado).
- Commits do fork começam com `[anima]`.

## De onde vem a UI

Quase toda a parte visual (componentes React, tema CSS, ícones, logos e fontes) vem do **styleguide**, que a extensão lê de `node_modules/passbolt-styleguide`.
O `package.json` e o `package-lock.json` **não** são alterados: continuam apontando para o pacote do upstream (os testes e o lint do lockfile continuam iguais).
No build, `anima-vault/scripts/build.sh` empacota o fork [anima-vault-styleguide](https://github.com/Anima-Solutions/anima-vault-styleguide) (`npm pack`) e o coloca no lugar de `node_modules/passbolt-styleguide`.

> Um `npm run build` direto neste repositório gera a extensão com o **visual do Passbolt**. Use sempre `anima-vault/scripts/build.sh`.

## Arquivos do upstream alterados (patches)

Cada linha alterada está marcada com `[anima]` (ou está listada aqui, no caso dos JSON, que não aceitam comentários). Num conflito de rebase, reaplique a intenção descrita aqui.

| Arquivo | Mudança | Por quê |
|---|---|---|
| `src/chrome-mv3/manifest.json` | `short_name`, `action.default_title` e a descrição do atalho com "Ânima Vault" | Nome do produto na barra do navegador (o `name` vem de `_locales`, aplicado no empacotamento) |
| `src/firefox/manifest.json` | O mesmo, mais `browser_specific_settings.gecko.id` = `vault@animahub.ai` | Nome do produto; ID próprio no AMO, que não conflita com a extensão oficial |
| `webpack.common.config.js` | Copia `fonts/anima/*` do styleguide | Inter do Design System, empacotada na extensão (a CSP proíbe fontes externas) |

## Textos (i18n)

`src/all/locales/**` e `src/all/_locales/**` **nunca** são alterados no git. O nome do produto (inclusive o nome e a descrição na loja) é aplicado no **empacotamento**, depois dos testes, por `anima-vault/scripts/brand-strings.mjs`, e os arquivos voltam ao original em seguida.

## Testes

Critério: **zero falhas** nos testes do upstream. Linha de base: v5.18.1, 5092 testes. Rode **um repositório por vez**, num container com limite de memória (o `scripts/build.sh` já faz isso).

## Licença e marca

O código continua sob **AGPL-3.0** (ver `LICENCE`). "Passbolt" é marca registrada da Passbolt SA. A marca, os logos e o nome **Ânima Vault** são da Ânima e **não** são licenciados pela AGPL.
