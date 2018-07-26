# ARTS 003

* [A: Same Tree](#a-same-tree)
* [R: Optimistic vs. Pessimistic Locking](#r-optimistic-vs-pessimistic-locking)
* [T: Mockito](#t-mockito)
* [S: 给自己打打鸡血](#s-给自己打打鸡血)

## A: Same Tree

> Given two binary trees, write a function to check if they are the same or not.
>
> Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

### Solutions

**Prefer Recursion**

* Time: $O(n)$ - n is the number of nodes
* Space: $O(d)$ - d is the depth of the tree

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p != null & q != null) {
            if (p.val == q.val) {
                boolean isLeftSame = isSameTree(p.left, q.left);
                boolean isRightSame = isSameTree(p.right, q.right);
                return isLeftSame && isRightSame;
            } else {
                return false;
            }
        } else if (p == null && q == null) {
            return true;
        } else {
            return false;
        }
    }
}
```

A shorter version would be

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null)
            return true;
        if (p == null || q == null)
            return false;
        if (p.val != q.val)
            return false;
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

## R: Optimistic vs. Pessimistic Locking

* **Optimistic Locking**: read a record, remember the version number (or other ways), and make sure the record hasn't changed before your write back (this step should be atomic).
  * For example, in [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBMapper.OptimisticLocking.html), you can use `@DynamoDBVersionAttribute` or customed conditional check so that the DB will enforce the atomicity.
  * Use when you assume less (write) collision, and you want high throughput (since pessimistic locking will lock a record for a long time, potentially locking the reads as well).
  * The entire transaction needs to be rolled back when collision does happen.
* **Pessimistic Locking**: lock the record for your exclusive use until you have finished with it.
  * Provides better integrity (meaning no rollback?)
  * Need careful design to avoid deadlock.

References: [stackoverflow](https://stackoverflow.com/questions/129329/optimistic-vs-pessimistic-locking), [jboss.org](https://docs.jboss.org/jbossas/docs/Server_Configuration_Guide/4/html/TransactionJTA_Overview-Pessimistic_and_optimistic_locking.html)

## T: Mockito 

这周在用 Mockito 写 unit test 的时候遇到一个问题。我需要测试我写的 FailureHandler. 

```java
this.failureHandler = spy(new MessageFailureHandler());
when(this.failureHandler.createSqsClient()).thenReturn(this.client); // does not work
doReturn(this.client).when(this.failureHandler).createSqsClient(); // works
```

我一直以为这两个用法是等价的。后来查了一下[文档](http://static.javadoc.io/org.mockito/mockito-core/2.20.0/org/mockito/Mockito.html#spy)，Mockito在 spy 的时候并不是 delegate calls to the passed real object，而是 create a copy of it，所以第一种方法 Mockito 并不知道 `failureHandler` 遭遇了什么。

另一个小感悟是 prefer source of information （指文档 in this case）。我最开始当然是 Google 到 stackoverflow 的问这[两个用法的区别](https://stackoverflow.com/questions/20353846/mockito-difference-between-doreturn-and-when/20360269)的问题，但是这些回答比较赋予表面，并没有告诉你 Mockito 具体是怎么处理的（create a copy in this case）。知道更本质的东西更有助于举一反三。当然，我们也不可能通读文档（读了也不记得），这时候就需要 stackoverflow 牵引我们到问题的根源处。

## S: 给自己打打鸡血 

前段时间懒了快两周，昨晚好好想了想自己为什么要下班回家之后还要学习。想了想什么事情能让我快乐。我想不断进步就是为了能让自己有更多的时间、更大的能力去做那些让我快乐的事情吧。当然形成习惯、正反馈之后，学习本身也会让自己越来越快乐。

搜到一篇[文章](https://www.douban.com/note/531392298/)（很好的文章，虽然标题有点那啥），今天实践了一下一早起来健身，果然感觉做事都有动力了，以后每天都要打卡。