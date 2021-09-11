<a href="https://jgltechnologies.com/discord">
<img src="https://discord.com/api/guilds/844418702430175272/embed.png">
</a>

# aiohttp-ratelimiter

This library allows you to add a rate limit to your aiohttp.web app.


Install from git
```
python -m pip install git+https://github.com/Nebulizer1213/aiohttp-ratelimiter
```

Install from pypi
```
python -m pip install aiohttp-ratelimiter
```

<br>


Example

```python
from aiohttplimiter import limit, default_keyfunc
from aiohttp import web

app = web.Application()
routes = web.RouteTableDef()

# This endpoint can only be requested one time per second per IP address.
@routes.get("/")
@limit(ratelimit="1/1", keyfunc=default_keyfunc)
async def test(request):
    return web.Response(text="test")

app.add_routes(routes)
web.run_app(app)
```

<br>

You can exempt an IP from ratelimiting using the exempt_ips kwarg.

```python
from aiohttplimiter import limit, default_keyfunc
from aiohttp import web

app = web.Application()
routes = web.RouteTableDef()

# 192.168.1.245 is exempt from ratelimiting.
# Keep in mind that exempt_ips takes a set not a list.
@routes.get("/")
@limit(ratelimit="1/1", keyfunc=default_keyfunc, exempt_ips={"192.168.1.245"})
async def test(request):
    return web.Response(text="test")

app.add_routes(routes)
web.run_app(app)
```

<br>

If you plan on having the same keyfunc and exempt IPs for each endpoint using the Limiter class would be easier.

```python
from aiohttp import web
from aiohttplimiter import default_keyfunc, Limiter

app = web.Application()
routes = web.RouteTableDef()

limiter = Limiter(keyfunc=default_keyfunc, exempt_ips={"192.168.1.235"})

@routes.get("/")
@limiter.limit("1/5")
def home(request):
    return web.Response(text="test")

app.add_routes(routes)
web.run_app(app)
```



