# NLW Expert (Node.js)

Um sistema de votação em tempo real onde os usuários podem criar uma enquete e outros usuários podem votar. O sistema gera um ranking entre as opções e atualiza os votos em tempo real.

## Funcionalidades ##

- Criar novas enquetes;\
- Editar enquete;\
- Deletar enquete;\
- Votar em enquete;\
- Ler informações de uma enquete e ver quantidades de votos;\
- Cada usuário só poderá voltar apenas um enquete;\

## Tecnologias ##

Abaixo segue uma lista das tecnologias utilizadas

- [Node.js](https://nodejs.org/en/)
- [FastAPI](https://fastapi.tiangolo.com/)
- [TypeScript](https://www.typescriptlang.org/)
- [Prisma](https://www.prisma.io/)
- [Zod](https://github.com/colinhacks/zod)
- [Redis](https://redis.io/)
- [PostgreSQL](https://www.postgresql.org/)
- [Docker](https://www.docker.com/)

## Requisitos

- Docker;
- Node.js;

## Setup

- Clone o repositorio;
- Instale as dependencias (`npm install`);
- Setup PostgreSQL e Redis (`docker compose up -d`);
- Copieo arquivo `.env.example` (`cp .env.example .env`);
- Rode a aplicação (`npm run dev`);
- Teste! (Recomendo testar com [Hoppscotch](https://hoppscotch.io/)).

## Rotas da aplicação ##

| Rotas                  | Métodos | Protocolo | Descrição                                                      |
|------------------------|---------|-----------|----------------------------------------------------------------|
| /polls                 | POST    | HTTP      | Cria uma nova enquete.                                         |
| /polls/:pollId         | GET     | HTTP      | Busca uma enquete específica.                                  |
| /polls/:pollId/vote    | POST    | HTTP      | Vota em uma opção da enquete.                                  |
| /polls/:pollId/results | --      | WS        | Abre uma conexão WebSocket que recebe os resultados dos votos. |

## HTTP

### POST `/polls`

Crie uma nova votação.

#### Request body

```json
{
  "title": "Qual a melhor linguagem de programação?",
  "options": [
    "JavaScript",
    "Java",
    "PHP",
    "C#"
  ]
}
```

#### Response body

```json
{
  "pollId": "194cef63-2ccf-46a3-aad1-aa94b2bc89b0"
}
```

### GET `/polls/:pollId`

Retornar dados de uma única votação.

#### Response body

```json
{
	"poll": {
		"id": "e4365599-0205-4429-9808-ea1f94062a5f",
		"title": "Qual a melhor linguagem de programação?",
		"options": [
			{
				"id": "4af3fca1-91dc-4c2d-b6aa-897ad5042c84",
				"title": "JavaScript",
				"score": 1
			},
			{
				"id": "780b8e25-a40e-4301-ab32-77ebf8c79da8",
				"title": "Java",
				"score": 0
			},
			{
				"id": "539fa272-152b-478f-9f53-8472cddb7491",
				"title": "PHP",
				"score": 0
			},
			{
				"id": "ca1d4af3-347a-4d77-b08b-528b181fe80e",
				"title": "C#",
				"score": 0
			}
		]
	}
}
```

### POST `/polls/:pollId/votes`

Adicionar um voto a uma votação específica.

#### Request body

```json
{
  "pollOptionId": "31cca9dc-15da-44d4-ad7f-12b86610fe98"
}
```

## WebSockets

### ws `/polls/:pollId/results`

#### Message

```json
{
  "pollOptionId": "da9601cc-0b58-4395-8865-113cbdc42089",
  "votes": 2
}
```
