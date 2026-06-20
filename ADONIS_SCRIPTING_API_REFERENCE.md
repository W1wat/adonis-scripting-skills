# ADONIS FEM Scripting API Reference

**Software:** ADONIS - A Free Finite Element Program for Geo-Engineers
**Author:** Roozbeh Geraili Mikola, Ph.D., P.E.
**Website:** roozbehgm.com
**Script Language:** JavaScript (.ajs files) + Python (via embedded adonis module)
**Manual Version:** V3.90 English

> This reference is derived from the official ADONIS User Manual V3.90.
> ADONIS is a trademark of its respective owner. This document is an independent
> work and is not affiliated with, endorsed by, or connected to the ADONIS developer.
> For the official manual, visit roozbehgm.com.

---

## Table of Contents

1. [JavaScript Language Fundamentals](#1-javascript-language-fundamentals)
2. [JavaScript Element Setter/Getter Reference](#2-javascript-element-settergetter-reference)
3. [JavaScript Node Setter/Getter Reference](#3-javascript-node-settergetter-reference)
4. [JavaScript Input-Output Functions](#4-javascript-input-output-functions)
5. [Python Scripting](#5-python-scripting)
6. [Geometry Commands](#6-geometry-commands)
7. [Mesh Commands](#7-mesh-commands)
8. [Material Commands](#8-material-commands)
9. [Boundary & Initial Conditions](#9-boundary--initial-conditions)
10. [Structural Elements](#10-structural-elements)
11. [Solve Commands](#11-solve-commands)
12. [Settings](#12-settings)
13. [Plot Commands](#13-plot-commands)
14. [File & Script Management](#14-file--script-management)

---

## 1. JavaScript Language Fundamentals

### 1.1 Variables

JavaScript variables are containers for storing data values. All variables must be identified with unique names (identifiers).

**Identifier Rules:**
1. Names can contain letters, digits, underscores, and dollar signs
2. Names must begin with a letter
3. Names can also begin with `$` and `_`
4. Names are case sensitive (`y` and `Y` are different variables)
5. Reserved words (JavaScript keywords) cannot be used as names

**Assignment Operator:** The equal sign (`=`) is an "assignment" operator, not an "equal to" operator.

**Data Types:** Variables can hold numbers (e.g., `100`) and text values (e.g., `"elastic"`). Strings are written inside double or single quotes. Numbers are written without quotes.

```javascript
var x = 5;
var y = 6;
var z = x + y;

var model;
model = "elastic";

var model = "elastic", name = "sand", density = 125;
```

### 1.2 Strings

Strings hold data in text form. Key operations include checking length, concatenation with `+` and `+=`, finding substrings with `indexOf()`, and extracting substrings with `substring()`.

**Creating Strings:**
```javascript
const string1 = "A string primitive";
const string2 = new String("A String object");
```

**Comparing Strings:**
```javascript
let a = 'a';
let b = 'b';
if (a < b) {       // true
    // do something
} else if (a > b) {
    // do something else
} else {
    // otherwise
}
```

**Long Literal Strings:**

Method 1 - Concatenation:
```javascript
let longString = "This is a very long string which needs " +
    "to wrap across multiple lines because " +
    "otherwise my code is unreadable.";
```

Method 2 - Backslash continuation:
```javascript
let longString = "This is a very long string which needs \
to wrap across multiple lines because \
otherwise my code is unreadable.";
```

**Type Conversion:**
```javascript
Number("3.14")   // returns 3.14
Number(" ")      // returns 0
Number("")       // returns 0
Number("99 88")  // returns NaN

String(x)        // returns a string from a number variable x
String(123)      // returns "123"
(123).toString() // returns "123"
```

**String Methods:**

| Method | Description |
|--------|-------------|
| `charAt()` | Returns the character at the specified index |
| `charCodeAt()` | Returns the Unicode of the character at the specified index |
| `concat()` | Joins two or more strings |
| `endsWith()` | Checks whether a string ends with specified characters |
| `fromCharCode()` | Converts Unicode values to characters |
| `includes()` | Returns true/false if string contains a value |
| `indexOf()` | Returns the position of the first occurrence of a specified value |
| `lastIndexOf()` | Returns the position of the last occurrence |
| `localeCompare()` | Compares two strings in the current locale |
| `match()` | Searches for a match against a regular expression |
| `repeat()` | Returns a new string with specified number of copies |
| `replace()` | Searches and replaces specified values |
| `search()` | Searches and returns the position of the match |
| `slice()` | Extracts a part of a string |
| `split()` | Splits a string into an array of substrings |
| `startsWith()` | Checks whether a string begins with specified characters |
| `substr()` | Extracts characters from a start position for a specified count |
| `substring()` | Extracts characters between two specified indices |
| `toLocaleLowerCase()` | Converts to lowercase per host locale |
| `toLocaleUpperCase()` | Converts to uppercase per host locale |
| `toLowerCase()` | Converts to lowercase |
| `toString()` | Returns the value of a String object |
| `toUpperCase()` | Converts to uppercase |
| `trim()` | Removes whitespace from both ends |
| `valueOf()` | Returns the primitive value |

### 1.3 Array Reference

The Array object stores multiple values in a single variable. Array indexes are zero-based.

**Creating an Array:**
```javascript
var IDs = [1, 2, 3, 4, 5];
var material = ["Elastic", "Clay", 2000];
```

**Accessing Elements:**
```javascript
var id = IDs[0];    // Access first element
IDs[0] = 1;         // Modify first element
```

**Array Properties:**

| Property | Description |
|----------|-------------|
| `constructor` | Returns the function that created the Array's prototype |
| `length` | Sets or returns the number of elements |
| `prototype` | Allows adding properties/methods to an Array object |

**Array Methods:**

| Method | Description |
|--------|-------------|
| `concat()` | Joins two or more arrays |
| `copyWithin()` | Copies elements within the array |
| `every()` | Checks if every element passes a test |
| `fill()` | Fills elements with a static value |
| `filter()` | Creates new array with elements that pass a test |
| `find()` | Returns value of first element that passes a test |
| `findIndex()` | Returns index of first element that passes a test |
| `forEach()` | Calls a function for each element |
| `indexOf()` | Searches array for element, returns position |
| `isArray()` | Checks whether object is an array |
| `join()` | Joins all elements into a string |
| `lastIndexOf()` | Searches from the end, returns position |
| `map()` | Creates new array with function result for each element |
| `pop()` | Removes last element |
| `push()` | Adds new elements to end |
| `reduce()` | Reduces values to a single value (left-to-right) |
| `reduceRight()` | Reduces values to a single value (right-to-left) |
| `reverse()` | Reverses element order |
| `shift()` | Removes first element |
| `slice()` | Selects a part of an array |
| `some()` | Checks if any element passes a test |
| `sort()` | Sorts elements |
| `splice()` | Adds/Removes elements |
| `toString()` | Converts to string |
| `unshift()` | Adds elements to beginning |
| `valueOf()` | Returns primitive value |

**Examples:**
```javascript
var names = ["Sand", "Clay", "Silt", "Null"];
names.length;                     // 4
names.push("Gravel");             // adds to end
names[6] = "Gravel";              // adds at index 6
var y = names.sort();             // sorts array
names[names.length] = "Gravel";   // adds to end
```

### 1.4 Operators

**Arithmetic Operators:**

| Operator | Description |
|----------|-------------|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulus |
| `++` | Increment |
| `--` | Decrement |

**Assignment Operators:**

| Operator | Example | Same As |
|----------|---------|---------|
| `=` | `x = y` | `x = y` |
| `+=` | `x += y` | `x = x + y` |
| `-=` | `x -= y` | `x = x - y` |
| `*=` | `x *= y` | `x = x * y` |
| `/=` | `x /= y` | `x = x / y` |
| `%=` | `x %= y` | `x = x % y` |

**Comparison Operators:**

| Operator | Description |
|----------|-------------|
| `==` | equal to |
| `===` | equal value and equal type |
| `!=` | not equal |
| `!==` | not equal value or not equal type |
| `>` | greater than |
| `<` | less than |
| `>=` | greater than or equal to |
| `<=` | less than or equal to |
| `?` | ternary operator |

**Logical Operators:**

| Operator | Description |
|----------|-------------|
| `&&` | logical and |
| `\|\|` | logical or |
| `!` | logical not |

### 1.5 Conditions (If...Else)

```javascript
if (condition) {
    // code if true
}

if (condition) {
    // code if true
} else {
    // code if false
}

if (condition1) {
    // code if condition1 is true
} else if (condition2) {
    // code if condition1 false and condition2 true
} else {
    // code if both false
}
```

**Example:**
```javascript
if (ID < 2) {
    model = "Elastic";
} else if (time < 4) {
    model = "Mohr-Coulomb";
} else {
    model = "Hoek-Brown";
}
```

### 1.6 Loops

**For Loop:**
```javascript
for (i = 0; i < 5; i++) {
    text += "The number is " + i;
}
```

**For/In Loop:**
```javascript
var material = {name:"Elastic", ID:1};
var text = "";
var x;
for (x in material) {
    text += material[x];
}
```

**While Loop:**
```javascript
while (i < 10) {
    text += "The number is " + i;
    i++;
}
```

**Do/While Loop:**
```javascript
do {
    text += "The number is " + i;
    i++;
} while (i < 10);
```

### 1.7 Break and Continue

**Break:** Exits a loop entirely.
```javascript
for (i = 0; i < 10; i++) {
    if (i === 3) { break; }
    text += "The number is " + i;
}
```

**Continue:** Skips one iteration.
```javascript
for (i = 0; i < 10; i++) {
    if (i === 3) { continue; }
    text += "The number is " + i;
}
```

**Label and Break:** Jump out of any code block.
```javascript
var IDs = [1, 2, 3, 4];
list: {
    id += 1;
    id += 2;
    id += 3;
    break list;
    id += 4;  // never executed
    id += 5;
    id += 6;
}
```

### 1.8 Comments

**Single-line:**
```javascript
// Draw line:
line("startPoint",0,0,"endPoint",5,0)
// Export Image:
exportmodel("image","filename","C:/Portable/ADONIS/test.png")
```

**Multi-line:**
```javascript
/*
The code below will create
a circle with center at 0,0
and radius equal with 10
*/
var cx = 0;
var cy = 0;
var rad = 5;
var nseg = 5;
circle("centerPoint",cx,cy,"radius",rad,"numSeg",nseg)
```

### 1.9 Functions

```javascript
function name(parameter1, parameter2, parameter3) {
    // code to be executed
}
```

**Return:**
```javascript
function myFunction(a, b) {
    return a * b;
}
var x = myFunction(4, 3);  // x = 12
```

### 1.10 Math Reference

**Math Object Properties:**

| Property | Description |
|----------|-------------|
| `Math.E` | Euler's number |
| `Math.LN2` | Natural logarithm of 2 |
| `Math.LN10` | Natural logarithm of 10 |
| `Math.LOG2E` | Base-2 logarithm of E |
| `Math.LOG10E` | Base-10 logarithm of E |
| `Math.PI` | PI |
| `Math.SQRT1_2` | Square root of 1/2 |
| `Math.SQRT2` | Square root of 2 |

**Math Methods:**

| Method | Description |
|--------|-------------|
| `Math.abs(x)` | Absolute value of x |
| `Math.acos(x)` | Arccosine of x, in radians |
| `Math.asin(x)` | Arcsine of x, in radians |
| `Math.atan(x)` | Arctangent of x (-PI/2 to PI/2) |
| `Math.atan2(y, x)` | Arctangent of the quotient |
| `Math.ceil(x)` | Rounded upwards to nearest integer |
| `Math.cos(x)` | Cosine of x (radians) |
| `Math.exp(x)` | Value of E^x |
| `Math.floor(x)` | Rounded downwards to nearest integer |
| `Math.log(x)` | Natural logarithm (base E) of x |
| `Math.max(x, y, ..., n)` | Highest value |
| `Math.min(x, y, ..., n)` | Lowest value |
| `Math.pow(x, y)` | x to the power of y |
| `Math.random()` | Random number between 0 and 1 |
| `Math.round(x)` | x rounded to nearest integer |
| `Math.sin(x)` | Sine of x (radians) |
| `Math.sqrt(x)` | Square root of x |
| `Math.tan(x)` | Tangent of an angle |

**Example:**
```javascript
var x = Math.PI;           // Returns PI
var y = Math.sqrt(16);     // Returns 4
```

---

## 2. JavaScript Element Setter/Getter Reference

### 2.1 Element Getter

These functions extract values of element data. Part of the scripting language only (not in GUI).

**Syntax:**
```javascript
value = getelem(<keywords>)
```

**Keywords:**

| Output Type | Keyword | Input | Description |
|-------------|---------|-------|-------------|
| Array of Integers | `"allid"` | - | List of all element IDs in the model |
| Array of Integers | `"activeid"` | - | List of all active element IDs |
| Array of Integers | `"nodeid",elid` | Element ID | Node IDs for element with ID = elid |
| Single Integer | `"numnode",elid` | Element ID | Number of nodes of element |
| Single Integer | `"numgauss",elid` | Element ID | Number of gauss points of element |
| Array of Floats `[xpos,ypos]` | `"gausspointpos",elid,<gid>` | Element ID, Gauss Point ID | Position of gauss point |
| Array of Floats `[exx,eyy,exy]` | `"strain",elid,<gid>` | Element ID, Gauss Point ID | Accumulated strain at gauss point. T3 has 1 GP, T6 has 3 GPs. Default gid=1 |
| Array of Floats `[sxx,syy,sxy,szz]` | `"stress",elid,<gid>` | Element ID, Gauss Point ID | Accumulated stress at gauss point. T3 has 1 GP, T6 has 3 GPs. Default gid=1 |
| Single Float | `"pp",elid,<gid>` | Element ID | Pore pressure at gauss point. Default gid=1 |
| Single Integer | `"idatpoint",xp,yp` | Point coordinates | ID of element containing point [xp,yp] |
| Single Float | `"prop",str,elid,<gid>` | str = `"density"` or `"shear"` or `"bulk"` or `"matid"`, Element ID | Property value at gauss point. Default gid=1 |

**Examples:**
```javascript
ids = getelem("activeid");                    // Array of active element IDs
stress = getelem("stress",11);                // Stress [sxx,syy,sxy,szz] at 1st GP of element 11
stress = getelem("stress",11,3);              // Stress at 3rd GP of element 11
```

### 2.2 Element Setter

These functions manipulate element data.

**Syntax:**
```javascript
setelem(<keywords>)
```

**Keywords:**

| Keyword | Description |
|---------|-------------|
| `"stress",elid,<gid>,sid,value` | Set stress at gauss point. sid: 1=sxx, 2=syy, 3=sxy, 4=szz. Default gid=1 |
| `"pp",elid,<gid>,value` | Set pore pressure at gauss point. Default gid=1 |

**Examples:**
```javascript
setelem("pp",11,1E4);                  // Set pore pressure at element 11 to 1E4
setelem("stress",11,2,-1E6);           // Set syy (sid=2) at 1st GP of element 11
setelem("stress",11,2,3,-1E6);         // Set sxy (sid=3) at 2nd GP of element 11
```

---

## 3. JavaScript Node Setter/Getter Reference

### 3.1 Node Getter

**Syntax:**
```javascript
value = getnode(<keywords>)
```

**Keywords:**

| Output Type | Keyword | Input | Description |
|-------------|---------|-------|-------------|
| Array of Integers | `"allid"` | - | List of all node IDs |
| Array of Integers | `"activeid"` | - | List of all active node IDs |
| Array of Integers | `"boundid"` | - | List of all boundary node IDs |
| Array of Floats `[xpos,ypos]` | `"pos",ndid` | Node ID | Position of node |
| Array of Floats `[xdisp,ydisp]` | `"disp",ndid` | Node ID | Total displacement of node |
| Array of Floats `[xunbal,yunbal]` | `"unbal",ndid` | Node ID | Unbalance force of node |
| Single Integer | `"idatpoint",xp,yp` | Point coordinates | ID of closest node to point |

**Examples:**
```javascript
ids = getnode("activeid");         // Array of active node IDs
disp = getnode("disp",11);         // Displacement [xdisp,ydisp] at node 11
```

### 3.2 Node Setter

**Syntax:**
```javascript
setnode(<keywords>)
```

**Keywords:**

| Keyword | Description |
|---------|-------------|
| `"xforce",ndid,value` | Set horizontal force at node |
| `"yforce",ndid,value` | Set vertical force at node |
| `"xvel",ndid,value` | Set horizontal velocity at node |
| `"yvel",ndid,value` | Set vertical velocity at node |
| `"xfix",ndid,value` | Fix horizontal displacement at node |
| `"yfix",ndid` | Fix vertical displacement at node |
| `"xfree",ndid` | Free horizontal constraint at node |
| `"yfree",ndid` | Free vertical constraint at node |

**Examples:**
```javascript
setnode("xforce",11,1E4);    // Apply horizontal force at node 11
setnode("yfix",11);          // Fix vertical displacement at node 11
```

---

## 4. JavaScript Input-Output Functions

### 4.1 File I/O

**Open File:**
```javascript
fid = fopen(filename, permission)
```
- `fid`: scalar integer file identifier. Returns -1 if cannot open.
- `permission`:
  - `"r"` - Open for reading
  - `"w"` - Open/create for writing; discard existing contents

**Write Data:**
```javascript
fprintf(fid, str)
```
- `fid`: file identifier from fopen
- `str`: string data to write

**Close File:**
```javascript
fclose(fid)
```

**Example:**
```javascript
fileID = fopen("output.txt","w");
fprintf(fileID,"Hello\n");
fprintf(fileID,"Thanks for using ADONIS\n");
fclose(fileID);
```

---

## 5. Python Scripting

### 5.1 Introduction

Python scripts interact with ADONIS through the `adonis` module. Import it as:
```python
import adonis as ad
```

The `ad.command()` function issues ADONIS JavaScript commands. Triple quotes support multi-line strings.

**Example:**
```python
import adonis as ad

ad.command("newmodel()")
ad.command("""
rect('startPoint',-5,-20,'endPoint',5,0)
triangle('elemtype','T3')
discretize('auto')
triangle('auto')
material('create','IsoElastic','matid',1,'matname','Material 1',
    'density',2500,'shear',2.8e+09,'bulk',3.9e+09)
material('assign','matid',1)
applybc('xfix','xlim',4.712,5.404,'ylim',-20.497,0.297)
applybc('xfix','xlim',-6.920,-4.646,'ylim',-20.596,0.857)
applybc('yfix','xlim',-5.866,6.129,'ylim',-20.925,-19.739)
set('gravity',0,10)
solve()
""")
```

### 5.2 ADONIS Module Functions

| Function | Return Type | Description |
|----------|-------------|-------------|
| `adonis.command(command: str)` | None | Issue an ADONIS command |
| `adonis.set_path(method: str)` | None | Modify current working path |
| `adonis.fos()` | float | Get the factor of safety |

### 5.3 adonis.element Module

| Function | Return Type | Description |
|----------|-------------|-------------|
| `adonis.element.list()` | Element object list | Get all element objects |

### 5.4 Element Class (`itasca.element.Element`)

> Do not instantiate directly. Use module functions.

| Method | Return Type | Description |
|--------|-------------|-------------|
| `id()` | int | Get element ID |
| `num_node()` | int | Number of nodes associated with element |
| `num_gauss()` | int | Number of gauss points |
| `gauss_pos(id: int)` | vector | Gauss point position. T3: id=1; T6: id=1,2,3 |
| `strain(id: int)` | tens3 | Strain increment tensor at gauss point |
| `stress(id: int)` | tens3 | Total stress tensor at gauss point |
| `pp(id: int)` | float | Pore pressure at gauss point |
| `prop(id: int, property_name: str)` | float | Property value at gauss point |

### 5.5 adonis.node Module

| Function | Return Type | Description |
|----------|-------------|-------------|
| `adonis.node.list()` | Node object list | Get all node objects |

### 5.6 ElemNode Class (`itasca.node.ElemNode`)

> Do not instantiate directly. Use module functions.

| Method | Return Type | Description |
|--------|-------------|-------------|
| `id()` | int | Get node ID |
| `pos()` | vector2d | Get node location |
| `disp()` | vector | Get node displacement |
| `unbal()` | vector | Get node out-of-balance force |

### 5.7 vect2 Class

| Method | Return Type | Description |
|--------|-------------|-------------|
| `dot(value: vector2d)` | float | 2D vector dot product |
| `mag()` | float | L2 norm |
| `norm()` | vector2d | Normalized vector (new vector) |
| `x()` | float | X component |
| `y()` | float | Y component |

### 5.8 vect3 Class

| Method | Return Type | Description |
|--------|-------------|-------------|
| `dot(value: vector3d)` | float | 3D vector dot product |
| `mag()` | float | L2 norm |
| `norm()` | vector3d | Normalized vector (new vector) |
| `cross(value: vector3d)` | float | Vector cross product |
| `x()` | float | X component |
| `y()` | float | Y component |
| `z()` | float | Z component |

### 5.9 tens3 Class

| Method | Return Type | Description |
|--------|-------------|-------------|
| `trace()` | float | Trace of tensor |
| `xx()` | float | XX component |
| `xy()` | vector3d | XY component |
| `xz()` | float | XZ component |
| `yx()` | float | YX component |
| `yy()` | float | YY component |
| `yz()` | float | YZ component |
| `zx()` | vector3d | ZX component |
| `zy()` | float | ZY component |
| `zz()` | float | ZZ component |

---

## 6. Geometry Commands

### 6.1 Draw Line

**Menu:** Geometry > Draw Line

```javascript
line("startPoint", xs, ys, "endPoint", xe, ye)
```

| Parameter | Description |
|-----------|-------------|
| `xs, ys` | Coordinates of start point |
| `xe, ye` | Coordinates of end point |

**Example:**
```javascript
line("startPoint", 0, 0, "endPoint", 5, 0)
```

### 6.2 Draw Rectangle

**Menu:** Geometry > Draw Rectangle

```javascript
rect("startPoint", xs, ys, "endPoint", xe, ye)
```

| Parameter | Description |
|-----------|-------------|
| `xs, ys` | Coordinates of start point (corner) |
| `xe, ye` | Coordinates of end point (opposite corner) |

**Example:**
```javascript
rect("startPoint", 0, 0, "endPoint", 10, 5)
```

### 6.3 Draw Circle

**Menu:** Geometry > Draw Circle

```javascript
circle("centerPoint", xc, yc, "radius", rad, "numSeg", nseg)
```

| Parameter | Description |
|-----------|-------------|
| `xc, yc` | Coordinates of center point |
| `rad` | Radius of circle |
| `nseg` | Number of segments |

**Example:**
```javascript
circle("centerPoint", 0, 0, "radius", 5, "numSeg", 40)
```

### 6.4 Draw Arc

**Menu:** Geometry > Draw Arc

```javascript
arc("startPoint", xs, ys, "midPoint", xm, ym, "endPoint", xe, ye, "numSeg", nseg)
```

| Parameter | Description |
|-----------|-------------|
| `xs, ys` | Coordinates of start point |
| `xm, ym` | Coordinates of midpoint |
| `xe, ye` | Coordinates of end point |
| `nseg` | Number of segments |

**Example:**
```javascript
arc("startPoint", 1, 0, "midPoint", 0.71, 0.71, "endPoint", 0, 1, "numSeg", 20)
```

### 6.5 Draw Crack

**Menu:** Geometry > Draw Crack

```javascript
crack("startPoint", xs, ys, "endPoint", xe, ye)
```

| Parameter | Description |
|-----------|-------------|
| `xs, ys` | Coordinates of start point |
| `xe, ye` | Coordinates of end point |

**Example:**
```javascript
crack("startPoint", 0, 0, "endPoint", 5, 0)
```

### 6.6 Draw Joint

**Menu:** Geometry > Draw Joint

```javascript
joint("startPoint", xs, ys, "endPoint", xe, ye, "id", id)
```

| Parameter | Description |
|-----------|-------------|
| `xs, ys` | Coordinates of start point |
| `xe, ye` | Coordinates of end point |
| `id` | Joint ID number (used to specify interface material) |

**Example:**
```javascript
joint("startPoint", 0, 0, "endPoint", 5, 0, "id", 1)
```

> **Note:** Mohr-Coulomb criteria is assigned by default to the interface between two sides of joint. Interface material properties should be modified through "Assign Material".

---

## 7. Mesh Commands

### 7.1 Discretize Model

**Menu:** Mesh > Discretize/Mesh

```javascript
discretize(<keywords>)
```

| Keyword | Description |
|---------|-------------|
| `"auto"` | Use default discretization values |
| `"maxedge", value` | Discretize based on maximum edge size |
| `"maxarea", value` | Discretize based on maximum area size |

**Example:**
```javascript
discretize("maxedge", 20)
```

### 7.2 Customize Discretize

**Menu:** Mesh > Discretize/Mesh

```javascript
segment("id", i1, i2, ..., in, "numedge", ne)
```

| Parameter | Description |
|-----------|-------------|
| `i1, i2, ..., in` | Segment ID numbers |
| `ne` | Number of edges per segment |

**Example:**
```javascript
segment("id", 5, "numedge", 50)
```

> **Note:** Must discretize the model first. Custom discretization applies only to already discretized boundary lines.

### 7.3 Generate Mesh (Triangulate)

**Menu:** Mesh > Discretize/Mesh

```javascript
gmsh(<keywords>)
```

| Keyword | Description |
|---------|-------------|
| `"size", "auto"` | Use default mesh generation values |
| `"maxedge", value` | Triangulate based on maximum edge size |
| `"maxarea", value` | Triangulate based on maximum area size |
| `"elemtype", str` | Element type: `"T3"` (3-node triangle) or `"Q4"` (4-node quadrilateral) |
| `"useNMD", str` | Nodal Mixed Discretization: `"on"` or `"off"` |

**Example:**
```javascript
gmsh("size", "auto", "elemtype", "T3", "useNMD", "on")
```

> **Note:** NMD (Nodal Mixed Discretization) is introduced by Detourney & Dzik (2006) as an improvement of Mixed Discretization (MD) technique.

---

## 8. Material Commands

### 8.1 Create Soil/Rock Material

**Menu:** Material > Create/Assign

```javascript
material("create", matname, "matid", id, "matname", name, <proplist>)
```

| Parameter | Description |
|-----------|-------------|
| `matname` | Material type: `"IsoElastic"`, `"Mohr-Coulomb"`, `"Hoek-Brown"`, `"Modified Hoek-Brown"`, `"CamClay"`, `"StrainSoftening"`, `"P-Hardening"`, `"Ubiquitous-Joint"` |
| `id` | Material ID (positive integer) |
| `name` | Optional display name for legend |
| `proplist` | Property key-value pairs |

#### IsoElastic Properties:
| Property | Description |
|----------|-------------|
| `"density", val` | Mass density |
| `"shear", val` | Elastic shear modulus |
| `"bulk", val` | Elastic bulk modulus |

#### Mohr-Coulomb Properties:
| Property | Description |
|----------|-------------|
| `"density", val` | Mass density |
| `"shear", val` | Elastic shear modulus |
| `"bulk", val` | Elastic bulk modulus |
| `"coh", val` | Cohesion |
| `"fric", val` | Friction angle |
| `"dil", val` | Dilation angle |
| `"tens", val` | Tension limit |

#### Hoek-Brown Properties:
| Property | Description |
|----------|-------------|
| `"density", val` | Mass density |
| `"shear", val` | Elastic shear modulus |
| `"bulk", val` | Elastic bulk modulus |
| `"sigci", val` | Hoek-Brown parameter, sigma_ci |
| `"mb", val` | Hoek-Brown parameter, mb |
| `"s", val` | Hoek-Brown parameter, s |
| `"a", val` | Hoek-Brown parameter, a |
| `"s3cv", val` | Hoek-Brown parameter, sigma_cv |

#### Modified Hoek-Brown Properties:
| Property | Description |
|----------|-------------|
| `"density", val` | Mass density |
| `"shear", val` | Elastic shear modulus |
| `"bulk", val` | Elastic bulk modulus |
| `"sigci", val` | Hoek-Brown parameter, sigma_ci |
| `"mb", val` | Hoek-Brown parameter, mb |
| `"s", val` | Hoek-Brown parameter, s |
| `"a", val` | Hoek-Brown parameter, a |
| `"tension", val` | Current value of tensile strength, sigma_t |

#### Cam-Clay Properties:
| Property | Description |
|----------|-------------|
| `"density", val` | Mass density |
| `"shear", val` | Elastic shear modulus |
| `"bulk_bound", val` | Elastic bulk modulus |
| `"poisson", val` | Poisson's ratio |
| `"kappa", val` | Slope of swelling line |
| `"lambda", val` | Slope of normal consolidation line |
| `"mm", val` | Frictional constant |
| `"mpc", val` | Pre-consolidation pressure |
| `"mp1", val` | Reference pressure |
| `"mv_l", val` | Specific volume at reference pressure on NCL |
| `"cv", val` | Current specific volume (calculated internally if zero) |

#### Strain-Softening Properties:
| Property | Description |
|----------|-------------|
| `"density", val` | Mass density |
| `"shear", val` | Elastic shear modulus |
| `"bulk", val` | Elastic bulk modulus |
| `"coh", val` | Cohesion |
| `"fric", val` | Friction angle |
| `"dil", val` | Dilation angle |
| `"tens", val` | Tension limit |

#### Plastic Hardening (P-Hardening) Properties:
| Property | Description |
|----------|-------------|
| `"density", val` | Mass density |
| `"E50_ref", val` | Secant stiffness at 50% of ultimate deviatoric stress |
| `"Eur_ref", val` | Unloading-reloading stiffness at sigma3 = -p_ref. Default: 4xE50_ref |
| `"p_ref", val` | Reference pressure |
| `"m", val` | Elastic modulus exponent (sand: 0.4-0.9; clay: ~1.0) |
| `"poisson", val` | Poisson's ratio. Default: 0.2 |
| `"Rf", val` | Failure ratio. Default: 0.9 |
| `"ocr", val` | Over consolidation ratio. Default: 100.0 |
| `"Knc", val` | Normal consolidation coefficient. Default: 1 - sin(phi) |
| `"Eoed_ref", val` | Oedometer tangent stiffness. Default: E50_ref |
| `"cohesion", val` | Cohesion, c. Default min: 1.0E-05 x p_ref |
| `"friction", val` | Ultimate friction angle, phi. Required. Min: 0.001 deg |
| `"dilation", val` | Ultimate dilation angle. Default: 0 |
| `"tension", val` | Tension limit. Default: 0.0 |
| `"sig1", val` | Initial min principal effective stress (REQUIRED first time) |
| `"sig2", val` | Initial mid principal effective stress (REQUIRED first time) |
| `"sig3", val` | Initial max principal effective stress (REQUIRED first time) |
| `"void_ini", val` | Initial void ratio. Default: 1.0 |
| `"void_max", val` | Maximum void ratio. Default: 999.0 |
| `"Fc", val` | Contraction factor. Default: 0. Range: 0-0.25 |
| `"fcut", val` | Cut-off factor. Default: 0.1 |
| `"flag_small", val` | Small-strain stiffness flag (1=on, 0=off). Default: 0 |
| `"E0_ref", val` | Initial stiffness at zero strain. Needed only when flag_small=1 |
| `"gamma70", val` | Shear strain at Gs/G0=72.2%. Default: 2.0E-04. Only for small-strain |

#### Ubiquitous-Joint Properties:
| Property | Description |
|----------|-------------|
| `"density", val` | Mass density |
| `"shear", val` | Elastic shear modulus |
| `"bulk", val` | Elastic bulk modulus |
| `"coh", val` | Rock cohesion |
| `"fric", val` | Rock friction angle |
| `"dil", val` | Rock dilation angle |
| `"tens", val` | Rock tension limit |
| `"jangle", val` | Joint angle (counterclockwise from x-axis) |
| `"jcoh", val` | Joint cohesion |
| `"jfric", val` | Joint friction angle |
| `"jdil", val` | Joint dilation angle |
| `"jtens", val` | Joint tension limit |

**Examples:**
```javascript
material("create", "P-Hardening", "matid", 5, "matname", "Layer1",
    "density", 1900, "E50_ref", 4.5E7, "Eur_ref", 1.8E8,
    "p_ref", 100000.0, "m", 0.55, "ocr", 1.0, "Eoed_ref", 4.5E7,
    "cohesion", cohesion, "friction", 35, "dilation", 5)

material("create", "Ubiquitous-Joint", "matid", 1, "matname", "Jointed Rock",
    "density", 2600, "shear", 3e+07, "bulk", 1e+08,
    "coh", 100000, "fric", 40, "jangle", 30, "jcoh", 5000, "jfric", 25)
```

> **Note:** Must discretize the model before using this command.

### 8.2 Assign Soil/Rock Material

**Menu:** Material > Create/Assign

```javascript
material("assign", "matid", id, "region", xi, yi)
```

| Parameter | Description |
|-----------|-------------|
| `id` | Existing material ID |
| `xi, yi` | Optional: coordinates inside the region |

**Example:**
```javascript
material("assign", "matid", 1, "region", 5.0, 2.0)
```

> **Note:** If no region specified, material is applied to entire model.

### 8.3 Assign/Edit Interface Material

**Menu:** Material > Create/Assign

```javascript
imaterial("assign", matname, "ifid", id, "matname", name, <proplist>)
imaterial("edit", matname, "ifid", id, "matname", name, <proplist>)
```

| Parameter | Description |
|-----------|-------------|
| `matname` | Interface type: `"Mohr-Coulomb"` or `"Glued"` |
| `id` | Interface material ID (positive integer) |
| `name` | Optional display name |

**Mohr-Coulomb Interface Properties:**

| Property | Description |
|----------|-------------|
| `"jkn", val` | Normal stiffness |
| `"jks", val` | Shear stiffness |
| `"friction", val` | Friction angle |
| `"cohesion", val` | Cohesion |
| `"tension", val` | Tension |

**Glued Interface Properties:**

| Property | Description |
|----------|-------------|
| `"jkn", val` | Normal stiffness |
| `"jks", val` | Shear stiffness |

**Example:**
```javascript
imaterial("assign", "Mohr-Coulomb", "ifid", 1, "matname", "Interface1",
    "jkn", 2e+8, "jks", 2e+8, "friction", 30, "cohesion", 0, "tension", 0)
imaterial("edit", "ifid", 1, "jkn", 1e+8, "jks", 1e+8)
```

### 8.4 Excavate

**Menu:** Material > Create/Assign

```javascript
excavate("region", xi, yi)
```

| Parameter | Description |
|-----------|-------------|
| `xi, yi` | Coordinates of point inside the region |

**Example:**
```javascript
excavate("region", 5.0, 2.0)
```

### 8.5 Fill (Backfill)

**Menu:** Material > Create/Assign

```javascript
fill("region", xi, yi)
```

| Parameter | Description |
|-----------|-------------|
| `xi, yi` | Coordinates of point inside the region |

**Example:**
```javascript
fill("region", 5.0, 2.0)
```

---

## 9. Boundary & Initial Conditions

### 9.1 Nodal Boundary Conditions

**Menu:** Initial > Apply Boundary Condition

```javascript
applybc(<keywords>, "xlim", xl, xu, "ylim", yl, yu)
```

| Parameter | Description |
|-----------|-------------|
| `xl, xu` | Lower and upper range in x-axis |
| `yl, yu` | Lower and upper range in y-axis |

**Keywords:**

| Keyword | Description |
|---------|-------------|
| `"xfree"` | Release x-direction constraint |
| `"yfree"` | Release y-direction constraint |
| `"xyfree"` | Release x and y direction constraints |
| `"xfix"` | Fix horizontal (x) velocity |
| `"yfix"` | Fix vertical (y) velocity |
| `"xyfix"` | Fix velocity in x and y directions |
| `"xforce", value` | Apply force in x direction |
| `"yforce", value` | Apply force in y direction |
| `"sxx", value` | Apply stress on boundary edge in x direction |
| `"syy", value` | Apply stress on boundary edge in y direction |
| `"nstress", value` | Apply normal stress (compressive = negative) |
| `"xremove"` | Erase all BCs in x direction |
| `"yremove"` | Erase all BCs in y direction |
| `"xyremove"` | Erase all BCs in x and y directions |
| `"xvel", value` | Apply velocity in x direction |
| `"yvel", value` | Apply velocity in y direction |

**Notes:**
1. Fixed displacement: velocities should be initialized to zero (default on startup)
2. Velocity/force/stress BC has no effect on fixed nodes
3. Velocity BC accumulates if node already has velocity BC
4. Velocity BC replaces existing force/stress BC
5. Force/stress BC accumulates if already applied
6. Force BC replaces existing velocity BC
7. Stress BC replaces existing velocity BC

### 9.2 Initial Conditions (Element/Nodal)

**Menu:** Initial > Apply Insitu

```javascript
initial(<keywords>, "xlim", xl, xu, "ylim", yl, yu, "xvar", xv, "yvar", yv)
```

| Parameter | Description |
|-----------|-------------|
| `xl, xu` | Lower and upper range in x-axis |
| `yl, yu` | Lower and upper range in y-axis |
| `"xvar", xv` | Optional: gradient in x direction |
| `"yvar", yv` | Optional: gradient in y direction |

Value varies as: `modified_value = value + xv * x + yv * y`

**Keywords:**

| Keyword | Description |
|---------|-------------|
| `"sxx", value` | Initial horizontal stress |
| `"syy", value` | Initial vertical stress |
| `"sxy", value` | Initial shear stress |
| `"szz", value` | Initial out-of-plane stress |
| `"movexdir", value` | Move nodes in x-direction |
| `"moveydir", value` | Move nodes in y-direction |

### 9.3 Structural Boundary Conditions

**Menu:** Initial > Apply Structural Boundary Condition

```javascript
applystruc(<keywords>, "xlim", xl, xu, "ylim", yl, yu)
```

| Parameter | Description |
|-----------|-------------|
| `xl, xu` | Lower and upper range in x-axis |
| `yl, yu` | Lower and upper range in y-axis |

**Keywords:**

| Keyword | Description |
|---------|-------------|
| `"xfix"` | Fix horizontal velocity |
| `"yfix"` | Fix vertical velocity |
| `"rotfix"` | Fix rotational velocity |
| `"allfix"` | Fix all velocities |
| `"pin"` | Pin connection at node |
| `"xforce", value` | Apply force in x direction |
| `"yforce", value` | Apply force in y direction |
| `"moment", value` | Apply moment |
| `"equaldof"` | Set slave condition of nodes |
| `"xvel", value` | Apply velocity in x direction |
| `"yvel", value` | Apply velocity in y direction |
| `"rotvel", value` | Apply rotational velocity |
| `"xfree"` | Release x constraint |
| `"yfree"` | Release y constraint |
| `"rotfree"` | Release rotational constraint |
| `"allfree"` | Release all constraints |

### 9.4 Structural Initial Conditions

**Menu:** Initial > Apply Structure Initial

```javascript
structure("initial", <keywords>, "xlim", xl, xu, "ylim", yl, yu)
```

**Keywords:**

| Keyword | Description |
|---------|-------------|
| `"reset", str` | Reset internal forces. str = `"axialforce"`, `"shearforce"`, or `"all"` |

---

## 10. Structural Elements

### 10.1 Add Beam/Liner

**Menu:** Structure > Add Beam

**Attach to Grid:**
```javascript
structure("drawbeam", "beamid", bid, "xlim", xl, xu, "ylim", yl, yu)
structure("drawbeam", "beamid", bid, "edgetags", t1, t2, ..., tn)
```

**Attach to Interface (one side):**
```javascript
structure("drawliner", "beamid", bid, "ifid1", iid1, "xlim", xl, xu, "ylim", yl, yu)
structure("drawliner", "beamid", bid, "ifid1", iid1, "edgetags", t1, t2, ..., tn)
```

**Attach to Interface (both sides):**
```javascript
structure("drawliner", "beamid", bid, "ifid1", iid1, "ifid2", iid2, "xlim", xl, xu, "ylim", yl, yu)
```

**Free mode:**
```javascript
structure("drawbeam", "beamid", bid, "frompoint", xs, ys, "topoint", xe, ye, "segnum", sn)
structure("drawbeam", "beamid", bid, "fromstrucnode", ns, "tostrucnode", ne, "segnum", sn)
structure("drawbeam", "beamid", bid, "fromstrucnode", ns, "topoint", xe, ye, "segnum", sn)
structure("drawbeam", "beamid", bid, "frompoint", xs, ys, "tostrucnode", ne, "segnum", sn)
```

| Parameter | Description |
|-----------|-------------|
| `bid` | Beam ID number |
| `iid1, iid2` | Interface IDs for right and left sides |
| `xl, xu, yl, yu` | Selection range |
| `t1, t2, ..., tn` | Edge tag numbers |
| `xs, ys` | Start point coordinates |
| `xe, ye` | End point coordinates |
| `ns, ne` | Structural node tag numbers |
| `sn` | Number of structural element segments |

**Examples:**
```javascript
structure("drawbeam", "beamid", 1, "xlim", 53.3, 283.5, "ylim", 125.7, 147.4)
structure("drawliner", "beamid", 1, "ifid1", 1, "xlim", 238.773, 444.958, "ylim", -211.198, -30.082)
```

> **Note:** From V2.0+, beam elements can only be attached to boundary edges. Interface defaults to Mohr-Coulomb with zero properties (must be modified).

### 10.2 Add Cable

**Menu:** Structure > Add Cable

```javascript
structure("drawcable", "cabid", id, "frompoint", xs, ys, "topoint", xe, ye, "segnum", sn)
structure("drawcable", "cabid", id, "fromstrucnode", ns, "tostrucnode", ne, "segnum", sn)
structure("drawcable", "cabid", id, "fromstrucnode", ns, "topoint", xe, ye, "segnum", sn)
structure("drawcable", "cabid", id, "frompoint", xs, ys, "tostrucnode", ne, "segnum", sn)
```

| Parameter | Description |
|-----------|-------------|
| `id` | Cable ID number |
| `xs, ys` | Start point coordinates |
| `xe, ye` | End point coordinates |
| `ns, ne` | Structural node tag numbers |
| `sn` | Number of structural element segments |

### 10.3 Add Tieback

**Menu:** Structure > Add Tieback

```javascript
structure("drawtieback", "tieid", id, "frompoint", xs, ys, "topoint", xe, ye, "grouted", gn, "segnum", sn)
structure("drawtieback", "tieid", id, "fromstrucnode", ns, "tostrucnode", ne, "grouted", gn, "segnum", sn)
structure("drawtieback", "tieid", id, "fromstrucnode", ns, "topoint", xe, ye, "grouted", gn, "segnum", sn)
structure("drawtieback", "tieid", id, "frompoint", xs, ys, "tostrucnode", ne, "grouted", gn, "segnum", sn)
```

| Parameter | Description |
|-----------|-------------|
| `id` | Tieback ID number |
| `xs, ys` | Start point coordinates |
| `xe, ye` | End point coordinates |
| `ns, ne` | Structural node tag numbers |
| `gn` | Grouted portion value [0 to 1] |
| `sn` | Number of structural element segments |

### 10.4 Add Strip

**Menu:** Structure > Add Strip

```javascript
structure("drawstrip", "gridid", id, "frompoint", xs, ys, "topoint", xe, ye, "segnum", sn)
structure("drawstrip", "gridid", id, "fromstrucnode", ns, "tostrucnode", ne, "segnum", sn)
structure("drawstrip", "gridid", id, "fromstrucnode", ns, "topoint", xe, ye, "segnum", sn)
structure("drawstrip", "gridid", id, "frompoint", xs, ys, "tostrucnode", ne, "segnum", sn)
```

| Parameter | Description |
|-----------|-------------|
| `id` | Strip ID number |
| `xs, ys` | Start point coordinates |
| `xe, ye` | End point coordinates |
| `ns, ne` | Structural node tag numbers |
| `sn` | Number of structural element segments |

### 10.5 Set Beam Properties

**Menu:** Structure > Add Beam > Set Beam Property

```javascript
structure("material", "beamid", id, <keywords>)
```

| Keyword | Unit | Description |
|---------|------|-------------|
| `"area", val` | length^2 | Cross-sectional area |
| `"I", val` | length^4 | Second moment of area (moment of inertia) |
| `"ymod", val` | stress | Elastic modulus |
| `"spacing", val` | length | Spacing (optional; if omitted, beams are continuous out-of-plane) |
| `"plasmom", val` | force-length | Plastic moment (optional; default = infinite) |
| `"ytens", val` | stress | Axial peak tensile yield strength (optional; default = infinite) |
| `"ycomp", val` | stress | Axial compressive yield strength (optional; default = infinite) |

### 10.6 Set Cable Properties

**Menu:** Structure > Add Cable > Set Cable Property

```javascript
structure("material", "cabid", id, <keywords>)
```

| Keyword | Unit | Description |
|---------|------|-------------|
| `"area", val` | length^2 | Cross-sectional area |
| `"I", val` | length^4 | Second moment of area |
| `"ymod", val` | stress | Elastic modulus |
| `"spacing", val` | length | Spacing (optional) |
| `"ytens", val` | stress | Axial peak tensile yield strength (optional) |
| `"ycomp", val` | stress | Axial compressive yield strength (optional) |
| `"kbond", val` | force/length/displacement | Grout stiffness |
| `"sbond", val` | force/length | Grout cohesive strength |
| `"fric", val` | degrees | Grout frictional resistance |
| `"perim", val` | length | Exposed perimeter of cable |

### 10.7 Set Tieback Properties

**Menu:** Structure > Add Tieback > Set Tieback Property

```javascript
structure("material", "tieid", id, <keywords>)
```

| Keyword | Unit | Description |
|---------|------|-------------|
| `"area", val` | length^2 | Cross-sectional area |
| `"I", val` | length^4 | Second moment of area |
| `"ymod", val` | stress | Elastic modulus |
| `"spacing", val` | length | Spacing (optional) |
| `"ytens", val` | stress | Axial peak tensile yield strength (optional) |
| `"ycomp", val` | stress | Axial compressive yield strength (optional) |
| `"kbond", val` | force/length/displacement | Grout stiffness |
| `"sbond", val` | force/length | Grout cohesive strength |
| `"fric", val` | degrees | Grout frictional resistance |
| `"perim", val` | length | Exposed perimeter of cable |

### 10.8 Set Strip Properties

**Menu:** Structure > Add Strip > Set Strip Property

```javascript
structure("material", "stripid", id, <keywords>)
```

| Keyword | Unit | Description |
|---------|------|-------------|
| `"calwidth", val` | length | Calculation width |
| `"numstrip", val` | - | Number of strips per calculation width |
| `"width", val` | length | Strip width |
| `"thickness", val` | length | Strip thickness |
| `"ymod", val` | stress | Elastic modulus |
| `"ytens", val` | stress | Axial peak tensile yield strength (optional) |
| `"ycomp", val` | stress | Axial compressive yield strength (optional) |
| `"kbond", val` | force/length/displacement | Grout stiffness |
| `"sbond", val` | force/length | Grout cohesive strength |
| `"fric", val` | degrees | Grout frictional resistance |

---

## 11. Solve Commands

### 11.1 Solve (Static Mode)

**Menu:** Solve > Solve

```javascript
solve(<keywords>)
```

#### Standard Solve:
| Keyword | Description |
|---------|-------------|
| (no keywords) | Solve until equilibrium or limit step |
| `"numstep", ns` | Number of calculation steps |

#### Elastic Solve:
| Keyword | Description |
|---------|-------------|
| `"elastic"` | Mechanical calculation assuming elastic behavior (prevents failure) |

#### Factor of Safety (FOS):
| Keyword | Description |
|---------|-------------|
| `"fos"` | Automatic search for factor of safety (strength reduction method) |
| `"fosLBLimit", val` | Lower bound FOS value |
| `"fosUBLimit", val` | Upper bound FOS value |
| `"fosResolution", val` | Resolution for bracketing comparison |
| `"isFosFric", str` | Include friction reduction: `"on"` or `"off"` |
| `"isFosCoh", str` | Include cohesion reduction: `"on"` or `"off"` |
| `"isFosTens", str` | Include tension reduction: `"on"` or `"off"` |

#### Relax (Excavation Simulation):
| Keyword | Description |
|---------|-------------|
| `"relax"` | Reduce boundary forces on excavation internal boundary |
| `"relaxFactor", val` | Reduction factor (default = 1.0) |
| `"relaxStep", val` | Number of force reduction steps (default = 250) |
| `"xlim", xl, xu` | X range limits |
| `"ylim", yl, yu` | Y range limits |

**Examples:**
```javascript
solve()
solve("numstep", 1000)
solve("elastic")
solve("fos")
solve("fos", "fosLBLimit", 0.6, "fosUBLimit", 15)
solve("relax", "relaxFactor", 0.4, "xlim", 0, 100, "ylim", -10, 10)
```

**Notes:**
- `"solve fos"` only applies to Mohr-Coulomb material model
- FOS calculation uses the strength reduction method
- `"solve elastic"` ignores nonlinear behavior by preventing failure
- `"solve relax"` sequence: (1) Calculate reaction forces on boundary nodes, (2) Ramp down forces, (3) Solve to equilibrium

---

## 12. Settings

### 12.1 Calculation Settings

**Menu:** Setting > Calculation Setting

```javascript
set(<keywords>)
```

| Keyword | Description |
|---------|-------------|
| `"steplimit", val` | Timestep limit (default: 100000) |
| `"equilratiolimit", val` | Equilibrium ratio limit (default: 0.001). Run terminates if ratio < val |
| `"minunballimit", val` | Minimum unbalance force limit. Run terminates if force < val |
| `"unit", str` | Unit system: `"none"`, `"stress-pa"`, `"stress-kpa"`, `"stress-mpa"`, `"stress-psf"`, `"stress-ksf"` |

**Example:**
```javascript
set("steplimit", 50000, "unit", "stress-kpa")
```

### 12.2 Mechanical Settings

**Menu:** Setting > Mechanical Setting

```javascript
set(<keywords>)
```

| Keyword | Description |
|---------|-------------|
| `"staticdampratio", val` | Mechanical damping ratio in static analysis (must be positive) |

**Example:**
```javascript
set("staticdampratio", 0.8)
```

### 12.3 Gravity Settings

**Menu:** Setting > Gravity Setting

```javascript
set(<keywords>)
```

| Keyword | Description |
|---------|-------------|
| `"gravity", gx, gy` | Gravitational acceleration (+left-to-right, +up-to-down) |

**Example:**
```javascript
set("gravity", 0, 9.81)
```

### 12.4 Water Table Settings

**Menu:** Setting > Water Table Setting

**Add water table:**
```javascript
watertable("add", "dens", dens, "elev", el)
```

**Remove water table:**
```javascript
watertable("remove")
```

| Parameter | Description |
|-----------|-------------|
| `dens` | Water density |
| `el` | Water table elevation |

**Example:**
```javascript
watertable("add", "dens", 1000, "elev", 0)
```

> **Note:** Water table can only be added after mesh generation.

---

## 13. Plot Commands

### 13.1 Contour Plot

**Menu:** Plot > Contour Plot

```javascript
plot("contour", <keywords>)
```

| Keyword | Description |
|---------|-------------|
| `"xdisp"` | X-displacement contour |
| `"ydisp"` | Y-displacement contour |
| `"totdisp"` | Total displacement contour |
| `"sxx"` | Horizontal stress contour |
| `"syy"` | Vertical stress contour |
| `"sxy"` | Shear stress contour |
| `"szz"` | Out-of-plane stress contour |
| `"esxx"` | Horizontal effective stress contour |
| `"esyy"` | Vertical effective stress contour |
| `"esxy"` | Shear effective stress contour |
| `"eszz"` | Out-of-plane effective stress contour |
| `"sig1"` | Maximum principal stress contour |
| `"sig3"` | Minimum principal stress contour |
| `"exx"` | Horizontal strain contour |
| `"eyy"` | Vertical strain contour |
| `"exy"` | Shear strain contour |
| `"ssi"` | Maximum shear strain contour |
| `"vsi"` | Maximum volumetric strain contour |
| `"pp"` | Pore pressure contour |

> **Export:** Contour data can be exported to VTK and Text files for visualization in ParaView.

### 13.2 Structure Plot

**Menu:** Plot > Structure Plot

```javascript
plot("struc", "beam", <keywords>, "id", id)
plot("struc", "cable", <keywords>, "id", id)
plot("struc", "tieback", <keywords>, "id", id)
plot("struc", "strip", <keywords>, "id", id)
```

| Keyword | Description |
|---------|-------------|
| `"id", id` | Optional: structural element ID |
| `"axialforce"` | Axial force |
| `"moment"` | Moment |
| `"shearforce"` | Shear force |
| `"shearbond"` | Shear force at cable-rock coupling springs |

### 13.3 Interface Plot

**Menu:** Plot > Interface Plot

```javascript
plot("interf", <keywords>, "id", id)
```

| Keyword | Description |
|---------|-------------|
| `"id", id` | Optional: interface ID |
| `"normalstress"` | Normal stress along interface |
| `"shearstress"` | Shear stress along interface |

### 13.4 Query Plot

**Menu:** Plot > Query Plot

```javascript
plotquery("startPoint", xs, ys, "endPoint", xe, ye, "numInterval", int, "plName", name, "plID", id)
```

| Parameter | Description |
|-----------|-------------|
| `xs, ys` | Start point of query line |
| `xe, ye` | End point of query line |
| `int` | Number of query intervals |
| `name` | Name for pairlist |
| `id` | ID for pairlist |

> **Note:** This command is active in contour plots only.

### 13.5 Chart Plot

**Menu:** Plot > Chart Plot

```javascript
plot("chart", <keywords>)
plot("edit", <keywords>)
plot("export", <keywords>)
```

| Keyword | Description |
|---------|-------------|
| `"id1", i1` | First pairlist ID |
| `"id2", i2` | Second pairlist ID |
| `"col1", c1` | Column for x-axis (0 = first, 1 = second) |
| `"col2", c2` | Column for y-axis (0 = first, 1 = second) |

**Example:**
```javascript
plot("chart", "id1", 1)    // Auto: col1=x, col2=y
```

### 13.6 Element Plot

**Menu:** Plot > Element Plot

```javascript
plot("element", <keywords>)
```

| Keyword | Description |
|---------|-------------|
| `"geom"` | Finite element mesh |
| `"state"` | Plastic state for solid elements |

---

## 14. File & Script Management

### 14.1 Load/Call Script

**Menu:** File > Load/Call Script

```javascript
script("load", "filename", fname)
script("call", "filename", fname)
```

| Parameter | Description |
|-----------|-------------|
| `fname` | Script filename (*.ajs) |

**Examples:**
```javascript
script("load", "filename", "C:/Portable/ADONIS/test.ajs")
script("call", "filename", "C:/Portable/ADONIS/test.ajs")
```

> **Note:** `load` checks syntax and displays in editor. `call` checks syntax and evaluates immediately.

### 14.2 Save Script

**Menu:** File > Save Script

```javascript
script("save", "filename", fname)
```

| Parameter | Description |
|-----------|-------------|
| `fname` | Script filename (*.ajs) |

**Example:**
```javascript
script("save", "filename", "C:/Portable/ADONIS/test.ajs")
```

### 14.3 Export Model Image

```javascript
exportmodel("image", "filename", filepath)
```

**Example:**
```javascript
exportmodel("image", "filename", "C:/Portable/ADONIS/test.png")
```

### 14.4 New Model

```javascript
newmodel()
```

Creates a new/fresh model (clears everything).

---

## Quick Reference: Common Workflow

```javascript
// 1. Create new model
newmodel()

// 2. Draw geometry
rect("startPoint", -10, -20, "endPoint", 10, 0)

// 3. Discretize boundaries
discretize("maxedge", 2)

// 4. Generate mesh
gmsh("size", "auto", "elemtype", "T3", "useNMD", "on")

// 5. Create material
material("create", "Mohr-Coulomb", "matid", 1, "matname", "Soil",
    "density", 2000, "shear", 5e6, "bulk", 1e7,
    "coh", 20000, "fric", 30, "dil", 0, "tens", 0)

// 6. Assign material
material("assign", "matid", 1)

// 7. Apply boundary conditions
applybc("xfix", "xlim", -10.5, -9.5, "ylim", -20.5, 0.5)
applybc("xfix", "xlim", 9.5, 10.5, "ylim", -20.5, 0.5)
applybc("yfix", "xlim", -10.5, 10.5, "ylim", -20.5, -19.5)

// 8. Set gravity
set("gravity", 0, 9.81)

// 9. Solve
solve()

// 10. Plot results
plot("contour", "totdisp")
```

---

*Extracted from ADONIS User Manual V3.90 English*
