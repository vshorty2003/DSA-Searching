# DSA-Searching

1. How many searches?
Given a sorted list 3, 5, 6, 8, 11, 12, 14, 15, 17, 18 and using the recursive binary search algorithm, identify the sequence of numbers that each recursive call will search to try and find 8.

Given a sorted list 3, 5, 6, 8, 11, 12, 14, 15, 17, 18 and using the recursive binary search algorithm, identify the sequence of numbers that each recursive call will search to try and find 16.

2. Adding a React UI
For exercises 1 and 2, you will be using a search algorithm to search for an item in a dataset. You will be testing the efficiency of 2 search algorithms, linear search and binary search. You will also have a UI (a simple textbox will do) through which you will be sending your input that you want to search. There is no server-side to this program. All of this should be done using React.

class _Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class Queue {
  constructor() {
    this.first = null;
    this.last = null;
    this.length = 0;
  }

  enqueue(data) {
    const node = new _Node(data);

    if(this.first === null) {
      this.first = node;
    }

    if(this.last) {
      this.last.next = node;
    }
    this.length++
    this.last = node;
  }

  dequeue() {
    if(this.first === null) {
      return;
    }
    const node = this.first;
    this.first = this.first.next;

    if(node === this.last) {
      this.last = null;
    }
    this.length--
    return node.value;
  }
}

module.exports = Queue

1) Linear search

Given the following dataset, find out how many tries it took to search for a particular item in the dataset. If the item is not in the dataset, provide a message and indicate how many searches it took to find that out.

89 30 25 32 72 70 51 42 25 24 53 55 78 50 13 40 48 32 26 2 14 33 45 72 56 44 21 88 27 68 15 62 93 98 73 28 16 46 87 28 65 38 67 16 85 63 23 69 64 91 9 70 81 27 97 82 6 88 3 7 46 13 11 64 76 31 26 38 28 13 17 69 90 1 6 7 64 43 9 73 80 98 46 27 22 87 49 83 6 39 42 51 54 84 34 53 78 40 14 5

2) Binary search

Use the same front end and the dataset from the previous exercise for this exercise. Use array.sort to sort the dataset. Then implement a binary search to find a particular value in the dataset. Display how many tries it took to search for a particular item in the dataset using binary search. If the item is not in the dataset, provide a message and indicate how many searches it took to find that out.

const Queue = require('./Queue')

class BinarySearchTree {
  constructor(key = null, value = null, parent = null) {
    this.key = key
    this.value = value
    this.parent = parent
    this.left = null
    this.right = null
  }

  insert(key, value) {
    if(this.key == null) {
      this.key = key
      this.value = value
    }
    else if (key < this.key) {
      if (this.left == null) {
        this.left = new BinarySearchTree(key, value, this)
      }
      else {
        this.left.insert(key, value)
      }
    }
    else {
      if (this.right == null) {
        this.right = new BinarySearchTree(key, value, this)
      }
      else {
        this.right.insert(key, value)
      }
    }
  }

  find(key) {
    if(this.key == key) {
      console.log(this.value)
      return this.value
    }

    else if (key < this.key && this.left) {
      return this.left.find(key)
    }

    else if (key > this.key && this.right) {
      return this.right.find(key)
    }
    else {
      throw new Error('Key not found')
    }
  }

  remove(key) {
    if (this.key == key) {
      if (this.left && this.right) {
        const successor = this.right._findMin()
        this.key = successor.key
        this.value = successor.value
        successor.remove(successor.key)
      }
      else if (this.left) {
        this._replaceWith(this.left)
      }
      else if (this.right) {
        this._replaceWith(this.right)
      }
      else {
        this._replaceWith(null)
      }
    }
    else if (key < this.key && this.left) {
      this.left.remove(key)
    }
    else if (key > this.key && this.right) {
      this.right.remove(key)
    }
    else {
      throw new Error('Key error')
    }
  }

