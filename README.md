androidbinary
=====

[![Build Status](https://github.com/liuweijack/androidbinary/workflows/Test/badge.svg)](https://github.com/liuweijack/androidbinary/actions)
[![GoDoc](https://godoc.org/github.com/liuweijack/androidbinary?status.svg)](https://godoc.org/github.com/liuweijack/androidbinary)

Android binary file parser

## High Level API

### Parse APK files

``` go
package main

import (
	"github.com/liuweijack/androidbinary/apk"
)

func main() {
	pkg, _ := apk.OpenFile("your-android-app.apk")
	defer pkg.Close()

	icon, _ := pkg.Icon(nil) // returns the icon of APK as image.Image
	pkgName := pkg.PackageName() // returns the package name

	resConfigEN := &androidbinary.ResTableConfig{
		Language: [2]uint8{uint8('e'), uint8('n')},
	}
	appLabel, _ = pkg.Label(resConfigEN) // get app label for en translation
}
```

## Low Level API

### Parse XML binary

``` go
package main

import (
	"encoding/xml"

	"github.com/liuweijack/androidbinary"
	"github.com/liuweijack/androidbinary/apk"
)

func main() {
	f, _ := os.Open("AndroidManifest.xml")
	xml, _ := androidbinary.NewXMLFile(f)
	reader := xml.Reader()

	// read XML from reader
	var manifest apk.Manifest
	data, _ := ioutil.ReadAll(reader)
	xml.Unmarshal(data, &manifest)
}
```

### Parse Resource files

``` go
package main

import (
	"fmt"
	"github.com/liuweijack/androidbinary"
)

func main() {
	f, _ := os.Open("resources.arsc")
	rsc, _ := androidbinary.NewTableFile(f)
	resource, _ := rsc.GetResource(androidbinary.ResID(0xCAFEBABE), nil)
	fmt.Println(resource)
}
```

## License

This software is released under the MIT License, see LICENSE.
