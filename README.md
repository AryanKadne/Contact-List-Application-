# Contact-List-Application-
The Contact List Application is a C program that manages contacts using a binary tree. Users can add, delete, and search for contacts by name, and display them in sorted order. The program features a user-friendly command-line interface and performs operations efficiently with an average time complexity of O(log n) for insertion, deletion
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Node {
    char name[50];
    char phone[15];
    struct Node *left, *right;
} Node;

// Function to create a new node
Node* createNode(char name[], char phone[]) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    strcpy(newNode->name, name);
    strcpy(newNode->phone, phone);
    newNode->left = newNode->right = NULL;
    return newNode;
}

// Insert a new contact in the tree
Node* insertContact(Node* root, char name[], char phone[]) {
    if (root == NULL) return createNode(name, phone);

    if (strcmp(name, root->name) < 0)
        root->left = insertContact(root->left, name, phone);
    else if (strcmp(name, root->name) > 0)
        root->right = insertContact(root->right, name, phone);

    return root;
}

// In-Order traversal to display contacts
void displayContacts(Node* root) {
    if (root != NULL) {
        displayContacts(root->left);
        printf("Name: %s, Phone: %s\n", root->name, root->phone);
        displayContacts(root->right);
    }
}

// Search for a contact by name
Node* searchContact(Node* root, char name[]) {
    if (root == NULL || strcmp(root->name, name) == 0)
        return root;

    if (strcmp(name, root->name) < 0)
        return searchContact(root->left, name);

    return searchContact(root->right, name);
}

// Free memory allocated for the tree nodes
void freeTree(Node* root) {
    if (root != NULL) {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}

// Preload default contacts
Node* preloadContacts(Node* root) {
    root = insertContact(root, "Aryan", "9136065370");
    root = insertContact(root, "Heer", "808434349");
    return root;
}

int main() {
    Node* root = NULL;
    int choice;
    char name[50], phone[15];
    Node* contact;

    // Load default contacts
    root = preloadContacts(root);

    do {
        printf("\nContact List Application\n");
        printf("1. Add Contact\n");
        printf("2. Display Contacts\n");
        printf("3. Search Contact\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter name: ");
                scanf(" %[^\n]s", name);
                printf("Enter phone number: ");
                scanf("%s", phone);
                root = insertContact(root, name, phone);
                printf("Contact added successfully!\n");
                break;
            case 2:
                printf("Contact List:\n");
                displayContacts(root);
                break;
            case 3:
                printf("Enter name to search: ");
                scanf(" %[^\n]s", name);
                contact = searchContact(root, name);
                if (contact)
                    printf("Contact found: %s - %s\n", contact->name, contact->phone);
                else
                    printf("Contact not found.\n");
                break;
            case 4:
                printf("Exiting and freeing memory...\n");
                freeTree(root);
                break;
            default:
                printf("Invalid choice. Try again.\n");
        }
    } while (choice != 4);

    return 0;
}
