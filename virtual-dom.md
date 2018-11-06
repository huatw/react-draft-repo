# vdom-diffing
Virtual DOM diffing contains several parts: ```tree diff``` , ```component diff```, ```element diff```.

## list-diffing
The core part is children diffing, which is not involved in most articles. React provides three operations on same level children.

* ```INSERT_NEW```, brand new child with new ```key``` .
* ```REMOVE_OLD```, old child with ```key``` no longer exists.
* ```MOVE_NODE```, node is updatable, and its ```key``` exists in both old and new tree.

#### Here are some resource links:

* [petit-dom](https://github.com/yelouafi/petit-dom)
* [DeepDiff](https://github.com/onmyway133/DeepDiff)
* [React diffing issues](https://github.com/facebook/react/issues/10703)
* [React list diffing issues](https://github.com/facebook/react/issues/10382)
* [react diff(CN)](https://zhuanlan.zhihu.com/p/20346379)
* [Optimizing React Apps](https://marmelab.com/blog/2017/02/06/react-is-slow-react-is-fast.html): ```shouldComponentUpdate``` for very large list rerender.

#### conclusion:
* Unique ```key``` is a must in every child node.
* Be careful of actions such as: **moving the last(or very latter) element in a list to front(or very up-front)** However, prepending new element, or moving the first element to last are totally OK! See source code for details!

## tree-diffing
React only compares nodes in same level of two tree.

* workflow: [virtual DOM](http://efe.baidu.com/blog/the-inner-workings-of-virtual-dom/)

#### conclusion:
* Don't move nodes across tree level.

## component-diffing
* If tag type changes, replace old node and its subtree with new one.
* If tag type not changes, patch props, and diff substree recursively.

#### conclusion:
* Don't change upper node type, while subtrees are mostly same.
