---
title: API Walkthrough
image: /img/posts/signpost.png
categories: [pmem, API, walkthrough]
---

This blog post gives a walkthrough on how to write a persistent memory application in Go using our programming model.

This guide assumes you have a persistent memory device configured for file system direct access. If not, visit this [RedHat documentation](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/configuring-persistent-memory-for-file-system-direct-access-dax) to learn how. If you do not have a persistent memory device, then it can also be emulated. See documentation [here](https://pmem.io/2016/02/22/pm-emulation.html).

Before starting, download our software suite following instructions in the [Documentation](/documentation) page.
A simple persistent memory application written using our programming model can be accessed [here](https://github.com/vmware/go-pmem-transaction/blob/master/examples/database.go). See below for more details on the API usage.

## 1.	Persistent Memory Initialization

API: ```Init(fileName string) bool```

Usage:
```go
if pmem.Init("/mnt/pmem/myTestFile") {
    // Indicates first time initialization of the pmem file
} else {
    // Not a first time initialization - application restarted
}
```

Each application uses persistent memory through a file in the persistent memory device. The `Pmem` library provides the `Init` API which takes care of initializing this persistent memory file. It takes the path to the persistent memory file that you want to create. The API returns a boolean that indicate if this is a first time initialization or not.  

If this is not the first time initialization, then the `Init` API library takes care doing any heap recovery or transaction rollback.

## 2.	Create named objects

The `Pmem` package provides ‘named objects’ using which you can associate an object in persistent memory with a name. You can then retrieve this object back using this name at any later point in time.

### a.	Normal objects

API: `New(name string, intf interface{}) unsafe.Pointer`

Usage:

```go
var a *int
a = (*int)(pmem.New("region1", a))
```

The New API takes two arguments. The first is the name of the object to be created. The second is an interface which is needed to understand the type of the object to be created. The New API returns an unsafe.Pointer which can then be cast to the object’s type to starting using it.
The second argument cannot be a slice

### b.	Slices

API:

```Make(name string, intf ...interface{}) interface{}```

Usage:

```go
var slice1 []float64
slice1 = pmem.Make("region2", slice1, 10).([]float64) // len(slice1) = 10
slice1[0] = 1.1
```

Similar to `New()`, this API allows to create a named slice in persistent memory. 

** does this support a third argument

## 3.	Use transactions
