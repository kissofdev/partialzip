# partialzip

[![GoDoc](https://godoc.org/github.com/kissofdev/partialzip?status.svg)](https://godoc.org/github.com/kissofdev/partialzip) [![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org)

> Partial Implementation of PartialZip in Go

---

## Why?

If you need to download a VERY large zip, but you only need some small file from it, this is a nice optimization to allow you to only extract what you need and nothing else. Saving time, bandwidth and space.

## Install

```bash
go get github.com/kissofdev/partialzip
```

## Example

```golang
package main

import (
    "fmt"
    "log"

    "github.com/kissofdev/partialzip"
)

func main() {
    pzip, err := partialzip.New("https://example.com/archive.zip")
    if err != nil {
        log.Fatal(err)
    }

    // List all files in remote ZIP.
    for _, file := range pzip.List() {
        fmt.Println(file)
    }

    // Download required file from the list above.
    buffer, err := pzip.Download("subfolder/some_file.txt")
    if err != nil {
        log.Fatal(err)
    }

    of, err := os.Create("my_folder/new_filename.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer of.Close()

    _, err = of.Write(buffer)
    if err != nil {
        log.Fatal(err)
    }
}
```

## CLI (unchanged within this fork)

### Install

Download **punzip** from [releases](https://github.com/blacktop/partialzip/releases)

### Usage

List zipped files

```bash
$ punzip list http://updates-http.cdn-apple.com/download/ipsw

NAME                                                         |SIZE
Firmware/                                                    |0 B
...SNIP...
kernelcache.release.iphone11                                 |18 MB
Firmware/all_flash/LLB.d321.RELEASE.im4p.plist               |331 B
048-16246-211.dmg                                            |107 MB
048-15811-213.dmg                                            |106 MB
Firmware/ICE18-1.00.08.Release.bbfw                          |39 MB
```

Download a file from the remote zip

```bash
$ punzip get --path kernelcache.release.iphone11 http://updates-http.cdn-apple.com/download/ipsw

Successfully downloaded "kernelcache.release.iphone11"
```

## Credits

- [planetbeing/partial-zip](https://github.com/planetbeing/partial-zip) _(written in C)_
- [marcograss/partialzip](https://github.com/marcograss/partialzip) _(written in Rust)_

## License

MIT Copyright (c) 2018 blacktop
