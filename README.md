# tp-data-struct-bst-avl-btree_Gaussot_Ribaute

# FIFO et LIFO
## Insertion

1. Donnez la séquence de valeurs affichées si S se comporte comme une pile (LIFO). 

La séquence de valeurs affichées si s se comporte comme une pile sera 5, 4, 3, 2, 1


2. Donnez la séquence de valeurs affichées si S se comporte comme une file (FIFO).

La séquence de valeurs afichées si s se comporte comme une file sera 1, 2, 3, 4, 5.

3. Que ce passe t’il si on inverse les opérations de push ? 5, 4, 3, 2, 1 ?

Si on inverse les opérations de push, on aura 1, 2, 3, 4, 5 si s se comporte comme une pile et 5, 4, 3, 2, 1 si s se comporte comme une file .

## Evaluation d'expression

//Arthur si tu peux faire ça

## BST
# Arbre
Les différents arbres sot disponbiles à la consultation dans le dossier associer

# Main de base avec arbre statique

1. Limites de la construction sur la pile et compatibilité avec malloc/free

Cette construction présente de fortes limites pour l'ajout ou la suppression de nœuds car elle est à la fois temporaire et figée. Les nœuds créés de cette façon sont détruits automatiquement à la fin de la fonction où ils sont déclarés, on ne peut donc pas utiliser une fonction externe pour ajouter un nouveau nœud. 
De plus, l'arbre étant statique, il est très difficile de le faire grandir ou de le modifier pendant que le programme tourne. Concernant la compatibilité avec malloc et free, elle est totalement inexistante. La commande free ne doit être utilisée que sur de la mémoire créée dynamiquement avec malloc. 
Faire un free sur cet arbre codé en dur fera planter le programme, et le fait de mélanger des nœuds statiques et dynamiques rendrait de toute façon le nettoyage de la mémoire impossible à gérer correctement.

2. code 
```c
#include <stdio.h>

struct tree_node {
  int data;
  struct tree_node *parent;
  struct tree_node *left;
  struct tree_node *right;
};

typedef struct tree_node TreeNode;

int main(void) {
    TreeNode root = {
      .data = 'F',
      .left = &(TreeNode){
        .data = 'B',
        .left = &(TreeNode){ .data = 'A', .left = NULL, .right = NULL, },
        .right = &(TreeNode){
          .data = 'D',
          .left = &(TreeNode){ .data = 'C', .left = NULL, .right = NULL, },
          .right = &(TreeNode){ .data = 'E', .left = NULL, .right = NULL, },
        },
      },
      .right = &(TreeNode){ 
        .data = 'G',
        .left = NULL,
        .right = &(TreeNode){
          .data = 'I',
          .left = &(TreeNode){ .data = 'H', .left = NULL, .right = NULL, },
          .right = NULL,
        },
      },
    };

    return 0;
```

3. Est-ce un BST en supposant l’ordre alphabétique ?

Oui, cet arbre est bien un Arbre Binaire de Recherche (BST). Dans un BST, pour chaque nœud, toutes les valeurs situées dans le sous-arbre gauche doivent être strictement plus petites, et celles du sous-arbre droit strictement plus grandes.
En se basant sur l'ordre alphabétique des codes ASCII, cette règle est respectée à chaque niveau de la structure.

## Fonctions de traversée d’arbre récursives

1. Adaptation des fonctions avec process_fn

```c
void preorder(struct tree_node *n, process_fn process) {
    if (!n) return;
    process(n);
    preorder(n->left, process);
    preorder(n->right, process);
}

void inorder(struct tree_node *n, process_fn process) {
    if (!n) return;
    inorder(n->left, process);
    process(n);
    inorder(n->right, process);
}

void postorder(struct tree_node *n, process_fn process) {
    if (!n) return;
    postorder(n->left, process);
    postorder(n->right, process);
    process(n);
}

```

2. Écriture du callback print_node 


```c
void print_node(struct tree_node *n) {
    if (n) {
        printf("%c ", n->data);
    }
}

```
Résultat du parcours infixe sur l’arbre précédent

Le parcours infixe d'un Arbre Binaire de Recherche explore toujours le sous-arbre gauche en premier, traite ensuite le nœud courant, et termine par le sous-arbre droit, ce qui a pour propriété fondamentale de restituer les valeurs dans leur ordre croissant naturel.
En appliquant ce parcours à notre arbre précédent construit avec des lettres, on parcourt la structure de gauche à droite et cela donne tout simplement la séquence alphabétique parfaitement triée : A B C D E F G H I.