  _replaceWith(node) {
    if(this.parent) {
      if(this == this.parent.left) {
        this.parent.left = node
      }
      else if (this == this.parent.right) {
        this.parent.right = node
      }

      if (node) {
        node.parent = this.parent
      }
    }
    else {
      if (node) {
        this.key = node.key
        this.value = node.value
        this.left = node.left
        this.right = node.right
      }
      else {
        this.key = null
        this.value = null
        this.left = null
        this.right = null
      }
    }
  }

  _findMin() {
    if(!this.left) {
      return this
    }
    return this.left._findMin()
  }

  _inOrder(values=[]) {
    if (this.left) {
      values = this.left._inOrder(values)
    }
    values.push(this.key)

    if (this.right) {
      values = this.right._inOrder(values)
    }
    return values
  }

  _preOrder(values=[]) {
    values.push(this.key)
    if (this.left) {
      values = this.left._preOrder(values)
    }

    if (this.right) {
      values = this.right._preOrder(values)
    }
    return values;
  }

  _postOrder(values=[]) {
    if (this.left) {
      values = this.left._postOrder(values)
    }

    if (this.right) {
      values = this.right._postOrder(values)
    }
    values.push(this.key)
    return values;
  }

  bfs(values=[]) {
    const queue = new Queue()
    queue.enqueue(this)
    console.log('queue', queue.length)
    while (queue.length) {
      const node = queue.dequeue()
      console.log('node', node)
      values.push(node.value)

      if (node.left) {
        queue.enqueue(node.left)
      }

      if (node.right) {
        queue.enqueue(node.right)
      }
    }

    return values
  }
}

module.exports = BinarySearchTree;

3. Find a book
Imagine you are looking for a book in a library with a Dewey Decimal index. How would you go about it? Can you express this process as a search algorithm? Implement your algorithm to find a book whose Dewey and book title is provided.
---------------------------------------
const BST = require('./BST')

function createDewey(num) {
  const system = []

  for (let i = 0; i < num; i++) {
    system.push(i)
  }
  return system
}
---------------------------------------

4. Searching in a BST
** No coding is needed for these drills**. Once you have answered it, you can then code the tree and implement the traversal to see if your answer is correct.

1) Given a binary search tree whose in-order and pre-order traversals are respectively 14 15 19 25 27 35 79 89 90 91 and 35 25 15 14 19 27 89 79 91 90. What would be its postorder traversal?

2) The post order traversal of a binary search tree is 5 7 6 9 11 10 8. What is its pre-order traversal?

5. Implement different tree traversals
Using your BinarySearchTree class from your previous lesson, create a binary search tree with the following dataset: 25 15 50 10 24 35 70 4 12 18 31 44 66 90 22. Then implement inOrder(), preOrder(), and postOrder() functions. Test your functions with the following datasets.

A pre-order traversal should give you the following order: 25, 15, 10, 4, 12, 24, 18, 22, 50, 35, 31, 44, 70, 66, 90

In-order: 4, 10, 12, 15, 18, 22, 24, 25, 31, 35, 44, 50, 66, 70, 90

Post-order: 4, 12, 10, 22, 18, 24, 15, 31, 44, 35, 66, 90, 70, 50, 25

--------------------------------------------------------
function findBook(array, indexVal, start, end) {
  var start = start === undefined ? 0 : start;
  var end = end === undefined ? array.length : end;

  if (start > end) {
    return 'not found'
  }

  const middle = Math.floor((start + end)/2)
  const book = array[middle]

  if (book === indexVal) {
    return middle;
  }

  else if (book < indexVal) {
    return findBook(array, indexVal, middle + 1, end)
  }

  else if (book > indexVal) {
    return findBook(array, indexVal, start, middle - 1)
  }
}

// console.log(findBook(createDewey(999), 508));

