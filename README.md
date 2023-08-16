# htmltab2stata 
## Loading html tables into Stata.

__Table of Contents__
1. [Syntax](#1-syntax)
2. [Options](#2-options)
3. [Description](#3-description)
4. [Examples](#4-examples)
5. [How to install](#5-how-to-install)
6. [About](#6-about)

# 1. Syntax

The general syntax is:
```
htmltab2stata  , url(_url_) [tablenumber(_integer_) firstrow href]
```

# 2. Options

**url(_url_)** the url of the html website to be processed. The url has to be a downloadable html website. _url_ can be a web address or a local html file.

**tablenumber(_integer_)** number of table within the html document. Default is 1, i.e. the first table is processed.

**firstrow** Use firstrow of table as variable names.

**href** Links enclosed in `<a href=....></a>` are added to the content transferred to Stata.

# 3. Description

**htmltab2stata** parses html code from websites. It detects tables enclosed with the html `<table>` environment and transforms the table into a Stata dataset. To do so, **htmltab2stata** parses the html code and uses `<tr>` as row identifiers and code enclosed in `<td>` as columns. It only transfers content which is not enclosed in `< >` to Stata, unless option **href** is used for links. Empty cells remain empty in the Stata dataset.

# 4. Examples

For all examples, the following table in html code saved in _table.html_ is processed:

```
Content
Table 1
<table>
 <tr><th>Country</th><th>Population</th><th>GDP</th></tr>
  <tr><td>Country A</td><td>10</td><td>100</td></tr>
  <tr><td>Country B</td><td>20</td><td>5</td></tr>
  <tr><td>Country C</td><td>500</td><td>10</td></tr>
</table>
More Content
Table 2
<table>
  <tr><th>Firstname</th><th>Surname</th><th>Webpage</th></tr>
  <tr><td>Adam</td><td>Smith</td><td>none</td></tr>
  <tr><td>Allan</td><td>Richards</td><td><a href="www.google.com">webpage</a></td></tr>
  <tr><td>Richard</td><td>Johnson</td><td></td></tr>
</table>
```

Loading Table into Stata as a dataset:

```
htmltab2stata , url(table.html)
```

Returns:

```
.list

     +---------------------------------+
     |    myvar1       myvar2   myvar3 |
     |---------------------------------|
  1. |   Country   Population      GDP |
  2. | Country A           10      100 |
  3. | Country B           20        5 |
  4. | Country C          500       10 |
     +---------------------------------+
```

To use the first column as variable names the option **firstrow** is required.

```
htmltab2stata , url(table.html) firstrow
```

```
.list
     +----------------------------+
     |   Country   Popula~n   GDP |
     |----------------------------|
  1. | Country A         10   100 |
  2. | Country B         20     5 |
  3. | Country C        500    10 |
     +----------------------------+
```

To process Table 2, use the first row as variable names and add the url of the hyperlink as text to the contents:

```
stata htmltab2stata , url(table.html) firstrow tablenumber(2) href
```

```
. list

     +----------------------------------------------+
     | Firstn~e    Surname                  Webpage |
     |----------------------------------------------|
  1. |     Adam      Smith                     none |
  2. |    Allan   Richards   www.google.com webpage |
  3. |  Richard    Johnson                          |
     +----------------------------------------------+
```

# 5. How to install

In Stata:

```
net install htmltab2stata, from(https://janditzen.github.io/htmltab2stata/)
```

# 6. About

Jan Ditzen

Email: jan.ditzen@unibz.it

Web: www.jan.ditzen.net

This version: 1.0
