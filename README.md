# go-nmap

## Overview
Nmap XML parsing library for Go

## Examples
Simple example that reads in a Nmap XML file and outputs IP and open port.

```go
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/lum8rjack/go-nmap"
)

func main() {
	// Read in the nmap file
	data, err := os.ReadFile("nmap-output.xml")
	if err != nil {
		log.Fatal(err)
	}

	// Parse the data
	n, err := nmap.Parse(data)
	if err != nil {
		log.Fatal(err)
	}

	// Get the list of hosts
	hosts := n.Hosts

	// Loop through each host
	for _, h := range hosts {
		// Loop through all of the ports
		for _, p := range h.Ports {
			// Make sure the port was open
			if p.State.State == "open" {
				h := h.Addresses[0].Addr
				port := p.PortId
				fmt.Printf("%s: %d\n", h, port)
			}
		}
	}
}
```

Once compiled, the output looks similar to:

```bash
./example 
10.10.1.51: 22
10.10.1.51: 53
10.10.1.51: 80
10.10.1.51: 443
10.10.1.70: 445
10.10.1.70: 3389
10.10.1.72: 22
10.10.1.72: 80
10.10.1.72: 443
10.10.1.72: 2049
```
