# Controle de versão — Calendário de Eventos ATI

## 2.1.0 — 03/09/2026
### Correções
- Corrigida a navegação das abas da área administrativa.
- Cada aba agora possui alternância real de painel usando a propriedade HTML `hidden`, classe CSS e `display`, evitando sobreposição.
- O clique é tratado diretamente pelo componente `#adminTabs`, incluindo clique no ícone/texto da aba.
- Adicionado suporte a teclado (`Enter` e `Espaço`) para as abas.
- Adicionado hash na URL (`#dashboard`, `#eventos`, `#inscricoes`, `#usuarios`) para identificar a aba atual e permitir retorno à aba por URL.
- Adicionado tratamento de `hashchange`.
- A aba **Usuários** continua restrita ao perfil administrador.

### Controle de versão
- Versão atual: **2.1.0**
- Toda alteração futura no código deve incrementar a versão e registrar a mudança neste arquivo.
- O arquivo `VERSION` contém exclusivamente a versão atual.
- A versão também é registrada na meta `app-version` e exibida no painel administrativo.

## 2.0.0
- Versão anterior entregue com calendário público, área administrativa, autenticação por link de e-mail, Firestore e Storage.
