#include <iostream>
using namespace std;
class TreeNode
{
private:
    int data;
    TreeNode *left;
    TreeNode *right;

public:
    TreeNode()
    {
        data = 0;
        left = NULL;
        right = NULL;
    }
    TreeNode(int data)
    {
        this->data = data;
        left = NULL;
        right = NULL;
    }
    ~TreeNode()
    {
        delete left;
        delete right;
    }

    friend class BinaryTree;
};

#define MAX 100
template <typename T>
class Stack
{
private:
    T data[MAX];
    int Top;

public:
    Stack()
    {
        Top = -1;
    }

    bool isEmpty()
    {
        if (Top == -1)
        {
            return true;
        }
        return false;
    }

    bool isFull()
    {
        if (Top == MAX - 1)
        {
            return true;
        }
        return false;
    }

    void push(T element)
    {
        if (isFull())
        {
            cout << " The Stack is Full !! " << endl;
            return;
        }
        data[++Top] = element;
    }

    void pop()
    {
        if (isEmpty())
        {
            cout << " The Stack is Empty !! " << endl;
            return;
        }

        Top--;
    }

    T top()
    {
        if (isEmpty())
        {
            cout << " The Stack is Empty !! " << endl;
            return NULL;
        }

        return data[Top];
    }

    friend class BinaryTree;
};

class BinaryTree
{
private:
    TreeNode *root;

public:
    BinaryTree()
    {
        root = NULL;
    }

    void create_BT()
    {
        TreeNode *temp;
        TreeNode *travel;
        int val;

        cout << "Enter the data to be added " << endl;
        cin >> val;
        temp = new TreeNode(val);
        if (root == NULL)
        {
            root = temp;
            cout << " BST Tree is empty ,Now creating new tree " << endl;
            return;
        }
        else
        {
            travel = root;
            while (travel != NULL)
            {
                if (temp->data == travel->data)
                {
                    cout << "Duplicate data " << endl;
                    return;
                }
                else if ((temp->data < travel->data) && travel->left == NULL)
                {
                    travel->left = temp;
                    cout << "value inserted" << endl;
                    break;
                }
                else if (temp->data < travel->data) // above condition will become false if
                {                                   // travel->left is not null,then this condition
                    travel = travel->left;          // will execute and traverse to left
                }
                else if ((temp->data > travel->data) && (travel->right == NULL))
                {
                    travel->right = temp;
                    cout << "Value inserted " << endl;
                    break;
                }
                else
                {
                    travel = travel->right;
                }
            }
        }
        /* int element;
          cout<<" Enter the data of the TreeNode : ";
          cin>>element;
          TreeNode *newNode=new TreeNode(element);
          if(root==NULL)
          {
              root=newNode;
          }
          else
          {

              TreeNode*travel;

             /* do
              {
                  char ch;
                  cout<<" Where you want to enter the TreeNode to (L/R) of "<<temp->data<<" : ";
                  cin>>ch;
                  if(ch=='L')
                  {
                      if(temp->left==NULL)
                      {
                          temp->left=newNode;
                          temp=newNode;
                      }
                      else
                      {
                          temp=temp->left;
                      }
                  }
                  else if(ch=='R')
                  {
                      if(temp->right==NULL)
                      {
                          temp->right=newNode;
                          temp=newNode;
                      }
                      else
                      {
                          temp=temp->right;
                      }
                  }

              }while(temp!=newNode);*/
    }
    void search(){
        int value;
        TreeNode*travel=root;
        cout<<"\nenter the value to be searched";
        cin>>value;
        while ((travel!=NULL))
        {
            if(travel->data==value){
                cout<<"\nfound";
                break;
            }
            else if(travel->data<value){
                travel=travel->right;
            }
            else if(travel->data>value){
                travel=travel->left;
            }
            else{
                cout<<"\nelement not found";
            }
        }
        


    }

    void PreOrder_recursive(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return;
        }

