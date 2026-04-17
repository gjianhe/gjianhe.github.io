# C语言-链表翻转


```c
typedef struct node_s
{
    int item;
    struct node_s *next;
} node_t;

node_t *reverse_list(node_t *head)
{
    node_t *n = head;
    node_t *tmp;
    head = NULL;

    while (n)
    {
        tmp = n;
        n = n->next;
        tmp->next = head;
        head = tmp;
    }

    return head;
}

int main()
{
    node_t node[3];
    node_t *p_node;
    p_node = &node[0];

    for (int i = 0; i < 3; i++)
    {
        node[i].item = i + 1;
        node[i].next = &node[i + 1];
    }

    p_node = reverse_list(&node[0]);

    for (p_node; p_node; p_node = p_node->next)
    {
        printf("%d\n", p_node->item);
        p_node = p_node->next;
    }

    return 0;
}

```