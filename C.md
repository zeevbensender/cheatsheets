## Iterating over an array of structures in C when given a pointer to the first structure and the number of elements in the array.

Assuming you have the following structure definition:
```c
typedef struct {
    int id;
    char name[100];
    float salary;
} my_struct;
```
Define a pointer to the first `my_struct` and the number of elements in the array:
```c
my_struct* ptr = ...; // pointer to the first my_struct
int numElements = ...; // number of elements in the array
```

Iterate over the array of structures using a loop as follows:
```c
for (int i = 0; i < numElements; i++) {
    // Access the current structure in the array
    my_struct current = *(ptr + i);
    
    // Access the members of the current structure
    int id = current.id;
    char* name = current.name;
    float salary = current.salary;
    
    // Do something with the members...
}
```

In the above code, we use pointer arithmetic `(ptr + i)` to access each structure in the array. The pointer is incremented by the `i` index to calculate the memory address of each structure in the array. The `*(ptr + i)` dereferences the pointer to access the value at that memory address.

We can access the members of the current structure similar to accessing members in a regular structure variable.

