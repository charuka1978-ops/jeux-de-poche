# Jeux de poche 🎮

Console de 12 mini-jeux LCD jouables dans le navigateur, sur téléphone ou ordinateur.

**Jouer en ligne** → https://charuka1978-ops.github.io/jeux-de-poche/

## Jeux disponibles

| Jeu | Description |
|-----|-------------|
| Serpent | Mange les pommes |
| Souris | Labyrinthe et chats |
| Pingouin | Lancer le plus loin |
| Cible | Vise le centre de la glace |
| Planeur | Vole et attrape les poissons |
| Briques | Casse toutes les briques |
| Glaçons | Remplis les lignes |
| Mémoire | Retrouve les paires |
| Sudoku | Grille de 1 à 9 |
| Morpion | Contre la console |
| Sauvetage | Rattrape les pingouins |
| Chat perché | Saute de toit en toit |

## Activer le classement mondial

Le classement partagé utilise [Supabase](https://supabase.com) (gratuit).

### 1. Créer un projet Supabase

1. Va sur https://supabase.com et crée un compte
2. **New project** → donne-lui un nom, un mot de passe, choisis une région proche
3. Attends ~1 min que le projet démarre

### 2. Créer la table des scores

Dans le projet Supabase, va dans **SQL Editor** et exécute :

```sql
create table scores (
  id          bigint generated always as identity primary key,
  game        text not null,
  pseudo      text not null,
  score       integer not null,
  created_at  timestamptz default now()
);

-- Lecture publique
create policy "lecture publique" on scores for select using (true);
-- Écriture publique (n'importe qui peut soumettre un score)
create policy "ecriture publique" on scores for insert with check (true);

alter table scores enable row level security;

-- Index pour les requêtes du classement
create index on scores (game, score desc);
```

### 3. Récupérer les clés

Dans **Project Settings → API** :
- **Project URL** → copie l'URL (ex: `https://abcdef.supabase.co`)
- **anon / public** → copie la clé

### 4. Coller les clés dans le jeu

Ouvre `index.html` et remplace les deux lignes :

```js
const SB_URL = '';   // ← colle ton Project URL ici
const SB_KEY = '';   // ← colle ta clé anon ici
```

### 5. Pousser sur GitHub

```bash
git add index.html
git commit -m "Activer classement Supabase"
git push
```

Le site se met à jour en quelques secondes. La prochaine fois qu'un joueur bat un record, il peut saisir son pseudo et figurer au classement mondial !

## Contribuer

Les suggestions et améliorations sont les bienvenues via Issues ou Pull Requests.
