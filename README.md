# AVL Tree

average/worst      |        indexing     | search              | add                 | remove(index or value)
-------------------|---------------------|---------------------|---------------------|-----------------------
AVL Tree           | O(log n) / O(log n) | O(log n) / O(log n) | O(log n) / O(log n) | O(log n) / O(log n)

* **Rotations**
  * **Left (LL)**
  ![](https://github.com/shamy1st/data-structure/blob/main/images/left-rotation-1.png)
  ![](https://github.com/shamy1st/data-structure/blob/main/images/left-rotation-2.png)
  
  * **Right (RR)**
  ![](https://github.com/shamy1st/data-structure/blob/main/images/right-rotation-1.png)
  ![](https://github.com/shamy1st/data-structure/blob/main/images/right-rotation-2.png)
  
  * **Left-Right (LR)**
  ![](https://github.com/shamy1st/data-structure/blob/main/images/left-right-rotation-1.png)
  ![](https://github.com/shamy1st/data-structure/blob/main/images/left-right-rotation-2.png)
  ![](https://github.com/shamy1st/data-structure/blob/main/images/left-right-rotation-3.png)
  
  * **Right-Left (RL)**
  ![](https://github.com/shamy1st/data-structure/blob/main/images/right-left-rotation-1.png)
  ![](https://github.com/shamy1st/data-structure/blob/main/images/right-left-rotation-2.png)
  ![](https://github.com/shamy1st/data-structure/blob/main/images/right-left-rotation-3.png)

        public class Main {
            public static void main(String[] args) {
                AVLTree<Integer> tree = new AVLTree<>();
                tree.insert(30);
                tree.insert(20);
                tree.insert(10);
            }
        }

        public class AVLTree<T extends Comparable<T>> {
            private class Node<T extends Comparable<T>> {
                private T value;
                private Node<T> left;
                private Node<T> right;
                private int height;

                public Node(T value, Node<T> left, Node<T> right, int height) {
                    this.value = value;
                    this.left = left;
                    this.right = right;
                    this.height = height;
                }

                @Override
                public String toString() {
                    return value.toString();
                }
            }

            private Node<T> root;

            public AVLTree() {

            }

            public void insert(T value) {
                root = insert(root, value);
            }

            private Node<T> insert(Node<T> node, T value) {
                if(node == null)
                    node = new Node(value, null, null, 0);
                else if(value.compareTo(node.value) < 0)
                    node.left = insert(node.left, value);
                else if(value.compareTo(node.value) > 0)
                    node.right = insert(node.right, value);

                setHeight(node);
                return balance(node);
            }

            private Node<T> balance(Node<T> node) {
                //left heavy case1: rightRotate(30)
                //    30
                //  20  (1)
                //10
                //left heavy case2: leftRotate(20) then rightRotate(30)
                //    30
                //  20  (-1)
                //    25
                if(isLeftHeavy(node)) {
                    if(balanceFactor(node.left) < 0)
                        node.left = leftRotate(node.left);
                    return rightRotate(node);
                }
                //right heavy case1: leftRotate(10)
                //10
                //  20 (-1)
                //    30
                //right heavy case2: rightRotate(20) then leftRotate(10)
                //10
                //  20 (1)
                //15
                else if(isRightHeavy(node)) {
                    if(balanceFactor(node.right) > 0)
                        node.right = rightRotate(node.right);
                    return leftRotate(node);
                }

                return node;
            }

            // 10 (node)            20 (newNode)
            //     20      ->  10      30
            //   15  30           15
            // leftRotate(10)
            private Node<T> leftRotate(Node<T> node) {
                Node<T> newNode = node.right;
                node.right = newNode.left;
                newNode.left = node;

                setHeight(node);
                setHeight(newNode);
                return newNode;
            }

            //     30 (node)           20 (newNode)
            //  20           ->    10      30
            //10  25                     25
            // leftRotate(30)
            private Node<T> rightRotate(Node<T> node) {
                Node<T> newNode = node.left;
                node.left = newNode.right;
                newNode.right = node;

                setHeight(node);
                setHeight(newNode);
                return newNode;
            }

            private int balanceFactor(Node<T> node) {
                return (node == null) ? 0 : height(node.left) - height(node.right);
            }

            private boolean isLeftHeavy(Node<T> node) {
                return balanceFactor(node) > 1;
            }

            private boolean isRightHeavy(Node<T> node) {
                return balanceFactor(node) < -1;
            }

            private void setHeight(Node<T> node) {
                node.height = 1 + Math.max(height(node.left), height(node.right));
            }

            private int height(Node<T> node) {
                return (node == null) ? -1 : node.height;
            }
        }
