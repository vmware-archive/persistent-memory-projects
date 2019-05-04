---
layout: page
title: Documentation
id: documentation
---
## What is Persistent Memory

Persistent memory provides disk like persistence at memory-like data access speeds. It provides a number of benefits such as faster application start up time, higher memory density and lower costs. More information about persistent memory can be found at [pmem.io](https://pmem.io/).

This website aims to provide you resources to develop applications designed for persistent memory using Go programming language.

## How to get Started

We provide libraries that you can use to develop persistent memory applications. Follow the below steps to get started.

1. Download the modified Go toolchain which contains changes for supporting persistent memory from [https://github.com/jerrinsg/go-pmem](https://github.com/jerrinsg/go-pmem)

2. Obtain the go-pmem-transaction package from [https://github.com/vmware/go-pmem-transaction](https://github.com/vmware/go-pmem-transaction)

    `$ go get -u github.com/vmware/go-pmem-transaction/...`

    Make sure to use the Go binary from step 1 to obtain these packages  

3. Build and run your persistent memory applications!

The `go-pmem-transaction` package contains an [example application](https://github.com/vmware/go-pmem-transaction/blob/master/examples/database.go) that you can refer to understand how to write a persistent memory application using our programming model.

For a more detailed implementation example, please refer to [Go Redis](https://github.com/vmware-samples/go-redis-pmem). Go Redis is a Go version of Redis designed for persistent memory using the new programming model.

More documentation about each of these projects are available in their respective GitHub repositories.
