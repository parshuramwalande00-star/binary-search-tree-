# binary-search-tree-
#include <iostream>
using namespace std;

// Template Node
template <typename T>
struct Node {
    T data;
    Node<T>* left;
    Node<T>* right;

    Node(T value) {
        data = value;
        left = nullptr;
        right = nullptr;
    }
};

// Template BST Class
template <typename T>
class BST {
private:
    Node<T>* root;

    // Insert helper
    Node<T>* insert(Node<T>* node, T value) {
        if (node == nullptr)
            return new Node<T>(value);

        if (value < node->data)
            node->left = insert(node->left, value);
        else if (value > node->data)
            node->right = insert(node->right, value);

        return node;
    }

    // Search helper
    bool search(Node<T>* node, T value) {
        if (node == nullptr)
            return false;

        if (node->data == value)
            return true;

        if (value < node->data)
            return search(node->left, value);

        return search(node->right, value);
    }

    // Find minimum node
    Node<T>* findMin(Node<T>* node) {
        while (node != nullptr && node->left != nullptr)
            node = node->left;

        return node;
    }

    // Delete helper
    Node<T>* remove(Node<T>* node, T value) {
        if (node == nullptr)
            return nullptr;

        if (value < node->data) {
            node->left = remove(node->left, value);
        }
        else if (value > node->data) {
            node->right = remove(node->right, value);
        }
        else {
            // No child
            if (node->left == nullptr && node->right == nullptr) {
                delete node;
                return nullptr;
            }

            // Only right child
            if (node->left == nullptr) {
                Node<T>* temp = node->right;
                delete node;
                return temp;
            }

            // Only left child
            if (node->right == nullptr) {
                Node<T>* temp = node->left;
                delete node;
                return temp;
            }

            // Two children
            Node<T>* temp = findMin(node->right);
            node->data = temp->data;
            node->right = remove(node->right, temp->data);
        }

        return node;