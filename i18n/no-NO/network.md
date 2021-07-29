# Nettverk

## Cookies

The `rod.Browser` and `rod.Page` both has several helper methods for setting or getting cookies.

## Hijack requests

You can use Rod to hijack any HTTP or HTTPS traffic.

The entire process of hijacking one request:

```text
   nettleser --req-> rod ---> server ---> rod --res-> nettleser
```

When the browser wants to send a request to a server, it will send the request to Rod first, then Rod will act like a proxy to send the request to the actual server and return the response to the browser. The `--req->` and `--res->` are the parts that can be modified.

For example, to replace a file `test.js` response from the server we can do something like this:

```go
nettleser := rod.New().MustConnect()

router := browser.HijackRequests()

router.MustAdd("*/test.js", funksjoner (ctx *rod.Hijack) {
    ctx.MustLoadResponse()
    ctx.Response.SetBody(`konsoll. og(«js fil erstattet»)
})

gå router.Run()

side := browser.MustPage("https://test.com/")

// Hijack forespørsler under omfanget for en side
page.HijackRequests()
```

For more info check the [hijack tests](https://github.com/go-rod/rod/blob/master/hijack_test.go)
