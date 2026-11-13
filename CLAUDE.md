# CLAUDE.md

Orientações para o Claude Code neste repositório.

## Visão geral

Repositório de material médico de referência (pediatria/plantão), publicado como site estático no GitHub Pages. Não há build, framework ou testes automatizados.

## Arquivos

- `04_folha-de-parada_Dossie_Integral.html` — dossiê "Folha de Parada" convertido de PDF via **pdf2htmlEX** (~8,6 MB, autocontido: CSS, fontes e imagens embutidos em base64). Texto posicionado em camadas absolutas por página (`.pf`/`.pc`); edições de conteúdo exigem cuidado para não deslocar o layout.
- `Untitled.ipynb` — notebook Colab que gera `diretriz_aap_2026_itu_pediatrica.pdf` (Guia de plantão AAP 2026: ITU pediátrica, 8 dias a 5 anos) com a biblioteca `fpdf2`.
- `.github/workflows/static.yml` — deploy do repositório inteiro (`path: '.'`) no GitHub Pages a cada push na `main` (ou manual via `workflow_dispatch`).
- `README.md` — mínimo.

## Comandos

```bash
# Dependência do notebook
pip install fpdf2

# Executar o código do notebook fora do Colab (gera o PDF no diretório atual)
python3 -c "import json;nb=json.load(open('Untitled.ipynb'));exec(''.join(nb['cells'][1]['source']))"

# Visualizar o site localmente
python3 -m http.server 8000   # abrir /04_folha-de-parada_Dossie_Integral.html
```

## Regras e cuidados

- **Fontes core do fpdf2 (helvetica) aceitam apenas Latin-1.** Caracteres como `•`, `≥`, `≤`, `→`, `–`, aspas tipográficas causam `FPDFUnicodeEncodingException`. Usar equivalentes ASCII (`-`, `>=`, `<=`, `->`) ou registrar uma fonte TTF Unicode (`pdf.add_font(...)`).
- Os avisos `DeprecationWarning` sobre o parâmetro `ln` do `cell()` são inofensivos (fpdf2 ≥ 2.5.2).
- Não alterar estrutura, layout ou organização do site/HTML; edições devem se limitar a correções e acréscimos de conteúdo.
- Conteúdo clínico: priorizar diretrizes de sociedades (AAP, SBP etc.) e evidência de alto nível; doses pediátricas devem ser conferidas em tripla checagem; uso *off label* sempre sinalizado como "off label".
- Não há `index.html`: a raiz do Pages não lista conteúdo; o dossiê é acessado pelo nome do arquivo.
- O HTML é muito grande: ler por trechos (`offset`/`limit` ou `grep`), nunca inteiro.
