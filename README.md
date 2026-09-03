# Calendário de Eventos ATI — V2

Versão revisada da aplicação original, com:

- calendário público sem autenticação;
- páginas públicas individuais `/evento/{id}`;
- inscrição pública sem login;
- área administrativa em `/admin`;
- autenticação administrativa por link enviado ao e-mail;
- somente domínio `@ati.pe.gov.br`;
- lista de e-mails autorizados em Firestore;
- perfis `editor` e `admin`;
- upload de card para Firebase Storage;
- limite de 2 MB;
- recomendação de 1080 × 1350 px / proporção 4:5;
- dashboard administrativo;
- separação dos dados de inscrições para não expor e-mails publicamente;
- Firestore Rules e Storage Rules.

## 1. Configuração do Firebase Authentication

No Firebase Console:

1. Abra **Authentication > Sign-in method**.
2. Ative **Email/Password**.
3. Ative **Email link (passwordless sign-in)**.
4. Em **Authentication > Settings > Authorized domains**, mantenha o domínio do Firebase Hosting e adicione o domínio institucional quando o site estiver usando domínio próprio.
5. Em produção, use HTTPS.

O fluxo usado pela aplicação é passwordless: o administrador informa seu e-mail ATI e recebe um link de acesso. O e-mail é verificado pelo próprio fluxo do Firebase.

## 2. Primeiro administrador (bootstrap)

Antes do primeiro acesso administrativo, crie manualmente no Firestore:

Coleção:
`administradores`

Documento:
`seu.email@ati.pe.gov.br`

Campos:

```text
nome: "Nome do administrador"
role: "admin"
ativo: true
```

Depois disso, esse usuário poderá entrar em `/admin` e cadastrar os demais e-mails autorizados.

A aplicação não cria contas do Firebase Authentication pela tela de usuários. Isso é proposital: a tela administra a autorização; a autenticação acontece por link no e-mail institucional.

## 3. Firestore

Crie/ative o Cloud Firestore e publique:

```bash
firebase deploy --only firestore
```

As regras são fechadas por padrão e somente os caminhos explicitamente liberados ficam acessíveis.

## 4. Storage

Ative o Cloud Storage e publique:

```bash
firebase deploy --only storage
```

As regras usam autorização cruzada com Firestore para verificar se o e-mail autenticado é um editor/admin ativo.

Ao salvar um evento novo, a aplicação primeiro gera o ID do documento, depois envia o card para:

```text
event-cards/{eventId}/card.{ext}
```

## 5. Hosting

Instale o Firebase CLI e faça login:

```bash
npm install -g firebase-tools
firebase login
```

Depois:

```bash
firebase use calendario-eventos-ati
firebase deploy --only hosting
```

Ou tudo de uma vez:

```bash
firebase deploy
```

## 6. Estrutura do banco

### eventos/{eventId}

Campos principais:

```text
title
categoria
start
end
local
limiteVagas
linkMeet
convidados
realizador
palestrantes
temCertificado
horasCertificado
linkMaterial
descricao
publicado
status
linkConviteImg
imagemStoragePath
criadoEm
criadoPor
atualizadoEm
```

### inscricoes/{registrationId}

O ID é SHA-256 de `eventId|email`, evitando duplicidade simples sem expor o e-mail na URL.

```text
eventId
nome
email
criadoEm
presente
```

A coleção de inscrições NÃO possui leitura pública.

### administradores/{email}

```text
nome
role: "editor" | "admin"
ativo: true | false
atualizadoEm
```

## 7. Importante sobre as inscrições

A inscrição pública é intencionalmente aberta, mas qualquer formulário público pode sofrer spam. Para produção institucional, recomendo ativar Firebase App Check e/ou adicionar CAPTCHA/reCAPTCHA antes de abrir a inscrição ao público.

Também é recomendável manter regras de retenção e tratamento de dados compatíveis com a política institucional/LGPD.

## 8. Card do evento

Orientação apresentada ao usuário:

- recomendado: **1080 × 1350 px**
- proporção: **4:5**
- mínimo aceito pelo frontend: **800 × 1000 px**
- formatos: JPG, PNG ou WebP
- tamanho máximo: **2 MB**

O Storage também impõe o limite de 2 MB e restringe o tipo MIME.

## 9. Domínio público

O sistema pode ser divulgado assim:

```text
https://SEU-DOMINIO/
```

Evento específico:

```text
https://SEU-DOMINIO/evento/ID_DO_EVENTO
```

Área administrativa:

```text
https://SEU-DOMINIO/admin
```

O ID do evento é estável e funciona como link público permanente.

## 10. Observação sobre domínio @ati.pe.gov.br

A aplicação valida o domínio no frontend para melhorar a experiência, mas a segurança real está nas Security Rules:

- e-mail autenticado precisa ser verificado;
- precisa terminar em `@ati.pe.gov.br`;
- precisa existir na coleção `administradores`;
- precisa estar `ativo: true`;
- o papel precisa ser `editor` ou `admin`.

Assim, esconder botões no navegador não é a camada de segurança.

## 11. Observação sobre o código original

A versão original permitia que qualquer usuário autenticado no Google chegasse às funções administrativas e também armazenava a lista de inscritos dentro do documento público do evento. Esta versão separa essas responsabilidades.

O calendário público lê somente eventos publicados e as inscrições ficam em uma coleção privada para evitar exposição dos e-mails dos participantes.

## 12. Deploy recomendado

```bash
firebase deploy --only firestore,storage,hosting
```

Antes do primeiro deploy em produção, teste no Emulator Suite e valide as Rules com casos:

- visitante lê evento publicado: permitido;
- visitante lê evento não publicado: negado;
- visitante cria inscrição válida: permitido;
- visitante lê inscrição: negado;
- editor cria/edita evento: permitido;
- editor exclui evento: negado;
- admin exclui evento: permitido;
- editor gerencia usuários: negado;
- admin gerencia usuários: permitido;
- usuário `@gmail.com` tenta administração: negado;
- usuário `@ati.pe.gov.br` não cadastrado: negado.
