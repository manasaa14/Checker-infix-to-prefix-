# Checker-infix-to-prefix-
Convert infix to prefix using c program 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define MAX 100

char stack[MAX];
int top = -1;

// Stack operations
void push(char c) {
    if (top == MAX - 1) {
        printf("Stack Overflow\n");
        return;
    }
    stack[++top] = c;
}

char pop() {
    if (top == -1) {
        return '\0'; // Stack underflow
    }
    return stack[top--];
}

char peek() {
    if (top == -1)
        return '\0';
    return stack[top];
}

// Function to check precedence
int precedence(char c) {
    switch (c) {
        case '+':
        case '-': return 1;
        case '*':
        case '/': return 2;
        case '^': return 3;
        default: return 0;
    }
}

// Function to check if character is operator
int isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '^');
}

// Function to reverse a string
void reverseString(char str[]) {
    int n = strlen(str);
    for (int i = 0; i < n / 2; i++) {
        char temp = str[i];
        str[i] = str[n - i - 1];
        str[n - i - 1] = temp;
    }
}

// Function to convert infix to prefix
void infixToPrefix(char infix[], char prefix[]) {
    char revInfix[MAX], tempPrefix[MAX];
    int k = 0;
    
    strcpy(revInfix, infix);
    reverseString(revInfix);

    // Swap '(' with ')' after reversing
    for (int i = 0; revInfix[i]; i++) {
        if (revInfix[i] == '(') revInfix[i] = ')';
        else if (revInfix[i] == ')') revInfix[i] = '(';
    }

    top = -1; // Reset stack
    for (int i = 0; revInfix[i]; i++) {
        char c = revInfix[i];

        if (isalnum(c)) {
            tempPrefix[k++] = c; // Operand goes to output
        } else if (c == '(') {
            push(c);
        } else if (c == ')') {
            while (top != -1 && peek() != '(')
                tempPrefix[k++] = pop();
            pop(); // Remove '('
        } else if (isOperator(c)) {
            while (top != -1 && precedence(peek()) >= precedence(c))
                tempPrefix[k++] = pop();
            push(c);
        }
    }

    while (top != -1)
        tempPrefix[k++] = pop();

    tempPrefix[k] = '\0';
    reverseString(tempPrefix);
    strcpy(prefix, tempPrefix);
}

int main() {
    char infix[MAX], prefix[MAX];

    printf("Enter an infix expression: ");
    scanf("%s", infix);

    infixToPrefix(infix, prefix);

    printf("Prefix expression: %s\n", prefix);
    return 0;
}
