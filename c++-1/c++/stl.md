# STL

## \<algorithm>

### sort

### binary_search

* only returns true or false

## \<iomanip>

* flagi na `cout`

## Containers

## Container Adaptors

## Associative Containers

### map

* work as default dic ??? works on keys that are not in map `m["abc"] = 6` 

```cpp
std::map <key_type, date_type>

map<string,int> m;
m.size();
m.insert(make_pair("hello",9));
// also can do
m["abc"] = 6 
m.earse(val);

// finding element
map<string,int>::iterator itr=m.find(val);  // gives the iterator to the element
//val if it is found otherwise return m.end()

m["key"] 
// or get iterator using find and then:
itr->second //access the value
```
