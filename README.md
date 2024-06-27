# Go WebOSTV

A small Go library for interacting with webOS-enabled TVs. Based on [go-webos](https://github.com/kaperys/go-webos) and some functionality from [aiowebostv](https://github.com/home-assistant-libs/aiowebostv).

```go
dialer := websocket.Dialer{
    HandshakeTimeout: 10 * time.Second,
    // the TV uses a self-signed certificate
    TLSClientConfig: &tls.Config{InsecureSkipVerify: true},
    NetDial: (&net.Dialer{Timeout: time.Second * 5}).Dial,
}

tv, err := webos.NewTV(&dialer, "<tv-ipv4-address>")
if err != nil {
    log.Fatalf("could not dial TV: %v", err)
}
defer tv.Close()

// the MessageHandler must be started to read responses from the TV
go tv.MessageHandler()

// AuthorisePrompt shows the authorisation prompt on the TV screen
key, err := tv.AuthorisePrompt()
if err != nil {
    log.Fatalf("could not authorise using prompt: %v", err)
}

// the key returned can be used for future request to the TV using the
// AuthoriseClientKey(<key>) method, instead of AuthorisePrompt()
fmt.Println("Client Key:", key)

// see commands.go for available methods
tv.Notification("📺👌")
```