function main() {
  const t = new BST(25);
  t.insert(15)
  t.insert(50)
  t.insert(10)
  t.insert(24)
  t.insert(35)
  t.insert(70)
  t.insert(4)
  t.insert(12)
  t.insert(18)
  t.insert(31)
  t.insert(44)
  t.insert(66)
  t.insert(90)
  t.insert(22)
  return t
}

let tree = main()

// console.log(tree._inOrder())
// console.log(tree._preOrder())
// console.log(tree._postOrder())
--------------------------------------------------------

6. Find the next commanding officer
Suppose you have a tree representing a command structure of the Starship USS Enterprise.

               Captain Picard
             /                \
    Commander Riker       Commander Data
      /         \               \
 Lt. Cmdr.   Lt. Cmdr.          Lt. Cmdr.
 Worf        LaForge            Crusher
   /                           /
Lieutenant                  Lieutenant
security-officer            Selar
This tree is meant to represent who is in charge of lower-ranking officers. For example, Commander Riker is directly responsible for Worf and LaForge. People of the same rank are at the same level in the tree. However, to distinguish between people of the same rank, those with more experience are on the left and those with less on the right (i.e., experience decreases from left to right). Suppose a fierce battle with an enemy ensues. Write a program that will take this tree of commanding officers and outlines the ranking officers in their ranking order so that if officers start dropping like flies, we know who is the next person to take over command.

--------------------------------------------------
function main2() {
  let commanderTree = new BST(5, 'Captain Pickard')
  commanderTree.insert(3, 'Commander Riker')
  commanderTree.insert(6, 'Commander Data')
  commanderTree.insert(2, 'Lt. Cmdr. Worf')
  commanderTree.insert(4, 'Lt. Cmdr. LaForge')
  commanderTree.insert(8, 'Lt. Cmdr. Crusher')
  commanderTree.insert(1, 'Lieutenant security-officer')
  commanderTree.insert(7, 'Lieutenant Selar')
  return commanderTree
}

let cTree = main2()

// console.log(cTree)
// console.log(cTree.bfs())
---------------------------------------------------------------------

7. Max profit
The share price for a company over a week's trading is as follows: [128, 97, 121, 123, 98, 97, 105]. If you had to buy shares in the company on a particular day, and sell the shares on a following day, write an algorithm to work out what the maximum profit you could make would be.

------------------------------------------
function findMaxProfit(arr) {
  let profit = 0
  for (let i = 0; i < arr.length; i++) {
    if(arr[i+1] - arr[i] > profit) {
      profit = arr[i+1] - arr[i]
    }
  }
  return profit
}

console.log(findMaxProfit([128, 97, 121, 123, 98, 97, 105]))
-------------------------------------------------------------

8. Egg drop (optional)
This is a fun exercise to do - consider this optional after you are done with all the exercises above. Imagine that you wanted to find the highest floor of a 100 story building that you could drop an egg from without the egg breaking. But you only have 2 eggs. Write an algorithm to find out in the most efficient way which floors you should drop the eggs from. After you have understood the question and made some attempts to solve the problem, go through this reading before you start coding: http://datagenetics.com/blog/july22012/index.html.

-----------------------------------------------------
1a. [3, 5, 6, 8, 11, 12, 14, 15, 17, 18] -> 8

middle is 11 
8 < 11 so our list is now 3, 5, 6, 8
new middle is 5
8 > 5 so our new list is 6, 8
new middle is 6
6 < 8 so we get rid of six, leaving us with 8

1b. [3, 5, 6, 8, 11, 12, 14, 15, 17, 18] -> 16

middle is 11
16 > 11 so our list is 12, 14, 15, 17, 18
new middle is 15
16 > 15 so our list is 17, 18
new middle is 17
16 < 17 so we must be missing

searching a bst: 

1.
in order: [14 15 19 25 27 35 79 89 90 91]
pre order: [35 25 15 14 19 27 89 79 91 90]
post order: [14 19 15 27 25 79 90 91 89 35]

2.
pre order: [8 6 5 7 10 9 11]
-----------------------------------------------------
