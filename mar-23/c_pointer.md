###Notice about Pointer in C/C++

* Name of an Array type is stored as a pointer memory address. No matter it is primitive type or struct type. For example:

```c

struct A{
    uint16_t id;
    char* name;
};

struct A names[2] = {{0,"Zero"},{1,"One"}};

int main(){
    struct A* p = names;
    locateById(1,p);
    printf(p->name);
    return 0;
}

void locateById(uint16_t id, struct a* p){

    // process data
}

```

> The idea here is `name` is nothing but memory address, only using pointer can dereference it and read the data in that memory address.

* Name of a non-Array type is stored as the real value. No matter it is a primitive type or struct type.

```c
struct A{
    uint16_t id;
    char* name;
};

struct A name = {3,"Three"};

int main(){
    struct A anName = name;
    printf(anName.name);
    return 0;
}
```


###Wild pointer and Null pointer in C

* Dangling pointer: A dangling pointer is a pointer taht points to invalid data or to data which is not valid anymore, for example:
```cpp
Class *object1 = new Class();
Class *object2 = object1;

delete object1;
object1 = nullPointer;

//now object2 points to something which is not valid anymore.
```

* wild pointer can refer to anywhere possible. It would mean a pointer that neither refers to a legitimate object, nore is NULL.

```c
int* p //unintialized and non-static, so wild pointer
```

###Static in C
* Same as `static` in Java: the variable location on stack is no change, so that the many times call or refer to this variable uses the same memory location for this variable.
