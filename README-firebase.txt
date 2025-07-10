# Como usar o Firebase para perfis compartilháveis

1. **Crie uma conta no [Firebase](https://firebase.google.com/).**
2. **Crie um novo projeto no console do Firebase.**
3. **No menu "Firestore Database", clique em "Criar banco de dados" e siga as instruções.**
4. **No menu "Authentication", ative apenas o método de login "Email/senha" ou "Anônimo" (NÃO ative Google).**
5. **No menu "Configurações do projeto" > "Suas apps", clique em "Adicionar app" (Web) e copie o código de configuração do Firebase.**

Exemplo de configuração (adicione no `<head>` do seu HTML **antes de usar o Firebase**):

```html
<!-- Adicione estes scripts no seu HTML -->
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
<script>
  // Substitua pelos dados do seu projeto
  const firebaseConfig = {
    apiKey: "SUA_API_KEY",
    authDomain: "SEU_PROJETO.firebaseapp.com",
    projectId: "SEU_PROJETO",
    storageBucket: "SEU_PROJETO.appspot.com",
    messagingSenderId: "SEU_ID",
    appId: "SEU_APP_ID"
  };
  firebase.initializeApp(firebaseConfig);
  const db = firebase.firestore();
  const auth = firebase.auth();
</script>
```

6. **No seu código, use autenticação por e-mail/senha ou anônima:**
   - Para autenticação anônima:
```js
auth.signInAnonymously().then(userCredential => {
  // Usuário autenticado anonimamente
  const userId = userCredential.user.uid;
  // ...salve/busque perfil usando userId...
});
```
   - Para autenticação por e-mail/senha:
```js
auth.createUserWithEmailAndPassword(email, senha).then(userCredential => {
  // Usuário registrado
});
auth.signInWithEmailAndPassword(email, senha).then(userCredential => {
  // Usuário logado
});
```

7. **Salve e busque perfis do Firestore usando o ID do usuário.**
   - Exemplo de salvar perfil:
```js
// Salvar perfil
db.collection('perfis').doc(userId).set({
  nome: 'Seu Nome',
  avatar: 'url_da_foto',
  badges: ['beta', 'fundador']
});
```
   - Exemplo de buscar perfil:
```js
// Buscar perfil
db.collection('perfis').doc(userId).get().then(doc => {
  if (doc.exists) {
    const perfil = doc.data();
    // Exiba os dados do perfil
  }
});
```

8. **Para compartilhar, gere um link como `perfil.html?id=USER_ID` e, ao abrir, busque o perfil pelo ID na URL.**

> **Resumo:**  
> - Crie projeto no Firebase  
> - Ative Firestore e Authentication (apenas Email/senha ou Anônimo)  
> - Adicione o SDK no HTML  
> - Salve/busque perfis pelo Firestore  
> - Compartilhe o link com o ID do perfil

**Procure por tutoriais de "Firebase Firestore Web" e "Firebase Auth Email/Senha" para exemplos completos.**
