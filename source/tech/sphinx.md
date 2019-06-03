# Sphinx

## sphinxでのtableのかきかた
- eval_rstつけてかこむとrstで解釈してくれる．
````
```eval_rst
  hoge
```
````

- Grid Table
```
+----------+----------+----------+
| Header 1 | Header 2 | Header 3 |
+==========+==========+==========+
| | Item 1 |          |          |
| | Item 2 |          |          |
+----------+----------+----------+
```
- Simple Table
```
======== ======== ========
Header 1 Header 2 Header 3
-------- -------- --------
Item 1   | Item 2
         | Item 3
======== ======== ========
```
- list-table
"|" の位置を合わせることが重要
```
.. list-table:: TheListTable
    :header-rows: 1
    :widths: 10, 10, 10

    * - Header 1
      - Header 2
      - Header 3
    * - | Item1
        | Item2
      - Item3
      - Item4
    * - Item5
      - Item6
      - Item7
```
改段落を利用
```
.. list-table:: TheListTable
    :header-rows: 1
    :widths: 10, 10, 10

    * - Header 1
      - Header 2
      - Header 3
    * - Item1

        Item2
      -
      -
```
- csv-table
```
.. csv-table:: TheCsvTable
    :header: Header 1, Header 2, Header 3
    :widths: 10, 10, 10
    
    "| Item 1
    | Item 2",Item 3,Item 4
```
- ref.) http://saponote.hatenablog.com/entry/2017/11/24/214309