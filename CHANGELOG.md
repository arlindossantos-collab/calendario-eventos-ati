# CHANGELOG

## 2.2.0 — 03/09/2026

### Correção estrutural do módulo administrativo
- Revisada toda a estrutura de navegação da área administrativa.
- Corrigido o problema em que o conteúdo das abas podia ser renderizado lado a lado/achatado.
- O painel administrativo agora é explicitamente `display: block`.
- Cada painel de aba usa `hidden` + CSS com `!important` para garantir exclusão visual dos painéis inativos.
- Os botões das abas passaram a ter listeners diretos, reduzindo dependência de delegação de eventos.
- Clique no ícone ou no texto da aba ativa corretamente o módulo correspondente.
- Mantida navegação por teclado com Enter/Espaço.
- Mantida navegação por hash: `#dashboard`, `#eventos`, `#inscricoes` e `#usuarios`.
- A aba Usuários permanece disponível somente para administradores.
- Renderização do gráfico protegida para evitar erro quando o Chart.js não estiver disponível.
- Revisada a atualização da aba ativa após login e após alteração do hash.

### Controle de versão
- Versão central registrada em `VERSION`.
- Versão exibida no HTML administrativo.
- Alterações registradas neste `CHANGELOG.md`.

## 2.1.1 — 03/09/2026
- Correção inicial da disposição dos painéis administrativos.

## 2.1.0 — 03/09/2026
- Primeira implementação das abas administrativas clicáveis.
