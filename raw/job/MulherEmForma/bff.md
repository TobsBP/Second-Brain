Levantado via GitHub API (`Mulher-em-Forma/bff`) em 2026-07-21.

## README
Vazio no repositório — informação obtida via `package.json` (`name: mef-bff-graphql`) e listagem de pastas.

## O que é
BFF GraphQL principal, front do app mobile ("absorve várias APIs", conforme descrição do repo na org). Node.js + TypeScript, build via Babel, testes com Vitest.

`src/resolvers/`: Auth, Calendar, CategoriesRecipes, Challenge, Comments, Conquest, Diet, Friendship, FriendshipRequest, Group, History, Home, Lessons, LibraryCourses, Like, LiveClasses, MegaBrain, Menu, Notification, NutritionalAssistant, OnBoarding, Post, Profile, Quizzes, Recipes, Subscription, Support, Training, UserTerms, Xray.

`src/`: dataSources, firebase-service-account.json (não lido — credencial), index.ts, resolvers, tests, translations, types, utils.

## Observação
Ampla superfície de domínio (praticamente todos os módulos do app passam por aqui) — é o hub que agrega core, social, notifications, nutritional-assistant, etc. para o app mobile.
