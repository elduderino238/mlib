M\*LIB: a Generic type-safe Container Library in C language
==========================================================

Overview
--------

M\*LIB (M star lib) is a library allowing the programmer to use **generic and
type safe container** in pure C language, aka handling generic
[containers](https://en.wikipedia.org/wiki/Container_%28abstract_data_type%29).
The objects within the containers can have proper constructor, destructor
(and other methods):
this will be handled by the library. This allows to construct fully
recursive objects (container-of[...]-container-of-type-T).

This is more or less an equivalent to the [C++](https://en.wikipedia.org/wiki/C%2B%2B) [STL](https://en.wikipedia.org/wiki/Standard_Template_Library).
This is not a strict mapping and both the STL and M\*LIB have their exclusive containers: See [here](https://github.com/P-p-H-d/mlib/wiki/STL-to-M*LIB-mapping) for details.

M\*LIB should be portable to any systems that support [ISO C99](https://en.wikipedia.org/wiki/C99)
(some optional features need [ISO C11](https://en.wikipedia.org/wiki/C11_(C_standard_revision)) support).

M\*LIB is **only** composed of a set of headers.
There is no C file. Just put the header in the search path of your compiler,
and it will work.
So there is no dependency (except some other headers of M\*LIB).

One of M\*LIB's design key is to ensure safety. This is done by multiple means:

* in debug mode, defensive programming is extensively used:
the contracts of the function are checked, ensuring
that the data are not corrupted. For example, 
[Buffer overflow](https://en.wikipedia.org/wiki/Buffer_overflow) are checked in this mode
through [bound checking](https://en.wikipedia.org/wiki/Bounds_checking)
or the intresic properties of a Red-Black tree are verified.
* very few casts are used within the library. Still the library can be used
with the greatest level of warnings by a C compiler without
any aliasing warning.
* the genericity is not done directly by macro, but indirectly by making them
define inline functions with the proper prototypes: this allows
the user calls to have proper warning checks.

Other key designs are:

* do not rewrite the C library and just wrap around it (for example don't rewrite sort but stable sort),
* do not make users pay the cost of what they don't need.


M\*LIB should still be quite-efficient: even if the implementation may not always be state
of the art, there is no overhead in using this library rather than using
direct C low-level access: the compiler is able to **fully** optimize
the library calls
since all the functions are declared as inline.
In [fact](https://github.com/P-p-H-d/mlib/wiki/performance), M\*LIB
is one of the fastest generic C library you can find.

M\*LIB uses internally the 'malloc', 'realloc' and 'free' functions to handle
the memory pool. This behavior can be overridden at different level.
M\*LIB default policy is to abort the 
program if there is a memory error. However, this behavior
can also be customized.

M\*LIB may use a lot of assertions in its implementation to ensure safety: 
it is highly recommended to properly define NDEBUG for released programs. 

M\*LIB is distributed under BSD-2 simplified license.

It is strongly advised not to read the source to know how to use the library
but rather read the examples or the tests.

Components
----------

The available containers which doesn't require the user structure to be modified are:

* [m-array.h](#m-array): header for creating array of generic type and of variable size,
* [m-list.h](#m-list): header for creating singly-linked list of generic type,
* [m-deque.h](#m-deque): header for creating double-ended queue of generic type and of variable size,
* [m-dict.h](#m-dict): header for creating generic dictionary or set of generic types,
* [m-tuple.h](#m-tuple): header for creating arbitrary tuple of generic type,
* [m-rbtree.h](#m-rbtree): header for creating binary sorted tree,
* [m-bptree.h](#m-bptree): header for creating B+TREE of generic type,
* [m-variant.h](#m-variant): header for creating arbitrary variant of generic type,
* [m-prioqueue.h](#m-prioqueue): header for creating priority queue of generic type and of variable size,

The available containers of M\*LIB for thread synchronization are:

* [m-buffer.h](#m-buffer): header for creating fixed-size queue (or stack) of generic type (multiple produce / multiple consummer),
* [m-snapshot](#m-snapshot): header for creating 'snapshot' buffer for sharing synchronously data (thread safe).
* [m-shared.h](#m-shared): header for creating shared pointer of generic type.

The following containers are intrusive (You need to modify your structure):

* [m-i-list.h](#m-i-list): header for creating dual-linked intrusive list of generic type,
* [m-i-shared.h](#m-i-shared): header for creating intrusive shared pointer of generic type (Thread Safe),


Other headers offering other functionality are:

* [m-bitset.h](#m-bitset): header for creating bit set (or "packed array of bool"),
* [m-string.h](#m-string): header for creating dynamic variable-length string,
* [m-algo.h](#m-algo): header for providing various generic algorithms to the previous containers.
* [m-mempool.h](#m-mempool): header for creating specialized & fast memory allocator.
* [m-worker.h](#m-worker): header for providing an easy pool of workers to handle work orders, used for parallelised tasks.
* [m-core.h](#m-core): header for meta-programming with the C preprocessor.

Finaly headers for compatibility with non C11 compilers:

* [m-atomic.h](#m-atomic): header for ensuring compatibility between C's stdatomic.h and C++'s atomic header. Provide also an implementation over mutex if none is available.
* [m-mutex.h](#m-mutex): header for providing a very thin layer across multiple implementation of mutex/threads (C11/PTHREAD/WIN32).

Each containers define their iterators.

All containers try to expose the same interface:
if the method name is the same, then it does the same thing
and is used in the same way.
In some rare case, the method has to be adapted to the container.

Each header can be used separately from others: dependency between headers have been kept to the minimum.

![Dependence between headers](https://raw.githubusercontent.com/P-p-H-d/mlib/master/depend.png)

Build & Installation
--------------------

M\*LIB is **only** composed of a set of headers, as such there is no build for the library.
The library doesn't depend on any other library than the libc.

To run the test suite, run:

       make check

To generate the documentation, run:

       make doc

To install the headers, run:

       make install PREFIX=/my/directory/to/install


External Reference
------------------

Many other implementation of generic container libraries in C exist.
For example:

* [BKTHOMPS/CONTAINERS](https://github.com/bkthomps/Containers)
* [BSD tree.h](http://fxr.watson.org/fxr/source/sys/tree.h)
* [CDSA](https://github.com/MichaelJWelsh/cdsa)
* [CELLO](http://libcello.org/)
* [C GENERIC CONTAINER LIBRARY](https://github.com/ta5578/C-Generic-Container-Library)
* [COLLECTIONS-C](https://github.com/srdja/Collections-C)
* [CONCURRENCY KIT](https://github.com/concurrencykit/ck)
* [CTEMPLATES](https://github.com/farhiongit/Ctemplates)
* [GDB internal library](https://sourceware.org/git/gitweb.cgi?p=binutils-gdb.git;a=blob;f=gdb/common/vec.h;h=41e41b5b22c9f5ec14711aac35ce4ae6bceab1e7;hb=HEAD)
* [GENCCONT: Generic C Containers](https://github.com/pmj/genccont)
* [Generic-Container-Lib](https://github.com/Siapran/Generic-Container-Lib)
* [GLIB](https://wiki.gnome.org/Projects/GLib)
* [KLIB](https://github.com/attractivechaos/klib)
* [LIBCHASTE](https://github.com/mgrosvenor/libchaste)
* [LIBCOLLECTION](https://bitbucket.org/manvscode/libcollections)
* [LIBCONTAINER](http://www.agottem.com/libcontainer)
* [LIBDICT](https://github.com/fmela/libdict)
* [LIBDYNAMIC](https://github.com/fredrikwidlund/libdynamic)
* [LIBLFDS](http://www.liblfds.org/)
* [LIBGENERICS](https://github.com/yudi-matsuzake/libgenerics)
* [LIBNIH](https://github.com/keybuk/libnih)
* [LIBSRT:  Safe Real-Time library for the C programming language](https://github.com/faragon/libsrt)
* [NEDTRIES](https://github.com/ned14/nedtries)
* [OKLIB](https://github.com/brackeen/ok-lib)
* [Open General C Container Collections ](https://github.com/kevin-dong-nai-jia/OpenGC3)
* [QLIBC](http://wolkykim.github.io/qlibc/)
* [RayLanguage](https://github.com/kojiba/RayLanguage)
* [Red-black tree implementation](http://www.canonware.com/rb/)
* [SGLIB](http://sglib.sourceforge.net/)
* [Smart pointer for GNUC](https://github.com/Snaipe/libcsptr)
* [STB stretchy_buffer](https://github.com/nothings/stb)
* [TommyDS](https://github.com/amadvance/tommyds)
* [UTHASH](http://troydhanson.github.io/uthash/index.html)

Each of theses can be classified into one of the following concept:

* Each object is handled through a pointer to void (with potential registered callbacks to handle the contained objects for the specialized methods). From a user point of view, this makes the code harder to use (as you don't have any help from the compiler) and type unsafe with a lot of cast (so no formal proof of the code is possible). This also generaly generate slower code (even if using link time optimization, this penality can be reduced). Properly used, it can yet be the most space efficient for the code, but can consumme a lot more for the data due to the obbligation of using pointers. This is however the easiest to design & code.
* Macros are used to access structures in a generic way (using known fields of a structure - typically size, number, etc.). From a user point of view, this can create subtil bug in the use of the library (as everything is done through macro expansion in the user defined code) or hard to understand warnings. This can generates fully efficent code. From a library developper point of view, this can be quite limitating in what you can offer.
* A known structure is put in an intrusive way in the type of all the objects you wan to handle. From a user point of view, he needs to modify its structure and has to perform all the allocation & deallocation code itself (which is good or bad depending on the context). This can generate efficient code (both in speed and size). From a library developper point of view, this is easy to design & code. You need internally a cast to go from a pointer to the known structure to the pointed object (a reverse of offsetof) which is generaly type unsafe (except if mixed with the macro generating concept). This is quite limitating in what you can do: you can't move your objects so any container which has to move some objects is out-of-question (which means that you cannot use the most efficient container).
* Header files are included multiple times with different contexts (some different values given to defined macros) in order to generate different code for each type. From a user point of view, this creates a new step before using the container: an instantiation stage which has to be done once per type and per compilation unit (The user is responsbible to create only one instance of the container, which can be troublesome if the library doesn't handle proper prefix for its naming convention). The debug of the library is generaly easy and can generate fully specialized & efficent code. Incorrectly used, this can generate a lot of code bloat. Properly used, this can even create smaller code than the void pointer variant. The interface used to configure the library can be quite tiresome in case of a lot of specialized methods to configure: it doesn't allow to chain the configuration from a container to another one easilly. It also doesn't allow heavy customisation of the code.
* Macros are used to generate context-dependent C code allowing to generate code for differenty type. This is pretty much like the headers solution but with added flexibility. From a user point of view, this creates a new step before using the container: an instantiation stage which has to be done once per type and per compilation unit (The user is responsbible to create only one instance of the container, which can be troublesome if the library doesn't handle proper prefix for its naming convention). This can generate fully specialized & efficent code. Incorrectly used, this can generate a lot of code bloat. Properly used, this can even create smaller code than the void pointer variant. From a library developper point of view, the library is harder to design and to debug: everything being expansed in one line, you can't step in the library (there is however a solution to overcome this limitation by adding another stage to the compilation process). You can however see the generated code by looking at the preprocessored file. You can perform heavy context-dependend customization of the code (transforming the macro preprocessing step into itw own language). Properly done, you can also chain the methods from a container to another one easilly, allowing expansion of the library. Errors within the macro expansion are generarly hard to decipher, but errors in code using containers are easy to read and natural.

M\*LIB's category is mainly the last one. Some macros are also defined to access structure in a generic way, but they are optional. There are also intrusive containers.
M\*LIB main added value compared to other libraries is its oplist feature
allowing it to chain the containers and/or use complex types in containers:
list of array of dictionary are perfectly supported by M\*LIB.

For the macro-preprocessing part, other libraries also exist. For example:

* [P99](http://p99.gforge.inria.fr/p99-html/)
* [C99 Lambda](https://github.com/Leushenko/C99-Lambda)
* [MAP MACRO](https://github.com/swansontec/map-macro)
* [C Preprocessor Tricks, Tips and Idioms](https://github.com/pfultz2/Cloak/wiki/C-Preprocessor-tricks,-tips,-and-idioms)
* [CPP MAGIC](http://jhnet.co.uk/articles/cpp_magic)
* [Zlang](https://github.com/pfultz2/ZLang)
* [Boost preprocessor](http://www.boost.org/doc/libs/1_63_0/libs/preprocessor/doc/index.html)

For the string library, there is for example:

* [The Better String Library](http://bstring.sourceforge.net/) (with a page which lists a lot of other string libraries).
* [VSTR](http://www.and.org/vstr/) with a [page](http://www.and.org/vstr/comparison) which lists a lot of other string libraries.
* [SDS](https://github.com/antirez/sds)

How to use
----------

To use these data structures, you include the desired header,
instantiate the definition of the structure and its associated methods by using a macro _DEF.
Then you use the defined functions. Let's see an example which is rather simple:

    #include <stdio.h>
    #include "m-list.h"
    
    LIST_DEF(list_uint, unsigned int)      /* Define struct list_uint_t and its methods */
    
    int main(void) {
       list_uint_t list ;             /* list_uint_t has been define above */
       list_uint_init(list);          /* All type needs to be initialized */
       list_uint_push_back(list, 42); /* Push 42 in the list */
       list_uint_push_back(list, 17); /* Push 17 in the list */
       list_uint_it_t it;             /* Define an iterator to scan each one */
       for(list_uint_it(it, list)     /* Start iterator on first element */
           ; !list_uint_end_p(it)     /* Until the end is not reached */
           ; list_uint_next(it)) {    /* Set the iterator to the next element*/
              printf("%d\n",          /* Get a reference to the underlying */
                *list_uint_cref(it)); /* data and print it */
       }
       list_uint_clear(list);         /* Clear all the list */
    }

[ Do not forget to add -std=c99 (or c11) to your compile command to request a C99 compatible build ]

This looks like a typical C program except the line with 'LIST\_DEF'
which doesn't have any semi-colon at the end. And in fact, except
this line, everything is typical C program and even macro free!
The only macro is in fact LIST\_DEF: this macro expands to the
good type for the list of the defined type and to all the necessary
functions needed to handle such type. It is heavily context dependent
and can generate different code depending on it.
You can use it as many times as needed to defined as many lists as you want.

You could replace LIST\_DEF by ARRAY\_DEF to change
the kind of container (an array instead of a linked list)
without changing the code below: the generated interface
of a list or of an array is very similar.
Yet the performance remains the same as hand-written code
for both the list variant and the array variant.

This is equivalent to this C++ program using the STL:

    #include <iostream>
    #include <list>
    
    typedef std::list<unsigned int> list_uint_t;
    typedef std::list<unsigned int>::iterator list_uint_it_t;
    
    int main(void) {
       list_uint_t list ;             /* list_uint_t has been define above */
       list.push_back(42);            /* Push 42 in the list */
       list.push_back(17);            /* Push 17 in the list */
       for(list_uint_it_t it = list.begin()  /* Iterator is first element*/
           ; it != list.end()         /* Until the end is not reached */
           ; ++it) {                  /* Set the iterator to the next element*/
           std::cout << *it << '\n';  /* Print the underlying data */
       }
    }

As you can see, this is rather equivalent with the following remarks:

* M\*LIB requires an explicit definition of the instance of the list,
* M\*LIB code is more verbose in the method name,
* M\*LIB needs explicit construction and destruction (as plain old C requests),
* M\*LIB doesn't return a value to the underlying data but a pointer to this value:
  - this was done for performance (it avoids copying all the data within the stack)
  - and for generality reasons (some structure may not allow copying data).

Note: list\_uint\_t is internally defined as an array of structure of size 1.
This has the following advantages:

* you effectively reserve the data whenever you declare a variable,
* you pass automatically the variable per reference for function calls,
* you can not copy the variable by an affectation (you have to use the API instead).

You can also condense the M\*LIB code by using the EACH & LET macros if you are not afraid of using syntactic macros:

    #include <stdio.h>
    #include "m-list.h"
    
    LIST_DEF(list_uint, unsigned int)   /* Define struct list_uint_t and its methods */
    
    int main(void) {
       M_LET(list, LIST_OPLIST(uint)) { /* Define & init list as list_uint_t */
         list_uint_push_back(list, 42); /* Push 42 in the list */
         list_uint_push_back(list, 17); /* Push 17 in the list */
         for M_EACH(item, list, LIST_OPLIST(uint)) {
           printf("%d\n", *item);       /* Print the item */
         }
       }                                /* Clear of list will be done now */
    }

Another example with a complete type (with proper initialization & clear function) by using the [GMP](https://gmplib.org/) library:

    #include <stdio.h>
    #include <gmp.h>
    #include "m-array.h"

    ARRAY_DEF(array_mpz, mpz_t, (INIT(mpz_init), INIT_SET(mpz_init_set), SET(mpz_set), CLEAR(mpz_clear)) )

    int main(void) {
       array_mpz_t array ;             /* array_mpz_t has been define above */
       array_mpz_init(array);          /* All type needs to be initialized */
       mpz_t z;                        /* Define a mpz_t type */
       mpz_init(z);                    /* Initialize the z variable */
       mpz_set_ui (z, 42);
       array_mpz_push_back(array, z);  /* Push 42 in the array */
       mpz_set_ui (z, 17);
       array_mpz_push_back(array, z); /* Push 17 in the array */
       array_it_mpz_t it;              /* Define an iterator to scan each one */
       for(array_mpz_it(it, array)     /* Start iterator on first element */
           ; !array_mpz_end_p(it)      /* Until the end is not reached */
           ; array_mpz_next(it)) {     /* Set the iterator to the next element*/
              gmp_printf("%Zd\n",      /* Get a reference to the underlying */
                *array_mpz_cref(it));  /* data and print it */
       }
       mpz_clear(z);                   /* Clear the z variable */
       array_mpz_clear(array);         /* Clear all the array */
    }

or the equivalent:

    #include <stdio.h>
    #include <gmp.h>
    #include "m-array.h"
    
    // Register the oplist of a mpz_t. It is a classic oplist.
    #define M_OPL_mpz_t() M_CLASSIC_OPLIST(mpz)
    // Define an instance of a array of mpz_t (both type and function)
    ARRAY_DEF(array_mpz, mpz_t)
    // Register the oplist of the created instance of array of mpz_t
    #define M_OPL_array_mpz_t() ARRAY_OPLIST(array_mpz, M_OPL_mpz_t())
    
    int main(void) {
      // Let's define 'array' as an 'array_mpz_t' & initiliaze it.
      M_LET(array, array_mpz_t)
        // Let's define 'z1' and 'z2' to be 'mpz_t' & initiliaze it
        M_LET (z1, z2, mpz_t) {
         mpz_set_ui (z1, 42);
         array_mpz_push_back(array, z1);  /* Push 42 in the array */
         mpz_set_ui (z2, 17);
         array_mpz_push_back(array, z2); /* Push 17 in the array */
         // Let's iterate over all items of the container
         for M_EACH(item, array, array_mpz_t) {
              gmp_printf("%Zd\n", *item);
         }
      } // All variables are cleared with the proper method beyond this point.
      return 0;
    }


Here we can see that we register the mpz\_t type into the container with
the minimum information of its interface needed.

Other examples are available in the example folder.

Internal fields of the structure are subject to change even for small revision
of the library.

The final goal of the library is to be able to write code like this in pure C while keeping type safety and compile time name resolution:

	let(list, list_uint_t) {
	  push(list, 42);
	  push(list, 17);
	  for each (item, list) {
	    print(item, "\n");
	  }
	}


OPLIST
------

An OPLIST is a fundamental notion of M\*LIB which hasn't be used in any other library.
In order to know how to operate on a type, M\*LIB needs additional information
as the compiler doesn't know how to handle properly any type (contrary to C++).
This is done by giving an operator list (or oplist in short) to any macro which
needs to handle the type. As such, an oplist as only meaning within a macro
expansion. Fundamentally, this is the exposed interface of a type with documentated
operators using an associative array implemented with the only C preprocessor
where the operators are the predefined keys and the methods are the values.

An oplist is an associative array of operator over methods in the following format:

(OPERATOR1(method1), OPERATOR2(method2), ...)

The function 'method1' is used to handle 'OPERATOR1'.
The function 'method2' is used to handle 'OPERATOR2'. etc.
The order of the operator in this list is the priority order:
in case the same operator appear multiple times in the list,
the first one is the prioritary.

It is used to perform the association between the operation on a type
and the function which performs this operation.
Without an oplist, M\*LIB has no way to known how to deal with your type
and will deal with it like a classic C type.

A function name can be followed by the token M\_IPTR in the oplist
(for example: (INIT(init\_func M\_IPTR)) )
to specify that the function accept as its *first* argument a pointer
to the type rather than the type itself
(aka the prototype is init\_func(type *) instead of init\_func(type)).
If you use the '[1]' trick (see below), you won't need this.

An oplist has no real form from a C language point of view. It is just an abstraction
which disappears after the macro expansion step of the preprocessing.

For each object / container, an oplist is needed and the following operators are
expected for an object otherwise default constructors are used:

* INIT constructor(obj): initialize the object 'obj' into a valid state.
* INIT\_SET constructor(obj, org): initialize the object 'obj' into the same state as the object 'org'.
* SET operator(obj, org): set the initialized object 'obj' into the same state as the initialized object org.
* CLEAR destructor(obj): destroy the initialized object 'obj', releasing any attached memory. This method shall never fail.

INIT, INIT\_SET & SET methods shall only fail due to ***memory errors***.

Not all operators are needed within an oplist to handle a container.
If some operator is missing, the associated default operator of the function is used.
To use C integer or float types, the default constructors are perfectly fine:
you may use M\_DEFAULT\_OPLIST to define the operator list of such types or you
can just omit it.

NOTE: An iterator doesn't have a constructor nor destructor methods
and it shall not allocate any memory.

Other documented operators are:

* NEW (type) -> type pointer: allocate a new object with suitable alignment and size. The returned object is **not initialized**. INIT operator shall be called afterwise. The default is M\_MEMORY\_ALLOC.
* DEL (&obj): free the allocated uninitialized object 'obj' (default is M\_MEMORY\_DEL). The object is not cleared before being free (CLEAR operator has to be called before). The object shall have been allocated by the NEW operator.
* REALLOC(type, type pointer, number) --> type pointer: realloc the given type pointer (either NULL or a pointer returned by the REALLOC operator itself) to an array of number objects of this type. Previously objects pointed by the pointer are kept up to the minimum of the new or old array size. New objects are not initialized (INIT operator has to be called afterwise). Freed objects are not cleared (CLEAR operator has to be called before). The default is M\_MEMORY\_REALLOC.
* FREE (&obj) : free the allocated uninitialized array object 'obj' (default is M\_MEMORY\_FREE). The objects are not cleared before being free (CLEAR operator has to be called before).  The object shall have been allocated by the REALLOC operator.
* INIT\_MOVE(objd, objc): Initialize 'objd' to the same state than 'objc' by stealing as resources as possible from 'objc', and then clear 'objc'. It is semantically equivalent to calling INIT\_SET(objd,objc) then CLEAR(objc) (but usually way faster). By default, all objects are assumed to be **trivially movable** (i.e. using memcpy to move an object is safe). Most C objects are trivially movable. If an object is not trivially movable, it shall provide an INIT\_MOVE method or disable the INIT\_MOVE method enterely.
* MOVE(objd, objc): Set 'objd' to the same state than 'objc' by stealing as resources as possible from 'objc' and then clear 'objc'. It is equivalent to calling SET(objd,objc) then CLEAR(objc) or CLEAR(objd) and then INIT\_MOVE(objd, objc). TBC if operator is really needed as calling CLEAR then INIT\_MOVE is what does all known implementation.
* SWAP(objd, objc): Swap the object 'objc' and the object 'objd' states.
* CLEAN(obj): Empty the container from all its objects. Nearly like CLEAR except that the container 'obj' remains initialized (but empty).
* HASH (obj) --> size_t: return a hash of the object (usable for a hash table). Default is performing a hash of the memory representation of the object (which may be invalid is the object holds pointer to other objects).
* EQUAL(obj1, obj2) --> bool: return true if both objects are equal, false otherwise. Default is using the C comparison operator.
* CMP(obj1, obj2) --> int: return a negative integer if obj1 < obj2, 0 if obj1 = obj2, a positive integer otherwise. Default is C comparison operators.
* ADD(obj1, obj2, obj3) : set obj1 to the sum of obj2 and obj3. Default is '+' C operator.
* SUB(obj1, obj2, obj3) : set obj1 to the difference of obj2 and obj3. Default is '-' C operator.
* MUL(obj1, obj2, obj3) : set obj1 to the product of obj2 and obj3. Default is '*' C operator.
* DIV(obj1, obj2, obj3) : set obj1 to the division of obj2 and obj3. Default is '/' C operator.
* PUSH(container, obj) : push 'object' into 'container'. How it is pushed is container dependent.
* POP(&obj, container) : pop an object from 'container' and save it in '*obj' if obj is not NULL. Which object is popped is container dependent. Undefined behavior is there is no object in the container.
* PUSH_MOVE(container, &obj) : push and move the object '*obj' into 'container'. How it is pushed is container dependent but '*obj' is cleared afterwise.
* POP_MOVE(&obj, container) : pop an object from 'container' and init & move it in '*obj'. Which object is popped is container dependent. '*obj' shall be uninitialized. Undefined behavior is there is no object in the container.
* SPLICE\_BACK(containerDst, containerSrc, it): move the object referenced by the iterator 'it' from 'containerSrc' into 'containerDst'. Where is move is container dependent. Afterwise 'it' points to the next item in 'containerSrc'.
* SPLICE\_AT(containerDst, itDst, containerSrc, itSrc): move the object referenced by the iterator 'itSrc' from 'containerSrc' just after the object referenced by the iterator 'itDst' in 'containerDst'. If 'itDst' doesn't reference a valid object, it is inserted as the first item of the container (See method 'INSERT'). Afterwise 'itSrc' references the next item in 'containerSrc'and 'itDst' references the moved item in 'containerDst'.
* TYPE() --> type: return the type associated to this oplist.
* SUBTYPE() --> type: return the type of the element stored in the container.
* OPLIST() --> oplist: return the oplist of the type of the elements stored in the container.
* IT\_TYPE() --> type: return the type of an iterator object of this container.
* IT\_FIRST(it\_obj, container): set the object iterator it\_obj to the first sub-element of container.
* IT\_LAST(it\_obj, container): set the object iterator it\_obj to the last sub-element of container.
* IT\_END(it\_obj, container): set the object iterator it\_obj to the end of the container (Can't use PREVIOUS or NEXT afterwise).
* IT\_SET(it\_obj, it\_obj2): set the object iterator it\_obj to it\_obj2.
* IT\_END\_P(it\_obj)--> bool: return true if it\_obj references an end of the container.
* IT\_LAST\_P(it\_obj)--> bool: return true if the iterator it\_obj has reached an end or if the iterator points to the last element (just before the end).
* IT\_EQUAL\_P(it\_obj, it\_obj2) --> bool: return true if both iterators references the same element.
* IT\_NEXT(it\_obj): move the iterator to the next sub-component.
* IT\_PREVIOUS(it\_obj): move the iterator to the previous sub-component.
* IT\_CREF(it\_obj) --> &obj: return a constant pointer to the object referenced by the iterator.
* IT\_REF(it\_obj) --> &obj: return a pointer to the object referenced by the iterator.
* IT\_REMOVE(container, it\_obj): remove it\_obj from the container (clearing it) and update it\_obj to point to the next object. All other iterators of the same container become invalidated.
* OUT\_STR(FILE* f, obj): Output 'obj' as a string into the FILE stream 'f'.
* IN\_STR(obj, FILE* f) --> bool: Set 'obj' to the string object in the FILE stream 'f'. Return true in case of success (in which case the stream 'f' has been advanced to the end of the parsing of the object), false otherwise (in which case, the stream 'f' is in an indetermined position).
* GET\_STR(string_t str, obj, bool append): Set 'str' to a string representation of the object 'obj'. Append to the string if 'append' is true, set it otherwise.
* PARSE\_STR(obj, const char *str, const char **endp) --> bool: Set 'obj' to the string object in the char stream 'str'. Return true in case of success (in which case if endp is not NULL, it points to the end of the parsing of the object), false otherwise (in which case, if endp is not NULL, it points to an indetermined position).
* UPDATE(dest, src): Update 'dest' with 'src'. What it does exactly is node dependent: it can either SET or ADD to the node the new 'src' (default is SET).
* OOR\_SET(obj, int\_value): some containers may want to store some information within some uninitialized objects (for example Open Addressing Hash Table). This method will store the integer value 'int\_value' into the uninitialized object 'obj'. The way to store this information is object dependent. In general, you use out-of-range value for detecting such values. The object remains unitialized but set to of out-of-range value. int\_value values can be 0 or 1.
* OOR\_EQUAL(obj, int\_value): This method will compare the object 'obj' to the out-of-range value used to represent int\_value and return true if both objects are equal.


More operators are expected.

Example:

        (INIT(mpz_init),SET(mpz_set),INIT_SET(mpz_init_set),CLEAR(mpz_clear))

If there is two operations with the same name in an oplist, the left one has the priority. This allows partial overriding.

Some pre-defined oplist exist:

* M_DEFAULT_OPLIST: Oplist for a C type (integer or float),
* M_POD_OPLIST: Oplist for a plain structure (not an array type),
* M_A1_OPLIST: Oplist for a  structure defined as an array of size 1,
* M_CLASSIC_OPLIST(name): Oplist for a type which provides standard functions: name##\_init, name##\_init\_set, wname##\_set, name##\_clear.
* M_CSTR_OPLIST: Oplist for a string represented by a const char pointer.

Memory Allocation
-----------------

Memory Allocation functions can be set by overriding the following macros before using the _DEF macros:

* M\_MEMORY\_ALLOC (type): return a pointer to a new object of type 'type'.
* M\_MEMORY\_DEL (ptr): free the single object pointed by 'ptr'.
* M\_MEMORY\_REALLOC (type, ptr, number): return a pointer to an array of 'number' objects of type 'type', reusing the old array pointed by 'ptr'. 'ptr' can be NULL, in which case the array will be created.
* M\_MEMORY\_FREE (ptr): free the array of objects pointed by 'ptr'.

ALLOC & DEL operators are supposed to allocate fixed size single element object (no array).
Theses objects are not expected to grow. REALLOC & FREE operators deal with allocated memory for growing objects.
Do not mix pointers between both: a pointer allocated by ALLOC (resp. REALLOC) is supposed to be only freed by DEL (resp. FREE). There are separated 'free' operators to allow specialization in the allocator (a good allocator can take this hint into account).

M\_MEMORY\_ALLOC and  M\_MEMORY\_REALLOC are supposed to return NULL in case of memory allocation failure.
The defaults are 'malloc', 'free', 'realloc' and 'free'.

You can also overide the methods NEW, DEL, REALLOC & DEL in the oplist given to a container.


Out-of-memory error
-------------------

When a memory exhaustion is reached, the global macro "M\_MEMORY\_FULL" is called
and the function returns immediately after.
The object remains in a valid (if previously valid) and unchanged state in this case.
By default, the macro prints an error message and abort the program:
handling non-trivial memory errors can be hard,
testing them is even harder but still mandatory to avoid security holes.
So the default behavior is rather conservative.

Moreover a good program design should handle a process entire failure (using for examples multiple
processes for doing the job) so even a process stops can be recovered.
See [here](http://joeduffyblog.com/2016/02/07/the-error-model/) for more
information about why abandonment is good software practice.

It can however be overloaded to handle other policy for error handling like:

* throwing an error (in which case, the user is responsible to free memory of the allocated objects - destructor can still be called),
* set a global error and handle it when the function returns,
* other policy.

This function takes the size in bytes of the memory which has been tried to be allocated.

If needed, this macro shall be defined ***prior*** to instantiate the structure.

ERRORS & COMPILERS
------------------

When defining the oplist of a type using M\*LIB, sometimes (often) the list of errors/warnings generated
by the compiler can be (very) huge (specially with latest compilers),
even if the error itself is minor. This is more or less the same as the use of template in C++.
You should focus mainly on the first reported error/warning
even if the link between what the compiler report and what the error is
is not immediate. The error is always in the **oplist definition**.
Examples of typical errors:

* lack of inclusion of an header,
* overriding locally operator names by macros (like NEW, DEL, INIT, OPLIST, ...),
* lack of ( ) or double level of ( ) around the oplist,
* an unknown variable (example using DEFAULT\_OPLIST instead of M\_DEFAULT\_OPLIST),
* a missing argument,
* a missing mandatory operator in the oplist.

Another way to debug is to generate the preprocessed file
(by usually calling the compiler with the '-E' option instead of '-c')
and check what's wrong in the preprocessed file:

          cc -std=c99 -E test-file.c > test-file.i
          perl -pi -e 's/;/;\n/g' test-file.i
          cc -std=c99 -c -Wall test-file.i

If there is a warning reported by the compiler in the generated code,
then there is definitely an **error** you should fix (except if it reports
shadowed variables).


Benchmarks
----------

All the benchs are available in the bench directory.
The results are available [in a separate page](https://github.com/P-p-H-d/mlib/wiki/performance).


API Documentation
-----------------

### M-LIST

This header is for creating [singly linked list](https://en.wikipedia.org/wiki/Linked_list).

A linked list is a linear collection of elements, in which each element points to the next, all representing a sequence.

#### LIST\_DEF(name, type, [, oplist])

Define the singly linked list list 'name##\_t' which contains the objects of type 'type' and its associated methods as "static inline" functions.
'name' shall be a C identifier which will be used to identify the list. It will be used to create all the types and functions to handle the container.
It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

A fundamental property of a list is that the objects created within the list
will remain at their initialized address, and won't moved due to
a new element being pushed/popped in the list.

The object 'oplist' is expected to have at least the following operators (INIT, INIT_SET, SET and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

For this structure, the back is always the first element, and the front is the last element: the list grows from the back.

Example:

	LIST_DEF(list_mpz, mpz_t,                                               \
		(INIT(mpz_init), INIT_SET(mpz_init_set), SET(mpz_set), CLEAR(mpz_clear)))

	list_mpz_t my_list;

	void f(mpz_t z) {
		list_mpz_push_back (my_list, z);
	}

If the given oplist contain the method MEMPOOL, then LIST\_DEF macro will create a dedicated mempool
which is named with the given value of the method MEMPOOL, optimized for this kind of list:

* it creates a mempool named by the concatenation of "name" and "\_mempool",
* it creates a variable named by the value of the method MEMPOOL with linkage defined
by the value of the method MEMPOOL\_LINKAGE (can be extern, static or none),
this variable will be shared by all lists of the same type.
* it overwrites memory allocation of the created list to use this mempool with this variable.

Using mempool allows to create heavily efficient list but it will be only worth the effort in some
heavy performance context. The created mempool has to be initialized before using any
methods of the created list by calling  mempool\_list\_name\_init(variable)
and cleared by calling mempool\_list\_name\_clear(variable).

Example:

        LIST_DEF(list_uint, unsigned int, (MEMPOOL(list_mpool), MEMPOOL_LINKAGE(static)))

        static void test_list (size_t n)
        {
          list_uint_mempool_init(list_mpool);
          M_LET(a1, LIST_OPLIST(uint)) {
              for(size_t i = 0; i < n; i++)
                  list_uint_push_back(a1, rand_get() );
          }
          list_uint_mempool_clear(list_mpool);
        }


#### LIST\_OPLIST(name [, oplist])

Return the oplist of the list defined by calling LIST\_DEF
& LIST\_DUAL\_PUSH\_DEF with name & oplist. 

#### Created methods

In the following methods, name stands for the name given to the macro which is used to identify the type.
The following types are automatically defined by the previous macro:

#### name\_t

Type of the list of 'type'.

#### name\_it\_t

Type of an iterator over this list.

The following methods are automatically and properly created by the previous macro.

##### void name\_init(name\_t list)

Initialize the list 'list' (aka constructor) to an empty list.

##### void name\_init\_set(name\_t list, const name\_t ref)

Initialize the list 'list' (aka constructor) and set it to the value of 'ref'.

##### void name\_set(name\_t list, const name\_t ref)

Set the list 'list' to the value of 'ref'.

##### void name\_init\_move(name\_t list, name\_t ref)

Initialize the list 'list' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared and can no longer be used.

##### void name\_move(name\_t list, name\_t ref)

Set the list 'list' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared and can no longer be used.

##### void name\_clear(name\_t list)

Clear the list 'list (aka destructor). The list can't be used anymore, except with a constructor.

##### void name\_clean(name\_t list)

Clean the list (the list becomes empty). The list remains initialized but is empty.

##### const type *name\_back(const name\_t list)

Return a constant pointer to the data stored in the back of the list.

##### void name\_push\_back(name\_t list, type value)

Push a new element within the list 'list' with the value 'value' contained within.

##### type *name\_push\_raw(name\_t list)

Push a new element within the list 'list' without initializing it and returns a pointer to the **non-initialized** data.
The first thing to do after calling this function is to initialize the data using the proper constructor. This allows to use a more specialized
constructor than the generic one.
Return a pointer to the **non-initialized** data.

##### type *name\_push\_new(name\_t list)

Push a new element within the list 'list' and initialize it with the default constructor of the type.
Return a pointer to the initialized object.

##### void name\_pop\_back(type *data, name\_t list)

Pop a element from the list 'list' and set *data to this value.

##### bool name\_empty\_p(const name\_t list)

Return true if the list is empty, false otherwise.

##### void name\_swap(name\_t list1, name\_t list2)

Swap the list 'list1' and 'list2'.

##### void name\_it(name\_it\_t it, name\_t list)

Set the iterator 'it' to the back(=first) element of 'list'.
There is no destructor associated to this initialization.

##### void name\_it\_set(name\_it\_t it, const name\_it\_t ref)

Set the iterator 'it' to the iterator 'ref'.
There is no destructor associated to this initialization.

##### bool name\_end\_p(const name\_it\_t it)

Return true if the iterator doesn't reference a valid element anymore.

##### bool name\_last\_p(const name\_it\_t it)

Return true if the iterator references the top(=last) element or if the iterator doesn't reference a valid element anymore.

##### bool name\_it\_equal\_p(const name\_it\_t it1, const name\_it\_t it2)

Return true if the iterator it1 references the same element than it2.

##### void name\_next(name\_it\_t it)

Move the iterator 'it' to the next element of the list, ie. from the back (=first) element to the front(=last) element.

##### type *name\_ref(name\_it\_t it)

Return a pointer to the element pointed by the iterator.
This pointer remains valid until the list is modified by another method.

##### const type *name\_cref(const name\_it\_t it)

Return a constant pointer to the element pointed by the iterator.
This pointer remains valid until the list is modified by another method.

##### type *name\_get(const name\_t list, size\_t i)

Return a pointer to the element i-th of the list (from 0). 
It is assumed than i is within the size of the list.

##### const type *name\_cget(const name\_t list, size\_t i)

Return a constant pointer to the element i-th of the list (from 0). 
It is assumed than i is within the size of the list.

##### size\_t name\_size(const name\_t list)

Return the number elements of the list (aka size). Return 0 if there no element.

##### void name\_insert(name\_t list, const name\_it\_t it, const type x)

Insert 'x' after the position pointed by 'it' (which is an iterator of the list 'list') or if 'it' doesn't point anymore to a valid element of the list, it is added as the back (=first) element of the 'list'

##### void name\_remove(name\_t list, name\_it\_t it)

Remove the element 'it' from the list 'list'.
After wise, 'it' points to the next element of the list.

##### void name\_splice\_back(name\_t list1, name\_t list2, name\_it\_t it)

Move the element pointed by 'it' (which is an iterator of 'list2') from the list 'list2' to the back position of 'list1'.
After wise, 'it' points to the next element of 'list2'.

##### void name\_splice\_at(name\_t list1, name\_it\_t it1, name\_t list2, name\_it\_t it2)

Move the element pointed by 'it2' (which is an iterator of 'list2') from the list 'list2' to the position just after 'it1' in the list 'list1'.
After wise, 'it2' points to the next element of 'list2'
and 'it1' points to the inserted element in 'list1'.
If 'it1' is the end position, it inserts it at the first element (just like \_insert\_at).

##### void name\_splice(name\_t list1, name\_t list2)

Move all the element of 'list2' into 'list1", moving the last element
of 'list2' after the first element of 'list1'.
After-wise, 'list2' is emptied.

##### void name\_reverse(name\_t list)

Reverse the order of the list.

##### void name\_get\_str(string\_t str, const name\_t list, bool append)

Generate a string representation of the list 'list' and set 'str' to this representation
(if 'append' is false) or append 'str' with this representation (if 'append' is true).
This method is only defined if the type of the element defines a GET\_STR method itself.

##### void name\_out\_str(FILE *file, const name\_t list)

Generate a string representation of the list 'list' and outputs it into the FILE 'file'.
This method is only defined if the type of the element defines a OUT\_STR method itself.

##### void name\_in\_str(FILE *file, const name\_t list)

Read from the file 'file' a string representation of a list and set 'list' to this representation.
This method is only defined if the type of the element defines a IN\_STR method itself.

##### bool name\_equal\_p(const name\_t list1, const name\_t list2)

Return true if both lists 'list1' and 'list2' are equal.
This method is only defined if the type of the element defines a EQUAL method itself.

##### size\_t name\_hash(const name\_t list)

Return the has value of 'list'.
This method is only defined if the type of the element defines a HASH method itself.

#### LIST\_DUAL\_PUSH\_DEF(name, type[, oplist])

Define the singly linked list list 'name##\_t' which contains the objects 
of type 'type' and  its associated methods as "static inline" functions.
'name' shall be a C identifier which will be used to identify the list. 
It will be used to create all the types and functions to handle the container.
It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions too.

The only difference with the list defined by LIST\_DEF is
the support of the method PUSH\_FRONT in addition to PUSH\_BACK (through DUAL PUSH).
There is still only POP method (POP\_BACK). The head of the list is a bit
bigger to be able to handle such methods to work.
This allows this list to be able to represent both stack (PUSH\_BACK + POP\_BACK)
and queue (PUSH\_FRONT + POP\_BACK).

A fundamental property of a list is that the objects created within the list
will remain at their initialized address, and won't moved due to
a new element being pushed/popped in the list.

The object 'oplist' is expected to have at least the following operators (INIT, INIT_SET, SET and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

For this structure, the back is always the first element, and the front is the last element.

Example:

	LIST_DUAL\_PUSH\_DEF(list_mpz, mpz_t,                                   \
		(INIT(mpz_init), INIT_SET(mpz_init_set), SET(mpz_set), CLEAR(mpz_clear)))

	list_mpz_t my_list;

	void f(mpz_t z) {
		list_mpz_push_front (my_list, z);
		list_mpz_pop_back (z, my_list);
	}

If the given oplist contain the method MEMPOOL, then macro will create a dedicated mempool
which is named with the given value of the method MEMPOOL, optimized for this kind of list:

* it creates a mempool named by the concatenation of "name" and "\_mempool",
* it creates a variable named by the value of the method MEMPOOL with linkage defined
by the value of the method MEMPOOL\_LINKAGE (can be extern, static or none),
this variable will be shared by all lists of the same type.
* it overwrites memory allocation of the created list to use this mempool with this variable.

Using mempool allows to create heavily efficient list but it will be only worth the effort in some
heavy performance context. The created mempool has to be initialized before using any
methods of the created list by calling  mempool\_list\_name\_init(variable)
and cleared by calling mempool\_list\_name\_clear(variable).

The methods follow closely the methods defined by LIST\_DEF.

#### Created methods

In the following methods, name stands for the name given to the macro which is used to identify the type.
The following types are automatically defined by the previous macro:

#### name\_t

Type of the list of 'type'.

#### name\_it\_t

Type of an iterator over this list.

The following methods are automatically and properly created by the previous macro.

##### void name\_init(name\_t list)

Initialize the list 'list' (aka constructor) to an empty list.

##### void name\_init\_set(name\_t list, const name\_t ref)

Initialize the list 'list' (aka constructor) and set it to the value of 'ref'.

##### void name\_set(name\_t list, const name\_t ref)

Set the list 'list' to the value of 'ref'.

##### void name\_init\_move(name\_t list, name\_t ref)

Initialize the list 'list' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared and can no longer be used.

##### void name\_move(name\_t list, name\_t ref)

Set the list 'list' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared and can no longer be used.

##### void name\_clear(name\_t list)

Clear the list 'list (aka destructor). The list can't be used anymore, except with a constructor.

##### void name\_clean(name\_t list)

Clean the list (the list becomes empty). The list remains initialized but is empty.

##### const type *name\_back(const name\_t list)

Return a constant pointer to the data stored in the back of the list.

##### void name\_push\_back(name\_t list, type value)

Push a new element within the list 'list' with the value 'value' contained within
into the back of the list.

##### type *name\_push\_back\_raw(name\_t list)

Push a new element within the list 'list' without initializing it 
into the back of the list
and returns a pointer to the **non-initialized** data.
The first thing to do after calling this function is to initialize the data using
the proper constructor. This allows to use a more specialized
constructor than the generic one.
Return a pointer to the **non-initialized** data.

##### type *name\_push\_back\_new(name\_t list)

Push a new element within the list 'list' 
into the back of the list
and initialize it with the default constructor of the type.
Return a pointer to the initialized object.

##### const type *name\_front(const name\_t list)

Return a constant pointer to the data stored in the front of the list.

##### void name\_push\_front(name\_t list, type value)

Push a new element within the list 'list' with the value 'value' contained within
into the front of the list.

##### type *name\_push\_front\_raw(name\_t list)

Push a new element within the list 'list' without initializing it 
into the front of the list
and returns a pointer to the **non-initialized** data.
The first thing to do after calling this function is to initialize the data using
the proper constructor. This allows to use a more specialized
constructor than the generic one.
Return a pointer to the **non-initialized** data.

##### type *name\_push\_front\_new(name\_t list)

Push a new element within the list 'list' 
into the front of the list
and initialize it with the default constructor of the type.
Return a pointer to the initialized object.

##### void name\_pop\_back(type *data, name\_t list)

Pop a element from the list 'list' and set *data to this value.

##### bool name\_empty\_p(const name\_t list)

Return true if the list is empty, false otherwise.

##### void name\_swap(name\_t list1, name\_t list2)

Swap the list 'list1' and 'list2'.

##### void name\_it(name\_it\_t it, name\_t list)

Set the iterator 'it' to the back(=first) element of 'list'.
There is no destructor associated to this initialization.

##### void name\_it\_set(name\_it\_t it, const name\_it\_t ref)

Set the iterator 'it' to the iterator 'ref'.
There is no destructor associated to this initialization.

##### bool name\_end\_p(const name\_it\_t it)

Return true if the iterator doesn't reference a valid element anymore.

##### bool name\_last\_p(const name\_it\_t it)

Return true if the iterator references the top(=last) element or if the iterator doesn't reference a valid element anymore.

##### bool name\_it\_equal\_p(const name\_it\_t it1, const name\_it\_t it2)

Return true if the iterator it1 references the same element than it2.

##### void name\_next(name\_it\_t it)

Move the iterator 'it' to the next element of the list, ie. from the back (=first) element to the front(=last) element.

##### type *name\_ref(name\_it\_t it)

Return a pointer to the element pointed by the iterator.
This pointer remains valid until the list is modified by another method.

##### const type *name\_cref(const name\_it\_t it)

Return a constant pointer to the element pointed by the iterator.
This pointer remains valid until the list is modified by another method.

##### size\_t name\_size(const name\_t list)

Return the number elements of the list (aka size). Return 0 if there no element.

##### void name\_insert(name\_t list, const name\_it\_t it, const type x)

Insert 'x' after the position pointed by 'it' (which is an iterator of the list 'list') or if 'it' doesn't point anymore to a valid element of the list, it is added as the back (=first) element of the 'list'

##### void name\_remove(name\_t list, name\_it\_t it)

Remove the element 'it' from the list 'list'.
After wise, 'it' points to the next element of the list.

##### void name\_splice\_back(name\_t list1, name\_t list2, name\_it\_t it)

Move the element pointed by 'it' (which is an iterator of 'list2') from the list 'list2' to the back position of 'list1'.
After wise, 'it' points to the next element of 'list2'.

##### void name\_splice(name\_t list1, name\_t list2)

Move all the element of 'list2' into 'list1", moving the last element
of 'list2' after the first element of 'list1'.
After-wise, 'list2' is emptied.

##### void name\_reverse(name\_t list)

Reverse the order of the list.

##### void name\_get\_str(string\_t str, const name\_t list, bool append)

Generate a string representation of the list 'list' and set 'str' to this representation
(if 'append' is false) or append 'str' with this representation (if 'append' is true).
This method is only defined if the type of the element defines a GET\_STR method itself.

##### void name\_out\_str(FILE *file, const name\_t list)

Generate a string representation of the list 'list' and outputs it into the FILE 'file'.
This method is only defined if the type of the element defines a OUT\_STR method itself.

##### void name\_in\_str(FILE *file, const name\_t list)

Read from the file 'file' a string representation of a list and set 'list' to this representation.
This method is only defined if the type of the element defines a IN\_STR method itself.

##### bool name\_equal\_p(const name\_t list1, const name\_t list2)

Return true if both lists 'list1' and 'list2' are equal.
This method is only defined if the type of the element defines a EQUAL method itself.

##### size\_t name\_hash(const name\_t list)

Return the has value of 'list'.
This method is only defined if the type of the element defines a HASH method itself.



### M-ARRAY

An [array](https://en.wikipedia.org/wiki/Array_data_structure) is a growable collection of element which are individually indexable.

#### ARRAY\_DEF(name, type [, oplist])

Define the array 'name##\_t' which contains the objects of type 'type' and its associated methods as "static inline" functions.
Compared to C arrays, the created methods handle automatically the size (aka growable array).
'name' shall be a C identifier which will be used to identify the container.

It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

The object oplist is expected to have at least the following operators (INIT, INIT_SET, SET and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

Example:

	ARRAY_DEF(array_mpfr_t, mpfr,                                                                  \
	   (INIT(mpfr_init), INIT_SET(mpfr_init_set), SET(mpfr_set), CLEAR(mpfr_clear)))

	array_mpfr_t my_array;

	void f(mpfr_t z) {
		array_mpfr_push_back (my_array, z);
	}


#### ARRAY\_OPLIST(name [, oplist])

Return the oplist of the array defined by calling ARRAY\_DEF with name & oplist. 


#### Created methods

In the following methods, name stands for the name given to the macro which is used to identify the type.
The following types are automatically defined by the previous macro:

#### name\_t

Type of the array of 'type'.

#### name\_it\_t

Type of an iterator over this array.

The following methods are automatically and properly created by the previous macros:

##### void name\_init(name\_t array)

Initialize the array 'array' (aka constructor) to an empty array.

##### void name\_init\_set(name\_t array, const name\_t ref)

Initialize the array 'array' (aka constructor) and set it to the value of 'ref'.

##### void name\_set(name\_t array, const name\_t ref)

Set the array 'array' to the value of 'ref'.

##### void name\_init\_move(name\_t array, name\_t ref)

Initialize the array 'array' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared.

##### void name\_move(name\_t array, name\_t ref)

Set the array 'array' by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared.

##### void name\_clear(name\_t array)

Clear the array 'array (aka destructor).

##### void name\_clean(name\_t array)

Clean the array (the array becomes empty but remains initialized).

##### void name\_push\_back(name\_t array, const type value)

Push a new element into the back of the array 'array' with the value 'value' contained within.

##### type *name\_push\_new(name\_t array)

Push a new element into the back of the array 'array' and initialize it with the default constructor.
Return a pointer to this element.

##### type *name\_push\_raw(name\_t array)

Push a new element within the array 'array' without initializing it and return
a pointer to the non-initialized data.
The first thing to do after calling this function is to initialize the data
using the proper constructor. This allows to use a more specialized
constructor than the generic one.

##### void name\_push\_at(name\_t array, size\_t key, const type x)

Push a new element into the position 'key' of the array 'array' with the value 'value' contained within.
'key' shall be a valid position of the array: from 0 to the size of array (included).

##### void name\_pop\_back(type *data, name\_t array)

Pop a element from the back of the array 'array' and set *data to this value.

##### const type *name\_back(const name\_t array)

Return a constant pointer to the last element of the array.

##### bool name\_empty\_p(const name\_t array)

Return true if the array is empty, false otherwise.

##### size\_t name\_size(const name\_t array)

Return the size of the array.

##### size\_t name\_capacity(const name\_t array)

Return the capacity of the array.

##### void name\_swap(name\_t array1, name\_t array2)

Swap the array 'array1' and 'array2'.

##### void name\_set\_at(name\_t array, size\_t i, type value)

Set the element 'i' of array 'array' to 'value'.
'i' shall be within the size of the array.

##### void name\_set\_at2(name\_t array, size\_t i, type value)

Set the element 'i' of array 'array' to 'value', increasing the size
of the array if needed.

##### void name\_resize(name\_t array, size\_t size)

Resize the array 'array' to the size 'size' (initializing or clearing elements).

##### void name\_reserve(name\_t array, size\_t capacity)

Extend or reduce the capacity of the 'array' to a rounded value based on 'capacity'.
If the given capacity is below the current size of the array, the capacity is set to the size of the array.

##### void name\_pop\_at(type *dest, name\_t array, size\_t key)

Set *dest to the value the element 'i' if dest is not NULL,
Remove this element from the array.
'key' shall be within the size of the array.

##### void name\_remove(name\_t array, name\_it\_t it)

Remove the element pointed by the iterator 'it' from the array 'array'.
'it' shall be within the array. Afterwise 'it' points to the next element, or points to the end.

##### void name\_remove\_v(name\_t array, size\_t i, size\_t j)

Remove the element 'i' (included) to the element 'j' (excluded)
from the array.
'i' and 'j' shall be within the size of the array, and i < j.

##### void name\_insert(name\_t array, size\_t i, const type x)

Insert the object 'x' at the position 'key' of the array.
'key' shall be within the size of the array.

##### void name\_insert\_v(name\_t array, size\_t i, size\_t j)

Insert from the element 'i' (included) to the element 'j' (excluded)
new empty elements to the array.
'i' and 'j' shall be within the size of the array, and i < j.

##### type *name\_get(name\_t array, size\_t i)

Return a pointer to the element 'i' of the array.
'i' shall be within the size of the array.

##### const type *name\_cget(const name\_t it, size\_t i)

Return a constant pointer to the element 'i' of the array.
'i' shall be within the size of the array.

##### void name\_it(name\_it\_t it, name\_t array)

Set the iterator 'it' to the first element of 'array'.

##### void name\_it\_set(name\_it\_t it1, name\_it\_t it2)

Set the iterator 'it1' to 'it2'.

##### bool name\_end\_p(name\_it\_t it)

Return true if the iterator doesn't reference a valid element anymore.

##### bool name\_last\_p(name\_it\_t it)

Return true if the iterator references the last element of the array, or doesn't reference a valid element.

##### bool name\_it\_equal\_p(const name\_it\_t it1, const name\_it\_t it2)

Return true if both iterators point to the same element.

##### void name\_next(name\_it\_t it)

Move the iterator 'it' to the next element of the array.

##### void name\_previous(name\_it\_t it)

Move the iterator 'it' to the previous element of the array.

##### type *name\_ref(name\_it\_t it)

Return a pointer to the element pointed by the iterator.
This pointer remains valid until the array is modified by another method.

##### const type *name\_cref(const name\_it\_t it)

Return a constant pointer to the element pointed by the iterator.
This pointer remains valid until the array is modified by another method.

##### void name\_get\_str(string\_t str, const name\_t array, bool append)

Generate a string representation of the array 'array' and set 'str' to this representation
(if 'append' is false) or append 'str' with this representation (if 'append' is true).
This method is only defined if the type of the element defines a GET\_STR method itself.

##### void name\_out\_str(FILE *file, const name\_t array)

Generate a string representation of the array 'array' and outputs it into the FILE 'file'.
This method is only defined if the type of the element defines a OUT\_STR method itself.

##### void name\_in\_str(FILE *file, const name\_t array)

Read from the file 'file' a string representation of a array and set 'array' to this representation.
This method is only defined if the type of the element defines a IN\_STR method itself.

##### bool name\_equal\_p(const name\_t array1, const name\_t array2)

Return true if both arrays 'array1' and 'array2' are equal.
This method is only defined if the type of the element defines a EQUAL method itself.

##### size\_t name\_hash(const name\_t array)

Return the has value of 'array'.
This method is only defined if the type of the element defines a HASH method itself.




### M-DEQUE

This header is for creating [double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue) (or deque). 
A deque is an abstract data type that generalizes a queue, 
for which elements can be added to or removed from either the front (head) or back (tail)

#### DEQUE\_DEF(name, type, [, opdeque])

Define the deque 'name##\_t' which contains the objects of type 'type' and its associated methods as "static inline" functions.
'name' shall be a C identifier which will be used to identify the deque. It will be used to create all the types and functions to handle the container.
It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

The 'oplist' is expected to have at least the following operators (INIT, INIT_SET, SET and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

Example:

	DEQUE_DEF(deque_mpz, mpz_t,                                               \
		(INIT(mpz_init), INIT_SET(mpz_init_set), SET(mpz_set), CLEAR(mpz_clear)))

	deque_mpz_t my_deque;

	void f(mpz_t z) {
		deque_mpz_push_back (my_deque, z);
	}


#### DEQUE\_OPLIST(name [, oplist])

Return the oplist of the deque defined by calling DEQUE\_DEF with name & oplist. 

#### Created methods

In the following methods, name stands for the name given to the macro which is used to identify the type.
The following types are automatically defined by the previous macro:

#### name\_t

Type of the deque of 'type'.

#### name\_it\_t

Type of an iterator over this deque.

The following methods are automatically and properly created by the previous macro.

##### void name\_init(name\_t deque)

Initialize the deque 'deque' (aka constructor) to an empty deque.

##### void name\_init\_set(name\_t deque, const name\_t ref)

Initialize the deque 'deque' (aka constructor) and set it to the value of 'ref'.

##### void name\_set(name\_t deque, const name\_t ref)

Set the deque 'deque' to the value of 'ref'.

##### void name\_init\_move(name\_t deque, name\_t ref)

Initialize the deque 'deque' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared and can no longer be used.

##### void name\_move(name\_t deque, name\_t ref)

Set the deque 'deque' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared and can no longer be used.

##### void name\_clear(name\_t deque)

Clear the deque 'deque (aka destructor). The deque can't be used anymore, except with a constructor.

##### void name\_clean(name\_t deque)

Clean the deque (the deque becomes empty). The deque remains initialized but is empty.

##### const type *name\_back(const name\_t deque)

Return a constant pointer to the data stored in the back of the deque.

##### void name\_push\_back(name\_t deque, type value)

Push a new element within the deque 'deque' with the value 'value' at the back of the deque.

##### type *name\_push\_back÷\_raw(name\_t deque)

Push at the back a new element within the deque 'deque' without initializing it and returns a pointer to the **non-initialized** data.
The first thing to do after calling this function is to initialize the data using the proper constructor. This allows to use a more specialized
constructor than the generic one.
Return a pointer to the **non-initialized** data.

##### type *name\_push\_back\_new(name\_t deque)

Push at the back a new element within the deque 'deque' and initialize it with the default constructor of the type.
Return a pointer to the initialized object.

##### void name\_pop\_back(type *data, name\_t deque)

Pop a element from the deque 'deque' and set *data to this value.
If data pointer is NULL, then the poped value is discarded.

##### const type *name\_front(const name\_t deque)

Return a constant pointer to the data stored in the front of the deque.

##### void name\_push\_front(name\_t deque, type value)

Push at the front a new element within the deque 'deque' with the value 'value'.

##### type *name\_push\_front\_raw(name\_t deque)

Push at the front a new element within the deque 'deque' without initializing it and returns a pointer to the **non-initialized** data.
The first thing to do after calling this function is to initialize the data using the proper constructor. This allows to use a more specialized
constructor than the generic one.
Return a pointer to the **non-initialized** data.

##### type *name\_push\_front\_new(name\_t deque)

Push at the front a new element within the deque 'deque' and initialize it with the default constructor of the type.
Return a pointer to the initialized object.

##### void name\_pop\_front(type *data, name\_t deque)

Pop a element from the deque 'deque' and set *data to this value.
If data pointer is NULL, then the poped value is discarded.

##### bool name\_empty\_p(const name\_t deque)

Return true if the deque is empty, false otherwise.

##### void name\_swap(name\_t deque1, name\_t deque2)

Swap the deque 'deque1' and 'deque2'.

##### void name\_it(name\_it\_t it, name\_t deque)

Set the iterator 'it' to the first element of 'deque' (aka the front).
There is no destructor associated to this initialization.

##### void name\_it\_set(name\_it\_t it, const name\_it\_t ref)

Set the iterator 'it' to the iterator 'ref'.
There is no destructor associated to this initialization.

##### bool name\_end\_p(const name\_it\_t it)

Return true if the iterator doesn't reference a valid element anymore.

##### bool name\_last\_p(const name\_it\_t it)

Return true if the iterator references the last element or if the iterator doesn't reference a valid element anymore.

##### bool name\_it\_equal\_p(const name\_it\_t it1, const name\_it\_t it2)

Return true if the iterator it1 references the same element than it2.

##### void name\_next(name\_it\_t it)

Move the iterator 'it' to the next element of the deque, ie. from the front element to the back element.

##### void name\_previous(name\_it\_t it)

Move the iterator 'it' to the previous element of the deque, ie. from the back element to the front element.

##### type *name\_ref(name\_it\_t it)

Return a pointer to the element pointed by the iterator.
This pointer remains valid until the deque is modified by another method.

##### const type *name\_cref(const name\_it\_t it)

Return a constant pointer to the element pointed by the iterator.
This pointer remains valid until the deque is modified by another method.

##### type *name\_get(const name\_t deque, size\_t i)

Return a pointer to the element i-th of the deque (from 0). 
It is assumed than i is within the size of the deque.
The algorithm complexity is in O(ln(n))

##### const type *name\_cget(const name\_t deque, size\_t i)

Return a constant pointer to the element i-th of the deque (from 0). 
It is assumed than i is within the size of the deque.
The algorithm complexity is in O(ln(n))

##### size\_t name\_size(const name\_t deque)

Return the number elements of the deque (aka size). Return 0 if there no element.

##### void name\_get\_str(string\_t str, const name\_t deque, bool append)

Generate a string representation of the deque 'deque' and set 'str' to this representation
(if 'append' is false) or append 'str' with this representation (if 'append' is true).
This method is only defined if the type of the element defines a GET\_STR method itself.

##### void name\_out\_str(FILE *file, const name\_t deque)

Generate a string representation of the deque 'deque' and outputs it into the FILE 'file'.
This method is only defined if the type of the element defines a OUT\_STR method itself.

##### void name\_in\_str(FILE *file, const name\_t deque)

Read from the file 'file' a string representation of a deque and set 'deque' to this representation.
This method is only defined if the type of the element defines a IN\_STR method itself.

##### bool name\_equal\_p(const name\_t deque1, const name\_t deque2)

Return true if both deques 'deque1' and 'deque2' are equal.
This method is only defined if the type of the element defines a EQUAL method itself.

##### size\_t name\_hash(const name\_t deque)

Return the has value of 'deque'.
This method is only defined if the type of the element defines a HASH method itself.

##### void name\_swap\_at(name\_t deque, size\_t i, size\_t j)

Swap the values within the deque pointed by 'i' and by 'j'.
'i' & 'j' shall be valid index within the deque.
This method is only defined if the type of the element defines a SWAP method itself.





### M-DICT

A [dictionary](https://en.wikipedia.org/wiki/Associative_array) (or associative array, map, symbol table) is an abstract data type
composed of a collection of (key, value) pairs,
such that each possible key appears at most once in the collection.

#### DICT\_DEF2(name, key\_type, key\_oplist, value\_type, value\_oplist)

Define the dictionary 'name##\_t' and its associated methods as "static inline" functions.
'name' shall be a C identifier which will be used to identify the container.
Current implementation uses chained Hash-Table and as such, elements in the dictionary are **unordered**.

It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

The object oplists are expected to have the following operators (INIT, INIT\_SET, SET and CLEAR), otherwise default operators are used.
If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.
The key_oplist shall also define the additional operators (HASH and EQUAL).

Interface is subjected to minor change.

Example:

	DICT_DEF2(dict_str, string_t, STRING_OPLIST, string_t, STRING_OPLIST)
	dict_str_t my_dict;
	void f(string_t key, string_t value) {
		dict_str_set_at (my_dict, key, value);
	}


#### DICT\_STOREHASH\_DEF2(name, key\_type, key\_oplist, value\_type, value\_oplist)

Define the dictionary 'name##\_t' and its associated methods as "static inline" functions
just like DICT\_DEF2.

The only difference is that it stores the computed hash in the dictionnary
which allows avoiding recomputing it in some occasions resulting in faster
dictionary if the hash is costly to compute, or slower otherwise.


#### DICT\_OA\_DEF2(name, key\_type, key\_oplist, value\_type, value\_oplist)

Define the dictionary 'name##\_t' and its associated methods
as "static inline" functions much like DICT\_DEF2.
The difference is that it uses an Open Addressiong Hash-Table as 
container.

It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

The object oplists are expected to have the following operators 
(INIT, INIT\_SET, SET, CLEAR), otherwise default operators are used.
If there is no given oplist, the default operators are also used. 
The created methods will use the operators to init, set and clear the contained object.
The key_oplist shall also define the additional operators :
HASH and EQUAL and **OOR\_EQUAL** and **OOR\_SET**

Interface is subjected to minor change.

This implementation is in general faster for small types of keys
(like integer).

Example:

	static inline bool oor_equal_p(int k, unsigned char n) {
	  return k == (int)-n-1;
	}
	static inline void oor_set(int *k, unsigned char n) {
	  *k = (int)-n-1;
	}

	DICT_OA_DEF2(dict_int, int, M_OPEXTEND(M_DEFAULT_OPLIST, OOR_EQUAL(oor_equal_p), OOR_SET(oor_set M_IPTR)), int64_t, M_DEFAULT_OPLIST)

	dict_int_t my_dict;
	void f(int key, int64_t value) {
		dict_int_set_at (my_dict, key, value);
	}


#### DICT\_OPLIST(name, key\_oplist, value\_oplist)

Return the oplist of the dictionary defined by calling DICT\_DEF2 with name & key\_oplist & value\_oplist. 

#### DICT\_SET\_DEF2(name, key\_type, key\_oplist)

Define the set 'name##\_t' and its associated methods as "static inline" functions.
A set is a specialized version of a dictionary with no value.

It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

The oplist is expected to have the following operators (INIT, INIT\_SET, SET, CLEAR, HASH and EQUAL), otherwise default operators are used.
If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

Interface is subjected to minor change.

Example:

	DICT_SET_DEF2(dict_strSet, string_t, STRING_OPLIST)
	dict_strSet_t set;
	void f(string_t key) {
		dict_strSet_set_at (set, key);
	}

#### DICT\_SET\_OPLIST(name, key\_oplist)

Return the oplist of the set defined by calling DICT\_SET\_DEF2 with name & key\_oplist.

#### Created methods

In the following methods, name stands for the name given to the macro which is used to identify the type.
The following types are automatically defined by the previous macro:

##### name\_t

Type of the dictionary of 'key_type' --> 'value_type'.

##### name\_it\_t

Type of an iterator over this dictionary.

The following methods are automatically and properly created by the previous macro:

##### void name\_init(name\_t dict)

Initialize the dictionary 'dict' to be empty.

##### void name\_clear(name\_t dict)

Clear the dictionary 'dict'.

##### void name\_init\_set(name\_t dict, const name\_t ref)

Initialize the dictionary 'dict' to be the same as 'ref'.

##### void name\_set(name\_t dict, const name\_t ref)

Set the dictionary 'dict' to be the same as 'ref'.

##### void name\_init\_move(name\_t dict, name\_t ref)

Initialize the dictionary 'dict' by stealing as resource as possible
from 'ref' and clear 'ref'.

##### void name\_move(name\_t dict, name\_t ref)

Set the dictionary 'dict'  by stealing as resource as possible
from 'ref' and clear 'ref'.

##### void name\_clean(name\_t dict)

Clean the dictionary 'dict'. 'dict' remains initialized.

##### size\_t name\_size(const name\_t dict)

Return the number of elements of the dictionary.

##### value\_type \*name\_get(const name\_t dict, const key\_type key)

Return a pointer to the value associated to the key 'key' in dictionary
'dict' or NULL if the key is not found.

##### void name\_set\_at(name\_t dict, const key\_type key, const value\_type value)

Set the value referenced by key 'key' in the dictionary 'dict' to 'value'.

##### void name\_remove(name\_t dict, const key\_type key)

Remove the element referenced by key 'key' in the dictionary 'dict'.
Do nothing if 'key' is no present in the dictionary.

##### void name\_it(name\_it\_t it, name\_t dict)

Set the iterator 'it' to the first element of 'dict'.

##### void name\_it\_set(name\_it\_t it, const name\_it\_t ref)

Set the iterator 'it' to the same element than 'ref'.

##### bool name\_end\_p(const name\_it\_t it)

Return true if 'it' references no longer a valid element.

##### bool name\_last\_p(const name\_it\_t it)

Return true if 'it' references the last element or is no longer valid.

##### void name\_next(name\_it\_t it)

Update the iterator 'it' to the next element.

##### struct name\_pair\_s *name\_ref(name\_it\_t it)

Return a pointer to the pair composed by the key ('key' field) and its value ('value' field).
'key' element can not be modified.
This pointer remains valid until the dictionary is modified by another method.

##### const struct name\_pair\_s *name\_ref(name\_it\_t it)

Return a constant pointer to the pair composed by the key('key' field)  and its value ('value' field).
This pointer remains valid until the dictionary is modified by another method.

##### void name\_get\_str(string\_t str, const name\_t dict, bool append)

Generate a string representation of the dict 'dict' and set 'str' to this representation
(if 'append' is false) or append 'str' with this representation (if 'append' is true).
This method is only defined if the type of the element defines a GET\_STR method itself.

##### void name\_out\_str(FILE *file, const name\_t dict)

Generate a string representation of the dict 'dict' and outputs it into the FILE 'file'.
This method is only defined if the type of the element defines a OUT\_STR method itself.

##### void name\_in\_str(FILE *file, const name\_t dict)

Read from the file 'file' a string representation of a dict and set 'dict' to this representation.
This method is only defined if the type of the element defines a IN\_STR method itself.

##### bool name\_equal\_p(const name\_t dict1, const name\_t dict2)

Return true if both dicts 'dict1' and 'dict2' are equal.
This method is only defined if the type of the element defines a EQUAL method itself.



### M-TUPLE

A [tuple](https://en.wikipedia.org/wiki/Tuple) is a finite ordered list of elements of different types. 

#### TUPLE\_DEF2(name, (element1, type1, oplist1) [, ...])

Define the tuple 'name##\_t' and its associated methods as "static inline" functions.
Each parameter of the macro is expected to be an element of the tuple.
Each element is defined by three parameters within parenthesis: 
the element name, the element type and the element oplist.
'name' and 'element' shall be a C identifier which will be used to identify the container.

This is more or less like a C structure. The main added value compared to using a struct
is that it generates also all the basic methods to handle it.
In fact, it generates a C struct with the given type and element.

It shall be done once per type and per compilation unit.

The object oplists are expected to have the following operators (INIT, INIT_SET, SET and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

The interface is subjected to change.

Example:

	TUPLE_DEF2(pair, (key, string_t, STRING_OPLIST),
			 (value, mpz_t, (INIT(mpz_init), INIT_SET(mpz_init_set), SET(mpz_set), CLEAR(mpz_clear) )))
	void f(void) {
		pair_t p1;
		pair_init (p1);
		string_set_str(p1->key, "HELLO");
		mpz_set_str(p1->value, "1742", 10);
		pair_clear(p1);
	}

#### TUPLE\_OPLIST(name, oplist1[, ...] )

Return the oplist of the tuple defined by calling TUPLE\_DEF2 with the given name & the oplists.

#### Created methods

In the following methods, name stands for the name given to the macro which is used to identify the type.
The following types are automatically defined by the previous macro:

#### name\_t

Type of the defined tuple.

The following methods are automatically and properly created by the previous macros:

##### void name\_init(name\_t tuple)

Initialize the tuple 'tuple' (aka constructor) to an empty tuple.

##### void name\_init\_set(name\_t tuple, const name\_t ref)

Initialize the tuple 'tuple' (aka constructor) and set it to the value of 'ref'.

##### void name\_init\_set2(name\_t tuple, const type1 element1[, ...])

Initialize the tuple 'tuple' (aka constructor) and set it to the value of the tuple ('element1'[, ...]).

##### void name\_set(name\_t tuple, const name\_t ref)

Set the tuple 'tuple' to the value of 'ref'.

##### void name\_init\_move(name\_t tuple, name\_t ref)

Initialize the tuple 'tuple' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared.
This method is created only if all oplists of the tuple define INIT\_MOVE method.

##### void name\_move(name\_t tuple, name\_t ref)

Set the tuple 'tuple' by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared.
This method is created only if all oplists of the tuple define MOVE method.

##### void name\_clear(name\_t tuple)

Clear the tuple 'tuple (aka destructor).

##### const type1 *name\_get\_element1(const name\_t tuple)

Return a constant pointer to the element 'element1' of the tuple.
There is as many methods as there are elements.

##### void name\_set\_element1(name\_t tuple, type1 element1)

Set the element of the tuple to 'element1'
There is as many methods as there are elements.

##### int name\_cmp(const name\_t tuple1, const name\_t tuple2)

Compare 'tuple1' to 'tuple2' using lexicographic order.
This method is created only if all oplists of the tuple define CMP method.

##### int name\_cmp\_element1(const name\_t tuple1, const name\_t tuple2)

Compare 'tuple1' to 'tuple2' using only the element element1 as reference.
This method is created only if the oplist of element1 defines the CMP method.

##### int name\_equal\_p(const name\_t tuple1, const name\_t tuple2)

Return true if 'tuple1' and 'tuple2' are identical.
This method is created only if all oplists of the tuple define EQUAL method.

##### void name\_get\_str(string\_t str, const name\_t tuple, bool append)

Generate a string representation of the tuple 'tuple' and set 'str' to this representation
(if 'append' is false) or append 'str' with this representation (if 'append' is true).
This method is only defined if all oplists define a GET\_STR method.

##### void name\_out\_str(FILE *file, const name\_t tuple)

Generate a string representation of the tuple 'tuple' and outputs it into the FILE 'file'.
This method is only defined if all oplists define a OUT\_STR method.

##### void name\_in\_str(FILE *file, const name\_t tuple)

Read from the file 'file' a string representation of a tuple and set 'tuple' to this representation.
This method is only defined if all oplists define a IN\_STR method.


### M-VARIANT

A [variant](https://en.wikipedia.org/wiki/Variant_type) is a finite exclusive list of elements of different types :
the variant can be only equal to one element at a time. 

#### VARIANT\_DEF2(name, (element1, type1, oplist1) [, ...])

Define the variant 'name##\_t' and its associated methods as "static inline" functions.
Each parameter of the macro is expected to be an element of the variant.
Each element is defined by three parameters within parenthesis: 
the element name, the element type and the element oplist.
'name' and 'element' shall be a C identifier which will be used to identify the container.

This is more or less like a C union. The main added value compared to using a union
is that it generates all the basic methods to handle it and it dynamically identifies
which element is stored within.

It shall be done once per type and per compilation unit.

The object oplists are expected to have the following operators (INIT, INIT_SET, SET and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

The interface is subjected to change.

Example:

	VARIANT_DEF2(either, (key, string_t, STRING_OPLIST),
			 (value, mpz_t, (INIT(mpz_init), INIT_SET(mpz_init_set), SET(mpz_set), CLEAR(mpz_clear) )))
	void f(sting_t s) {
		either_t p1;
		either_init (p1);
		either_set_key(p1, s);
		either_clear(p1);
	}

#### VARIANT\_OPLIST(name, oplist1[, ...] )

Return the oplist of the variant defined by calling VARIANT\_DEF2 with the given name & the oplists.

#### Created methods

In the following methods, name stands for the name given to the macro which is used to identify the type.
The following types are automatically defined by the previous macro:

#### name\_t

Type of the defined variant.

The following methods are automatically and properly created by the previous macros:

##### void name\_init(name\_t variant)

Initialize the variant 'variant' (aka constructor) to be empty.

##### void name\_init\_set(name\_t variant, const name\_t ref)

Initialize the variant 'variant' (aka constructor) and set it to the value of 'ref'.

##### void name\_set(name\_t variant, const name\_t ref)

Set the variant 'variant' to the value of 'ref'.

##### void name\_init\_move(name\_t variant, name\_t ref)

Initialize the variant 'variant' (aka constructor) by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared.
This method is created only if all oplists of the variant define INIT\_MOVE method.

##### void name\_move(name\_t variant, name\_t ref)
##### void name\_move_elementN(name\_t variant, typeN ref)

Set the variant 'variant' by stealing as many resources from 'ref' as possible.
After-wise 'ref' is cleared.
This method is created only if all oplists of the variant define MOVE method.

##### void name\_clear(name\_t variant)

Clear the variant 'variant (aka destructor).

##### void name\_clean(name\_t variant)

Clean the variant 'variant and make it empty.

##### void name\_init_elementN(name\_t variant)

Initialize the variant 'variant' to the type of 'element1'

##### void name\_init\_set_elementN(name\_t variant, const typeN elementN)

Initialize and set the variant 'variant' to the type and value of 'elementN'.

##### void name\_set_elementN(name\_t variant, const typeN elementN)

Set the variant 'variant' to the type and value of 'elementN'.

##### typeN * name\_get_elementN(name\_t variant)

Return a pointer to the 'varian' viewed as of type 'typeN'.
If the variant isn't an object of such type, it returns NULL.

##### bool name\_empty\_p(const name\_t variant)

Return true if the variant is empty, false otherwise.

##### bool name\_elementN\_p(const name\_t variant)

Return true if the variant is of the type of 'elementN'.

##### size_t name\_hash(const name\_t variant)

Return a hash associated to the variant.
All types associated to the variant shall have a hash function
for this function to be defined.

##### bool name\_equal\_p(const name\_t variant1, const name\_t varian2)

Return true if both objects are equal, false otherwise.
All types associated to the variant shall have a equal\_p function
for this function to be defined.

##### void name\_swap(name\_t variant1, name\_t varian2)

Swap both objects.

##### void name\_get\_str(string\_t str, name\_t variant, bool append)

Convert the variant into a string, appending it into 'str' or not.
All types associated to the variant shall have a get\_str function
for this function to be defined.

##### void name\_out\_str(FILE *f, name\_t variant)

Convert the variant into a string and send it to the stream 'f'.
All types associated to the variant shall have a out\_str function
for this function to be defined.

##### void name\_in\_str(name\_t variant, FILE *f)

Read a string representation of the variant from the stream 'f'
and update the object variant with it.
All types associated to the variant shall have a in\_str function
for this function to be defined.


### M-RBTREE

#### RBTREE\_DEF(name, type[, oplist])

Define the binary ordered tree 'name##\_t' and its associated methods as "static inline" functions.
A binary tree is a tree data structure in which each node has at most two children, which are referred to as the left child and the right child.
In this kind of tree, all elements of the tree are totally ordered.
The current implementation is [RED-BLACK TREE](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree).
It shall not be confused with a [B-TREE](https://en.wikipedia.org/wiki/B-tree).
'name' shall be a C identifier which will be used to identify the container.

The CMP operator is used to perform the total ordering of the elements.

The UPDATE operator is used to update an element if the 
pushed item already exist in the container. The default behavior
will overwrite the recorded value with the new one.
 
It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

The object oplist is expected to have the following operators (INIT, INIT_SET, SET, CMP and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

Example:

	RBTREE_DEF(rbtree_uint, unsigned int)
	void f(unsigned int num) {
		rbtree_uint_t tree;
		rbtree_uint_init(tree);
		for(unsigned int i = 0; i < num; i++)
			rbtree_uint_push(tree, i);
		rbtree_uint_clear(tree);                              
	}

#### RBTREE\_OPLIST(name [, oplist])

Return the oplist of the Red-Black defined by calling RBTREE\_DEF with name & oplist.


#### Created methods

The following methods are automatically and properly created by the previous macros. 
In the following methods, name stands for the name given to the macro which is used to identify the type.

##### name\_t

Type of the Red Black Tree of 'type'.

##### name\_it_\_t

Type of an iterator over this Red Black Tree.

##### void name\_init(name\_t rbtree)

Initialize the Red Black Tree 'rbtree' to be empty.

##### void name\_clear(name\_t rbtree)

Clear the Red Black Tree 'rbtree'.

##### void name\_init\_set(name\_t rbtree, const name\_t ref)

Initialize the Red Black Tree 'rbtree' to be the same as 'ref'.

##### void name\_set(name\_t rbtree, const name\_t ref)

Set the Red Black Tree 'rbtree' to be the same as 'ref'.

##### void name\_init\_move(name\_t rbtree, name\_t ref)

Initialize the Red Black Tree 'rbtree' by stealing as resource as possible
from 'ref' and clear 'ref'.

##### void name\_move(name\_t rbtree, name\_t ref)

Set the Red Black Tree 'rbtree' by stealing as resource as possible
from 'ref' and clear 'ref'.

##### void name\_clean(name\_t rbtree)

Clean the Red Black Tree 'rbtree'. 'rbtree' remains initialized but empty.

##### size\_t name\_size(const name\_t rbtree)

Return the number of elements of the Red Black Tree.

##### void name\_push(name\_t rbtree, const type data)

Push 'data' into the Red Black Tree 'rbtree' at its ordered place
while keeping the tree balanced.

##### void name\_pop(type *dest, name\_t rbtree, const type data)

Pop 'data' from the Red Black Tree 'rbtree'
and save the poped value into 'dest' if the pointer is not null
while keeping the tree balanced.
Do nothing if 'data' is no present in the Red Black Tree.

##### type * name\_min(const name\_t rbtree)
##### const type * name\_cmin(const name\_t rbtree)

Return a pointer to the minimum element of the tree
or NULL if there is no element.

##### type * name\_max(const name\_t rbtree)
##### const type * name\_cmax(const name\_t rbtree)

Return a pointer to the maximum element of the tree
or NULL if there is no element.

##### type * name\_get(const name\_t rbtree, const type *data)
##### const type * name\_cget(const name\_t rbtree, const type *data)

Return a pointer to the element of the tree 'rbtree' which is equal to 'data',
or NULL if there is no match.

##### void name\_swap(name\_t rbtree1, name\_t rbtree2)

Swap both trees.

##### bool name\_empty\_p(const name\_t rbtree)

Return true if the tree is empty, false otherwise.

##### void name\_it(name\it_\_t it, name\_t rbtree)

Set the iterator 'it' to the first element of 'rbtree'.

##### void name\_it\_set(name\it_\_t it, const name\it_\_t ref)

Set the iterator 'it' to the same element than 'ref'.

##### void name\_it\_last(name\it_\_t it, name\_t rbtree)

Set the iterator 'it' to the last element of 'rbtree'.

##### void name\_it\_end(name\it_\_t it, name\_t rbtree)

Set the iterator 'it' to no element of 'rbtree'.

##### void name\_it\_from(name\it_\_t it, const name\_t rbtree, const type data)

Set the iterator 'it' to the greatest element of 'rbtree'
lower of equal than 'data' or the first element is there is none.

##### bool name\_end\_p(const name\it_\_t it)

Return true if 'it' references no longer a valid element.

##### bool name\_last\_p(const name\it_\_t it)

Return true if 'it' references the last element or is no longer valid.

##### bool name\_to\_p(const name\it_\_t it, const type data)

Return true if 'it' references an element which is greater or equal than 'data'.

##### void name\_next(name\it_\_t it)

Update the iterator 'it' to the next element.

##### void name\_previous(name\it_\_t it)

Update the iterator 'it' to the previous element.

##### type *name\_ref(name\it_\_t it)
##### const type *name\_ref(name\it_\_t it)

Return a pointer to the element pointer by the iterator 'it'.
This pointer remains valid until the Red Black Tree is modified by another method.

##### void name\_get\_str(string\_t str, const name\_t rbtree, bool append)

Generate a string representation of the rbtree 'rbtree' and set 'str' to this representation
(if 'append' is false) or append 'str' with this representation (if 'append' is true).
This method is only defined if the type of the element defines a GET\_STR method itself.

##### void name\_out\_str(FILE *file, const name\_t rbtree)

Generate a string representation of the rbtree 'rbtree' and outputs it into the FILE 'file'.
This method is only defined if the type of the element defines a OUT\_STR method itself.

##### void name\_in\_str(FILE *file, const name\_t rbtree)

Read from the file 'file' a string representation of a rbtree and set 'rbtree' to this representation.
This method is only defined if the type of the element defines a IN\_STR method itself.

##### bool name\_equal\_p(const name\_t rbtree1, const name\_t rbtree2)

Return true if both rbtrees 'rbtree1' and 'rbtree2' are equal.
This method is only defined if the type of the element defines a EQUAL method itself.

##### size\_t name\_hash\_p(const name\_t rbtree)

Return the hash of the tree.
This method is only defined if the type of the element defines a HASH method itself.

### M-BPTREE

A [B+TREE](https://en.wikipedia.org/wiki/B%2B_tree) is a variant of
[BTREE](https://en.wikipedia.org/wiki/B-tree) which itself is
a generalization of [Binary Tree](https://en.wikipedia.org/wiki/Binary_tree).

A B+TREE is an N-ary tree with a variable but often large number of children per node.
It is mostly used for handling slow media by file system and database.

It provides a fully sorted container allowing fast access to individual item
or range of items, and as such is concurrent to Red-Black Tree.
On modern architecture, a B+TREE is typically faster than a red-black tree due to being
more cache friendly (The RAM itself can be considered as a slow media nowadays!)

When defining a B+TREE it is necessary to give the type of the item within, but also
the maximum number of child per node. The best maximum number of child per node
depends on the type itself (its size, its compare cost) and the cache of the
processor. 

#### BPTREE\_DEF2(name, N, key_type, key_oplist, value, value_oplist)

Define the B+TREE tree of rank N 'name##\_t' and its associated methods as
"static inline" functions. This B+TREE will be created as an associative
array of the key_type to the value_type.

The CMP operator is used to perform the total ordering of the key elements.

It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

The object oplist is expected to have the following operators (INIT, INIT_SET, SET, CMP and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

Example:

	BPTREE_DEF2(tree_uint, unsigned int, (), float, ())
	void f(unsigned int num) {
		tree_uint_t tree;
		tree_uint_init(tree);
		for(unsigned int i = 0; i < num; i++)
			tree_uint_set_at(tree, i, (float) i);
		tree_uint_clear(tree);
	}

#### BPTREE\_OPLIST2(name, key_oplist, value_oplist)

Return the oplist of the BPTREE defined by calling BPTREE\_DEF2 with name, key_oplist
and value_oplist.

#### BPTREE\_DEF(name, N, key_type[, key_oplist])

Define the B+TREE tree of rank N 'name##\_t' and its associated methods as
"static inline" functions. This B+TREE will be created as an ordered set
of key_type.

The CMP operator is used to perform the total ordering of the key elements.

It shall be done once per type and per compilation unit.
It also define the iterator name##\_it\_t and its associated methods as "static inline" functions.

The object oplist is expected to have the following operators (INIT, INIT_SET, SET, CMP and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

In the following specification, in this case, value\_type will be defined as the same
as key\_type.

Example:

	BPTREE_DEF(tree_uint, unsigned int)
	void f(unsigned int num) {
		tree_uint_t tree;
		tree_uint_init(tree);
		for(unsigned int i = 0; i < num; i++)
			tree_uint_push(tree, i);
		tree_uint_clear(tree);
	}

#### BPTREE\_OPLIST(name[, key_oplist])

Return the oplist of the BPTREE defined by calling BPTREE\_DEF with name, key_oplist
and value_oplist.


#### Created methods

The following methods are automatically and properly created by the previous macros. 
In the following methods, name stands for the name given to the macro which is used to identify the type.

##### name\_t

Type of the B+Tree of 'type'.

##### name\_it_\_t

Type of an iterator over this B+Tree.

##### void name\_init(name\_t tree)

Initialize the B+Tree 'tree' and set it to empty.

##### void name\_clear(name\_t tree)

Clear the B+Tree 'tree'.

##### void name\_init\_set(name\_t tree, const name\_t ref)

Initialize the B+Tree 'tree' to be the same as 'ref'.

##### void name\_set(name\_t tree, const name\_t ref)

Set the B+Tree 'tree' to be the same as 'ref'.

##### void name\_init\_move(name\_t tree, name\_t ref)

Initialize the B+Tree 'tree' by stealing as resource as possible
from 'ref' and clear 'ref'.

##### void name\_move(name\_t tree, name\_t ref)

Set the B+Tree 'tree' by stealing as resource as possible
from 'ref' and clear 'ref'.

##### void name\_clean(name\_t tree)

Clean the B+Tree 'tree'. 'tree' remains initialized but empty.

##### size\_t name\_size(const name\_t tree)

Return the number of elements of the B+Tree.

##### void name\_push(name\_t tree, const key\_type data)

Push 'data' into the B+Tree 'tree' at the right order
while keeping the tree balanced.
This function is defined only if the tree is not defined as an associative array.

##### void name\_set\_at(name\_t tree, const key\_type data, const value\_type val)

Associate the value 'val' to the key 'data' in the B+Tree 'tree'
while keeping the tree balanced.
This function is defined only if the tree is defined as an associative array.

##### void name\_pop(value\_type *dest, name\_t tree, const key\_type data)

Pop 'data' from the B+Tree 'tree'
and save the popped value into 'dest' if the pointer is not null
while keeping the tree balanced.
Do nothing if 'data' is no present in the B+Tree.

##### bool name\_remove(name\_t tree, const key\_type data)

Remove 'data' from the B+Tree 'tree'
while keeping the tree balanced.
Return true if the data is removed, false if nothing is done (data is not present).

##### value\_type * name\_min(const name\_t tree)
##### const value\_type * name\_cmin(const name\_t tree)

Return a pointer to the minimum element of the tree
or NULL if there is no element.

##### value\_type * name\_max(const name\_t tree)
##### const value\_type * name\_cmax(const name\_t tree)

Return a pointer to the maximum element of the tree
or NULL if there is no element.

##### value\_type * name\_get(const name\_t tree, const key\_type *data)
##### const value\_type * name\_cget(const name\_t tree, const key\_type *data)

Return a pointer to the value of the tree 'tree' which is associated to 'data',
or NULL if there is no match.

##### void name\_swap(name\_t tree1, name\_t tree2)

Swap both trees.

##### bool name\_empty\_p(const name\_t tree)

Return true if the tree is empty, false otherwise.

##### void name\_it(name\it_\_t it, name\_t tree)

Set the iterator 'it' to the first element of 'tree'.

##### void name\_it\_set(name\it_\_t it, const name\it_\_t ref)

Set the iterator 'it' to the same element than 'ref'.

##### void name\_it\_end(name\it_\_t it, name\_t tree)

Set the iterator 'it' to no element of 'tree'.

##### void name\_it\_from(name\it_\_t it, const name\_t tree, const type data)

Set the iterator 'it' to the greatest element of 'tree'
lower of equal than 'data' or the first element is there is none.

##### bool name\_end\_p(const name\it_\_t it)

Return true if 'it' references no longer a valid element.

##### bool name\_to\_p(const name\it_\_t it, const type data)

Return true if 'it' references an element which is greater or equal than 'data'.

##### void name\_next(name\it_\_t it)

Update the iterator 'it' to the next element.

##### type *name\_ref(name\it_\_t it)
##### const type *name\_ref(name\it_\_t it)

Return a pointer to the element pointer by the iterator 'it'.
This pointer remains valid until the B+Tree is modified by another method.

##### void name\_get\_str(string\_t str, const name\_t tree, bool append)

Generate a string representation of the tree 'tree' and set 'str' to this representation
(if 'append' is false) or append 'str' with this representation (if 'append' is true).
This method is only defined if the type of the element defines a GET\_STR method itself.

##### void name\_out\_str(FILE *file, const name\_t tree)

Generate a string representation of the tree 'tree' and outputs it into the FILE 'file'.
This method is only defined if the type of the element defines a OUT\_STR method itself.

##### bool name\_parse\_str(name\_t tree, const char str[], const char **endp)

Parse the string 'str' which is assumed to be a string representation of a tree
and set 'tree' to this representation.
This method is only defined if the type of the element defines a PARSE\_STR method itself.
It returns true if success, false otherwise.
If endp is not NULL, it sets '*endp' to the pointer of the first character not
decoded by the function.

##### void name\_in\_str(FILE *file, const name\_t tree)

Read from the file 'file' a string representation of a tree and set 'tree' to this representation.
This method is only defined if the type of the element defines a IN\_STR method itself.

##### bool name\_equal\_p(const name\_t tree1, const name\_t tree2)

Return true if both trees 'tree1' and 'tree2' are equal.
This method is only defined if the type of the element defines a EQUAL method itself.

##### size\_t name\_hash\_p(const name\_t tree)

Return the hash of the tree.
This method is only defined if the type of the element defines a HASH method itself.



### M-PRIOQUEUE

A [priority queue](https://en.wikipedia.org/wiki/Priority_queue) is a queue  where each element has a "priority" associated with it: an element with high priority is served before an element with low priority. It is currently implemented as a [heap](https://en.wikipedia.org/wiki/Heap_(data_structure)).


TODO: priority queue


### M-BUFFER

A [circular buffer](https://en.wikipedia.org/wiki/Circular_buffer) 
(or ring buffer) is a data structure using a single, bounded buffer
as if it were connected end-to-end. 

#### BUFFER\_DEF(name, type, size, policy[, oplist])

Define the buffer 'name##\_t' and its associated methods as "static inline" functions.
A buffer is a fixed size queue or stack.
If it is built with the BUFFER\_THREAD\_SAFE option (default) it can be used to transfert message
from mutiple producer threads to multiple consummer threads.
This is done internaly using a mutex and conditional waits.

'name' shall be a C identifier which will be used to identify the container.

The size parameter defined the fixed size of the queue.
It can be 0, in which case, the fixed size will be defined at initialization time
and the needed objects to handle the buffer will be allocated at initialization time too.
Otherwise the needed objects will be embedded within the structure, preventing
any other allocations.

Multiple additional policy can be applied to the buffer by performing a logical or of the following properties:

* BUFFER\_QUEUE : define a FIFO queue (default),
* BUFFER\_STACK : define a stack (exclusive with BUFFER\_QUEUE),
* BUFFER\_BLOCKING : define blocking calls for the default push/pop methods (default); this is a synonym to BUFFER\_BLOCKING\_PUSH|BUFFER\_BLOCKING\_POP,
* BUFFER\_UNBLOCKING : define unblocking calls for the default push/pop methods; this is a synonum to BUFFER\_UNBLOCKING\_PUSH|BUFFER\_UNBLOCKING\_POP,
* BUFFER\_BLOCKING\_PUSH : define blocking calls for the default push methods (default),
* BUFFER\_UNBLOCKING\_PUSH : define unblocking calls for the default push methods,
* BUFFER\_BLOCKING\_POP : define blocking calls for the default pop methods (default),
* BUFFER\_UNBLOCKING\_POP : define unblocking calls for the default pop methods,
* BUFFER\_THREAD\_SAFE : define thread safe functions (default),
* BUFFER\_THREAD\_UNSAFE : define thread unsafe functions,
* BUFFER\_PUSH\_INIT\_POP\_MOVE : change the behavior of PUSH to push a new initialized object, and POP as moving this new object into the new emplacement (this is mostly used for performance reasons or to handle properly a shared_ptr semantic). In practice, it works as if POP performs the initialization of the object. 
* BUFFER\_PUSH\_OVERWRITE : PUSH will always overwrite the first entry (this is mostly used to reduce latency).
* BUFFER\_DEFERRED\_POP : do not consider the object to be fully popped from the buffer by calling the pop method until the call to pop_deferred ; this allows to handle object which are in-progress of being consummed by the thread.

This container is designed to be used for easy synchronization inter-threads 
(and the variable shall be a global shared one).

It shall be done once per type and per compilation unit.

The object oplist is expected to have the following operators (INIT, INIT\_SET, SET, INIT\_MOVE and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

Example:

	BUFFER_DEF(buffer_uint, unsigned int, 10, BUFFER_QUEUE|BUFFER_BLOCKING)

	buffer_uint_t g_buff;

	void f(unsigned int i) {
		buffer_uint_init(g_buff, 10);
		buffer_uint_push(g_buff, i);
		buffer_uint_pop(&i, g_buff);
		buffer_uint_clear(g_buff);
	}


#### Created methods

The following methods are automatically and properly created by the previous macros. In the following methods, name stands for the name given to the macro which is used to identify the type.

##### void name\_init(buffer\_t buffer, size\_t size)

Initialize the buffer 'buffer' to fill in at least 'size-1' elements.
The 'size' argument shall be the same as the one used to create the buffer
or the one used to create the buffer was '0'.
This function is not thread safe.

##### void name\_clear(buffer\_t buffer)

Clear the buffer and destroy all its allocations.
This function is not thread safe.

##### void name\_clean(buffer\_t buffer)

Clean the buffer making it empty but remain initialized.
This function is thread safe if the buffer was built thread safe.

##### bool name\_empty\_p(const buffer\_t buffer)

Return true if the buffer is empty, false otherwise.
This function is thread safe if the buffer was built thread safe. 

##### bool name\_full\_p(const buffer\_t buffer)

Return true if the buffer is full, false otherwise.
This function is thread safe if the buffer was built thread safe. 

##### size\_t name\_size(const buffer\_t buffer)

Return the number of elements in the buffer which can be enqueued.
This function is thread safe if the buffer was built thread safe. 

##### size\_t name\_overwrite(const buffer\_t buffer)

If the buffer is built with the BUFFER\_PUSH\_OVERWRITE option,
this function returns the number of elements which have been overwritten
and thus discarded.
If the buffer was not built with the BUFFER\_PUSH\_OVERWRITE option,
it returns 0.

##### bool name\_push(buffer\_t buffer, const type data)

Push the object 'data' in the buffer 'buffer'.
It waits for any empty room to come if the buffer was built as blocking.
Returns true if the data was pushed, false otherwise.
Always return true if the buffer is blocking.
This function is thread safe if the buffer was built thread safe. 

##### bool name\_pop(type *data, buffer\_t buffer)

Pop from the buffer 'buffer' into the object pointed by 'data'.
It waits for any data to come if the buffer was built as blocking.
If the buffer is built with the BUFFER\_PUSH\_INIT\_POP\_MOVE option,
the object pointed by 'data' shall be uninitialized
(the pop function will perform a quick initialization of the object
using an INIT_MOVE operator)
, otherwise it shall be an initialized object (the pop function will 
perform a SET operator).
Returns true if a data was popped, false otherwise.
Always return true if the buffer is blocking.
This function is thread safe if the buffer was built thread safe. 

##### bool name\_push\_blocking(buffer\_t buffer, const type data, bool blocking)

Same as name\_push except that the blocking policity is decided by the 'blocking' parameter.

##### bool name\_pop\_blocking(buffer\_t buffer, const type data, bool blocking)

Same as name\_pop except that the blocking policity is decided by the 'blocking' parameter.

TODO: Describe QUEUE\_MPMC\_DEF

TODO: Describe QUEUE\_SPSC\_DEF


### M-SNAPSHOT

This header is for created snapshots.

A snapshot is a mechanism allowing a reader thread and a writer thread,
 **working at different speeds**, to exchange messages in a fast, reliable and thread safe way
(the message is always passed atomatically from a thread point of view) without waiting
for synchronization.
The consummer thread has only access to the latest published data of 
the producer thread.
This is implemented in a fast way as the writer directly writes the message in the buffer
which will be passed to the reader (avoiding copy of the buffer) and a simple exchange
of index is sufficient to handle the switch.

This container is designed to be used for easy synchronization inter-threads 
(and the variable shall be a global shared one).

This is linked to [shared atomic register](https://en.wikipedia.org/wiki/Shared_register) in the litterature 
and [snapshot](https://en.wikipedia.org/wiki/Shared_snapshot_objects) names vector of such registers
where each element of the vector can be updated separetly. They can be classified according to the
number of producers/consummers:
SPSC (Single Producer, Single Consummer),
MPSC (Multiple Producer, Single Consummer),
SPMC (Single Producer, Multiple Consummer),
MPMC (Multiple Producer, Multiple Consummer),

The provided containers by the library are designed to handle huge
structure efficiently and as such deal with the memory reclamation needed to handle them.
If the data you are sharing are supported by the atomic header (like bool or integer), 
using atomic_load and atomic_store is a much more efficient and simple way to do
even in the case of MPMC.


#### SNAPSHOT\_SPSC\_DEF(name, type[, oplist])

Define the snapshot 'name##\_t' and its associated methods as "static inline" functions.
Only a single reader thread and a single writer thread are supported.
It is a SPSC atomic shared register.
In practice, it is implemented using a triple buffer (lock-free).

It shall be done once per type and per compilation unit. Not all functions are thread safe.

The object oplist is expected to have the following operators (INIT, INIT\_SET, SET and CLEAR), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object. It can have the optional methods INIT\_MOVE and MOVE.

Example:

	SNAPSHOT_DEF(snapshot_uint, unsigned int)
	snapshot_uint_t message;
	void f(unsigned int i) {
		unsigned *p = snapshot_uint_get_write_buffer(message);
		*p = i;
                snapshot_uint_write(message);
	}
	unsigned int g(void) {
		unsigned *p = snapshot_uint_read(message);
		return *p;
	}


#### Created methods

The following methods are automatically and properly created by the previous macros. In the following methods, name stands for the name given to the macro which is used to identify the type.

##### void name\_init(snapshot\_t snapshot)

Initialize the snapshot 'snapshot'.
This function is not thread safe.

##### void name\_clear(snapshot\_t snapshot)

Clear the snapshot and destroy all its allocations.
This function is not thread safe.

##### void name\_init\_set(snapshot\_t snapshot, const snapshot\_t org)

Initialize the snapshot 'snapshot' from the state of 'org'.
This function is not thread safe.

##### void name\_set(snapshot\_t snapshot, const snapshot\_t org)

Set the snapshot 'snapshot' from the state of 'org'.
This function is not thread safe.

##### void name\_init\_move(snapshot\_t snapshot, snapshot\_t org)

Move the contain of the snapshot 'org' to the uninitialized 'snapshot',
clearing 'org' in the process.
This function is not thread safe.
This function is defined only if the underlying type has defined the
INIT\_MOVE operator.

##### void name\_move(snapshot\_t snapshot, snapshot\_t org)

Move the contain of the snapshot 'org' to the initialized 'snapshot',
clearing 'org' in the process.
This function is not thread safe.
This function is defined only if the underlying type has defined the
MOVE operator.

##### type *name\_write(snapshot\_t snap)

Publish the 'in-progress' data of the snapshot 'snap so that the read
thread can have access to the data.
It returns the pointer to the new 'in-progress' data buffer 
of the snapshot (which is not yet published but will be pusblished 
for the next call of name\_write).
This function is thread-safe and performs atomic operation on the snapshot.

##### type *name\_read(snapshot\_t snap)

Get access to the last published data of the snapshot 'snap'.
It returns the pointer to the data.
If a publication has been performed since the last call to name\_read
a new pointer to the data is returned. 
Otherwise the previous pointer is returned.
This function is thread-safe and performs atomic operation on the snapshot.

##### bool name\_updated\_p(snapshot\_t snap)

Return true if the buffer has updated data compared to the last time
it was read.
This function is thread-safe and performs atomic operation on the snapshot.

##### type *name\_get\_write\_buffer(snapshot\_t snap)

Return a pointer to the active 'in-progress' data of the snapshot 'snap'.
It is the same as the last return from name\_write.
This function is thread-safe and performs atomic operation on the snapshot.

##### type *name\_get\_read\_buffer(snapshot\_t snap)

Return a pointer to the active published data of the snapshot 'snap'.
It is the same as the last return from name\_read.
It doesn't perform any switch of the data which has to be read.
This function is thread-safe and performs atomic operation on the snapshot.

TODO: Document SPMC & MPMC snapshots


### M-SHARED

This header is for creating shared pointer.
A [shared pointer](https://en.wikipedia.org/wiki/Smart_pointer)
 is a smart pointer that retains shared ownership of an object.
Several shared pointers may own the same object, sharing ownership of an object. 


#### SHARED\_PTR\_DEF(name, type[, oplist])

Define the shared pointer 'name##\_t' and its associated methods as "static inline" functions.
A shared pointer is a mechanism to keep tracks of all users of an object
and performs an automatic destruction of the object whenever all users release
their need on this object.

The tracking of ownership is atomic and the destruction of the object is thread safe.

The object oplist is expected to have the following operators (CLEAR and DEL), otherwise default operators are used. If there is no given oplist, the default operators are also used. The created methods will use the operators to init, set and clear the contained object.

There are designed to work with buffers with policy BUFFER\_PUSH\_INIT\_POP\_MOVE
to send a shared pointer across multiple threads.

Example:

	SHARED_PTR_DEF(shared_mpz, mpz_t, (CLEAR(mpz_clear)))
	void f(void) {
		shared_mpz_t p;
		mpz_t z;
		mpz_init(z);
		shared_mpz_init2 (p, z);
		buffer_uint_push(g_buff1, p);
		buffer_uint_push(g_buff2, p);
		shared_mpz_clear(p);
	}


#### Created methods

The following methods are automatically and properly created by the previous macros. In the following methods, name stands for the name given to the macro which is used to identify the type.

##### void name\_init(shared\_t shared)

Initialize the shared pointer 'shared' to NULL (no object is pointed).
This function is not thread safe.

##### void name\_init2(shared\_t shared, type *data)

Initialize the shared pointer 'shared' to 'data'.
User code shall not use 'data' anymore.
This function is not thread safe.

##### void name\_init\_new(shared\_t shared)

Initialize the shared pointer 'shared' to a new object of type 'type'.
The default constructor of type is used to initialize the objet.
This function is not thread safe.

##### void name\_init\_set(shared\_t shared, const shared\_t src)

Initialize the shared pointer 'shared' to the same object than the one
pointed by 'src'.
This function is thread safe from 'src' point of view.

##### bool name\_NULL\_p(const shared\_t shared)

Return true if shared doesn't point to any object.

##### void name\_clear(shared\_t shared)

Clear the shared pointer, destroying the shared object if no longer
any other shared pointers point to the object.
This function is thread safe.

##### void name\_clean(shared\_t shared)

Make the shared pointer points to no object anylonger,
destroying the shared object if no longer
any shared pointers point to the object.
This function is thread safe.

##### void name\_set(shared\_t shared, const shared\_t src)

Destroy the shared object pointed by 'shared' if no longer any other shared
pointers point to it, set the shared pointer 'shared' to the same object 
than the one pointed by 'src'.
This function is thread safe.

##### void name\_init\_move(shared\_t shared, shared\_t src)

Move the shared pointer from the initialized 'src' to 'shared'.

##### void name\_init\_move(shared\_t shared, shared\_t src)

Move the shared pointer from the initialized 'src' to 'shared',
clearing first the shared object pointed by 'src' if needed.

##### void name\_swap(shared\_t shared1, shared\_t shared2)

Swap the shared pointer.

##### bool name\_equal\_p(const shared\_t shared1, const shared\_t shared2)

Return true if both shared pointers point to the same object.

##### const type *name\_cref(const shared\_t shared)

Return a constant pointer to the shared object pointed by the shared pointer.
Keeping the pointer whereas the shared pointer is destroyed is undefined
behavior.

##### type *name\_ref(const shared\_t shared)

Return a pointer to the shared object pointed by the shared pointer.
Keeping the pointer whereas the shared pointer is destroyed is undefined
behavior.

TODO: Document shared ressource

### M-I-SHARED

This header is for creating intrusive shared pointer.

#### ISHARED\_INTERFACE(name, type)

Extend the definition of the structure of an object of type 'type' by adding the
necessary interface to handle it as a shared pointer named 'name'.
It shall be put within the structure definition of the object (See example).

#### ISHARED\_PTR\_DEF(name, type[, oplist])

Define the associated methods to handle the shared pointer named 'name'
as "static inline" functions.
A shared pointer is a mechanism to keep tracks of all users of an object
and performs an automatic destruction of the object whenever all users release
their need on this object.

The destruction of the object is thread safe and to occure when all users
of the object release it. The last user which releases it is the one which
performs the destruction of the object. The destruction of the object implies:

* calling the CLEAR operator to clear the object,
* calling the DEL operator to release the memory used by the object 
(if the method has not been disabled).

The object oplist is expected to have the following operators (CLEAR and DEL),
otherwise default operators are used. If there is no given oplist, the default
operators are also used. The created methods will use the operators to init, set
and clear the contained object.

There are designed to work with buffers with policy BUFFER\_PUSH\_INIT\_POP\_MOVE
to send a shared pointer across multiple threads.

It is recommended to use the intrusive shared pointer over the standard one if
possible (They are faster & cleaner).

Example:

        typedef struct mystruct_s {
                ISHARED_PTR_INTERFACE(ishared_mystruct, struct mystruct_s);
                char *message;
        } mystruct_t;

        static inline void mystruct_clear(mystruct_t *p) { free(p->message); }

        ISHARED_PTR_DEF(ishared_mystruct, mystruct_t, (CLEAR(mystruct_clear M_IPTR)))

        void f(void) {
                mystruct_t *p = ishared_mystruct_new();
                p->message = strdup ("Hello");
                buffer_mystruct_push(g_buff1, p);
                buffer_mystruct_push(g_buff2, p);
                ishared_mystruct_clear(p);
        }


#### Created methods

The following methods are automatically and properly created by the previous macros. In the following methods, name stands for the name given to the macro which is used to identify the type.

##### typedef type *name_t

It will define name_t as a pointer to shared counted object.
This is a synonymous to a pointer to the object.

##### name_t name\_init(type *object)

Return a shared pointer to 'object' with one user counter.
The shared pointer part of 'object' shall not have been initialized.
This function is not thread safe.

##### name_t name\_init\_set(name_t shared)

Return a new shared pointer to the same object than the one pointed by 'shared',
incrementing the user counter to it.
This function is thread safe.

##### void name\_init\_set2(name_t *ptr, name_t shared)

Set '*ptr' to a new shared pointer to 'shared', 
incrementing the user counter to the object pointed by 'shared'.
This function is thread safe (providing the ptr address is local to a thread).

##### name_t name\_init\_new(void)

Allocate a new object, initialize it and return an initialized shared pointer to it.
This function is thread safe if the allocator and the initialize function is.

The used allocation function is the ALLOC operator.
In this case, it is assumed that the DEL operator has not been disabled.


##### void name\_clear(name_t shared)

Clear the shared pointer, destroying the shared object if no longer
any other shared pointers point to the object.
This function is thread safe.

##### void name\_set(name_t *shared1, name_t shared2)

Update the shared pointer '*shared1' to point to the same object than
the shared pointer 'shared2'.
Destroy the shared object pointed by '*shared1' if no longer any other shared
pointers point to it, set the shared pointer 'shared' to the same object 
than the one pointed by 'src'.
This function is thread safe.



### M-I-LIST

This header is for creating intrusive dual-chained list.

#### ILIST\_INTERFACE(name, type)

Extend an object by adding the necessary interface to handle it within 
a dual linked intrusive list.
This is the intrusive part.
It shall be put within the structure of the object to link, at the top
level of the structure.
See example of ILIST\_DEF.

#### ILIST\_DEF(name, type[, oplist])

Define the dual linked list 
and define the associated methods to handle it as "static inline" functions.
'name' shall be a C identifier which will be used to identify the list. It will be used to create all the types and functions to handle the container.
It shall be done once per type and per compilation unit.

An object is expected to be part of only one list of a kind in the entire program at a time.
An intrusive list allows to move from an object to the next object without needing to go through the entire list,
or to remove an object from a list in O(1).
It may, or may not, be better than standard list. It depends on the context.

The object oplist is expected to have the default operators. If there is no given oplist, the default values for the operators are used. The created methods will use the operators to init, set and clear the contained object.

The given interface won't allocate anything to handle the objects as
all allocations and initialization are let to the user.

However the objects within the list can be automaticly be destroyed
by calling the CLEAR method to destruct the object and the DEL
method to free the used memory (only if the FREE operator is defined in the
oplist).
If there is no FREE operator, it is up to the user to free the used memory.
The default CLEAR operator will do nothing on the object.

Example:

	typedef struct test_s {
	  int n;
	  ILIST_INTERFACE (ilist_tname, struct test_s);
	} test_t;

	ILIST_DEF(ilist_tname, test_t)

	void f(void) {
		test_t x1, x2, x3;
		ilist_tname_t list;

		x1.n = 1;
		x2.n = 2;
		x3.n = 3;

		ilist_tname_init(list);
		ilist_tname_push_back (list, &x3);
		ilist_tname_push_front (list, &x1);
		ilist_tname_push_after (&x1, &x2);

		int n = 1;
		for M_EACH(item, list, ILIST_OPLIST(ilist_tname)) {
			assert (n == item->n);
			n++;
		}
		ilist_tname_clear(list);
	}


#### Created methods

The following methods are automatically and properly created by the previous macros. In the following methods, name stands for the name given to the macro which is used to identify the type.

#### name\_t

Type of the list of 'type'.

#### name\_it\_t

Type of an iterator over this list.

The following methods are automatically and properly created by the previous macro.

##### void name\_init(name\_t list)

Initialize the list 'list' (aka constructor) to an empty list.

##### void name\_init\_field(type *obj)

Initialize the additional fields of the object '*obj'.

##### void name\_clear(name\_t list)

Clear the list 'list (aka destructor). The list can't be used anymore, except with a constructor.
If the DEL operator is available in the oplist of the type,
the cleared object will also be deleted.

##### void name\_clean(name\_t list)

Clean the list (the list becomes empty). The list remains initialized but is empty.
If the DEL operator is available in the oplist of the type,
the cleared object will also be deleted.

##### type *name\_back(const name\_t list)

Return a constant pointer to the data stored in the back of the list.

##### type *name\_front(const name\_t list)

Return a constant pointer to the data stored in the front of the list.

##### void name\_push\_back(name\_t list, type *obj)

Push the object '*obj' itself at the back of the list 'list'.

##### void name\_push\_front(name\_t list, type *obj)

Push the object '*obj' itself at the front of the list 'list'.

##### void name\_push\_after(type *position, type *obj)

Push the object '*obj' after the object '*position'.

##### type *name\_pop\_back(name\_t list)

Pop the object from the back of the list 'list'
and return a pointer to the popped object.

##### type *name\_pop\_front(name\_t list)

Pop the object from the front of the list 'list'
and return a pointer to the popped object.

##### bool name\_empty\_p(const name\_t list)

Return true if the list is empty, false otherwise.

##### void name\_swap(name\_t list1, name\_t list2)

Swap the list 'list1' and 'list2'.

##### void name\_unlink(type *obj)

Remove the object '*obj' from the list.

##### type *name\_next\_obj(const name\_t list, const type *obj)

Return the object which is after the object '*obj' in the list
or NULL if there is no more object.

##### type *name\_previous\_obj(const name\_t list, const type *obj)

Return the object which is before the object '*obj' in the list
or NULL if there is no more object.

##### void name\_it(name\_it\_t it, name\_t list)

Set the iterator 'it' to the back(=first) element of 'list'.
There is no destructor associated to this initialization.

##### void name\_it\_set(name\_it\_t it, const name\_it\_t ref)

Set the iterator 'it' to the iterator 'ref'.
There is no destructor associated to this initialization.

##### void name\_it\_last(name\_it\_t it, name\_t list)

Set the iterator 'it' to the last element of the list.
There is no destructor associated to this initialization.

##### void name\_it\_end(name\_it\_t it, name\_t list)

Set the iterator 'it' to the end of the list (i.e. not a valid element).
There is no destructor associated to this initialization.

##### bool name\_end\_p(const name\_it\_t it)

Return true if the iterator doesn't reference a valid element anymore.

##### bool name\_last\_p(const name\_it\_t it)

Return true if the iterator references the last element or if the iterator doesn't reference a valid element anymore.

##### bool name\_it\_equal\_p(const name\_it\_t it1, const name\_it\_t it2)

Return true if the iterator it1 references the same element than it2.

##### void name\_next(name\_it\_t it)

Move the iterator 'it' to the next element of the list.

##### void name\_previous(name\_it\_t it)

Move the iterator 'it' to the previous element of the list.

##### type *name\_ref(name\_it\_t it)

Return a pointer to the element pointed by the iterator.
This pointer remains valid until the list is modified by another method.

##### const type *name\_cref(const name\_it\_t it)

Return a constant pointer to the element pointed by the iterator.
This pointer remains valid until the list is modified by another method.

##### size\_t name\_size(const name\_t list)

Return the number elements of the list (aka size). Return 0 if there no element.

##### void name\_insert(name\_t list, name\_it\_t it, type x)

Insert a copy of 'x' after the position pointed by 'it' 
(which is an iterator of the list 'list') or if 'it' doesn't point anymore to a valid element of the list, it is added as the back (=first) element of the 'list'
This service is available only if a NEW operator is available for the type.

##### void name\_remove(name\_t list, name\_it\_t it)

Remove the element 'it' from the list 'list'.
After wise, 'it' points to the next element of the list.

##### void name\_splice\_back(name\_t list1, name\_t list2, name\_it\_t it)

Move the element pointed by 'it' (which is an iterator of 'list2') from the list 'list2' to the back position of 'list1'.
After wise, 'it' points to the next element of 'list2'.

##### void name\_splice(name\_t list1, name\_t list2)

Move all the element of 'list2' into 'list1", moving the last element
of 'list2' after the first element of 'list1'.
After-wise, 'list2' is emptied.


### M-BITSET

This header is for using bitset.

A [bitset](https://en.wikipedia.org/wiki/Bit_array) can be seen as a specialized version of an array of bool, where each item takes only 1 bit.
It allows for compact representation of such array.

Example:

	void f(void) {
		bitset_t set;
		bitset_init(set);
		for(int i = 0; i < 100; i ++)
			bitset_push_back(set, i%2);
		bitset_clear(set);
	}


#### methods

The methods are mostly the same than for an array. The following methods are available:

TODO: document the API.

### M-STRING

This header is for using dynamic [string](https://en.wikipedia.org/wiki/String_(computer_science)).
The size of the string is automatically updated in function of the needs.

Example:

	void f(void) {
		string_t s1;
		string_init (s1);
		string_set_str (s1, "Hello, world!");
		string_clear(s1);
	}

#### methods

The following methods are available:

##### string\_t

This type defines a dynamic string and is the primary type of the module.
The provided methods are just handy wrappers to the C library.
It only provides few algorithms on its own.
(Typically replacement is not designed to be fast on huge strings).

##### STRING_FAILURE

Constant Macro defined as the index value returned in case of error.
(equal as -1U).

##### string\_fgets\_t

This type defines the different enumerate value for the fgets function:

* STRING_READ_LINE: read a full line until the EOL character (included),
* STRING_READ_PURE_LINE: read a full line until the EOL character (excluded),
* STRING_READ_FILE: read the full file.

##### void string\_init(string\_t str)

Init the string 'str' to an empty string.

##### void string\_clear(string\_t str)

Clear the string 'str' and frees any allocated memory.

##### char *string\_clear\_get\_str(string\_t v)

Clear the string 'str' and returns the allocated array of char,
representing a C string, giving ownership of the array to the caller.
This array will have to be freed. It can return NULL if no array
was allocated by the string.

##### void string\_clean(string\_t str)

Set the string 'str' to an empty string.

##### size\_t string\_size(const string\_t str)

Return the size in bytes of the string.
It can be also the number of characters of the string
if the encoding type is one character per byte.
If the character are encoded as UTF8, the function string\_length\_u is prefered.

##### size\_t string\_capacity(const string\_t str)

Return the capacity in bytes of the string.
The capacity is the number of bytes the string accept before a
reallocation of the underlying array of char has to be performed.

##### char string\_get\_char(const string\_t v, size\_t index)

Return the byte 'index' of the string 'v'.
'index' shall be within the allowed range of bytes of the string.

##### bool string\_empty\_p(const string\_t v)

Return true if the string is empty, false otherwise.

##### void string\_reserve(string\_t v, size\_t alloc)

Update the capacity of the string to be able to receive at least
'alloc' bytes.
Calling with 'alloc' lower or equal than the size of the string
allows the function to perform a shrink
of the string to its exact needs. If the string is
empty, it will free the memory.

##### void string\_set\_str(string\_t v, const char str[])

Set the string to the array of char 'str'. 'str' is supposed
to be 0 terminated as any C string.

##### void string\_set\_strn(string\_t v, const char str[], size\_t n)

Set the string to the array of char 'str' by copying at most 'n'
char from the array.'str' is supposed
to be 0 terminated as any C string.

##### const char* string\_get\_cstr(const string\_t v)

Return a constant pointer to the underlying array of char of the string.
This array of char is terminated by 0, allowing the pointer to be passed
to standard C function.

##### void string\_set (string\_t v1, const string\_t v2)

Set the string 'v1' to the value of the string 'v2'.

##### void string\_set\_n(string\_t v, const string\_t ref, size\_t offset, size\_t length)

Set the string to the value of the string 'ref' by skipping the first 'offset'
char of the array of char of 'ref' and copying at most 'length' char
in the remaining array of characters of 'ref'.
'offset' shall be within the range of index of the string 'ref'.
'ref' and 'v' cannot be the same string.

##### void string\_init\_set(string\_t v1, const string\_t v2)

Initialize 'v1' to the value of the string 'v2'.

##### void string\_init\_set\_str(string\_t v1, const char str[])

Initialize 'v1' to the value of the array of char 'str'.
The array of char shall be terminated with 0.

##### void string\_init\_move(string\_t v1, string\_t v2)

Initialize 'v1' by stealing as most ressource from 'v2' as possible
and clear 'v2' afterwise.

##### void string\_move(string\_t v1, string\_t v2)

Set 'v1' by stealing as most ressource from 'v2' as possible
and clear 'v2' afterwise.

##### void string\_swap(string\_t v1, string\_t v2)

Swap the content of both strings.

##### void string\_push\_back (string\_t v, char c)

Append the string with the character 'c'

##### void string\_cat\_str(string\_t v, const char str[])

Append the string with the array of char 'str'.
The array of char shall be terminated with 0.

##### void string\_cat(string\_t v, const string\_t v2)

Apprend the string with the string 'v2'.

##### int string\_cmp\_str(const string\_t v1, const char str[])

Perform a byte comparaison of the string and the array of char
by using the strcmp function and return the result of this comparison.

##### int string\_cmp(const string\_t v1, const string\_t v2)

Perform a byte comparaison of both string
by using the strcmp function and return the result of this comparison.

##### bool string\_equal\_str\_p(const string\_t v1, const char str[])

Return true if the string is equal to the array of char, false otherwise.

##### bool string\_equal\_p(const string\_t v1, const string\_t v2)

Return true if both strings are equal, false otherwise.

##### int string\_cmpi\_str(const string\_t v1, const char p2[])

This function compares the string and the array of char
by ignoring the difference due to the casse.
This function doesn't work with UTF-8 strings.
It returns a negative integer if the string is before the array,
0 if there are equal,
a positive integer if the string is after the array.

##### int string\_cmpi(const string\_t v1, const string\_t v2)

This function compares both strings by ignoring the difference due to the casse.
This function doesn't work with UTF-8 strings.
It returns a negative integer if the string is before the array,
0 if there are equal,
a positive integer if the string is after the array.

##### size\_t string\_search\_char (const string\_t v, char c [, size\_t start])

Search for the character 'c' in the string from the offset 'start'.
'start' shall be within the valid ranges of offset of the string.
'start' is an optionnal argument. If it is not present, the default
value 0 is used instead.
This doesn't work if the function is used as function pointer.
Return the offset of the string where the character is first found,
or STRING_FAILURE otherwise.

##### size\_t string\_search\_rchar (const string\_t v, char c [, size\_t start])

Search backwards for the character 'c' in the string from the offset 'start'.
'start' shall be within the valid ranges of offset of the string.
'start' is an optionnal argument. If it is not present, the default
value 0 is used instead.
This doesn't work if the function is used as function pointer.
Return the offset of the string where the character is last found,
or STRING_FAILURE otherwise.

##### size\_t string\_search\_str (const string\_t v, char str[] [, size\_t start])

Search for the array of char 'str' in the string from the offset 'start'.
'start' shall be within the valid ranges of offset of the string.
'start' is an optionnal argument. If it is not present, the default
value 0 is used instead.
This doesn't work if the function is used as function pointer.
Return the offset of the string where the array of char is first found,
or STRING_FAILURE otherwise.

##### size\_t string\_search (const string\_t v, string\_t str [, size\_t start])

Search for the string 'str' in the string from the offset 'start'.
'start' shall be within the valid ranges of offset of the string.
'start' is an optionnal argument. If it is not present, the default
value 0 is used instead.
This doesn't work if the function is used as function pointer.
Return the offset of the string where 'str' is first found,
or STRING_FAILURE otherwise.

##### size\_t string\_pbrk(const string_t v, const char first\_of[] [, size_t start])

Search for the first occurrence in the string 'v' from the offset 'start' of
any of the bytes in the string 'first\_of'.
'start' shall be within the valid ranges of offset of the string.
'start' is an optionnal argument. If it is not present, the default
value 0 is used instead.
This doesn't work if the function is used as function pointer.
Return the offset of the string where 'str' is first found,
or STRING_FAILURE otherwise.

##### int string\_strcoll\_str(const string\_t str1, const char str2[])
##### int string\_strcoll(const string\_t str1, const string\_t str2[])

Compare the two strings str1 and str2.
It returns an integer less than, equal to, or greater than zero if  s1  is  found,
respectively,  to  be  less than, to match, or be greater than s2. The
comparison is based on strings interpreted as appropriate for the program's
current locale.

##### size\_t string\_spn(const string\_t v1, const char accept[])

Calculate the length (in bytes) of the initial
segment of the string which consists entirely of bytes in accept.
       
##### size\_t string\_cspn(const string\_t v1, const char reject[])

Calculate the length (in bytes) of the initial
segment of the string which consists entirely of bytes not in reject.
       
##### void string\_left(string\_t v, size\_t index)

Keep at most the 'index' left bytes of the string,
terminating the string at the given index.
index can be out of range.

##### void string\_right(string\_t v, size\_t index)

Keep the right part of the string, after the index 'index'.

##### void string\_mid (string\_t v, size\_t index, size\_t size)

Extract the medium string from offset 'index' and up to 'size' bytes.

##### size\_t string\_replace\_str (string\_t v, const char str1[], const char str2[] [, size_t start])
##### size\_t string\_replace (string\_t v, const string\_t str1, const string\_t str2 [ , size_t start])

Replace in the string 'v' from the offset start
the string str1 by the string str2 once.
Returns the offset of the replacement or STRING_FAILURE if no replacement
was performed.

##### void string\_replace\_at (string\_t v, size\_t pos, size\_t len, const char str2[])

Replace in the string 'v' the sub-string defined as starting from 'pos' and
of size 'len' by the string str2.
It assumes that pos+len is within the range.

##### int string\_printf (string\_t v, const char format[], ...)

Set the string 'v' to the formatted string format.
'format' follows the printf function family.

##### int string\_cat\_tprintf (string\_t v, const char format[], ...)

Appends to the string 'v' the formatted string format.
'format' follows the printf function family.

##### bool string\_fgets(string\_t v, FILE \*f, string\_fgets\_t arg)

Read from the opened file 'f' a stream of characters and set 'v'
with this stream.
It stops after the character end of line
if arg is STRING\_READ\_PURE\_LINE or STRING\_READ\_LINE,
and until the end of the file if arg is STRING\_READ\_FILE.
If arf is STRING\_READ\_PURE\_LINE, the character end of line
is removed from the string.
Return true if something has been read, false otherwise.

##### bool string\_fget\_word (string\_t v, const char separator[], FILE \*f)

Read a word from the file 'f' and set 'v' with this word.
A word is separated from another by the list of characters in the array 'separator'.
(Example: "^ \t.\n").
It is highly recommended for separator to be a constant string.

##### void string\_fputs(FILE \*f, const string\_t v)

Put the string in the file.

##### bool string\_start\_with\_str\_p(const string\_t v, const char str[])
##### bool string\_start\_with\_string\_p(const string_t v, const string_t str)

Return true if the string start with the same characters than 'str',
false otherwise.

##### size\_t string\_hash(const string\_t v)

Return a hash of the string.

##### void string\_strim(string\_t v [, const char charTab[]])

Remove from the string any leading or trailing space-like characters
(space or tabulation or end of line).
If charTab is given, it get the list of characters to remove from
this argument.

##### void string\_get\_str(string\_t v, const string\_t v2, bool append)

Convert a string into a string usable for I/O:
Outputs the input string with quote around,
replacing any \" by \\\" within the string
into the output string.

##### void string\_out\_str(FILE *f, const string\_t v)

Write a string into a FILE:
Outputs the input string with quote around,
replacing any \" by \\\" within the string.

##### bool string\_in\_str(string\_t v, FILE *f)

Read a string from a FILE. The string shall have be written
by string\_out\_str.
It returns true if it has successfully parsed the string,
false otherwise. In this case, the position within the FILE
is undefined.

##### string\_unicode\_t

Define a type suitable to store a unicode character.

##### string\_it\_t

Define an iterator over the string, allowing to
iterate the string over UTF8 encoded character.

##### void string\_it(string\_it\_t it, const string\_t str)

Initialize the iterator 'it' to iterate over the string 'str'
over UTF8 encoded character.

##### bool string\_end\_p (string\_it\_t it)

Return true if the iterator has reached the end of the string,
false otherwise.

##### void string\_next (string\_it\_t it)

Move the iterator to the next UTF8 encoded character.
It is assumed that string\_end\_p has been called at least once
per UTF8 character before.

##### string\_unicode\_t string\_get_cref (const string\_it\_t it)

Return the unicode character associated to the UTF8 encoded character
pointer by the iterator.
It is assumed that string\_end\_p has been called at least once
per UTF8 character before.
It returns -1 in case of error in decoding the UTF8 string.

##### void string\_push\_u (string\_t str, string\_unicode\_t u)

Push the unicode character 'u' into the string 'str'
encoding it as a UTF8 encoded characters.

##### size\_t string\_length\_u(string\_t str)

Return the number of UTF8 encoded characters in the string.

##### bool string\_utf8\_p(string\_t str)

Return true if the string is a valid UTF8,
false otherwise.
It doesn't check for unique canonical form for UTF8 string,
so it may report 'true' whereas the string is not stricly conformant.

##### STRING\_CTE(string)

Macro to convert a constant array string into a temporary string\_t variable
suitable only for being called within a function.

##### STRING_OPLIST

The oplist of a string\_t

##### BOUNDED_STRING_DEF(name, size)

aka char[N+1]
TODO: Document the API.


### M-CORE

This header is the internal core of M\*LIB, providing a lot of functionality 
by extending a lot the preprocessing capability.
Working with these macros is not easy and the developer needs to know
how the macro preprocessing works.
It also adds the needed macro for handling the oplist.
As a consequence, it is needed by all other header files.

Some macros are using recursivity to work.
This is not an easy feat to do as it needs some tricks to work (see
reference).
This still work well with only one major limitation: it can not be chained.
For example, if MACRO is a macro implementing recursivity, then
MACRO(MACRO()) won't work.


Example:

	M_MAP(f, 1, 2, 3, 4)
	M_REDUCE(f, g, 1, 2, 3, 4)
	M_SEQ(1, 20, f)
	
#### Compiler Macros

The following compiler macros are available:

##### M\_ASSUME(cond)

M\_ASSUME is equivalent to assert, but gives hints to compiler
about how to optimize the code if NDEBUG is defined.

##### M\_LIKELY(cond) / M\_UNLIKELY(cond)

M\_LIKELY / M\_UNLIKELY gives hints on the compiler of the likehood
of the given condition.

#### Preprocessing macro extension

##### M\_MAX\_NB\_ARGUMENT

Maximum number of argument which can be handled by this header.

##### M\_C(a,b), M\_C3(a,b,c), M\_C4(a,b,c,d)

Return a symbol corresponding to the concatenation of the input arguments.

##### M\_INC(number)

Increment the number given as argument (from [0..52]) and return 
a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).
If number is not within the range, the behavior is undefined.

##### M\_DEC(number)

Decrement the number given as argument (from [0..52]) and return 
a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).
If number is not within the range, the behavior is undefined.

##### M\_ADD(x, y)

Return x+y (resolution is performed at preprocessing time).
x and y shall be within [0..52].

##### M\_SUB(x, y)

Return x-y (resolution is performed at preprocessing time).
x and y shall be within [0..52] and x >= y.

##### M\_RET\_ARG1(arglist,...) / M\_RET\_ARGN(...)

Return the argument 1 of the given arglist (respectively 2 and N, with which
is within [2..53]).
The argument shall exist in the arglist.

##### M\_SKIP(N,...)

Skip the Nth first arguments of the argument list.
N can be within [0..52].

##### M\_KEEP(N,...)

Keep the Nth first arguments of the argument list.
N can be within [0..52].

##### M\_MID(first, len,...)

Keep the medium arguments of the argument list,
starting from the 'first'-th one and up to 'len' arguments.
first and len can be within [0..52].

##### M\_BOOL(cond)

Convert an integer or a symbol into 0 (if 0) or 1 (if not 0 or symbol unknown).
Return a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).

##### M\_INV(cond)

Inverse 0 into 1 and 1 into 0. It is undefined if cond is not 0 or 1
(use M\_BOOL to convert). 
Return a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).

##### M\_AND(cond1, cond2)

Perform a logical 'and' between cond1 and cond2. 
cond1 and cond2 shall be 0 or 1 otherwise it is undefined
(You shall use M\_bool otherwise).
Return a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).

##### M\_OR(cond1, cond2)

Perform a logical 'or' between cond1 and cond2. 
cond1 and cond2 shall be 0 or 1 otherwise it is undefined.
(You shall use M\_bool otherwise).
Return a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).

##### M\_IF(cond)(action\_if\_true, action\_if\_false)

Return the pre-processing token 'action_if_true' if 'cond' is true, action\_if\_false otherwise (meaning it is evaluated
at macro processing stage, not at compiler stage).
cond shall be 0 or 1 otherwise it is undefined.

##### M\_COMMA\_P(arglist)

Return 1 if there is a comma inside the argument list, 0 otherwise.
Return a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).

##### M\_AS\_STR(expression)

Return the string representation of the evaluated expression.
NOTE: Neeed to be used with M\_APPLY to defer the evaluation.

##### M\_EMPTY\_P(expression)

Return 1 if the argument 'expression' is 'empty', 0 otherwise.
Return a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).

##### M\_DEFERRED\_COMMA

Return a comma ',' at a later phase of the macro processing steps.

##### M\_IF\_EMPTY(cond)(action\_if\_true, action\_if\_false)

Return the pre-processing token 'action_if_true' if 'cond' is empty, action\_if\_false otherwise (meaning it is evaluated
at macro processing stage, not at compiler stage).
cond shall be 0 or 1 otherwise it is undefined.

##### M\_PARENTHESIS\_P(expression)

Return 1 if the argument 'expression' starts a parenthesis and ends it
(like '(...)'), 0 otherwise.
Return a pre-processing token corresponding to this value (meaning it is evaluated
at macro processing stage, not at compiler stage).

##### M\_DELAY1(expr) / M\_DELAY2(expr) / M\_DELAY3(expr) / M\_DELAY4(expr) / M\_ID

Delay the evaluation by 1, 2, 3 or 4 steps.
This is necessary to write macros which are recursive.
The argument is a macro-function which has to be deferred.
M\_ID is an equivalent of M\_DELAY1.

##### M\_EVAL(expr)

Perform a complete stage evaluation of the given expression,
removing recursive expression within it.
Only ONE M\_EVAL expression is expected in the evaluation chain.
Can not be chained.

##### M\_APPLY(func, args...)

Apply 'func' to '(args...) ensuring
that a() isn't evaluated until all 'args' have been also evaluated.
It is used to delay evaluation.

##### M\_MAP(func, args...)

Apply 'func' to each argument of the 'args...' list of argument.

##### M\_MAP\_C(func, args...)

Apply 'func' to each argument of the 'args...' list of argument,
putting a comma between each expanded 'func(argc)'

##### M\_MAP2(func, data, args...)

Apply 'func' to each couple '(data, argument)' 
with argument an element of the 'args...' list.

##### M\_MAP2\_c(func, data, args...)

Apply 'func' to each couple '(data, argument)' 
with argument an element of the 'args...' list,
putting a comma between each expanded 'func(argc)'

##### M\_MAP\_PAIR(func, args...)

Map a macro to all given pair of arguments (Using recursivity).
Can not be chained.

##### M\_REDUCE(funcMap, funcReduce, args...)

Map the macro funcMap to all given arguments 'args'
and reduce all theses computation with the macro 'funcReduce'.
Example: M_REDUCE(f, g, a, b, c) ==> g( f(a), g( f(b), f(c))

##### M\_REDUCE2(funcMap, funcReduce, data, args...)

Map the macro funcMap to all pair (data, arg) of the given argument list 'args' 
and reduce all theses computation with the macro 'funcReduce'.
Do not use recursivity.

##### M\_SEQ(init, end, macro, data)

Generate a sequence of number from 'init' to 'end'
and apply to the macro the pair '(data, num)' for each number 'num'.

##### M\_EAT(...)

Globber the input, whatever it is.

##### M\_NARGS(args...)

Return the number of argument of the given list.
Doesn't work for empty argument.

##### M\_IF\_NARGS\_EQ1(argslist)(action\_if\_one\_arg, action\_otherwise)

Return the pre-processing token 'action\_if\_one\_arg' if 'argslist' has only one argument, action\_otherwise otherwise
(meaning it is evaluated at macro processing stage, not at compiler stage).

##### M\_IF\_NARGS\_EQ2(argslist)(action\_if\_two\_arg, action\_otherwise)

Return the pre-processing token 'action\_if\_two\_arg' if 'argslist' has two arguments, action\_otherwise otherwise
(meaning it is evaluated at macro processing stage, not at compiler stage).

##### M\_IF\_DEBUG(action)

Return the pre-processing token 'action' if the build is compiled in debug mode (i.e. NDEBUG is undefined).

##### M\_IF\_DEFAULT1(default_value, argumentlist)

Helper macro to redefine a function with a default value.
If there is only one variable as the argument list, print
the variable of the argument list then ', value',
instead only print the argument list (and so two arguments).
Example:
    int f(int a, int b);
    #define f(...) M_APPLY(f, M_IF_DEFAULT1(0, __VA_ARGS__))
This need to be called within a M_APPLY macro.
   
##### M\_DEFAULT\_ARGS(nbExpectedArg, (defaultArgumentlist), argumentList )

Helper macro to redefine a function with one or more default values.
defaultArgumentlist is a list of the default value to complete the
list argumentList to reach the number nbExpectedArg arguments.
Example:
    int f(int a, int b, long p, void *q);
    #define f(...) f(M_DEFAULT_ARGS(4, (0, 1, NULL), __VA_ARGS__))
The last 3 arguments have their default value as 0 (for b),
1 (for p) and NULL (for q).

##### M\_NOTEQUAL(x, y)

Return 1 if x != y, 0 otherwise (resolution is performed at preprocessing time).
x and y shall be within the maximum argument value.

##### M\_EQUAL(x, y)

Return 1 if x == y, 0 otherwise (resolution is performed at preprocessing time).
x and y shall be within the maximum argument value.

##### M\_INVERT(args...)

Reverse the argument list. For example, if args was a,b,c, it return c,b,a.

### C11 Macro

Theses macros are only valid if the program is built in C11 mode:

##### M\_PRINTF\_FORMAT(x)

Return the printf format associated to the type of 'x'.
'x' shall be printable with printf.

##### M\_PRINT\_ARG(x)

Print using printf the variable 'x'. The format of 'x' is deduced.

##### M\_FPRINT\_ARG(file, x)

Print into a file 'file' using fprintf the variable 'x'. The format of 'x' is deduced.

##### M\_PRINT(args...)

Print using printf all the variable in 'args'. The format of the arguments are deduced.

##### M\_FPRINT(file, args...)

Print into a file 'file' using fprintf all the variables in 'args'. The format of 'x' is deduced.

##### M\_AS\_TYPE(type, x)

Within a C11 \_Generic statement, all expressions shall be valid C
expression even if the case if always false, and is not executed.
This can seriously limit the _Generic statement.
This macro overcomes this limitation by returning :

* either the input 'x' if it is of type 'type',
* or the value 0 view as a type 'type'.

So the returned value is **always** of type 'type' and is safe in a \_Generic statement.


### C Macro

Theses macros expand their code at compilation level:

##### M\_MIN(x, y)
 
Return the minimum of 'x' and 'y'. x and y shall not have any side effect.

##### M\_MAX(x, y)
 
Return the maximum of 'x' and 'y'. x and y shall not have any side effect.

##### M\_POWEROF2\_P(n)
 
Return true if 'n' is a power of 2. n shall not have any side effect.

##### M\_SWAP(type, a, b)
 
Swap the values of 'a' and 'b'. 'a' and 'b' are of type 'type'. a and b shall not have any side effect.

##### M\_ASSIGN\_CAST(type, a)
 
Check if 'a' can be assigned to a temporary of type 'type' and returns this temporary.
If it cannot, the compilation failed.

##### M\_CONST\_CAST(type, a)
 
Check if 'a' can be properly casted to (const type *) and returns this casted pointer if possible.
If it cannot, the compilation failed.

##### M\_TYPE\_FROM\_FIELD(type, ptr, fieldType, field)
 
Assuming 'ptr' is a pointer to a fieldType object which is stored within a structure of type 'type'
at the position 'field', it returns a pointer to the structure.

#### HASH Functions

##### M\_HASH\_SEED --> size\_t

User shall overwrite this macro by a random seed (of type size_t) before including
the header m-core.h o that all hash functions will use this variable
as a seed for the hash functions. If no user macro is defined,
the default is to expand it to 0,
making all hash computations predictible.

##### M\_HASH\_DECL(hash)

Declare and initialize a new hash computation, named 'hash' which
is an integer.

##### M\_HASH\_UP(hash, value)

Update the 'hash' variable with the given 'value'
by incorporating the 'value' within the 'hash'. 'value' can be up to a 'size_t' 
variable.

##### uint32_t m_core_rotl32a (uint32_t x, uint32_t n)
##### uint64_t m_core_rotl64a (uint64_t x, uint32_t n)

Perform a rotation of 'n' bits of the input 'x'.
n shall be within 1 and the number of bits of the type minus 1.

##### uint64_t m_core_roundpow2(uint64_t v)

Round to the next highest power of 2.

##### unsigned int m_core_clz32(uint32\_t limb)
##### unsigned int m_core_clz64(uint64\_t limb)

Count the number of leading zero and return it.
limb can be 0.

##### size\_t m\_core\_hash (const void *str, size\_t length)

Compute the hash of the binary representation of the data pointer by 'str'
of length 'length'. 'str' shall have the same alignement restriction
than a 'size\_t'.

#### OPERATORS Functions

##### M\_GET\_INIT oplist
##### M\_GET\_INIT\_SET oplist
##### M\_GET\_INIT\_MOVE oplist
##### M\_GET\_SET oplist
##### M\_GET\_MOVE oplist
##### M\_GET\_SWAP oplist
##### M\_GET\_CLEAR oplist
##### M\_GET\_NEW oplist
##### M\_GET\_DEL oplist
##### M\_GET\_REALLOC oplist
##### M\_GET\_FREE oplist
##### M\_GET\_MEMPOOL oplist
##### M\_GET\_MEMPOOL\_LINKAGE oplist
##### M\_GET\_HASH oplist
##### M\_GET\_EQUAL oplist
##### M\_GET\_CMP oplist
##### M\_GET\_UPDATE oplist
##### M\_GET\_TYPE oplist
##### M\_GET\_SUBTYPE oplist
##### M\_GET\_OPLIST oplist
##### M\_GET\_SORT oplist
##### M\_GET\_IT\_TYPE oplist
##### M\_GET\_IT\_FIRST oplist
##### M\_GET\_IT\_LAST oplist
##### M\_GET\_IT\_END oplist
##### M\_GET\_IT\_SET oplist
##### M\_GET\_IT\_END\_P oplist
##### M\_GET\_IT\_LAST\_P oplist
##### M\_GET\_IT\_EQUAL\_P oplist
##### M\_GET\_IT\_NEXT oplist
##### M\_GET\_IT\_PREVIOUS oplist
##### M\_GET\_IT\_REF oplist
##### M\_GET\_IT\_CREF oplist
##### M\_GET\_IT\_REMOVE oplist
##### M\_GET\_IT\_INSERT oplist
##### M\_GET\_ADD oplist
##### M\_GET\_SUB oplist
##### M\_GET\_MUL oplist
##### M\_GET\_DIV oplist
##### M\_GET\_CLEAN oplist
##### M\_GET\_PUSH oplist
##### M\_GET\_POP oplist
##### M\_GET\_REVERSE oplist
##### M\_GET\_GET\_STR oplist
##### M\_GET\_OUT\_STR oplist
##### M\_GET\_IN\_STR oplist
##### M\_GET\_SEPARATOR oplist
##### M\_GET\_EXT\_ALGO oplist
##### M\_GET\_INC\_ALLOC oplist
##### M\_GET\_OOR\_SET oplist
##### M\_GET\_OOR\_EQUAL oplist

Return the associated method to the given operator within the given oplist.

##### M\_DEFAULT\_OPLIST

Default oplist for C standard types (int & float)

##### M\_CSTR\_OPLIST

Default oplist for the C type const char *

##### M\_POD\_OPLIST

Default oplist for a structure C type without any init & clear
prerequesites.

##### M\_A1\_OPLIST

Default oplist for a array of size 1 of a structure C type without any init & clear
prerequesites.

##### M\_CLASSIC\_OPLIST(name)

Create the oplist with the classic operators using the pattern 'name', i.e.
name##_init, name##_clear, etc.

##### M\_OPFLAT oplist

Remove the parenthesis around an oplist.

##### M\_OPCAT(oplist1,oplist2)

Concat two oplists in one. 'oplist1''s operators will have higher priority to 'oplist2'

##### M\_OPEXTEND(oplist, ...)

Extend an oplist with the given list of operators.
Theses new operators will have higher priority than the ones
in the given oplist.

##### M\_TEST\_METHOD\_P(method, oplist)

Test if a method is present in an oplist. Return 0 or 1.

##### M\_IF\_METHOD(method, oplist) 

Perfom a preprocessing M\_IF, if the method is present in the oplist.
Example: M\_IF\_METHOD(HASH, oplist)(define function which uses HASH method, ) 

##### M\_IF\_METHOD\_BOTH(method, oplist1, oplist2)     

Perform a preprocessing M\_IF if the method exists in both oplist.
Example: M\_IF\_METHOD\_BOTH(HASH, oplist1, oplist2) (define function , )

##### M\_IF\_METHOD\_ALL(method, ...)

Perform a preprocessing M\_IF if the method exists for all oplist.
Example: M\_IF\_METHOD\_ALL(HASH, oplist1, oplist2, oplist3) (define function, ) 

##### M_IPTR

By putting this after a method for an operator in the oplist,
it specifies that the first argument of the moethod shall be a pointer
to the destination type, rather than the type.

##### M\_DO\_INIT\_MOVE(oplist, dest, src)
##### M\_DO\_MOVE(oplist, dest, src)

Perform an INIT\_MOVE/MOVE if present, or emulate it otherwise.
Note: default methods for INIT\_MOVE/MOVE are not robust enough yet.

##### M\_GLOBAL\_OPLIST(a)

Check if a is an oplist, and return a
or if a symbol composed of M_OPL_##a() is defined as an oplist, and returns it
otherwise return a.
In short, if a global oplist is defined for the argument, it returns it
otherwise it returns the argument.
Global oplist is limited to typedef types.
   
##### M_GLOBAL_OPLIST_OR_DEF(a)

Check if a a symbol composed of M_OPL_##a() is defined as an oplist, and returns it
otherwise return M_DEFAULT_OPLIST.
In short, if a global oplist is defined for the argument, it returns it
otherwise it returns the default oplist.
Global oplist is limited to typedef types.
   

#### Syntax enhancing

##### M\_EACH(item, container, oplist)

This macro allows to iterate over the given 'container' of oplist 'oplist'.
It shall be used after the for C keyword.
'item' will be a created pointer variable to the underlying type,
only available within the 'for'.
There can only have one M\_EACH per line.
Example: 
         for M_EACH(item, list, LIST_OPLIST) { action; }

##### M\_LET(var1[,var2[,...]], oplist)

This macro allows to define the variable 'var1'(resp. var2, ...) 
of oplit 'oplist', 
initialize 'var1' (resp. var2, ...) by calling the initialization method,
and clear 'var1' (resp. var2, ...) by calling the initialization method
when the bracket associated to the M\_LET go out of scope.
There can only have one M\_LET per line.
Example:

     M_LET(a, STRING_OPLIST) { do something with a }  or
     M_LET(a, b, c, STRING_OPLIST) { do something with a, b & c }

NOTE: The user code can not perform a return or a goto outside the {}
otherwise the clear code of the object won't be called .

#### Memory functions

##### type *M\_MEMORY\_ALLOC (type)

Return a pointer to a new allocated object of type 'type'.
The object is not initialized.
In case of allocation error, it returns NULL.
The default function is the malloc function.
It can be overriden before including the header m-core.h

##### void M\_MEMORY\_DEL (type *ptr)

Delete the cleared object pointed by the pointer 'ptr'.
The pointer was previously allocated by the macro M\_MEMORY\_ALLOC.
'ptr' can not be NULL.
The default function is the free function.
It can be overriden before including the header m-core.h

##### type *M\_MEMORY\_REALLOC (type, ptr, number)

Return a pointer to an array of 'number' objects of type 'type'.
The objects are not initialized, nor the state of previous objects changed.
'ptr' is either NULL, or pointer returned from a previous call 
of M\_MEMORY\_REALLOC.
In case of allocation error, it returns NULL.
The default function is the realloc function.
It can be overriden before including the header m-core.h

##### void M\_MEMORY\_FREE (type *ptr)

Delete the cleared object pointed by the pointer 'ptr'.
The pointer was previously allocated by the macro M\_MEMORY\_REALLOC.
'ptr' can not be NULL.
The default function is the free function.
It can be overriden before including the header m-core.h
A pointer allocated by M\_MEMORY\_ALLOC can not be freed by this function.

##### void M\_MEMORY\_FULL (size_t size)

This macro is called when a memory exception error shall be raised.
It can be overriden before including the header m-core.h
The default is to abort the execution.
The macro can :

* abort the execution,
* throw an exception (In this case, the state of the object is unchanged),
* set a global error variable and return.

NOTE: The last case is not 100% supported. 

##### void M\_INIT\_FAILURE (void)

This macro is called when an initialization error shall be raised.
It can be overriden before including the header m-core.h
The default is to abort the execution.
The macro can :

* abort the execution,
* throw an exception (In this case, the state of the object is unchanged),
* set a global error variable and return.

NOTE: The last case is not currently supported. 

##### void M\_ASSERT\_INIT\_FAILURE(expression)

This macro is called when an assertion in an initialization context
is called.
If the expression is false, the execution is aborted.
The assertion is kept in release programs.
It can be overriden before including the header m-core.h
The default is to abort the execution.


### M-MUTEX

This header is for providing very thin layer around OS implementation of threads, conditional variables and mutexs.
It has backends for WIN32, POSIX thread or C11 thread.

It was needed due to the low adoption rate of the C11 equivalent layer.

Example:

	m_thread_t idx_p;
	m_thread_t idx_c;

	m_thread_create (idx_p, conso_function, NULL);
	m_thread_create (idx_c, prod_function, NULL);
	m_thread_join (idx_p;
	m_thread_join (idx_c);

#### methods

The following methods are available:

#### m\_mutex\_t

A type representating a mutex.

##### void m\_mutex\_init(mutex)

Initialize the variable mutex of type m\_mutex\_t. 
If the initialization fails, the program aborts.

##### void m\_mutex\_clear(mutex)

Clear the variable mutex of type m\_mutex\_t. 
If the variable is not initialized, the behavior is undefined.

##### void m\_mutex\_lock(mutex)

Lock the variable mutex of type m\_mutex\_t for exclusive use.
If the variable is not free, it will wait indefinitely until it is.
If the variable is not initialized, the behavior is undefined.

##### void m\_mutex\_unlock(mutex)

Unlock the variable mutex of type m\_mutex\_t for exclusive use.
If the variable is not locked, the behavior is undefined.
If the variable is not initialized, the behavior is undefined.

##### M\_LOCK\_DECL(name)

Define the lock 'name'. This shall be called in the global space (reserved for global variables).

##### M\_LOCK(name)

Use the lock 'name': the encapsulation instructions are protected by the lock.
Example:

        M_LOCK_DECL(n_lock);
        unsigned long n = 0;
        void f(void) {
             M_LOCK(n_lock) {
               n ++;
             }
        }

#### m\_cond\_t

A type representating a conditionnal variable, used within a mutex section.

##### void m\_cond\_init(m\_cond\_t cond)

Initialize the conditional variable cond of type m\_cond\_t. 
If the initialization failed, the program aborts.

##### void m\_cond\_clear(m\_cond\_t cond)

Clear the variable cond of type m\_cond\_t. 
If the variable is not initialized, the behavior is undefined.

##### void m\_cond\_signal(m\_cond\_t cond)

Within a mutex exclusive section,
signal that the event associated to the variable cond of type m\_cond\_t 
has occured to at least a single thread.
If the variable is not initialized, the behavior is undefined.

##### void m\_cond\_broadcast(m\_cond\_t cond)

Within a mutex exclusive section,
signal that the event associated to the variable cond of type m\_cond\_t 
has occured to all waiting threads.
If the variable is not initialized, the behavior is undefined.

##### void m\_cond\_wait(m\_cond\_t cond, m\_mutex\_t mutex)

Within a mutex exclusive section defined by mutex,
wait indefinitely for the event associated to the variable cond of type m\_cond\_t
to occur.
IF multiple threads wait for the same event, which thread to awoken
is not specified.
If any variable is not initialized, the behavior is undefined.

#### m\_thread\_t

A type representating an id of a thread.

##### void m\_thread\_create(m\_thread\_t thread, void (\*function)(void\*), void \*argument)

Create a new thread and set the it of the thread to 'thread'.
The new thread run the code function(argument) with
argument a 'void \*' and function taking a 'void \*' and returning
nothing.
If the initialization fails, the program aborts.

##### void m\_thread\_join(m\_thread\_t thread)

Wait indefinetly for the thread 'thread' to exit.


### M-WORKER

This header is for providing a pool of workers.
Each worker run in a separate thread and can handle work orders
sent by the main thread. A work order is a computation task.
Work orders are organized around synchronization points.

This implements parallelism just like OpenMP or CILK++.

Example:

        worker_t worker;
        worker_init(worker, 0, 0, NULL);
        worker_sync_t sync;
        worker_start(sync);
        void *data = ...;
        worker_spawn (worker, sync, taskFunc, data);
        taskFunc(otherData);
        worker_sync(sync);
        

Currently, there is no support for:

* exceptions by the worker tasks,
* the worker tasks shall not lock a mutex without closing it (same for other synchronization structures).

Thread Local Storage variables have to be reinitialized properly
with the reset function. This may result in subtle difference between the
serial code and the parallel code.


#### methods

The following methods are available:

#### worker\_t

A pool of worker.

#### worker\_sync\_t

A synchronization point between workers.

#### void worker\_init(worker\_t worker[, unsigned int numWorker, unsigned int extraQueue, void (*resetFunc)(void), void (*clearFunc)(void) ])

Initialize the pool of workers 'worker' with 'numWorker' workers.
if 'numWorker' is 0, then it will detect how many core is available on the
system.
Between each work order and before the first one, the function 'resetFunc'
is called by the worker to reset its state (or call nothing if the function
pointer is NULL).
'extraQueue' is the number of tasks which can be accepted by the work order
queue in case if there is no worker available.
Before terminating, each worker will call 'clearFunc' if the function is not NULL.
Default values are respectively 0, 0, NULL and NULL.

#### void worker\_clear(worker\_t worker)

Clear the pool of workers, and wait for the workers to terminate.
It is undefined if there is any work order in progress.

#### void worker\_start(worker\_block\_t syncBlock)

Start a new synchronization block for a pool of work orders.

#### void worker\_spawn(worker\_t worker, worker\_block\_t syncBlock, void (*func)(void *data), void *data)

Request the work order 'func(data)' to the pool of worker 'worker',
registered into the synchronization point syncBlock.
If no worker is available, the work order 'func(data)' will be handled
by the caller. Otherwise the work order 'func(data)' will be handled
by an asynchronous worker and the function immediately returns.

#### bool worker\_sync\_p(worker\_block\_t syncBlock)

Test if all work orders registered to this synchronization point are
finished (true) or not (false). 

#### void worker\_sync(worker\_t worker, worker\_block\_t syncBlock)

Wait for all work orders registered to this synchronization point 'syncBlock' to be
finished for the worker 'worker'.

#### size\_t worker\_count(worker\_t worker)

Return the number of workers of the pool.


#### WORKER\_SPAWN(worker, syncBlock, input, core, output)

Request the work order '_core' to the pool of worker 'worker',
registered into the synchronization point syncBlock.
If no worker is available, the work order 'core' will be handled
by the caller. Otherwise the work order 'core' will be handled
by an asynchronous worker.
'core' is any C code which doesn't break the control flow (you
cannot use return / goto to go outside the flow).
'input' is the list of input variables of the 'core' block within "( )".
'output' is the list of output variables of the 'core' block within "( )".
This macro needs either GCC (for nested function) or CLANG (for blocks)
or a C++11 compiler (for lambda and functional) to work.

NOTE1: Even if nested functions are used for GCC, it doesn't generate
a trampoline and the stack doesn't need to be executable.

NOTE2: For CLANG, you need to add -fblocks to CFLAGS and -lBlocksRuntime to LIB (See CLANG manual).

NOTE3: It will generate warnings about shadow variables. There is no way to avoid this.

NOTE4: arrays are not supported as input / output variables due to 
technical limitations.


### M-ATOMIC

This header goal is to provide the C header 'stdatomic.h'
to any C compiler (C11 or C99 compliant) or C++ compiler.
If available, it uses the C11 header stdatomic.h,
otherwise if the compiler is a C++ compiler,
it uses the header 'atomic' and imports all definition
into the global namespace (this is needed because the
C++ standard doesn't support officialy the stdatomic header,
resulting in broken compilation when building C code with
a C++ compiler).
Otherwise it provides a non-thin emulation of atomics
using mutex.


### M-ALGO

This header is for generating generic algorithm to containers.

#### ALGO\_DEF(name, container_oplist)

Define the available algorithms for the container which oplist is container_oplist.
The defined algorithms depend on the availability of the methods of the containers
(For example, if there no CMP operation, there is no MIN operation defined).

Example:

	ARRAY_DEF(array_int, int)
	ALGO_DEF(array_int, ARRAY_OPLIST(int))
	void f(void) {
		array_int_t l;
		array_int_init(l);
		for(int i = 0; i < 100; i++)
			array_int_push_back (l, i);
		array_int_push_back (l, 17);
		assert( array_int_contains(l, 62) == true);
		assert( array_int_contains(l, -1) == false);
		assert( array_int_count(l, 17) == 2);
		array_int_clear(l);
	}

#### Created methods

The following methods are automatically and properly created by the previous macros. In the following methods, name stands for the name given to the macro which is used to identify the type.

In the following descriptions, it\_t is an iterator of the container
container\_t is the type of the container and type\_t is the type
of object contained in the container.

##### void name\_find(it\_t it, container\_t c, const type\_t data)

Search for the first occurence of 'data' within the container.
Update the iterator with the found position or return end iterator. 
The search is linear.

##### bool name\_contains(container\_t c, const type\_t data)

Return true if 'data' is within the container, false otherwise.
The search is linear.

##### void name\_find\_last(it\_t it, container\_t c, const type\_t data)

Search for the last occurence of 'data' within the container.
Update the iterator with the found position or return end iterator. 
The search is linear and can be backward or forwards depending
on the possibility of the container.

##### size\_t name\_count(container\_t c, const type\_t data)

Return the number of occurence of 'data' within the container.
The search is linear.

TODO: map, reduce, map_reduce, min, max, minmax, sort_p, uniq, sort
add, sub, mul, div,  union, intersect






#### ALGO\_MAP(container, oplist, func[, arguments..])

Apply the function 'func' to each element of the container 'container' of oplist 'oplist' :
     
     for each item in container do
     	 func([arguments,] item)

The function 'func' is a method which takes as argument an object of the
container and returns nothing.


#### ALGO\_EXTRACT(containerDest, oplistDest, containerSrc, oplistSrc, func[, arguments..])

Extract the items of the container containerSrc of oplist oplistSrc
into the containerDest of oplist oplistDest: 
     
     CLEAN (containerDest)
     for each item in containerSrc do
     	 if func([arguments,] item) 
	      	 Push item in containerDest

The function 'func' is a method which takes as argument an object of the
container and returns a boolean which is true if the object shall be added
to the other container.


#### ALGO\_REDUCE(dest, container, oplist, reduceFunc[, mapFunc[, arguments..])

Reduce the items of the container 'container' of oplist 'oplist'
into a single element by applying the reduce function:

     dest = reduceFunc(mapFunc(item[0]), reduceFunc(..., reduceFunc(mapFunc(item[N-2]), mapFunc(item[N-1]))))


'mapFunc' is a method which prototype is:
    
    void mapFunc(dest, item)

with both dest & item which are of the same type than the one of
the container. It transforms the 'item' into another form which is suitable
for the reduceFunc.
If mapFunc is not specified, identiy will be used instead.

'reduceFunc' is a method which prototype is:
 
     void reduceFunc(dest, item)

It integrates the new 'item' into the partial 'sum' 'dest.
The reduce function can be the special keywords add, sum, and, or
in which case the specila function performing a sum/sum/and/or operation
will be used.

#### ALGO\_INIT\_VA(container, oplist, param1[, other params..])

Initialize & set a container with a variable array list.



### M-MEMPOOL

This header is for generating specialized optimized memory pools:
it will generate specialized functions to alloc & free only one kind of an object.
The mempool functions are not thread safe for a given mempool.

#### MEMPOOL\_DEF(name, type)

Generate specialized functions & types prefixed by 'name' 
to alloc & free a 'type' object.

Example:

	MEMPOOL_DEF(mempool_uint, unsigned int)

	void f(void) {
          mempool_uint_t m;
          mempool_uint_init(m);
          unsigned int *p = mempool_uint_alloc(m);
          mempool_uint_free(m, p);
          mempool_uint_clear(m);
        }

#### Created methods

The following methods are automatically and properly created by the previous macros. In the following methods, name stands for the name given to the macro which is used to identify the type.

##### name\_t

The type of a mempool.

##### void name\_init(name\_t m)

Initialize the mempool 'm'.

##### void name\_clear(name\_t m)

Clear the mempool 'm'.
All allocated objects associated to the this mempool which weren't explicitly freed
will be deleted too (without calling their clear method).

##### type *name\_alloc(name\_t m)

Create a new object of type 'type' and return a new pointer to the uninitialized object.

##### void name\_free(name\_t m, type *p)

Free the object 'p' created by the call to name\_alloc.
The clear method of the type is not called.


