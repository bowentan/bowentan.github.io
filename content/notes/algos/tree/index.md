---
title: Tree
weight: 20
menu:
  notes:
    name: Tree
    identifier: notes-algos-tree
    parent: notes-algos
    weight: 20
---

{{< note title="Breadth-First Traversal (BFT)" >}}

Also known as _level-order_ traversal when applied to trees.

It starts at a node and explores all its immediate _neighbors_ before moving on to child nodes.

{{< /note >}}

{{< note title="Depth-First Traversal (DFT)" >}}

Also known as _depth-first search_.

It starts at a node and explores as for as possible along one branch before backtracking to other branches.

{{< /note >}}

{{< note title="Preorder" >}}

Visit first root and then children.

```text
preorder(node):
  if node is null:
    return

  visit(node)
  preorder(node.left)
  preorder(node.right)
```

{{< /note >}}

{{< note title="Inorder" >}}

Visit first left child, then root, finally right child. Put root _in-between_ children.

```text
inorder(node):
  if node is null:
    return

  inorder(node.left)
  visit(node)
  inorder(node.right)
```

{{< /note >}}

{{< note title="Postorder" >}}

Visit first left child, then right child, finally the root. Put root _after_ children.

```text
postorder(node):
  if node is null:
    return

  postorder(node.left)
  postorder(node.right)
  visit(node)
```

{{< /note >}}
