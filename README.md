
# type generic AVL tree
a pure C preprocesor based templete

##### how to use:
```c
#define [option 1]
#define [option 2]
       ...
#define [option N]
#include "avl_templete.h"
```

###### where [option 1..N] can be 
* AVL_TEMPLATE_PREFIX
    
    prefix added to every function and datatype. no defalut value, has to be defined before include
* AVL_KEY_TYPE
    
    key type, defaults to int
* AVL_VALUE_TYPE

    value type, defaults to int
* AVL_KEY_LE(a,b)
 
    lesser realation macro, defaults to (a<b)
* AVL_KEY_NEQ(a,b)

    inequal relation macro, defaults to (a!=b)
* AVL_SIZE_TRESHOLD_HIGH

    tree is reallocated if the current tree size multiplied by this value is larger then the current memory space. defaults to 1.00
* AVL_SIZE_TRESHOLD_LOW

    tree is reallocated if the current tree size multiplied by this value is smaller then the current memory space. defaults to 1.75
* AVL_SIZE_SIZE_HIGH

    if tree is resized up, the new size is equal to current size multiplied by this value. defaults to 1.50
* AVL_SIZE_SIZE_LOW

    if tree is resized down, the new size is equal to current size multiplied by this value. defaults to 1.25
* AVL_COPY_STACK_SIZE

    internal stack size. Should be not less then log2(2*(n+2)) where n is the maximum tree size. defaults to 64
* AVL_RESIZE_NONE

    disables tree resizeing. by default not defined
* AVL_RESIZE_UP

    tree only grows, is never resized down. by default not defined
* AVL_INCLUDE_BODY

    include c body after the header. by default not defined
* AVL_ALL_STATIC

    declare all functions static implies AVL_INCLUDE_BODY. by default not defined
    
###### note that:
* all options are automatically undefined
* file can be included multiply times in the same compilation block

###### example:
```c
//declare <char, int> tree
#define AVL_TEMPLATE_PREFIX		charavl
#define AVL_KEY_TYPE 			char*
#define AVL_KEY_NEQ(a,b)		(strcmp(a,b)!=0)
#define AVL_KEY_LE(a,b)			(strcmp(a,b)<0)
#define AVL_ALL_STATIC
#include "avl_templete.h"
//declare <int, int> tree
#define AVL_TEMPLATE_PREFIX		intavl
#define AVL_ALL_STATIC
#include "avl_templete.h"

int main(){
	intavl_tree_t avl1;
	charavl_tree_t avl2;
	intavl_create(&avl1,0);
	charavl_create(&avl2,0);
	intavl_set(&avl1,0,1);
	charavl_set(&avl2,"key",1);
}
```
###### templete declares following datatypes:

```c
typedef struct {
	int size; //current node count
} prefix_tree_t;

typedef struct { 
	key_type key;
	value_type value;
} prefix_node_t;

typedef *prefix_node_t prefix_t; //type returned by api functions
```

###### templete declares following function:
```c
//create tree with initial size. can be 0
void prefix_create(prefix_tree_t* tree, int init_size)
//remove all data from tree
void prefix_clear(prefix_tree_t* tree)
//destroy tree
void prefix_destroy(prefix_tree_t* tree)
//move to possibly small memory block
void prefix_compact(prefix_tree_t* tree)
//copy tree. does NOT preform destenation cleanup before copy
void prefix_copy(prefix_tree_t* src, prefix_tree_t* dst)
//remove node from tree
void prefix_removenode(prefix_tree_t* tree, prefix_t node)
//remove node from tree by key, return 1 on success
int prefix_remove(prefix_tree_t* tree, key_t key)
//insert node to tree. undefined behavior if key allready exists
prefix_t prefix_insert(prefix_tree_t* tree, key_t key, value_t value)
//find node by key, return NULL if node not in tree
prefix_t prefix_get(prefix_tree_t* tree, key_t key) 
//return node with smallest key
prefix_t prefix_first(prefix_tree_t* tree)
//return node with key next in order
prefix_t prefix_next(prefix_t n)
//bind value with key, insert if not exists, return node
prefix_t prefix_set(avltree_t* tree, key_t key, value_t value) 
//find node with key, if not found insert with value, return node
prefix_t prefix_addget(avltree_t* tree, key_t key, value_t value)
````
