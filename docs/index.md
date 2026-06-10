# Cookbook do Leandro

Receitas técnicas por assunto: coisas que já fiz uma vez e quero conseguir
refazer sozinho, sem redescobrir do zero.

Use a **busca** no topo da página — é o jeito mais rápido de achar uma receita.

## Como adicionar uma receita

O jeito mais simples, direto pelo navegador:

1. Abra o repositório: <https://github.com/leandrometeoro/cookbook>
2. Entre em `docs/`, escolha a pasta do assunto (ou crie uma nova, ex.: `docker/`)
3. **Add file → Create new file**, nomeie como `minha-receita.md`
4. Escreva em Markdown e faça o commit na branch `main`

Em 1–2 minutos o GitHub Actions reconstrói e publica o site automaticamente.

Pela linha de comando:

```bash
git clone https://github.com/leandrometeoro/cookbook.git
cd cookbook
# criar/editar arquivos em docs/<assunto>/<receita>.md
git add -A && git commit -m "Receita: <título>" && git push
```

!!! tip "Estrutura de uma boa receita"
    - **Contexto**: que problema isso resolve, quando usar
    - **Passos**: comandos copiáveis, na ordem
    - **Verificação**: como saber que funcionou
    - **Armadilhas**: o que pode dar errado e como notar

!!! warning "Repositório público"
    Nunca cole tokens, senhas ou chaves nas receitas — o site é aberto.
