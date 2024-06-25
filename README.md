# Program_Binary_tree.cpp
Tugas sebelum UAS

#include <iostream>
#include <cstdlib> // untuk menggunakan fungsi system("pause")
using namespace std;

// Struktur node untuk BST
struct TreeNode {
    string nama;
    string nim; // Mengubah tipe data nim menjadi string
    TreeNode *left;
    TreeNode *right;
    
    // Konstruktor untuk node
    TreeNode(string name, string number) : nama(name), nim(number), left(nullptr), right(nullptr) {}
};

// Kelas untuk Binary Search Tree
class BinarySearchTree {
private:
    TreeNode *root;
    
    // Fungsi rekursif untuk menambahkan data ke dalam BST
    TreeNode* insertRecursive(TreeNode *root, string name, string number) {
        if (root == nullptr) {
            return new TreeNode(name, number);
        }
        
        // Memasukkan ke subtree kiri jika nim kurang dari root->nim
        if (number < root->nim) {
            root->left = insertRecursive(root->left, name, number);
        }
        // Memasukkan ke subtree kanan jika nim lebih dari atau sama dengan root->nim
        else {
            root->right = insertRecursive(root->right, name, number);
        }
        
        return root;
    }
    
    // Fungsi rekursif untuk traversal inorder
    void inorderRecursive(TreeNode *root) {
        if (root != nullptr) {
            inorderRecursive(root->left);
            cout << "Nama: " << root->nama << ", NIM: " << root->nim << endl;
            inorderRecursive(root->right);
        }
    }
    
    // Fungsi rekursif untuk traversal preorder
    void preorderRecursive(TreeNode *root) {
        if (root != nullptr) {
            cout << "Nama: " << root->nama << ", NIM: " << root->nim << endl;
            preorderRecursive(root->left);
            preorderRecursive(root->right);
        }
    }
    
    // Fungsi rekursif untuk traversal postorder
    void postorderRecursive(TreeNode *root) {
        if (root != nullptr) {
            postorderRecursive(root->left);
            postorderRecursive(root->right);
            cout << "Nama: " << root->nama << ", NIM: " << root->nim << endl;
        }
    }
    
    // Fungsi rekursif untuk mencari data
    TreeNode* searchRecursive(TreeNode *root, string number) {
        if (root == nullptr || root->nim == number) {
            return root;
        }
        
        // Cari ke subtree kiri jika nim kurang dari root->nim
        if (number < root->nim) {
            return searchRecursive(root->left, number);
        }
        // Cari ke subtree kanan jika nim lebih dari root->nim
        else {
            return searchRecursive(root->right, number);
        }
    }
    
    // Fungsi rekursif untuk menghapus node dengan nilai terkecil dari subtree kanan
    TreeNode* findMin(TreeNode* node) {
        TreeNode* current = node;
        while (current && current->left != nullptr) {
            current = current->left;
        }
        return current;
    }
    
    // Fungsi rekursif untuk menghapus node berdasarkan nilai NIM
    TreeNode* deleteRecursive(TreeNode *root, string number) {
        if (root == nullptr) return root;
        
        // Cari node yang akan dihapus
        if (number < root->nim) {
            root->left = deleteRecursive(root->left, number);
        } else if (number > root->nim) {
            root->right = deleteRecursive(root->right, number);
        } else {
            // Node dengan nilai NIM yang sesuai ditemukan untuk dihapus
            
            // Node dengan satu anak atau tanpa anak
            if (root->left == nullptr) {
                TreeNode *temp = root->right;
                delete root;
                return temp;
            } else if (root->right == nullptr) {
                TreeNode *temp = root->left;
                delete root;
                return temp;
            }
            
            // Node dengan dua anak: mencari successor in-order (node terkecil dari subtree kanan)
            TreeNode* temp = findMin(root->right);
            
            // Salin nilai successor in-order ke node saat ini
            root->nama = temp->nama;
            root->nim = temp->nim;
            
            // Hapus successor in-order
            root->right = deleteRecursive(root->right, temp->nim);
        }
        return root;
    }
    
    // Fungsi rekursif untuk menghapus semua node BST
    void deleteTree(TreeNode *root) {
        if (root != nullptr) {
            deleteTree(root->left);
            deleteTree(root->right);
            delete root;
        }
    }
    
public:
    // Konstruktor BST
    BinarySearchTree() : root(nullptr) {}
    
