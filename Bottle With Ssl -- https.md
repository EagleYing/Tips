# Bottle With Ssl -- https

### Basic

( You have to download your own ssl certificate for your domain first. I got my ssl.pem free through aliyun . )

Bottle is a fast, simple and lightweight [WSGI](http://www.wsgi.org/) micro web-framework for [Python](http://python.org/). We use it in local debugging very simply

```python
from bottle import Bottle, run

app = Bottle()

@app.route('/hello')
def printhello():
    return "Hello World!"

run(host = '0.0.0.0', port = 8080)
```

Then type `python test.py` on your terminal as usual and you can see `Hello world` on your local web page.

### Operates

WeChat web program needs `https` instead of  `http`, but general [bottle of python](http://bottlepy.org/docs/dev/) only support `http` local degug, we need to add some codes. I found following codes [Bottle with ssl](www.socouldanyone.com/2014/01/bottle-with-ssl.html) and it works well

```python
from bottle import Bottle, get, run, ServerAdapter

# copied from bottle. Only changes are to import ssl and wrap the socket
class SSLWSGIRefServer(ServerAdapter):
    def run(self, handler):
        from wsgiref.simple_server import make_server, WSGIRequestHandler
        import ssl
        if self.quiet:
            class QuietHandler(WSGIRequestHandler):
                def log_request(*args, **kw): pass
            self.options['handler_class'] = QuietHandler
        srv = make_server(self.host, self.port, handler, **self.options)
        srv.socket = ssl.wrap_socket (
         srv.socket,
         certfile='ssl.pem',  # path to certificate
         server_side=True)
        srv.serve_forever()

```

Then you need to add 

```python
srv = SSLWSGIRefServer(host="0.0.0.0", port=8080)
run(server=srv)
```

Run your .py file and type `https://yourdomain/hello...` and you will be pleasantly surprised to find a little lock before url.

### Add domain

Add your own domain on your wechat miniprogram public account ( developers domain configuration ).