#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

struct Node {
    char data;
    struct Node *left, *right;
};

struct Node *stack[100];
int top = -1;

void push(struct Node *node) {
    stack[++top] = node;
}

struct Node *pop() {
    return stack[top--];
}

struct Node *newNode(char data) {
    struct Node *node = (struct Node *)malloc(sizeof(struct Node));
    node->data = data;
    node->left = node->right = NULL;
    return node;
}

int evaluate(struct Node *root) {
    int left, right;

    if (root == NULL)
        return 0;

    if (isdigit(root->data))
        return root->data - '0';

    left = evaluate(root->left);
    right = evaluate(root->right);

    switch (root->data) {
        case '+': return left + right;
        case '-': return left - right;
        case '*': return left * right;
        case '/': return left / right;
    }

    return 0;
}

struct Node *buildTree(char postfix[]) {
    int i;
    struct Node *node;

    for (i = 0; postfix[i] != '\0'; i++) {
        node = newNode(postfix[i]);

        if (isdigit(postfix[i])) {
            push(node);
        } else {
            node->right = pop();
            node->left = pop();
            push(node);
        }
    }

    return pop();
}

int main() {
    char postfix[100];
    struct Node *root;

    printf("Enter postfix expression: ");
    scanf("%s", postfix);

    root = buildTree(postfix);

    printf("Result = %d\n", evaluate(root));

    return 0;
}