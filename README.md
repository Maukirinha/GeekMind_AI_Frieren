# Atualizações da GEEKMIND I.A. (Frieren IA)

Este repositório é só o **canal de atualizações** do programa. O código
fica em outro lugar, privado; aqui moram duas coisas:

- **`version.json`** — qual é a versão mais nova e onde baixá-la.
- **Releases** — o instalador de cada versão.

O programa instalado lê o `version.json` por conta própria, quando você
clica em *Configurações → Atualizações → Verificar atualizações*. Nenhum
computador precisa ficar ligado servindo nada: é um arquivo de texto
parado num endereço público.

## O endereço que o programa lê

```
https://raw.githubusercontent.com/Maukirinha/GeekMind_AI_Frieren/main/version.json
```

Ele está escrito em `core/updater.py`, na constante `URL_DO_VERSION_JSON`.

## O formato

```json
{
  "versao": "1.1.0",
  "url": "https://github.com/Maukirinha/GeekMind_AI_Frieren/releases/latest/download/Instalar-FRIEREN-IA.exe",
  "notas": "O que mudou nesta versão, em uma ou duas frases.",
  "obrigatoria": false
}
```

| campo | obrigatório | o que é |
|---|---|---|
| `versao` | sim | comparada com a versão instalada, número a número: `1.10.0` é maior que `1.9.0` |
| `url` | não | o arquivo que o botão "Baixar agora" abre. Sem ela, o programa só avisa que há versão nova |
| `notas` | não | aparece no aviso |
| `obrigatoria` | não | hoje só acrescenta uma frase ao aviso |

## Como publicar uma versão nova

### Pelo script (o jeito curto)

No projeto, com o instalador já anexado ao Release:

```
venv\Scripts\python.exe scripts\publicar_versao.py 1.1.0 "O que mudou"
```

Ele sobe o número em `core/versao.py`, escreve o `version.json` e o
`CHANGELOG.md` aqui, empurra, e confere no endereço público se o que
subiu é o que devia. Antes de tudo isso ele confere se o instalador
responde no Release — e se recusa a anunciar uma versão com link
quebrado. Para ver o que ele faria sem mexer em nada: `--so-conferir`.

### Na mão

1. No projeto, suba o número em `core/versao.py` (`__version__`) e gere o
   instalador com `scripts/construir_protegido.py`.
2. Aqui no GitHub: *Releases → Draft a new release*, crie a tag (por
   exemplo `v1.1.0`) e anexe o instalador com o nome
   **`Instalar-FRIEREN-IA.exe`** — o mesmo nome do `url` acima, para o
   link `releases/latest/download/...` continuar valendo sozinho.
3. Edite o `version.json` deste repositório com o novo número e as notas.

A ordem importa: publique o Release **antes** de mexer no `version.json`.
Quem clicar em "Verificar atualizações" no meio do caminho receberia um
aviso de versão nova com um link que ainda não existe.

### O nome do arquivo, e por que renomear

`scripts/construir_protegido.py` gera **`Instalar Frieren IA.exe`**, com
espaços. O GitHub troca espaço por ponto no nome do anexo, então ele
viraria `Instalar.Frieren.IA.exe` e o link acima quebraria.

Renomeie para **`Instalar-FRIEREN-IA.exe`** antes de anexar. É o nome que
está no `version.json`, e sem espaços ele atravessa o GitHub intacto.

## Por que este repositório é público

O `raw.githubusercontent.com` de um repositório privado exige token, e o
programa instalado não tem (nem deve ter) um. Então o arquivo de versão
mora aqui, público, e o código continua privado no outro repositório.
