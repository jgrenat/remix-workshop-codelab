---
sidebar_position: 1
---

# Récupération des données

Pour pouvoir afficher des données dynamique sur nos pages, nous allons devoir charger ces données. C'est le rôle de la fonction `loader` exportée dans un module route.

À noter que cette fonction n'est utilisée que côté serveur. Elle ne se retrouvera jamais dans un navigateur.

:::info Exercice
Définir un `loader` dans le module route d'une playlist. Afin d'afficher les informations suivants:

- Nom de la playlist
- Titre et auteur des chansons de la playlist.

:::

## Guide

💿 **Définir une fonction loader**

Ajouter le code suivant dans votre route

```tsx title="app/routes/_layout.playlists.$id.tsx"
export const loader = () => {
  return null;
};
```

💿 **Récuperer l'id de la playlist à afficher**

Remix appelle notre fonction `loader` avec différent données:

- une `request` objet `Request` de l'`API Fetch` [standard du web]
- les `params` correspondant aux segments dynamiques de l'url
- un `context` remix

Pour récupérer le segment dynamique de l'URL dans `params`.

```tsx title="app/routes/_layout.playlists.$id.tsx"
// highlight-next-line
export const loader = ({ params }: LoaderArgs) => {
  // highlight-next-line
  const id = params.id;

  return null;
};
```

💿 **Récuperer les données de la playlist**

Maintenant que nous avons l'id, nous allons pouvoir récuperer les données dans notre base de données.

Pour faire simple, nous allons directement appeler notre repository dans le loader. Pas de risque, la fonction n'existe que côté serveur.

:::tip
Vous pouvez utiliser le repository `playlists` pour retrouver les données d'une playlist.
:::

On aura donc le code suivant

```tsx title="app/routes/_layout.playlists.$id.tsx"
// highlight-next-line
export const loader = async ({ params }: LoaderArgs) => {
  const id = params.id;
  // highlight-next-line
  const playlist = await playlists.find(id || "");

  return null;
};
```

💿 **Retourner les données en réponse du loader**

```tsx title="app/routes/_layout.playlists.$id.tsx"
// highlight-next-line
export const loader = async ({ params }: LoaderArgs) => {
  const id = params.id;
  // highlight-next-line
  const playlist = await playlists.find(id || "");
  if (!playlist) {
    throw new Error("playlist not found");
  }

  return json(playlist);
};
```