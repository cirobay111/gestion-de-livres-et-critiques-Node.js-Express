# Bookstore Project

API REST pour la gestion d'une librairie avec authentification utilisateur.

## Installation

```bash
npm install
```

## Démarrage du serveur

```bash
npm start
```

Le serveur démarre sur `http://localhost:3000`

## Structure du projet

```
bookstore-project/
├── package.json
├── server.js
├── routes/
│   ├── general.js         # Routes accessibles à tous les utilisateurs
│   ├── users.js           # Routes pour les utilisateurs enregistrés
├── controllers/
│   ├── generalController.js
│   ├── userController.js
├── middleware/
│   └── auth.js            # Middleware d'authentification JWT
├── data/
│   └── books.json         # Données des livres (simule la base de données)
└── axiosMethods/
    ├── getAllBooks.js
    ├── getBookByISBN.js
    ├── getBookByAuthor.js
    └── getBookByTitle.js
```

## Endpoints API

### Utilisateurs généraux

#### Tâche 1: Obtenir la liste des livres disponibles
```
GET /api/books
```

#### Tâche 2: Obtenir les livres en fonction de l'ISBN
```
GET /api/books/isbn/:isbn
```

#### Tâche 3: Obtenir tous les livres par auteur
```
GET /api/books/author/:author
```

#### Tâche 4: Obtenir tous les livres en fonction du titre
```
GET /api/books/title/:title
```

#### Tâche 5: Obtenir la critique du livre
```
GET /api/books/isbn/:isbn/review
```

#### Tâche 6: Enregistrer un nouvel utilisateur
```
POST /api/register
Body: {
  "username": "string",
  "password": "string",
  "email": "string"
}
```

#### Tâche 7: Se connecter en tant qu'utilisateur enregistré
```
POST /api/login
Body: {
  "username": "string",
  "password": "string"
}
Response: {
  "token": "JWT_TOKEN",
  "user": {...}
}
```

### Utilisateurs enregistrés (nécessite authentification)

Toutes les routes nécessitent un header d'authentification:
```
Authorization: Bearer YOUR_JWT_TOKEN
```

#### Tâche 8: Ajouter/modifier une critique de livre
```
PUT /api/users/books/isbn/:isbn/review
Body: {
  "rating": 1-5,
  "comment": "string"
}
```

#### Tâche 9: Supprimer une critique de livre
```
DELETE /api/users/books/isbn/:isbn/review
```

## Méthodes Axios

### Tâche 10: Obtenir tous les livres (async callback)
```javascript
const getAllBooks = require('./axiosMethods/getAllBooks');

getAllBooks((error, books) => {
  if (error) {
    console.error(error);
  } else {
    console.log(books);
  }
});
```

### Tâche 11: Recherche par ISBN (Promises)
```javascript
const getBookByISBN = require('./axiosMethods/getBookByISBN');

getBookByISBN('978-0-123456-78-9')
  .then(book => console.log(book))
  .catch(error => console.error(error));
```

### Tâche 12: Recherche par auteur (async/await)
```javascript
const getBookByAuthor = require('./axiosMethods/getBookByAuthor');

(async () => {
  try {
    const books = await getBookByAuthor('John Smith');
    console.log(books);
  } catch (error) {
    console.error(error);
  }
})();
```

### Tâche 13: Recherche par titre (async/await)
```javascript
const getBookByTitle = require('./axiosMethods/getBookByTitle');

(async () => {
  try {
    const books = await getBookByTitle('Adventure');
    console.log(books);
  } catch (error) {
    console.error(error);
  }
})();
```

## Exemples d'utilisation

### Enregistrement d'un utilisateur
```bash
curl -X POST http://localhost:3000/api/register \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"test123","email":"test@example.com"}'
```

### Connexion
```bash
curl -X POST http://localhost:3000/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"test123"}'
```

### Ajouter une critique (avec token)
```bash
curl -X PUT http://localhost:3000/api/users/books/isbn/978-0-123456-78-9/review \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"rating":5,"comment":"Excellent book!"}'
```

## Utilisateur de test

Un utilisateur de test est inclus dans `data/books.json`:
- **Username:** `alice123`
- **Password:** `password123`
- **Email:** `alice@example.com`

Vous pouvez utiliser ces identifiants pour tester la connexion et les fonctionnalités réservées aux utilisateurs enregistrés.

## Technologies utilisées

- Node.js
- Express.js
- Axios
- JWT (jsonwebtoken)
- bcryptjs
- body-parser

