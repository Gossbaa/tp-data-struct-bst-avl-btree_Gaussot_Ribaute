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

1. Lecture d'un operande : On l'empile (push) directement sur la pile.

*Lecture d'un opérateur (+, -, , /) :

    On dépile (pop) la valeur au sommet de la pile. Elle devient l'opérande droit.

    On dépile (pop) la nouvelle valeur au sommet. Elle devient l'opérande gauche.

    On applique l'opérateur sur ces deux valeurs.

    On empile (push) le résultat de cette opération sur la pile.

 Il ne reste qu'une seule valeur dans la pile, qui est le résultat final de l'évaluation.

 2. Empile le nombre 3	[3]
	Empile le nombre 4	[3, 4]
	Dépile 4 (droit), dépile 3 (gauche), calcule 3 + 4 = 7, empile 7	[7]
	Empile le nombre 2	[7, 2]
	Dépile 2 (droit), dépile 7 (gauche), calcule 7 * 2 = 14, empile 14	[14]

3. code 
```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

int eval_postfix(const char *expr) {
    int stack[100]; 
    int top = -1;
    const char *p = expr;

    while (*p != '\0') {
        // Ignorer les espaces
        if (isspace(*p)) {
            p++;
            continue;
        }

        // Si c'est un chiffre, on lit le nombre entier et on l'empile
        if (isdigit(*p)) {
            int val = strtol(p, (char **)&p, 10);
            stack[++top] = val; // push
        } 
        // Si c'est un opérateur
        else {
            int b = stack[top--]; 
            int a = stack[top--]; 
            
            switch (*p) {
                case '+': stack[++top] = a + b; break;
                case '-': stack[++top] = a - b; break;
                case '*': stack[++top] = a * b; break;
                case '/': stack[++top] = a / b; break;
            }
            p++; 
        }
    }
    
    
    return stack[top];
}
```

4. La difference est dans la gestion de la memoire, la solution classique  en C a une complexite de O(1) tandis que la solution IA a une complexite spatiale de O(n).

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



## Mise en œuvre de tree_free 

Pour libérer l'arbre correctement, il est impératif d'utiliser le parcours postfixe (postorder). Cela garantit que l'on détruit d'abord les enfants (gauche puis droite) avant de détruire le nœud parent, évitant ainsi d'accéder à des pointeurs de mémoire qui ont déjà été libérés.


```c
#include <stdlib.h>

// 1. Le callback qui libère un nœud
void free_node(struct tree_node *n) {
    if (n) {
        free(n);
    }
}

// 2. La fonction principale
void tree_free(struct tree_node *subroot) {
    // On utilise la fonction postorder définie précédemment
    postorder(subroot, free_node);
}

```
## Mettre en œuvre tree_delete

1. Implémentation de tree_delete 


```c
struct tree_node *tree_minimum(struct tree_node *node) {
    while (node->left != NULL) node = node->left;
    return node;
}

void transplant(struct tree_node **root, struct tree_node *u, struct tree_node *v) {
    if (u->parent == NULL) *root = v;
    else if (u == u->parent->left) u->parent->left = v;
    else u->parent->right = v;
    if (v != NULL) v->parent = u->parent;
}

struct tree_node *tree_delete(struct tree_node **root, int key) {
    // 1. Recherche du nœud
    struct tree_node *z = *root;
    while (z != NULL && z->data != key) {
        if (key < z->data) z = z->left;
        else z = z->right;
    }
    if (z == NULL) return NULL; 

    // 2. Suppression selon les 3 cas
    if (z->left == NULL) {
        transplant(root, z, z->right);
    } else if (z->right == NULL) { 
        transplant(root, z, z->left);
    } else { 
        struct tree_node *y = tree_minimum(z->right); 
        if (y->parent != z) {
            transplant(root, y, y->right);
            y->right = z->right;
            y->right->parent = y;
        }
        transplant(root, z, y);
        y->left = z->left;
        y->left->parent = y;
    }
    
    z->parent = z->left = z->right = NULL; 
    return z; 
}

```

2. Évolution de l'arbre après suppressions

Après `tree_delete(&root, 24)` : voir dossier arbre


Après `tree_delete(&root, 15)` : voir dossier arbre


3. Commutativité de la suppression 

La suppression dans un Arbre Binaire de Recherche n'est généralement pas commutative. L'ordre dans lequel on supprime les éléments modifie la structure finale de l'arbre.

Si vous supprimez un nœud $X$ qui a deux enfants, il est remplacé par son successeur $Y$. Si vous aviez supprimé $Y$ en premier, le successeur de $X$ aurait été un autre nœud $Z$. La topologie de l'arbre (qui est le parent de qui, et la hauteur des branches) dépend donc fortement de l'ordre d'exécution des suppressions.

## Variante de tree_delete avec prédécesseur

1. Variante avec le prédécesseur