    // Fungsi untuk menambah data ke BST (public)
    void insert(string name, string number) {
        root = insertRecursive(root, name, number);
    }
    
    // Fungsi untuk melakukan traversal inorder (public)
    void inorderTraversal() {
        inorderRecursive(root);
    }
    
    // Fungsi untuk melakukan traversal preorder (public)
    void preorderTraversal() {
        preorderRecursive(root);
    }
    
    // Fungsi untuk melakukan traversal postorder (public)
    void postorderTraversal() {
        postorderRecursive(root);
    }
    
    // Fungsi untuk mencari data dalam BST (public)
    TreeNode* search(string number) {
        return searchRecursive(root, number);
    }
    
    // Fungsi untuk menghapus data dari BST berdasarkan NIM (public)
    void deleteNode(string number) {
        root = deleteRecursive(root, number);
    }
    
    // Fungsi untuk menghapus semua node dalam BST (public)
    void deleteAll() {
        deleteTree(root);
        root = nullptr;
    }
    
    // Destructor BST
    ~BinarySearchTree() {
        deleteTree(root);
    }
};

int main() {
    BinarySearchTree bst;
    int choice;
    
    do {
        // Menampilkan menu
        cout << "Menu Binary Tree" << endl;
        cout << "1. Masukkan Data pada Binary Tree" << endl;
        cout << "2. Hapus Data" << endl;
        cout << "3. Traversal" << endl;
        cout << "   1. In Order" << endl;
        cout << "   2. Pre Order" << endl;
        cout << "   3. Post Order" << endl;
        cout << "4. Cari Data" << endl;
        cout << "5. Kosongkan Binary Tree" << endl;
        cout << "6. Keluar" << endl;
        cout << "Pilihan Anda: ";
        cin >> choice;
        
        switch (choice) {
            case 1: {
                cout << "Masukkan sampai 10 data  \n";
                // Memasukkan 10 data secara berurutan
                for (int i = 1; i <= 10; ++i) {
                    
                    cout << "Masukkan Data " << i << " Nama : ";
                    string name;
                    cin.ignore(); // untuk membersihkan buffer
                    getline(cin, name);
                    
                    string nim;
                    cout << "NIM : ";
                    cin >> nim;
                    
                    bst.insert(name, nim);
                    
                    if (i == 10) {
                        cout << "Data telah dimasukkan." << endl;
                    }
                }
                break;
            }
            case 2: {
                // Hapus data
                cout << "Masukkan NIM yang ingin dihapus: ";
                string nim;
                cin >> nim;
                
                bst.deleteNode(nim);
                cout << "Data dengan NIM " << nim << " telah dihapus." << endl;
                break;
            }
            case 3: {
                // Traversal
                int traversalChoice;
                cout << "Pilih jenis traversal:" << endl;
                cout << "1. In Order" << endl;
                cout << "2. Pre Order" << endl;
                cout << "3. Post Order" << endl;
                cout << "Pilih nomer 1/2/3 :" ;
                cin >> traversalChoice;
                
                switch (traversalChoice) {
                    case 1:
                        cout << "Traversal In Order:" << endl;
                        bst.inorderTraversal();
                        break;
                    case 2:
                        cout << "Traversal Pre Order:" << endl;
                        bst.preorderTraversal();
                        break;
                    case 3:
                        cout << "Traversal Post Order:" << endl;
                        bst.postorderTraversal();
                        break;
                    default:
                        cout << "Pilihan tidak valid." << endl;
                        break;
                }
                break;
            }
            case 4: {
                // Cari data
                cout << "Masukkan NIM yang ingin dicari: ";
                string nim;
                cin >> nim;
                
                TreeNode *result = bst.search(nim);
                if (result != nullptr) {
                    cout << "Data ditemukan:" << endl;
                    cout << "Nama: " << result->nama << endl;
                    cout << "NIM: " << result->nim << endl;
                } else {
                    cout << "Data tidak ditemukan." << endl;
                }
                break;
            }
            case 5: {
                // Kosongkan BST
                bst.deleteAll();
                cout << "Binary Tree telah dikosongkan." << endl;
                break;
            }
            case 6:
                // Keluar dari program
                cout << "Terima kasih!" << endl;
                break;
            default:
                cout << "Pilihan tidak valid." << endl;
                break;
        }
        
        // Memberikan jeda dan membersihkan buffer
        cout << endl;
        system("pause");
        cout << endl;
        
    } while (choice != 6);
    
    return 0;
}
