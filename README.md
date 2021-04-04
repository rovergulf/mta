# rovergulf/mta

### Golang Mail Transfer Agent

**MTA** is a simple and efficient package to send emails.

**MTA** can only send emails using an SMTP server. But the API is flexible and it is easy to implement other methods for
sending emails using a local Postfix, an API, etc.

## Features

**MTA** supports:

- Attachments
- Embedded images
- HTML and text templates
- Automatic encoding of special characters
- SSL and TLS
- Sending multiple emails with the same SMTP connection

## Documentation

https://godoc.org/github.com/rovergulf/mta

## Download

```shell
# may require GOPRIVATE environment to be set
go get github.com/rovergulf/emailia
```

## Examples

See the [examples in the documentation](https://rovergulf.net/docs/golang/mta).

## FAQ

### x509: certificate signed by unknown authority

If you get this error it means the certificate used by the SMTP server is not considered valid by the client running
MTA. As a quick workaround you can bypass the verification of the server's certificate chain and host name by using
`SetTLSConfig`:

```go

package main

import (
	"crypto/tls"

	"github.com/rovergulf/mta"
)

func main() {
	d := mta.NewDialer("smtp.example.com", 587, "user", "123456")
	d.TLSConfig = &tls.Config{InsecureSkipVerify: true}

	msg := mta.NewMessage()
	msg.SetAddressHeader("From", "sender@example.com", "Test MTA Sender")
	msg.SetAddressHeader("To", "recipient@example.com", "")
	msg.SetHeader("Subject", "Hello!")
	msg.SetHeader("MIME-version: 1.0")
	msg.SetBody("text/plain", "Hello Gophers!")

	if err := d.DialAndSend(msg); err != nil {
		// log error
	}
	// Send emails using d.
}
```

Note, however, that this is insecure and should not be used in production.
