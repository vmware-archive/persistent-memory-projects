---
title: API Walkthrough
image: /img/posts/signpost.png
categories: [pmem, API, walkthrough]
---

This blog post gives a walkthrough on how to write a persistent memory application in Go using our programming model.

This guide assumes you have a persistent memory device configured for file system direct access. If not, visit this [RedHat documentation](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/configuring-persistent-memory-for-file-system-direct-access-dax) to learn how. If you do not have a persistent memory device, then it can also be emulated. See documentation [here](https://pmem.io/2016/02/22/pm-emulation.html).

Before starting, download our software suite following instructions in the [Documentation](/documentation) page.
The [go-pmem-transaction](https://github.com/vmware/go-pmem-transaction) provides two packages for persistent memory application development - `pmem` and `transaction`. `pmem` package provides an API to initialize persistent memory and also methods to access objects in persistent memory. The `transaction` package provides APIs to update data in persistent memory in a crash-consistent manner. It supports undo logging and redo logging.

A simple persistent memory application written using our programming model can be accessed [here](https://github.com/vmware/go-pmem-transaction/blob/master/examples/database.go). See below for more details on the API usage.

## 1.	Persistent Memory Initialization

API: ```Init(fileName string) bool```

Usage:
```go
firstInit := pmem.Init("/mnt/ext4-pmem0/database")
if firstInit {
    // Indicates first time initialization of the pmem file
} else {
    // Not a first time initialization - application restarted
}
```

Each application uses persistent memory through a file in the persistent memory device. The `Pmem` library provides the `Init` API which takes care of initializing this persistent memory file. It takes the path to the persistent memory file that you want to create. The API returns a boolean that indicate if this is a first time initialization or not.  

If this is not the first time initialization, then the `Init` API library takes care doing any heap recovery or transaction rollback.

## 2.	Creating named objects

The `Pmem` package provides ‘named objects’ using which you can associate an object in persistent memory with a name. You can then retrieve this object back using this name at any later point in time.

API: `New(name string, intf interface{}) unsafe.Pointer`

Usage:
```go
var rptr *root
rptr = (*root)(pmem.Get("dbRoot", rptr))
```

The `New` API takes two arguments. The first is the name of the object to be created. The second is an interface which is needed to understand the type of the object to be created. The `New` API returns an `unsafe.Pointer` which can then be cast to the object’s type to start using it.
The second argument cannot be a slice.

## 3. Retrieving Named Objects

API: `func Get(name string, intf interface{}) unsafe.Pointer`

Usage:
```go
var rptr *root
rptr = (*root)(pmem.Get("dbRoot", rptr))
if rptr == nil {
    // dbRoot does not exist
}
```

## 3.	Using transactions

Data in persistent memory has to be updated in a crash-consistent manner. The transaction package provides an implementation of undo logging and redo logging. 

### Transaction handle
Before logging some data in memory, a transaction handle has to be acquired first. For undo logging, this is achieved using the `NewUndoTx` API

API: `func NewUndoTx() TX`

Usage:
```go
tx := transaction.NewUndoTx()
```

Once the usage of the transaction handle is completed, it has to be released back using the `Release` API.

API: `func Release(t TX)`

Usage:
```go
transaction.Release(tx)
```

### Logging data using undo log
The transaction boundary is specified using the `Begin` and `End` APIs. Within a transaction boundary, data can be logged using the `Log` API.

API:
```go
Begin() error
Log(...interface{}) error
End() error
```

Usage:
```go
tx.Begin()
tx.Log(rptr)
rptr.magic = magic
tx.End()
```

## Further Reading

For an in-depth documentation of the `pmem` and `transaction` package, refer to the respective GitHub pages - [pmem README](https://github.com/vmware/go-pmem-transaction/blob/master/pmem/README.md) and [transaction README](https://github.com/vmware/go-pmem-transaction/blob/master/transaction/README.md)