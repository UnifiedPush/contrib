# Matrix

## Links

* [Project](https://matrix.org)
* [Push gateway specs](https://matrix.org/docs/spec/push_gateway/unstable#post-matrix-push-v1-notify)

## Matrix Gateway

### Nginx

Until [MSC2970](https://github.com/matrix-org/matrix-doc/pull/2970) is figured out we unfortunately
need another re-write proxy. 

The one at https://matrix.gateway.unifiedpush.org is publically available, however you can easily self-host it.

Here is a rewrite proxy, change `accepted` with your own endpoint, do not forget the last `/` :

```
resolver 127.0.0.1;

location /_matrix/push/v1/notify {
    set $target '';
    if ($request_method = GET ) {
        return 200 '{"gateway":"matrix"}';
    }
    access_by_lua_block {
        local cjson = require("cjson")
        ngx.req.read_body()
        local body = ngx.req.get_body_data()
        local parsedBody = cjson.decode(body)
        local accepted = "https://relay.example.tld/"
        ngx.var.target = parsedBody["notification"]["devices"][1]["pushkey"]
        ngx.req.set_body_data(body)
        if(string.sub(ngx.var.target,1,string.len(accepted))~=accepted) then ngx.var.target="http://0.0.0.0/"
        end
    }
    proxy_set_header Content-Type application/json;
    proxy_set_header Host $host;
    proxy_pass $target;
    subs_filter_types * ;
    subs_filter '.*' '{}' r;
}

```

