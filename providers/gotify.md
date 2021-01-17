# Gotify

## Links

* [Android application](https://github.com/gotify/android/tree/unifiedpush)
* [Server](https://github.com/gotify/server)

## Using Gotify as a UnifiedPush provider

The version of the android application working as a distributor is on the [unifiedpush branch](https://github.com/gotify/android/tree/unifiedpush). This version is available on the repo.unifiedpush.org repository.

Gotify server needs a new reverse proxy rule to work.

### Nginx
The additional rule is as follow:
```
location /UP {
    access_by_lua_block{
        ngx.req.read_body()
        local req = ngx.req.get_body_data()
        local newreq, n, err = ngx.re.gsub(req, '\\\\', '\\\\')
        local newreq, n, err = ngx.re.gsub(newreq, '"', '\\"')
        local newreq, n, err = ngx.re.gsub(newreq, "^", "{\"message\":\"")
        local newreq, n, err = ngx.re.gsub(newreq, "$", "\"}")
        ngx.req.set_body_data(newreq)
    }

    proxy_set_header        Content-Type application/json;
    proxy_pass            http://127.0.0.1:8080/message;
    proxy_set_header            Host $host;

    # Force https
    if ($scheme = http) {
        rewrite ^ https://$server_name$request_uri? permanent;
     }
}
```
