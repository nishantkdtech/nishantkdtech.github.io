---
layout: post
title: "How to create a C++ Header file
---

This post is clipped [from my blog](http://blog.nishantkompella.me/2022/07/26/how-to-create-a-header-file-in-c/), which has both helpful posts and personal posts.

# How to create Header files in C++

Creating a header file is a lot easier than creating one in C. While normally (in C, that is), you would have to use GCC to link the header definitions to their corresponding source file, creating an object (.o) file, creating one in C++ is as easy as creating two files.

>Note: If you are looking for a quick explanation, feel free to jump to the summary in the end.

## Header files

Normally, header files would include class and function definitions; the corresponding implementations would go in the C++ source file. This tutorial will follow that standard; but if you wanted to, you could combine the two and put it in one header file.

Header files in C++ can be .h or .hpp files, though .hpp is normally used for C++ (while .h is reserved for C). The template for creating a C++ file is such:

```c++
#ifndef HEADER_NAME_H
#define HEADER_NAME_H

/* Function code here... */

#endif
```

### Why do we need `#ifndef`?

The C preprocessor will generate an error if we define functions twice. Sometimes, we may need to create multiple header files, each with their own uses. Some header files may include others; without `#ifndef`, we would end up with [multiple definitions of the same function, from the same file](https://stackoverflow.com/questions/39823162/error-redefinition-of-class-on-same-line).

Also, you do not need to include other header files in your header file. Do this in the C++ source file. You can, however, include the line `using namespace std`; if you are using `string.h`.

>Note: If you are using a self-contained, standalone header file (that does not depend on other header files you have written), you can use the line

  #pragma once

In place of `#ifndef/#define/#endif`. This is another preprocessor command which tells the preprocessor to read the file once.

You don’t *need* to name your `#ifndef` the same name as your header; it just needs to be unique (and should not conflict with other header files) — hence the common practice of using the header name in your `#ifndef`. Let’s put this to practice with a simple example.

File 1: `niko.h`

```c++
#ifndef NIKO_H
#define NIKO_H

class myClass {
    public:
       int add(int, int);
       void printName();
}

void printSomething();

#endif // NIKO_H
```

### What's up with the comment at the end?

You may notice the comment at the end, which stated which `ifndef` I was closing. Why? There is, after all, one header This is because, in more complicated header files, you may need to define multiple functions.

Say, for instance, I have the standard header file `<iostream>`. This header file includes tools for both the input *and* output stream. There are other header files, however, namely `<istream>` (for the input stream) and `<ostream>` (for the output stream). We do not want to overlap the files (and functions included in them). As you can probably guess, however, there are a couple of safety `#ifndef`s in case the `<istream>` and `<ostream>` have not been included. How does a reader keep track of which `#endif` closes which if? Comments to the rescue!

## C++ Source File

Now that we have gotten the abnormal part of the process out of the way, we can implement the C++ source file. The general template of the C++ source file is this:

```c++
#include "header-name.h"

// implement source code as normal
// without the function headers, of course
```

One important thing: **The C++ source file must have the same name as the header file**. So, in the case of `header-name.h`, we would name our C++ source file `header-name.cpp`.

Let’s put this to the test:

File 2: `niko.cpp`

```c++
#include "niko.hpp"

// now begins normal implementation...

#include <iostream>

using namespace std;

int myClass::add(int a, int b) {
    return a + b;
}

void myClass::printName() {
    cout << "Nishant Kompella" << endl;
}

void printSomething() {
    cout << "something";
}
```

We don’t really need to do anything past here; just add to another (`main.cpp`) file and compile that one.

File 3: `main.cpp`

```c++
#include "niko.hpp" // unless your header is system-defined, use quotes instead of <>.

using namespace std;

int main() {
    myClass c1;
    c1.printName();
    printSomething();
    return 0;
}
```

Compiling as normal and executing should print out this:

```
$ ./a.out
Nishant Kompella
something
```

## Summary

To create a C++ header file, just do this:

```c++
#ifndef HEADER_NAME_H
#define HEADER_NAME_H

// headers go here...

#endif // HEADER_NAME_H
```

To create a corresponding C++ source file, do this:

```c++
#include "header-name.h"

#include <iostream>

using namespace std;

// implement as usual
```

To use this header file, no excess compilation is necessary! Just `#include` our header into your `main.cpp` file.

```c++
#include "header-name.h"

using namespace std;

int main() {
    // yada, yada, yada
}

// etc.
```
