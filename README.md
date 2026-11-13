# Administração da Baratinho de Angola

Painel administrativo independente do marketplace público.

- URL personalizada: https://admin.baratinho.apsanlda.com/
- Repositório: https://github.com/mateus-max/Administracao-da-Baratinho
- Firebase: projecto `baratinho-de-angola`
- Base de dados: Firebase Realtime Database
- Autenticação: Firebase Authentication (e-mail/palavra-passe)
- Não utiliza Firebase Storage, Firestore, Local Storage ou IndexedDB para persistência da aplicação.

## Funcionalidades do painel

- Login administrativo sem registo público.
- Verificação de autorização em `admins/{uid}` com `active: true`.
- Dashboard com totais de utilizadores, vendedores, produtos, encomendas, pagamentos e reclamações.
- Aprovação, rejeição e suspensão de vendedores.
- Consulta de utilizadores.
- Moderação de produtos.
- Consulta e gestão de encomendas.
- Consulta de registos de pagamentos (sem declarar liquidação bancária).
- Consulta de expedições e reclamações.
- Consulta de avaliações.
- Gestão de categorias.
- Configurações de comissão e manutenção.
- Consulta de registos de auditoria.

## Preparação obrigatória no Firebase

1. Activar o fornecedor E-mail/palavra-passe no Firebase Authentication.
2. Adicionar `admin.baratinho.apsanlda.com` aos domínios autorizados do Authentication.
3. Criar a conta administrativa no Authentication.
4. Copiar o UID dessa conta.
5. Na consola do Realtime Database, criar manualmente:
   
   ```json
   admins
     UID_DA_CONTA
       active: true
       name: "Administrador principal"
   ```

6. As regras do Realtime Database têm de permitir leitura e escrita administrativa apenas quando `root.child('admins').child(auth.uid).child('active').val() === true`. Nunca activar permissões administrativas através do formulário público.
7. Configurar GitHub Pages para publicar a partir da branch `main`, pasta raiz `/`, e confirmar que o domínio personalizado aponta para este repositório.

## Estrutura partilhada com o marketplace

O painel utiliza o mesmo projecto Firebase e os mesmos caminhos de dados do marketplace:

`admins`, `users`, `sellers`, `products`, `orders`, `payments`, `shipments`, `complaints`, `reviews`, `categories`, `settings`, `auditLogs`, `transactions`, `refunds`, `transportCompanies` e `commissions`.

## Limitações e segurança

- A aplicação usa `inMemoryPersistence`; a sessão não é mantida depois de fechar ou actualizar a página.
- As operações administrativas dependem das regras do Realtime Database. Testar todas as permissões no Firebase Emulator antes de produção.
- Não existe integração de pagamento, custódia, reembolso bancário ou transferência ao vendedor nesta versão.
- O painel não deve ser utilizado para confirmar pagamentos com base apenas em dados inseridos pelo utilizador. A confirmação real deve vir do prestador financeiro através de integração autenticada e confiável.
- Não há criação de administradores pela interface. Adicionar ou revogar administradores directamente pela consola, com controlo de acesso rigoroso.
