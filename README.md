<h1 align="center">lotacaosefazrn</h1>

<p align="center">
  Simulador não-oficial da escolha de lotações dos novos<br/>
  <strong>Auditores Fiscais de Receitas Estaduais (AFRE)</strong> da Secretaria de Estado da Tributação do Rio Grande do Norte (Sefaz/RN).
</p>

<p align="center">
  🔗 <strong><a href="https://bacalhaunabrisa.github.io/lotacaosefazrn/">bacalhaunabrisa.github.io/lotacaosefazrn</a></strong>
</p>

---

## Sobre o projeto

Este repositório contém uma única página HTML autocontida (`index.html`) que simula, em ordem de convocação, a distribuição das lotações entre os 46 novos AFRE da Sefaz/RN, com base nas preferências individuais de cada auditor fiscal. Não há servidor, backend ou build: basta abrir o arquivo no navegador (ou acessá-lo via GitHub Pages).

O projeto é uma ferramenta de simulação pessoal/coletiva, sem qualquer vínculo institucional com a Sefaz/RN. O quantitativo de vagas por lotação foi inserido manualmente com base nas informações disponibilizadas para a convocação e pode ficar desatualizado caso os quantitativos oficiais mudem.

## Como usar

1. Na tabela de auditores fiscais (listados em ordem alfabética), cada linha corresponde a um servidor e traz 8 menus suspensos ("dropdowns").
2. Em cada um dos 8 dropdowns, selecione uma lotação, da esquerda para a direita, em ordem de preferência: o 1º dropdown é a 1ª preferência, o 2º é a 2ª preferência, e assim sucessivamente até a 8ª.
3. Uma lotação já escolhida em um dos dropdowns de uma pessoa deixa automaticamente de aparecer como opção nos demais dropdowns dessa mesma pessoa.
4. A última coluna da tabela ("Lotação designada") mostra, em tempo real, o resultado da simulação para cada auditor, recalculado a cada alteração feita por qualquer pessoa na tabela.
5. Logo abaixo, 8 pequenas tabelas exibem, por lotação, a relação nominal de quem está atualmente designado para ela, também atualizadas automaticamente.
6. Suas seleções são salvas automaticamente no navegador (`localStorage`) e permanecem preenchidas da próxima vez que a página for acessada no mesmo dispositivo/navegador. O botão "Limpar todas as seleções" apaga todas as preferências de todos os 46 auditores de uma só vez.

## Lógica de atribuição

Todo o cálculo roda **no navegador do usuário**, em JavaScript puro (nenhum dado é enviado a um servidor).

### 1. Vagas por lotação

| Lotação | Vagas |
|---|---:|
| SUMAT (Volante) | 15 |
| SUFISE | 10 |
| SUCADI | 5 |
| COTIN | 5 |
| SUSCOMEX | 5 |
| SUMAT (NIF Caraú) | 4 |
| Corregedoria | 1 |
| Educação Fiscal | 1 |
| **Total** | **46** |

### 2. Ordem de convocação

Cada um dos 46 auditores fiscais possui um número de ordem de convocação (posição no concurso público), que define sua prioridade de escolha: quem foi convocado primeiro tem preferência sobre os demais na disputa por qualquer lotação; quem foi convocado em segundo lugar tem preferência sobre todos, exceto o primeiro; e assim sucessivamente. A listagem nominal exibida na página segue ordem alfabética apenas para facilitar a localização de cada nome — a ordem de convocação de cada pessoa é exibida em uma coluna própria e é ela quem efetivamente comanda o algoritmo de atribuição.

### 3. Algoritmo de atribuição (mecanismo sequencial por prioridade)

A cada alteração em qualquer dropdown de qualquer pessoa, a designação de **todos** os 46 auditores é recalculada do zero, na seguinte ordem:

1. Zera-se o quantitativo de vagas restantes de cada uma das 8 lotações (igual ao total da tabela de vagas).
2. Percorre-se a lista de auditores **em ordem crescente de convocação** (do nº 1 ao último).
3. Para cada auditor, percorrem-se suas 8 preferências, da 1ª à 8ª: a primeira lotação da sua lista de preferências que ainda tiver vaga disponível é a lotação designada a ele, e o quantitativo de vagas restantes dessa lotação é decrementado em 1.
4. Se nenhuma das lotações escolhidas pelo auditor (dentre as que ele efetivamente preencheu) tiver mais vaga disponível no momento em que se chega a ele na fila — ou se ele ainda não preencheu nenhuma preferência —, ele fica temporariamente sem lotação designada ("—"), até que as preferências sejam ajustadas.
5. Repete-se o processo até o último auditor da ordem de convocação.

Esse é o mesmo mecanismo usado, por exemplo, em processos de escolha de vaga por ordem de classificação: cada pessoa, na sua vez, garante a melhor opção disponível dentre as que ela mesma escolheu, e a rodada nunca é reaberta para quem já escolheu antes.

### Persistência

As preferências de cada um dos 46 auditores são salvas no `localStorage` do navegador (chave `lotacaosefazrn_v1`), associadas ao nome de cada pessoa. Como o armazenamento é local ao navegador/dispositivo, seleções feitas em um computador não aparecem automaticamente em outro; para compartilhar o resultado de uma simulação com outras pessoas, é necessário informá-lo manualmente (print, mensagem etc.).

## Tecnologias utilizadas

- **HTML5** — estrutura da página, em arquivo único (`index.html`).
- **CSS3** puro — sem framework; variáveis CSS (`:root`) para cores/tema, tabela com colunas fixas (`position: sticky`) para facilitar a rolagem horizontal, grid para o layout responsivo das tabelas de resumo, e fontes do Google Fonts (*Space Grotesk*, *Inter*, *JetBrains Mono* para números e códigos de ordem).
- **JavaScript (ES5/ES6)** — toda a lógica de geração dos dropdowns, validação de escolhas duplicadas, algoritmo de atribuição e persistência em `localStorage`.
- **jQuery 3.7** (via CDN) — manipulação do DOM e eventos (`change`) que disparam o recálculo automático.
- **GitHub Pages** — hospedagem estática, sem backend, sem build step, sem dependências instaladas: o repositório é publicado como está.

Não há framework de front-end (React, Vue etc.), bundler ou etapa de compilação — o projeto é intencionalmente simples para poder ser mantido e publicado direto pela interface do GitHub, do mesmo modo que os projetos irmãos [`remunerasefazrn`](https://github.com/BacalhauNaBrisa/remunerasefazrn) e [`cebraspe`](https://github.com/BacalhauNaBrisa/cebraspe).

## Estrutura do repositório

```
lotacaosefazrn/
├── index.html   # página única: HTML + CSS + JS embutidos
└── README.md    # este arquivo
```

## Aviso legal

Simulador independente e não-oficial, sem qualquer vínculo com a Sefaz/RN. O quantitativo de vagas, a ordem de convocação e a lógica de atribuição aqui reproduzidos têm caráter meramente estimativo e ilustrativo, servindo apenas como ferramenta de apoio à organização informal das escolhas entre os próprios auditores fiscais. Consulte sempre a Secretaria de Estado da Tributação do RN para fins oficiais antes de tomar qualquer decisão com base nos resultados desta página.
