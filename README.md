# OrçaFácil AI

SaaS web/PWA para pequenos prestadores de serviços criarem propostas profissionais, enviarem ao cliente, acompanharem a aprovação e controlarem recebimentos.

## Fluxo principal

Cliente → descrição do serviço → IA → orçamento → link público → aprovação → ordem de serviço → cobrança → recebimento.

## MVP

- Dashboard comercial
- Clientes
- Catálogo de produtos e serviços
- Orçamentos com produtos + serviços
- Subtotal, desconto, acréscimo e total automáticos
- IA para melhorar a descrição/escopo da proposta
- Fotos/anexos da proposta
- Link público do orçamento
- Aprovar, recusar ou solicitar alteração
- Histórico de status
- PDF profissional
- Conversão de orçamento aprovado em ordem de serviço
- PIX e condições de pagamento
- Contas a receber simplificado
- Firebase Authentication
- Firestore multiempresa
- Interface responsiva para celular, tablet e desktop
- PWA instalável

## Diretrizes

1. O usuário deve conseguir criar um orçamento em menos de 2 minutos.
2. O MVP não deve virar um ERP genérico.
3. Prioridade absoluta: orçamento → aprovação → recebimento.
4. IA deve reduzir trabalho de redação, não criar complexidade.
5. Credenciais/configurações técnicas não devem aparecer para o cliente final.
6. Todo dado de negócio deve possuir `tenantId` e ser isolado por empresa.
7. Não implementar bloqueio de sessão única: celular e PC podem ser usados pela mesma conta.
8. Firestore em produção deve usar regras restritivas; nunca depender de modo de teste.

## Stack alvo

- Next.js (App Router)
- TypeScript
- Tailwind CSS
- Firebase Authentication
- Cloud Firestore
- Firebase Storage
- PWA
- Integração de IA somente no servidor

## Estrutura funcional

### Dashboard
Mostrar valor enviado, valor aprovado, taxa de conversão, valor recebido, saldo a receber e orçamentos recentes.

### Clientes
CRUD de cliente com nome, CPF/CNPJ opcional, telefone/WhatsApp, e-mail, endereço e observações.

### Catálogo
Produtos/materiais e serviços reutilizáveis, com descrição, unidade e preço padrão.

### Orçamentos
Cliente, validade, itens, quantidades, preços, desconto, acréscimo, condições de pagamento, escopo, observações, fotos e total.

Status: `draft`, `sent`, `viewed`, `approved`, `changes_requested`, `rejected`, `expired`.

### IA
Endpoint server-side que recebe uma descrição curta do serviço e devolve uma versão profissional do escopo. A chave do provedor nunca deve ir ao navegador.

### Proposta pública
URL não autenticada com token público não sequencial. Exibir empresa, proposta, itens, condições e ações Aprovar / Solicitar alteração / Recusar.

### Ordem de serviço
Criada a partir de orçamento aprovado. Não construir módulo operacional complexo nesta primeira versão.

### Recebimentos
Parcelas previstas e registro simples de pagamento. Não implementar contabilidade, conciliação bancária ou fluxo de caixa completo no MVP.

## Arquitetura de dados inicial

- tenants
- users
- customers
- catalogItems
- quotes
- quoteEvents
- workOrders
- receivables

Entidades operacionais devem incluir `tenantId`, `createdAt`, `updatedAt` e, quando aplicável, `createdBy`.

## Segurança

- Autenticação obrigatória nas áreas internas.
- Isolamento por `tenantId` validado também nas Firestore Security Rules.
- Rotas públicas acessam apenas a representação pública necessária da proposta.
- Nenhuma chave privada de IA no frontend.
- Não armazenar segredos no repositório.

## Fora do MVP

- ERP financeiro completo
- Rastreamento GPS
- CRM complexo
- Emissão fiscal
- Estoque completo
- Licença vitalícia
- Painel de vendedor dentro do app do cliente
- Configuração manual do Firebase pelo usuário final
- Bloqueio obrigatório de múltiplos dispositivos
