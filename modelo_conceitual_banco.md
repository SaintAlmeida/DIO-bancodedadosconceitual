
# 🧠 Modelo Conceitual - Descrição Geral

## 🎯 Regras de Negócio Aplicadas

- Uma **conta** está associada a um único cliente, que pode ser **PF ou PJ**, mas **nunca ambos**.
- Uma **conta pode ter várias formas de pagamento** associadas.
- Uma **entrega** tem um **status** (ex: "Pendente", "Enviado", "Entregue") e um **código de rastreio único**.

---

## 🔗 Entidades e Relacionamentos

### 1. Conta
- `id_conta` (PK)
- `data_criacao`
- `tipo_cliente` (enum: 'PF', 'PJ') ✅ usado para garantir exclusividade

### 2. Cliente_PF
- `id_conta` (PK, FK → Conta)
- `nome`
- `cpf`
- `data_nascimento`

### 3. Cliente_PJ
- `id_conta` (PK, FK → Conta)
- `razao_social`
- `cnpj`
- `inscricao_estadual`

> 🔐 **Regra de exclusividade**: Um `id_conta` deve existir em apenas **uma** das tabelas `Cliente_PF` ou `Cliente_PJ`.

### 4. Pagamento
- `id_pagamento` (PK)
- `id_conta` (FK → Conta)
- `tipo_pagamento` (ex: 'Cartão', 'Pix', 'Boleto', etc.)
- `detalhes` (ex: últimos dígitos do cartão)

> 💳 Uma conta pode ter **múltiplas formas de pagamento**.

### 5. Entrega
- `id_entrega` (PK)
- `id_conta` (FK → Conta)
- `status` (ex: 'pendente', 'em trânsito', 'entregue', etc.)
- `codigo_rastreio` (único)
- `data_prevista`
- `data_entrega`

> 🚚 Ligação direta com a conta para saber quem fez o pedido.

---

## 📘 Exemplo de Notação Entidade-Relacionamento (ER) – Texto

```
[Conta] ←(1:1)→ [Cliente_PF]
        ←(1:1)→ [Cliente_PJ]
        ←(1:N)→ [Pagamento]
        ←(1:N)→ [Entrega]
```

---

## 🛠️ Regras de integridade a considerar

### Integridade Exclusiva
- Uma **trigger**, **enum** ou **lógica de aplicação** deve garantir que uma conta tenha **ou um PF ou um PJ**, **nunca ambos**.

### Relacionamentos
- **Pagamentos** e **Entregas** se relacionam diretamente com `Conta` via `id_conta`.

---

## ✅ Resumo das Entidades

| Entidade     | Chave Primária     | Atributos principais                                 |
|--------------|--------------------|------------------------------------------------------|
| Conta        | id_conta           | data_criacao, tipo_cliente ('PF' ou 'PJ')           |
| Cliente_PF   | id_conta (FK + PK) | nome, cpf, data_nascimento                          |
| Cliente_PJ   | id_conta (FK + PK) | razao_social, cnpj, inscricao_estadual             |
| Pagamento    | id_pagamento       | tipo_pagamento, detalhes, id_conta (FK)             |
| Entrega      | id_entrega         | status, codigo_rastreio (único), id_conta (FK)      |