        cout << temp->data << " ";
        PreOrder_recursive(temp->left);
        PreOrder_recursive(temp->right);
    }
    void PreOrder_nonrecursive(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return;
        }

        Stack<TreeNode *> stack;
        TreeNode *current = temp;
        while (current != NULL || !stack.isEmpty())
        {
            while (current != NULL)
            {
                cout << current->data << " ";
                if (current->right != NULL)
                {
                    stack.push(current->right);
                }
                current = current->left;
            }

            if (!stack.isEmpty())
            {
                current = stack.top();
                stack.pop();
            }
        }
    }
    void PreOrder()
    {
        TreeNode *temp = root;
        int option;
        cout << " -------------- Menu -------------- " << endl;
        cout << " 1. Recursive " << endl;
        cout << " 2. Non-Recursive " << endl;
        cout << "Enter the option : ";
        cin >> option;
        cout << " Displaying the Preorder traversal of Binary Tree : ";
        if (option == 1)
        {
            return PreOrder_recursive(temp);
        }
        else if (option == 2)
        {
            return PreOrder_nonrecursive(temp);
        }
        else
        {
            cout << " Invalid Option. " << endl;
            return;
        }
    }

    void InOrder_recursive(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return;
        }

        InOrder_recursive(temp->left);
        cout << temp->data << " ";
        InOrder_recursive(temp->right);
    }
    void InOrder_nonrecursive(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return;
        }

        Stack<TreeNode *> stack;
        TreeNode *current = temp;
        while (current != NULL || !stack.isEmpty())
        {
            while (current != NULL)
            {
                stack.push(current);
                current = current->left;
            }

            current = stack.top();
            stack.pop();
            cout << current->data << " ";
            current = current->right;
        }
    }
    void InOrder()
    {
        TreeNode *temp = root;
        int option;
        cout << " -------------- Menu -------------- " << endl;
        cout << " 1. Recursive " << endl;
        cout << " 2. Non-Recursive " << endl;
        cout << "Enter the option : ";
        cin >> option;
        cout << " Displaying the Inorder traversal of Binary Tree : ";
        if (option == 1)
        {
            return InOrder_recursive(temp);
        }
        else if (option == 2)
        {
            return InOrder_nonrecursive(temp);
        }
        else
        {
            cout << " Invalid Option. " << endl;
            return;
        }
    }

    void PostOrder_recursive(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return;
        }

        PostOrder_recursive(temp->left);
        PostOrder_recursive(temp->right);
        cout << temp->data << " ";
    }
    void PostOrder_nonrecursive(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return;
        }

        Stack<TreeNode *> stack1, stack2;
        stack1.push(temp);
        TreeNode *current;
        while (!stack1.isEmpty())
        {
            current = stack1.top();
            stack1.pop();
            stack2.push(current);
            if (current->left != NULL)
            {
                stack1.push(current->left);
            }
            if (current->right != NULL)
            {
                stack1.push(current->right);
            }
        }

        while (!stack2.isEmpty())
        {
            current = stack2.top();
            stack2.pop();
            cout << current->data << " ";
        }
    }
    void PostOrder()
    {
        TreeNode *temp = root;
        int option;
        cout << " -------------- Menu -------------- " << endl;
        cout << " 1. Recursive " << endl;
        cout << " 2. Non-Recursive " << endl;
        cout << "Enter the option : ";
        cin >> option;
        cout << " Displaying the Postorder traversal of Binary Tree : ";
        if (option == 1)
        {
            return PostOrder_recursive(temp);
        }
        else if (option == 2)
        {
            return PostOrder_nonrecursive(temp);
        }
        else
        {
            cout << " Invalid Option. " << endl;
            return;
        }
    }

    int heightOfBinaryTree(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return 0;
        }

        return 1 + max(heightOfBinaryTree(temp->left), heightOfBinaryTree(temp->right));
    }
    void heightOfBinaryTree()
    {
        TreeNode *temp = root;
        cout << " The Height of the Binary Tree is : " << heightOfBinaryTree(temp) << endl;
        return;
    }

    int leafNodesOfBinaryTree(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return 0;
        }
        if (temp->left == NULL && temp->right == NULL)
        {
            return 1;
        }

        return leafNodesOfBinaryTree(temp->left) + leafNodesOfBinaryTree(temp->right);
    }
    void leafNodesOfBinaryTree()
    {
        TreeNode *temp = root;
        cout << " Number of Leaf Nodes in the Binary Tree are : " << leafNodesOfBinaryTree(temp) << endl;
        return;
    }

    int internalNodesOfBinaryTree(TreeNode *temp)
    {
        if (temp == NULL || (temp->left == NULL && temp->right == NULL))
        {
            return 0;
        }
        return 1 + internalNodesOfBinaryTree(temp->left) + internalNodesOfBinaryTree(temp->right);
    }
    void internalNodesOfBinaryTree()
    {
        TreeNode *temp = root;
        cout << " Number of Internal Nodes in the Binary Tree are : " << internalNodesOfBinaryTree(temp) << endl;
        return;
    }

    void mirrorOfBinaryTreeHelper(TreeNode *root)
    {
        if (root == NULL)
        {
            return;
        }

        TreeNode *temp = root->left;
        root->left = root->right;
        root->right = temp;

        mirrorOfBinaryTreeHelper(root->left);
        mirrorOfBinaryTreeHelper(root->right);
    }
    void mirrorOfBinaryTree()
    {
        mirrorOfBinaryTreeHelper(root);
        cout << " Mirror of Binary Tree created Successfully !! " << endl;
        return;
    }

    TreeNode *copyBinaryTree(TreeNode *temp)
    {
        if (temp == NULL)
        {
            return NULL;
        }

        TreeNode *root_copy = new TreeNode(temp->data);

        root_copy->left = copyBinaryTree(temp->left);
        root_copy->right = copyBinaryTree(temp->right);

        return root_copy;
    }
    BinaryTree &operator=(const BinaryTree &obj)
    {
        if (this == &obj)
        {
            return *this;
        }

        this->root = copyBinaryTree(obj.root);
        return *this;
    }

    void delete_BT()
    {
        delete root;
    }
    void minimum()
    {
        TreeNode *travel;

        if (root == NULL)
        {
            cout << "tree is empty." << endl;
            return;
        }
        else
        {
            travel = root;
            while (travel->left != NULL)
            {
                travel = travel->left;
            }
            cout << "Minimum is " << travel->data;
        }
    }
};
int main()
{
    BinaryTree obj_BT;
    int option;
    do
    {
        cout << endl;
        cout << " ----------------- Menu ----------------- " << endl;
        cout << " 1. Create a Binary Tree " << endl;
        cout << " 2. Display Preorder traversal of Binary Tree " << endl;
        cout << " 3. Display Inorder traversal of Binary Tree " << endl;
        cout << " 4. Display Postorder traversal of Binary Tree " << endl;
        cout << " 5. Height of the Binary Tree " << endl;
        cout << " 6. Number of Leaf Nodes of the Binary Tree " << endl;
        cout << " 7. Number of Internal Nodes of the Binary Tree " << endl;
        cout << " 8. Mirror of the Binary Tree " << endl;
        cout << " 9. minimum " << endl;
        cout << " 10. search" << endl;
        cout << " 11. Exit " << endl;
        cout << "Enter the option : ";
        cin >> option;
        switch (option)
        {

        case 1:
            obj_BT.create_BT();
            break;
        case 2:
            obj_BT.PreOrder();
            break;
        case 3:
            obj_BT.InOrder();
            break;
        case 4:
            obj_BT.PostOrder();
            break;
        case 5:
            obj_BT.heightOfBinaryTree();
            break;
        case 6:
            obj_BT.leafNodesOfBinaryTree();
            break;
        case 7:
            obj_BT.internalNodesOfBinaryTree();
            break;
        case 8:
            obj_BT.mirrorOfBinaryTree();
            break;
        case 9:
            obj_BT.minimum();

            break;
        case 10:
             obj_BT.search();
         
            break;
        }
        cout << endl;
    } while (option >= 1 && option <= 10);

    return 0;
}