```c
struct tree_node *tree_maximum(struct tree_node *node) {
    while (node->right != NULL) node = node->right;
    return node;
}

// Dans tree_delete, on remplace le cas à 2 enfants par ceci :
} else { 
    struct tree_node *y = tree_maximum(z->left); // Prédécesseur
    if (y->parent != z) {
        transplant(root, y, y->left);
        y->left = z->left;
        y->left->parent = y;
    }
    transplant(root, z, y);
    y->right = z->right;
    y->right->parent = y;
}

```


2. Comparaison des suppressions successives

En partant du BST de référence initial, on supprime 25, puis 20, puis 10.

Avec le successeur :

* 25 est remplacé par 30.
* 20 est remplacé par 22.
* 10 est remplacé par 12.
L'arbre a tendance à se "vider" par la droite. Les éléments du sous-arbre droit remontent continuellement, ce qui crée un déséquilibre visuel : l'arbre devient plus lourd et plus profond sur son flanc gauche.

Avec le prédécesseur :

* 25 est remplacé par 24.
* 20 est remplacé par 17.
* 10 est remplacé par 2.
Le phénomène inverse se produit. L'arbre se vide par la gauche et devient asymétrique en s'allongeant vers la droite (surtout visible avec la racine 10 remplacée par 2, qui n'a pas d'enfant gauche).

---

3. Suppression aléatoire

```c
#include <stdlib.h> // Pour rand()

// Dans tree_delete, cas à 2 enfants :
} else {
    struct tree_node *y;
    if (rand() % 2 == 0) {
        // Logique du successeur (minimum à droite)
        y = tree_minimum(z->right);
        // ... (code de transplantation du successeur)
    } else {
        // Logique du prédécesseur (maximum à gauche)
        y = tree_maximum(z->left);
        // ... (code de transplantation du prédécesseur)
    }
}

```

Intérêt de la randomisation :
La suppression asymétrique classique (algorithme de Hibbard, qui utilise toujours le successeur) est connue pour dégrader progressivement la structure de l'arbre au fil de nombreuses insertions et suppressions. La hauteur de l'arbre tend à s'allonger vers $O(\sqrt{n})$ au lieu de rester proche de $O(\log n)$.
En choisissant aléatoirement entre le successeur et le prédécesseur, on répartit les réaménagements structurels des deux côtés. Cela empêche l'arbre de pencher systématiquement d'un côté et permet de conserver statistiquement une forme globale beaucoup mieux équilibrée.


# Arbres AVL

## Questions

1. Non les deux arbres ne satisfont pas la condition AVL car  la difference de hauteur entre le sous arbre de gauche et le sous arbre de droit  pour  certains  noeuds est superieur a 1. Par exemple, pour les deux arbres, il y a un probleme au niveau du noeud 25 avec  des differences de hauteur respectivement de 2 et 3.

3. Pour respecter la condition AVL des noeuds pour un arbre de hauteur h, les sous arbres doivent avoir  des hauteurs de h-1 et h-2.
Le nombre total de nœuds est donc : N(h)=1+N(h−1)+N(h−2).
On sait que N(h−1)>N(h−2) car la fonction est croissante.
On peut donc ecrire que : N(h)>2⋅N(h−2) et on peut arrriver a N(h)>2h/2.
Pour un arbre AVL  contenant n nœuds, on a  n≥N(h)>2h/2.

En appliquant la fonction log en base 2, cela devient log2​(n)>2h​.

On obtient h<2⋅log2​(n), ce qui prouve mathématiquement que la hauteur est en Olog​(n).

4. Comme explique precedemment il s agit de la fonction log en base 2.

5. Cette distinction n'affecte pas la classe de complexite car la notation O ignore les constantes multiplicative et que l on passe de  log base 2 a log base 10 par une constante  multiplicative.


## Rotations

1. Soit FE le fqcteur d'equilibre d'un noeud.
Precondition cas LL : FE(x)=2 et FE(x→left)≥0
Precondition cas RR : FE(x)=−2 et FE(x→right)≤0
Precondition cas LR : FE(x)=2 et FE(x→left)<0
Precondition cas RL : FE(x)=−2 et FE(x→right)>0

2. Lors d'une insertion, la hauteur d'un sous-arbre peut augmenter de 1. Si cette augmentation provoque un déséquilibre au niveau d'un nœud ancêtre x , on applique la rotation appropriée.
Après cette rotation de rééquilibrage, la hauteur du sous-arbre  redevient exactement la même que ce qu'elle était avant l'insertion.
Puisque la hauteur globale de ce sous-arbre n'a pas changé du point de vue des nœuds supérieurs, le déséquilibre ne peut pas se propager plus haut vers la racine. Donc, une seule opération de rééquilibrage  suffit. Le nombre de rotations est donc borné par une constante.

# B-TREE
## Construction



## Nombre de cles et hauteur
