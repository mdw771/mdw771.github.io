---
layout: post
title: Understanding fundamentals of libCST transformer through an example
---

LibCST is a Python code parser based on concrete syntax tree (CST), which features the
preservation of formatting elements like blank lines, white spaces, and comments compared
to Python's built-in library AST (abstract syntax tree). LibCST is intended to be a
formalized tool to refactor codes than piling up regular expressions, string replacements, etc.,
and an important way to realize that is to create subclasses of `CSTTransformer`. However,
quick-start tutorials that helps one to easily understand how a CST transformer works and how to
create one seems to be lacking. In this article, we attempt to demonstrate the most basic
(but practical) ideas of a CST transformer through a simple example.

Given a piece of Python code, `cst.parse_module` parses it and return a syntax tree object.
Every node of the tree is a CST object. For example:

`c = a + b` is a `SimpleStatementLine`;

`def myfunc(a, b):` is a `FunctionDef`;

`class MyClass:` is a `ClassDef`.

As in any tree structure, a node can contain children. For example, these
are roughly the children (and children of children) of a
`SimpleStatementLine` of `c = a + b`:
```commandline
SimpleStatementLine
- Assign
    - AssignTarget
        - Name: "c"
    - BinaryOperation
        - Name: "a"
        - Name: "b"
```

A transformer is a subclass of `CSTTransformer` that defines the rules
about what to do when the parser encounters a node object - be it a `SimpleStatementLine`,
a `FunctionDef`, or a `Name`. When doing code parsing or refactoring, an object
of the defined transformer is passed to CST's visitor, where it visits every
node in the syntax tree and acts based on the rules defined in the transformer.

In the following example, the transformer overrides 4 methods that define
the actions to take when encountering a `SimpleStatementLine` and
a `Name`. 
```python
class Transformer(cst.CSTTransformer):
    def visit_SimpleStatementLine(self, node):
        ...
    def leave_SimpleStatementLine(self, original_node, updated_node):
        ...
    def visit_Name(self, node):
        ...
    def leave_Name(self, original_node, updated_node):
        ...
```
For each type of node, one can define a `visit` method and a
`leave` method. `visit` defines the actions to take when the visitor
just arrives at the node, and `leave` defines the action when leaving
that node. Between `visit` and `leave` of a node, the visitor visits
its children, which might also have their own `visit` and/or `leave`
methods. 

In the case of `c = a + b`, this is what happens to the visitor if we
only consider `SimpleStatementLine` and `Name`s:
```commandline
visit SimpleStatementLine
visit Name (c)
leave Name (c)
visit Name (a)
leave Name (a)
visit Name (b)
leave Name (b)
leave SimpleStatementLine
```

`visit` methods are read-only: one cannot make changes to the node in `visit`
methods, but one can still modify other attributes in the transformer object.
`leave` methods on the other hand allows one to change the node. `leave`
methods should take 2 input nodes: one `original_node` and one `updated_node`.
These are two different objects, but their contents are initially identical.
Any changes to the node should be made in `updated_node`, which is then
returned by the method.

With all these, we now consider the following toy example:
```python
import numpy
from abs import dd

numpy.load('1.npz')
```
We want to change the imported library from `numpy` to `scipy`, but keep
the `numpy` in `numpy.load` unchanged (probably hardly anyone wants to do
things like this in reality; let's just say `numpy` is imported in another module
so it just doesn't have to imported here again).

To create a transformer, one may imagine something like:
```python
class Transformer(cst.CSTTransformer):
    def leave_Name(self, original_node, updated_node):
        if updated_node.value == 'numpy':
            # This is the way of modifying the value of a node
            return updated_node.with_changes(value='scipy')
```
However, this would also change the `numpy` in `numpy.load`. We should
only change the name when the visited `Name` has the value of `"numpy"`
AND if it is in an import statement. 

We might then think about this:
```python
class Transformer(cst.CSTTransformer):
    def leave_SimpleStatementLine(self, original_node, updated_node):
        if isinstance(node.body[0], cst.Import):
            for i_obj, obj in enumerate(node.body[0].names):
                if obj.name.value == 'numpy':
                    # Then change the name of imported library
                    obj.name.value = 'scipy'
```
This is closer, but the renaming in the last line won't work because
`obj.name.value` is immutable. Node values should be changed by calling
`updated_node.with_changes` and returning it, but here the node is
a `SimpleStatementLine`, and it is hard to directly change the value of
one of its children from here. 

As a better way, we can create a class attribute called `is_numpy_import`
in the transformer class. In `visit_SimpleStatementLine`, we check if the
statement is an "import" and whether it contains `numpy`; if so, 
we set `is_numpy_import` to True. Then in `leave_Name` which will be
called when the visitor visits the children of the `SimpleStatementLine`,
we check if the name's value is `"numpy"`, and replace it if so. 
Finally, when leaving the `SimpleStatementLine`, we set `is_numpy_import`
back to `False`, so that it won't cause any `"numpy"` to be replaced when
visiting other nodes that's not an import statement. 

Together, the example code looks like this:
```python
from typing import Union, Optional

import libcst as cst
from libcst import FlattenSentinel, RemovalSentinel

code = """import numpy
from abs import dd

numpy.load('1.npz')
"""

class ImportRenamer(cst.CSTTransformer):
    def __init__(self):
        self.is_numpy_import = False
    
    def visit_SimpleStatementLine(self, node: "cst.SimpleStatementLine") -> Optional[bool]:
        """
        If the current statement is an import, and "numpy" is in the imported libraries, set self.is_numpy_import to
        True.
        """
        if isinstance(node.body[0], cst.Import):
            for i_obj, obj in enumerate(node.body[0].names):
                if obj.name.value == 'numpy':
                    self.is_numpy_import = True
        # Return True to make sure its children are also visited.
        return True

    def leave_SimpleStatementLine(
        self, original_node: "cst.SimpleStatementLine", updated_node: "cst.SimpleStatementLine"
    ) -> Union["cst.BaseStatement", FlattenSentinel["cst.BaseStatement"], RemovalSentinel]:
        """
        Set self.is_numpy_import back to False, so that it won't trigger renaming in `leave_Name` if the next name
        is not in an import statement. We don't make any changes here - renaming is the job of `leave_Name`.
        """
        self.is_numpy_import = False
        return updated_node

    def leave_Name(
        self, original_node: "cst.Name", updated_node: "cst.Name"
    ) -> "cst.BaseExpression":
        """
        This method will hit every variable in the code. How to make sure it doesn't change variable
        names other than "numpy" in the import (particularly, not the other reference of "numpy")?
        If the transformer is at an import statement, `visit_SimpleStatementLine` will be called,
        and self.is_numpy_import will be changed to True. Then we can use an if statement here to change the
        node name. If it is not at an import statement, self.is_numpy_import will remain False, so the renaming
        won't be triggered.
        """
        if self.is_numpy_import and updated_node.value == 'numpy':
            return updated_node.with_changes(value='scipy')
        return updated_node

tree = cst.parse_module(code)
new_tree = tree.visit(ImportRenamer())
print(new_tree.code)
"""
```
The transformed code will be
```python
import scipy
from abs import dd

numpy.load('1.npz')
```


