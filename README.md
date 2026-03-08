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

## Transformer en allocation dynamique 

1. Construction et dessin du BST

En insérant la séquence (10, 2, 25, 15, 30, 12, 20, 16, 24, 17, 22) dans un arbre vide, on place chaque élément à gauche s'il est plus petit que le nœud courant, et à droite s'il est plus grand. L'arbre résultant peut être observée dans le dossier arbres adjoints sous le nom result_inser


2. Résultat avec une séquence initiale triée

Si la séquence initiale avait été triée, l'arbre résultant aurait été un arbre dit dégénéré (ou pathologique) ressemblant à une simple liste chaînée. Chaque nouveau nœud inséré étant strictement supérieur au précédent, il aurait été systématiquement placé comme enfant droit. La hauteur de l'arbre aurait alors été égale au nombre d'éléments insérés, ce qui est le pire cas possible, dégradant les performances des futures opérations de recherche qui passeraient d'une efficacité logarithmique à une efficacité linéaire.

3. Fonction d'allocation node_new

```c
#include <stdlib.h>

struct tree_node *node_new(int data) {
    // Allocation sur le tas
    struct tree_node *new_node = malloc(sizeof(struct tree_node));
    
    // Vérification que l'allocation a réussi
    if (!new_node) {
        return NULL; 
    }
    
    // Initialisation des champs
    new_node->data = data;
    new_node->parent = NULL;
    new_node->left = NULL;
    new_node->right = NULL;
    
    return new_node;
}

```

4. Fonction d'insertion bst_insert 

```c
struct tree_node *bst_insert(struct tree_node **root, int data) {
    struct tree_node *new_node = node_new(data);
    if (!new_node) return NULL; // Échec d'allocation

    // Cas d'un arbre vide
    if (*root == NULL) {
        *root = new_node;
        return new_node;
    }

    struct tree_node *current = *root;
    struct tree_node *parent = NULL;

    // Recherche de la position d'insertion
    while (current != NULL) {
        parent = current;
        if (data < current->data) {
            current = current->left;
        } else if (data > current->data) {
            current = current->right;
        } else {
            // Le nœud existe déjà (règle des BST stricts), on annule et on libère
            free(new_node);
            return current; 
        }
    }

    // Liaison du nouveau nœud avec son parent
    new_node->parent = parent;
    if (data < parent->data) {
        parent->left = new_node;
    } else {
        parent->right = new_node;
    }

    return new_node;
}

```
## Fonctions de traversée d’arbre itératives

1. Version itérative de preorder

```c
void preorder_iterative(struct tree_node *root, process_fn process) {
    struct tree_node *curr = root, *prev = NULL, *next = NULL;
    while (curr) {
        if (prev == curr->parent) { // On descend
            process(curr);
            next = curr->left ? curr->left : (curr->right ? curr->right : curr->parent);
        } else if (prev == curr->left) { // On remonte de la gauche
            next = curr->right ? curr->right : curr->parent;
        } else { // On remonte de la droite
            next = curr->parent;
        }
        prev = curr;
        curr = next;
    }
}

```

---

2. Version itérative de inorder 


```c
void inorder_iterative(struct tree_node *root, process_fn process) {
    struct tree_node *curr = root, *prev = NULL, *next = NULL;
    while (curr) {
        if (prev == curr->parent) { // On descend
            if (curr->left) next = curr->left;
            else { process(curr); next = curr->right ? curr->right : curr->parent; }
        } else if (prev == curr->left) { // On remonte de la gauche
            process(curr);
            next = curr->right ? curr->right : curr->parent;
        } else { // On remonte de la droite
            next = curr->parent;
        }
        prev = curr;
        curr = next;
    }
}

```

3. Stratégie générale 

L'algorithme agit comme une machine à états grâce aux pointeurs `curr` et `prev`. En comparant `prev` avec le parent ou les enfants de `curr`, on sait exactement dans quelle direction on voyage (descente, remontée par la gauche, remontée par la droite), ce qui permet de naviguer sans jamais utiliser de pile de mémorisation.


4. Comparaison de la complexité 

 Complexité temporelle : Identique. $O(n)$ dans les deux cas car on visite chaque lien un nombre constant de fois.
 Complexité spatiale : C'est ici que l'itératif gagne. La version récursive nécessite $O(h)$ d'espace en mémoire (où $h$ est la hauteur de l'arbre) à cause de la pile d'appels. Notre version itérative nécessite un espace de $O(1)$, car elle n'utilise que trois pointeurs locaux, quelle que soit la taille de l'arbre.


