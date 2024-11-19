





原文：

[Unreal Engine C++: TArray doc sheet - Rodolphe Vaillant's homepage (rodolphe-vaillant.fr)](http://rodolphe-vaillant.fr/entry/143/unreal-engine-c-tarray-doc-sheet)

## Basic

|                                                        | std::vector\<T\>   | TArray\<T\>                                            |
| ------------------------------------------------------ | ------------------ | ------------------------------------------------------ |
| Create a new element (at the end of the vector/TArray) | .push_back()       | .Add() .Push()                                         |
| Number of elements                                     | .size()            | .Num()                                                 |
| Allocated memory (in terms of number of elements)      | .capacity()        | .Max()                                                 |
| Raw pointer to the array                               | .data()            | .GetData()                                             |
| Get last element                                       | .back()            | .Last() .Top()                                         |
| Get first element                                      | .front()           | my_array[0] *(my_array.begin()) (no better equivalent) |
| Check (size == 0) / (Num == 0)                         | .empty()           | .IsEmpty()                                             |
| Set number of elements to 0                            | .clear()           | .Empty()                                               |
| Resize and fill (with default constructor T())         | .resize(int size)  | .SetNum(int size);                                     |
| Pre-allocate memory (without changing actual size)     | .reserve(int size) | .Reserve(int size)                                     |


## Advanced

|                                                              | std::vector\<T\>                                             | TArray\<T\>                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Create a new element at the end, use default constructor T() | .push_back( T() )                                            | .AddDefaulted(1)                                             |
| Remove element at: int index;                                | .vec.erase(vec.begin() + (int)index)                         | .RemoveAt(int index)                                         |
| Check index is within bounds: 0 <= index < size()/Num()      | index >= 0 && index < vec.size()                             | .IsValidIndex(int index)                                     |
|                                                              |                                                              |                                                              |
|                                                              | std::transform(    inVec.begin(),    inVec.end(),    std::back_inserter(outVec),    lambdaFunction ); | Algo::Transform(    inTArray,     outTArray,     lambdaFunction ); |

## 示例

```cpp
{
    // []
    TArray<float> my_array;
    std::vector<float> my_vector;
 
    // ------------------------------------
    // Add/Push elements to the last entry
    // ------------------------------------
     
    // std::vector<>::push_back() <=> TArray<>::Add(), TArray<>::Push()
     
    // {1.0f}
    my_array.Add(1.0f); // Num=1; Max=4
    my_vector.push_back(1.0f); // size=1; capacity=1   
 
    // {1.0f, 2.0f}
    my_array.Add(2.0f); // Num=2
    my_vector.push_back(2.0f); // size=2
 
    // {1.0f, 2.0f, 3.0f}
    my_array.Push(3.0f); // Alias to my_array.Add(3.0f);
    my_vector.push_back(3.0f);
 
    // std::vector<>::size() <=> TArray<>::Num()
 
    int32 size_a = my_array.Num(); // size_a = 3
    size_t size_v = my_vector.size(); // size_v = 3
 
    // std::vector<>::capacity() <=> TArray<>::Max()
 
    int32 unreal_max = my_array.Max(); // unreal_max = 4
    size_t std_capacity = my_vector.capacity(); // std_capacity = 3
 
    // ----------------------------
    // Get raw pointer to the array
    // ----------------------------
     
    // std::vector<>::data() <=> TArray<>::GetData()
 
    float* raw0 = my_array.GetData();
    float* raw1 = my_vector.data();    
 
    //-----------------
    // Get last element (index: 2, value: 3.0f)
    //-----------------
 
    // std::vector<>::back() <=> TArray<>::Last(), TArray<>::Top()
 
    float elt2 = my_array.Last(); // elt2 = 3.0f
    elt2 = my_array.Top(); // elt2_ = 3.0f
    float elt2_ = my_vector.back(); // elt2_ = 3.0f
 
    //------------------
    // Get first element (index: 0, value: 1.0f)
    //------------------
 
    // std::vector<>::front() <=> TArray<>::[0], *TArray<>::begin()
 
    float elt0_ = my_vector.front(); // elt0_ = 1.0f
    // no equivalent of the above
    float elt0 = my_array[0]; // elt0 = 1.0f
    elt0 = *(my_array.begin()); // elt0 = 1.0f
}
 
{
    // []
    TArray<float> my_array = {1.0f, 2.0f, 3.0f}; // Num=3
    std::vector<float> my_vector = { 1.0f, 2.0f, 3.0f }; // size=3; capacity=3
 
    //todo: my_array.Reset()
    my_array.Empty(); // Num=0; Max=0
    my_vector.clear(); // size=0; capacity=3
 
    bool s0 = my_array.IsEmpty(); // true
    bool s1 = my_vector.empty(); // true
 
}
 
/*
 Resizing and reserving memory:
*/
 
struct Stuff {
    int a = -2;
};
 
{
    // Initialize and fill with default constructor T() of the container:
 
    std::vector<Stuff> my_vector(10);
    // note: TArray<float> my_array(10); not available in unreal
    // instead we use:
    TArray<Stuff> my_array;
    // or perhaps alternatively: my_array.Init(Stuff(), 10); // ??
    my_array.SetNum(10);
 
     
    my_vector.reserve(20);     
    my_array.Reserve(20);
 
    // Resize and fill with default constructor T() of the container:      
    my_vector.resize(30); // Num=30;
    my_array.SetNum(30); // size=30
}
 
/*
     Resizing and reserving memory:
     (unreal only features)
*/
 
{      
    // Specific to unreal              
    TArray<Stuff> my_array;
    my_array.AddDefaulted(2);     // {Stuff.a = -2, Stuff.a = -2 } same as std::vector<float> my_vector(2);
    my_array.AddUninitialized(2); // {Stuff.a = 'random value in memory', Stuff.a = 'rand val as well' }
    my_array.AddZeroed(2);        // {Stuff.a = 0, Stuff.a = 0 } sets memory to zero
 
    my_array.SetNumUninitialized(3);
    my_array.SetNumZeroed(4); // Zero out only *new* elements
}

```