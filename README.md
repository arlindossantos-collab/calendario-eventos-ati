# Calendário de Eventos ATI Pernambuco — V3.0.0

## Versão

3.0.0

## Correção administrativa

A versão 3.0.0 reestrutura completamente a navegação administrativa. Os painéis não usam mais `style.display` ou a classe Tailwind `hidden` para trocar de conteúdo. O atributo HTML `hidden` é a única fonte de verdade da visibilidade dos módulos.

Abas:
- `#dashboard` — Visão geral
- `#eventos` — Eventos
- `#inscricoes` — Inscrições
- `#usuarios` — Usuários, apenas administradores

Para diagnóstico no console do navegador:
`window.ATIAdmin.setTab('eventos')`

## Publicação

Publique o conteúdo desta pasta no Firebase Hosting. Depois do deploy, abra `/admin` e confirme que o cabeçalho mostra **v3.0.0**. Se ainda aparecer a versão antiga, faça `Ctrl+F5` ou teste em janela anônima; isso também ajuda a confirmar que o Hosting está servindo o novo `index.html`.
