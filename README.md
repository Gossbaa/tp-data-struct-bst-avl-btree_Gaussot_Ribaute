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

 
```
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
