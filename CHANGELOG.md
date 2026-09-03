# CHANGELOG

## 3.0.0 — 03/09/2026

### Reengenharia do módulo administrativo
- Navegação administrativa reescrita com estado centralizado.
- Cada botão possui `data-admin-tab` e cada conteúdo possui `data-admin-panel`.
- A alternância de conteúdo usa exclusivamente o atributo HTML `hidden`.
- Removida a dependência de `style.display` para as abas.
- Removida a dependência da classe Tailwind `.hidden` para os painéis.
- Clique no botão, Enter e Espaço ativam a aba correspondente.
- Hash da URL acompanha a aba ativa e é restaurado no carregamento.
- Adicionada indicação visual inequívoca da versão `v3.0.0`.
- Adicionado indicador do módulo administrativo atualmente aberto.
- Criada API `window.ATIAdmin.setTab()` para diagnóstico e futuras integrações.
- Mantida a proteção da aba Usuários para administradores.
- Mantidas as funções de eventos, inscrições, presença, usuários, Firebase Authentication, Firestore e Storage.

### Controle de versão
- `VERSION` = `3.0.0`.
- Meta HTML `app-version` = `3.0.0`.
- Versão visível no cabeçalho e no painel administrativo.
