# go-sdp

Go implementation of SDP (Session Description Protocol)

[![Build Status](https://travis-ci.org/pixelbender/go-sdp.svg)](https://travis-ci.org/pixelbender/go-sdp)
[![Coverage Status](https://coveralls.io/repos/github/pixelbender/go-sdp/badge.svg?branch=master)](https://coveralls.io/github/pixelbender/go-sdp?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/pixelbender/go-sdp)](https://goreportcard.com/report/github.com/pixelbender/go-sdp)
[![GoDoc](https://godoc.org/github.com/pixelbender/go-sdp?status.svg)](https://godoc.org/github.com/pixelbender/go-sdp)

## Features

- [x] SDP Encoder/Decoder
- [ ] SDP Answer/Offer Negotiation
- [ ] SDP Media Capabilities Negotiation

## Installation

```sh
go get github.com/pixelbender/go-sdp/...
```

## SDP Decoding

```go
package main

import (
	"github.com/pixelbender/go-sdp/sdp"
	"fmt"
)

func main() {
	desc, err := sdp.Parse(`v=0
o=alice 2890844526 2890844526 IN IP4 alice.example.org
s=Example
c=IN IP4 127.0.0.1
t=0 0
a=sendrecv
m=audio 10000 RTP/AVP 0 8
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000`)

	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(desc.Media[0].Formats[0].Codec)
	}
}
```

## SDP Encoding

```go
package main

import (
	"github.com/pixelbender/go-sdp/sdp"
	"fmt"
)

func main() {
	desc := &sdp.Description{
    		Origin: &sdp.Origin{
    		    Username: "alice",
    		    Address: "alice.example.org",
    		    SessionID: 2890844526,
    		    SessionVersion: 2890844526,
    		},
    		Session: "Example",
    		Connection: &sdp.Connection{ Address: "127.0.0.1" },
    		Media: []*sdp.Media{
    			{
    				Type: "audio",
    				Port: 10000,
    				Proto: "RTP/AVP",
    				Formats: map[int]*sdp.Format{
    					0: { Payload: 0, Codec: "PCMU", Clock: 8000 },
    					8: { Payload: 8, Codec: "PCMA", Clock: 8000 },
    				},
    			},
    		},
    		Mode: sdp.ModeSendRecv,
    	}

	fmt.Println(desc.String())
}
```

## Specifications

- [RFC 5389: Session Description Protocol](https://tools.ietf.org/html/rfc4566)
- [RFC 3264: Offer/Answer Model with SDP](https://tools.ietf.org/html/rfc3264)
- [RFC 5939: SDP Capability Negotiation](https://tools.ietf.org/html/rfc5939)
- [RFC 6871: SDP Media Capabilities Negotiation](https://tools.ietf.org/html/rfc6871)
- [RFC 5761: Multiplexing RTP Data and Control Packets on a Single Port](https://tools.ietf.org/html/rfc5761)
- [RFC 5888: SDP Grouping Framework](https://tools.ietf.org/html/rfc5888)
- [RFC 4145: TCP-Based Media Transport in SDP](https://tools.ietf.org/html/rfc4145)
- [RFC 4572: Connection-Oriented Media Transport over TLS in SDP](https://tools.ietf.org/html/rfc4572)
- [Draft: Using the SDP Offer/Answer Mechanism for DTLS](https://tools.ietf.org/html/draft-ietf-mmusic-dtls-sdp-14)
