## 20. Understanding servers

* Browser `More Tools` -> `Developer Tools` -> `Network` -> type `www.reddit.com` 

```
$ curl -v google.com
```

***

## 21. TCP server - write to connection

* HTTP runs on top of TCP
    - HTTP request -> TCP server
    - TCP server -> HTTP response

***

## 22. TCP server - read from connection using `bufio.Scanner`

***

## 23. TCP server - read from & write to connection

***

## 24. TCP server - code a client

***

## 25. TCP server - `rot13` & in-memory database

***

## 26. TCP server - HTTP request / response foundation hands-on exercise

***

## 27. TCP server - HTTP method & URI retrieval hands-on exercise

***

## 28. TCP server - HTTP multiplexer

***
***

# TCP server essentials

## Listen
 
[net.Listen](https://godoc.org/net#Listen)
``` Go
func Listen(network, laddr string) (Listener, error)
```

***

## Listener

[net.Listener](https://godoc.org/net#Listener)
``` Go
type Listener interface {
    // Accept waits for and returns the next connection to the listener.
    Accept() (Conn, error)

    // Close closes the listener.
    // Any blocked Accept operations will be unblocked and return errors.
    Close() error

    // Addr returns the listener's network address.
    Addr() Addr
}
```

***

## Connection

[net.Conn](https://godoc.org/net#Conn)
``` Go
type Conn interface {
    // Read reads data from the connection.
    Read(b []byte) (n int, err error)

    // Write writes data to the connection.
    Write(b []byte) (n int, err error)

    // Close closes the connection.
    // Any blocked Read or Write operations will be unblocked and return errors.
    Close() error

    // LocalAddr returns the local network address.
    LocalAddr() Addr

    // RemoteAddr returns the remote network address.
    RemoteAddr() Addr

    SetDeadline(t time.Time) error

    SetReadDeadline(t time.Time) error

    SetWriteDeadline(t time.Time) error
}
```

* NOTE: `Conn` implements both `Reader` and `Writer` types (interfaces). 

* Polymorphism in action!

***

## Read

[io.Reader](https://pkg.go.dev/io#Reader)
```Go
type Reader interface {
	Read(p []byte) (n int, err error)
}
```

[ioutil.ReadAll](https://godoc.org/io/ioutil#ReadAll)
``` Go
func ReadAll(r io.Reader) ([]byte, error)
```
***

## Write

[io.Writer](https://pkg.go.dev/io#Writer)
```Go
type Writer interface {
	Write(p []byte) (n int, err error)
}
```

[io.WriteString](https://godoc.org/io#WriteString)
``` Go
func WriteString(w Writer, s string) (n int, err error)
```

[fmt.Fprintln](https://godoc.org/fmt#Fprintln)
``` Go
func Fprintln(w io.Writer, a ...interface{}) (n int, err error)
```

[fmt.Fprintf](https://pkg.go.dev/fmt#Fprintf)
```
func Fprintf(w io.Writer, format string, a ...any) (n int, err error)
```

***

## Scanner

[bufio.NewScanner](https://godoc.org/bufio#NewScanner)
``` Go
func NewScanner(r io.Reader) *Scanner
```

``` Go
type Scanner struct {
    // contains filtered or unexported fields
}
```

[bufio.Scan](https://godoc.org/bufio#Scanner.Scan)
``` Go
func (s *Scanner) Scan() bool
```

[bufio.Text](https://godoc.org/bufio#Scanner.Text)
``` Go
func (s *Scanner) Text() string
```

***

## Dial

[net.Dial](https://godoc.org/net#Dial)
``` Go
func Dial(network, address string) (Conn, error)
```

***

# Read & Write

[io.Copy](https://godoc.org/io#Copy)
``` GO
func Copy(dst Writer, src Reader) (written int64, err error)
```

``` Go
package main

import (
	"io"
	"log"
	"os"
	"strings"
)

func main() {
	r := strings.NewReader("some io.Reader stream to be read\n")

	if _, err := io.Copy(os.Stdout, r); err != nil {
		log.Fatal(err)
	}

}
```

***

