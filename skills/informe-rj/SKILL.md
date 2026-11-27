---
name: informe-rj
description: "Gera o informe de situação de um processo de Recuperação Judicial (RJ) ou Falência a partir dos documentos de uma pasta local (ex.: _informe-falimentar). Use quando o usuário digitar /informe-rj, pedir 'informe da RJ', 'informe falimentar', 'status da recuperação judicial', 'resumo do processo de falência' ou apontar uma pasta com peças processuais, relatórios do administrador judicial, plano de recuperação ou lista de credores."
---

# Informe RJ — Recuperação Judicial / Falência

Objetivo: entregar, em uma página de leitura rápida, **onde o processo está, quanto se deve a quem, o que vence e o que fazer agora** — usando apenas o que está nos documentos.

## Regra número 1: nada inventado

- Só use dados que estejam nos arquivos lidos. Todo número, data e nome precisa ter fonte.
- Cite a fonte ao lado do dado: `(arquivo.pdf, p. 12)`.
- Se um campo não aparece nos documentos, escreva **[não localizado nos documentos]**. Nunca estime, nunca complete "de cabeça".
- Prazos legais (Lei 11.101/2005) podem ser citados como regra, mas a data calculada só entra se a data-base estiver em um documento. Mostre o cálculo.
- Se houver conflito entre documentos (ex.: dois valores de passivo), mostre os dois com as fontes e diga qual é o mais recente.

## Passo 1 — Localizar a pasta e os arquivos

1. Pasta padrão: `_informe-falimentar` dentro da pasta de trabalho atual. Se o usuário passou um caminho ou nome de cliente (`/informe-rj ezequiel`), use esse.
2. Liste todos os arquivos (PDF, DOCX, XLSX, CSV, TXT, MD, imagens).
3. Se a pasta não existir ou estiver vazia: **pare** e diga exatamente qual caminho foi procurado e o que falta. Não gere informe vazio.

## Passo 2 — Ler e classificar os documentos

Leia cada arquivo (PDF: use a skill `pdf`; planilhas: `xlsx`; Word: `docx`). Classifique cada um:

| Tipo | Exemplos |
|---|---|
| Petição inicial | pedido de RJ, autofalência |
| Decisões judiciais | deferimento do processamento, stay period, convolação em falência, homologação do plano |
| Relatórios do Administrador Judicial (RMA) | relatórios mensais de atividades |
| Plano de recuperação | plano, aditivos, modificativos |
| Credores | relação do art. 52 §1º, edital do art. 7º §2º, Quadro Geral de Credores (QGC), habilitações/impugnações |
| Assembleia (AGC) | edital de convocação, ata |
| Financeiro | balanços, DRE, fluxo de caixa, extratos |
| Outros | contratos, notificações, e-mails |

Registre para cada arquivo: tipo, data do documento, páginas relevantes.

## Passo 3 — Extrair os dados

Preencha, com fonte:

1. **Identificação** — devedora(s), CNPJ, número do processo, vara/comarca, juiz, administrador judicial, advogado da devedora.
2. **Tipo e fase** — RJ, RJ extrajudicial ou falência. Fase atual: pedido → deferimento do processamento → verificação de créditos → plano apresentado → AGC → plano homologado (concessão) → fiscalização (até 2 anos) → encerramento / convolação em falência.
3. **Datas-chave** — ajuizamento, deferimento do processamento, publicação do edital, apresentação do plano, AGC (1ª e 2ª convocação), homologação.
4. **Passivo por classe** (art. 41):
   - Classe I — trabalhistas
   - Classe II — garantia real
   - Classe III — quirografários
   - Classe IV — ME/EPP
   - Extraconcursais / não sujeitos (fiscal, alienação fiduciária, ACC etc.)
   Para cada classe: nº de credores, valor total, % do total. Liste os 10 maiores credores.
5. **Plano de recuperação** — principais condições por classe (deságio, carência, prazo, correção), meios de recuperação (art. 50), venda de ativos/UPI.
6. **Situação operacional** — faturamento, caixa, funcionários, conforme o último RMA.
7. **Movimentações recentes** — últimas decisões e petições relevantes, em ordem cronológica.

## Passo 4 — Prazos e alertas

Verifique e calcule (mostrando a data-base e a fonte):

- **Stay period**: 180 dias do deferimento do processamento, prorrogável uma vez por igual período (art. 6º §4º).
- **Plano**: 60 dias da publicação da decisão de deferimento (art. 53). Não apresentar = risco de convolação em falência (art. 73).
- **Habilitações/divergências**: 15 dias do edital do art. 52 §1º (art. 7º §1º).
- **Objeções ao plano**: 30 dias (art. 55).
- **AGC**: até 150 dias do deferimento (art. 56 §1º).
- **Fiscalização pós-concessão**: até 2 anos (art. 61).
- Parcelas do plano vencidas e não pagas.

Classifique cada alerta: 🔴 vencido/crítico · 🟡 vence em até 30 dias · 🟢 sob controle.

## Passo 5 — Montar o informe

Salve em `_informe-falimentar/informe-rj-AAAA-MM-DD.md` e mostre no chat. Estrutura fixa:

```markdown
# Informe RJ — <Devedora> — <data>

## 1. Resumo em 5 linhas
- Fase atual:
- Passivo total sujeito à RJ:
- Próximo prazo crítico:
- Principal risco:
- Ação recomendada:

## 2. Identificação do processo
(tabela)

## 3. Linha do tempo
(tabela: data | evento | fonte)

## 4. Passivo por classe
(tabela: classe | nº credores | valor | % | fonte)
### Top 10 credores

## 5. Plano de recuperação
(condições por classe)

## 6. Prazos e alertas
(tabela: prazo | data | status 🔴🟡🟢 | base legal | fonte)

## 7. Riscos
(o que pode dar errado, com base nos documentos)

## 8. Próximos passos
(ações concretas, responsável, prazo)

## 9. Lacunas
(lista de tudo marcado como [não localizado nos documentos] e qual documento resolveria)

## Fontes consultadas
(lista de arquivos lidos, tipo e data)
```

## Passo 6 — Checagem final antes de entregar

- [ ] Todo número tem fonte?
- [ ] As somas por classe batem com o total? Se não batem, aponte a diferença.
- [ ] Toda data calculada mostra a data-base?
- [ ] A seção "Lacunas" lista o que faltou?
- [ ] Nenhum dado veio de fora dos documentos (exceto regras da Lei 11.101/2005)?

Termine com uma linha no chat: fase atual, prazo mais urgente e onde o arquivo foi salvo.

## Observação

Este informe é um apoio operacional. Não substitui parecer jurídico. Decisões processuais devem ser validadas pelo advogado responsável.
