# 📊 Coimbra Ass&Ferra — Contabilidade da Empresa

![Status](https://img.shields.io/badge/status-ativo-brightgreen)
![Armazenamento](https://img.shields.io/badge/armazenamento-localStorage-yellow)
![Offline](https://img.shields.io/badge/offline-100%25-blue)
![Contexto](https://img.shields.io/badge/contexto-contabilidade-red)

Sistema web simples e local para controle contábil de uma pequena empresa (Coimbra Ass&Ferra). Permite registrar indicadores financeiros do dia (caixa, vendas, investimento, lucro, etc.) e cálculos de distribuição (pagamento de funcionários, reinvestimento, lucro final). Todos os dados são salvos **no próprio navegador**, via `localStorage` — sem internet, sem servidor, sem banco de dados externo. Privacidade total e funcionamento 100% offline.

---

## 📋 Índice

- [🎯 Sobre o Projeto](#-sobre-o-projeto)
- [✨ Funcionalidades](#-funcionalidades)
- [🗂 Estrutura da Tela](#-estrutura-da-tela)
  - [Tabela 1 — Indicadores](#tabela-1--indicadores)
  - [Tabela 2 — Cálculos de Vendas e Distribuição](#tabela-2--cálculos-de-vendas-e-distribuição)
- [🚀 Como Usar](#-como-usar)
- [🧠 Arquitetura do Sistema](#-arquitetura-do-sistema)
  - [Por que localStorage?](#por-que-localstorage)
  - [Como funciona o salvamento](#como-funciona-o-salvamento)
  - [Como funciona o carregamento](#como-funciona-o-carregamento)
  - [Onde os dados ficam guardados](#onde-os-dados-ficam-guardados)
- [✅ Boas Práticas de Uso](#-boas-práticas-de-uso)
- [⚠️ Limitações e Cuidados](#️-limitações-e-cuidados)
- [🛠 Tecnologias Envolvidas](#-tecnologias-envolvidas)
- [🤝 Contribuição](#-contribuição)
- [📄 Licença](#-licença)

---

## 🎯 Sobre o Projeto

Este projeto nasceu da necessidade de uma ferramenta **simples, privada e offline** para registrar o dia a dia financeiro de uma pequena empresa de assistência e ferragens. Em vez de planilhas complexas ou sistemas online que exigem cadastro e conexão, a proposta é: **abrir o HTML, preencher, salvar, fechar**.

Os dados **nunca saem da máquina do usuário**. Não há chamadas de rede, não há envio para servidores, não há rastreamento. Tudo é armazenado no `localStorage` do navegador e persiste entre sessões (até que o usuário limpe o cache ou os dados do site).

O foco é:

- **Privacidade** — nenhum dado trafega pela internet;
- **Offline** — funciona sem conexão, em qualquer máquina;
- **Simplicidade** — um único arquivo HTML, sem dependências, sem build, sem instalação.

---

## ✨ Funcionalidades

| Funcionalidade | Descrição |
|---|---|
| 📅 Data | Registro da data do dia analisado |
| 💰 Caixa | Valor em caixa no início do dia |
| 🛒 Vendas Hoje | Total de vendas realizadas |
| 💸 Quanto Investir | Valor destinado a investimento |
| 📦 Quantidade de Produtos | Quantidade de itens vendidos/produzidos |
| 🏷 Valor do Produto | Preço unitário do produto |
| 💵 Valor da Venda | Total da venda realizada |
| 📈 Lucro Esperado | Lucro projetado |
| 📊 Resultado Bruto | Resultado bruto do dia |
| 👷 Pagamento dos Funcionários | Folha de pagamento |
| 💼 Quanto Restou | Saldo após pagamentos |
| 💹 Lucro Agora | Lucro após distribuição |
| 🧾 Quanto é seu | Parte do proprietário |
| 📈 Lucro Agora 02 | Lucro após reinvestimento |
| 🔁 Quanto vai Reinvestir | Valor destinado a reinvestimento |
| 🏁 Valor Bruto Final | Resultado final consolidado |
| 💾 Salvamento Local | Tudo salvo no `localStorage`, sem internet |

---

## 🗂 Estrutura da Tela

A página tem duas tabelas principais, ambas com o padrão **Indicador | Valor**.

### Tabela 1 — Indicadores

| Indicador | Tipo de campo |
|---|---|
| Data | `date` |
| Caixa | `number` (step 0.01) |
| Vendas Hoje | `number` (step 0.01) |
| Quanto eu Investir | `number` (step 0.01) |
| Quantidade de Produtos | `number` (inteiro) |
| Valor do Produto | `number` (step 0.01) |
| Valor da Venda | `number` (step 0.01) |
| Lucro Esperado | `number` (step 0.01) |
| Resultado Bruto | `number` (step 0.01) |

### Tabela 2 — Cálculos de Vendas e Distribuição

| Indicador 02 | Tipo de campo |
|---|---|
| Data | `date` |
| Pagamento dos funcionários | `number` (step 0.01) |
| Quanto Restou | `number` (step 0.01) |
| Lucro Agora | `number` (step 0.01) |
| Quanto é seu | `number` (step 0.01) |
| Lucro Agora 02 | `number` (step 0.01) |
| Quanto vai Reinvestir | `number` (step 0.01) |
| Valor bruto final | `number` (step 0.01) |

Abaixo das tabelas há o botão **Salvar**, que grava todos os campos no `localStorage`.

---

## 🚀 Como Usar

1. Abra o arquivo `index.html` no navegador (duplo clique já funciona — não precisa de servidor).
2. Preencha os campos da **Tabela 1** com os indicadores do dia.
3. Preencha os campos da **Tabela 2** com os cálculos de distribuição.
4. Clique em **Salvar** → aparece o alerta "Dados salvos com sucesso!".
5. Feche o navegador. Ao abrir de novo, os valores voltam automaticamente (carregamento acontece no `DOMContentLoaded`).

Não há login, não há envio para servidor, não há sincronização. **Cada navegador tem seus próprios dados.**

---

## 🧠 Arquitetura do Sistema

O projeto é **100% frontend** — um único arquivo HTML com CSS e JavaScript embutidos. Não há backend, não há banco de dados externo, não há chamadas de rede.

```
┌────────────────────────┐
│ index.html             │
│ (navegador do usuário) │
│                        │
│  ┌──────────────────┐  │
│  │   localStorage   │  │  ← dados persistem aqui, na máquina
│  └──────────────────┘  │
└────────────────────────┘
```

### Por que localStorage?

O `localStorage` é uma API nativa dos navegadores que armazena pares chave-valor **no próprio dispositivo**, com as seguintes vantagens para este projeto:

- **Privacidade total** — os dados nunca saem da máquina;
- **Offline** — funciona sem internet;
- **Persistência** — os dados sobrevivem ao fechamento do navegador e ao desligamento do computador;
- **Simplicidade** — sem instalação, sem configuração, sem servidor;
- **Gratuidade** — não há custo de hospedagem de banco.

Limitações do `localStorage` (tratadas na seção de cuidados):

- Capacidade de ~5 MB por origem — mais que suficiente para 17 campos numéricos;
- Os dados são **por navegador e por máquina** — não sincronizam entre dispositivos;
- Podem ser apagados se o usuário limpar o cache/dados do site.

### Como funciona o salvamento

O array `campos` lista os 17 IDs dos inputs:

```javascript
const campos = [
    'data', 'caixa', 'vendas', 'investir', 'quantidade',
    'valorProduto', 'valorVenda', 'lucro', 'resultadoBruto',
    'data2', 'pagFunc', 'quantoRestou', 'lucroAgora',
    'quantoSeu', 'lucroAgora02', 'quantoReinvestir', 'valorBrutoFinal'
];
```

Ao clicar em **Salvar**, a função `salvar()` percorre esse array e, para cada campo:

1. Localiza o elemento pelo `id`;
2. Chama `localStorage.setItem(id, el.value)` — grava o valor sob a chave igual ao `id`;
3. Ao final, mostra `alert('Dados salvos com sucesso!')`.

### Como funciona o carregamento

No evento `DOMContentLoaded` (quando a página termina de carregar), a função `carregarDados()` percorre o mesmo array e, para cada campo:

1. Localiza o elemento pelo `id`;
2. Lê `localStorage.getItem(id)`;
3. Se o valor existir (`!== null`), preenche `el.value`.

Isso garante que o usuário reabra a página e encontre os campos exatamente como deixou.

### Onde os dados ficam guardados

Cada valor fica sob uma chave igual ao `id` do input. Exemplo do `localStorage` após salvar:

```json
{
  "data": "2025-09-18",
  "caixa": "1500.00",
  "vendas": "320.50",
  "investir": "200.00",
  "quantidade": "25",
  "valorProduto": "12.90",
  "valorVenda": "322.50",
  "lucro": "100.00",
  "resultadoBruto": "220.50",
  "data2": "2025-09-18",
  "pagFunc": "150.00",
  "quantoRestou": "170.50",
  "lucroAgora": "170.50",
  "quantoSeu": "120.00",
  "lucroAgora02": "50.50",
  "quantoReinvestir": "50.50",
  "valorBrutoFinal": "200.50"
}
```

Para visualizar: **F12 → Application (Aplicação) → Local Storage → origem do arquivo**.

---

## ✅ Boas Práticas de Uso

- 💾 Clique em **Salvar** sempre ao final do preenchimento;
- 🔢 Use `ponto` como separador decimal nos campos `number` (o navegador normaliza);
- 📅 Preencha sempre as duas datas (Tabela 1 e Tabela 2) para manter o contexto;
- 🖥 Use sempre o **mesmo navegador** para não perder os dados (eles não sincronizam);
- 🧹 Faça backup manual periódico se os dados forem importantes (veja abaixo);
- 🔒 Não use em computador compartilhado sem limpar os dados ao final.

### Como fazer backup

Como os dados ficam no `localStorage`, o backup é manual:

1. Abra **F12 → Application → Local Storage**;
2. Copie o conteúdo (ou use "Save all as HAR" / extensões de exportação de localStorage);
3. Guarde o JSON em um arquivo seguro.

Para restaurar, cole os pares de volta no `localStorage` pela mesma aba.

---

## ⚠️ Limitações e Cuidados

- 🖥 **Dados por navegador/máquina** — se abrir em outro computador ou em outro navegador, os dados **não estarão lá**;
- 🧹 **Limpar cache apaga os dados** — se o usuário limpar "dados de sites" do navegador, tudo é perdido;
- 🚫 **Modo anônimo** — em janelas privativas, o `localStorage` é descartado ao fechar a janela;
- 📦 **Sem histórico** — cada "Salvar" sobrescreve o valor anterior. Não há versões nem auditoria;
- 🔢 **Validação mínima** — o `type="number"` impede letras, mas nada impede valores absurdos;
- 🧮 **Sem cálculo automático** — os campos não são recalculados: "Lucro Esperado", "Resultado Bruto" etc. são preenchidos manualmente;
- 🏢 **Uso multiusuário** — não há separação por usuário; qualquer pessoa com acesso ao navegador vê os dados;
- 💾 **Sem backup automático** — é responsabilidade do usuário exportar/copiar se quiser guardar.

⚠️ **Aviso:** por ser um sistema simples de controle diário, não substitui um sistema contábil formal. Para obrigações fiscais, use ferramentas apropriadas.

---

## 🛠 Tecnologias Envolvidas

- **HTML5** — estrutura única, sem frameworks;
- **CSS3** — layout centralizado, tabelas estilizadas, tipografia com Poppins e Georgia;
- **JavaScript (vanilla)** — toda a lógica de salvamento e carregamento;
- **localStorage** — persistência local no navegador, sem backend;
- **Google Fonts (Poppins)** — tipografia dos títulos e cabeçalhos.

---

## 🤝 Contribuição

Contribuições são bem-vindas! Para contribuir:

1. Faça um fork do projeto;
2. Crie uma branch para sua feature (`git checkout -b feature/nova-funcionalidade`);
3. Commit suas alterações (`git commit -m 'Adiciona nova funcionalidade'`);
4. Push para a branch (`git push origin feature/nova-funcionalidade`);
5. Abra um Pull Request.

---

## 📄 Licença

Este projeto está licenciado sob a MIT License. Consulte o arquivo `LICENSE` para mais detalhes.
