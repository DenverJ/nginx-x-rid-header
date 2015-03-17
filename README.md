OVERVIEW
=======

nginx-x-rid-header is a small module which adds a request-scoped id (uuid) variable that can be used to correlate frontend logging/activity with backend logging/activity.

Currently supports NGX_LINUX, NGX_DARWIN & NGX_FREEBSD.

CREDITS
======

Brian Long (<mailto:newobj@gmail.com>, <mailto:brian@dotspots.com>, <http://newobj.net>)
Gábor Farkas (https://github.com/gabor)

USAGE
=====

1) Add `--add-module=../git/nginx-x-rid-header` to your nginx configure command. Add the necessary compiler & linker arguments as below if necessary.
  a) Debian/Ubuntu: Make sure you have libossp-uuid-dev installed. You should add `--with-cc-opt=-I/usr/include/ossp` and `--with-ld-opt=-lossp-uuid` to the configure command.
  b) Centos/Fedora/RHEL: Make sure you have libuuid-devel installed. You should add `--with-cc-opt=-I/usr/include/uuid` and `--with-ld-opt=-luuid` to the configure command.

2) Now run `make` and `make install`.

3) You now have access to a `$request_id` variable. Suggested use:

    log_format main  '$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent" "$http_x_forwarded_for" - $connection $request_time $upstream_cache_status $request_id';
    server {
        listen       80;
        server_name  example.com;
        location / {
            proxy_set_header x-exampledotcom-rid $request_id;
            proxy_pass   http://localhost:8080;
        }
    }

4) On your backend (8080), you can pull the request header `x-exampledotcom-rid`, and log it or tie it to whatever you may like. This makes it really easy to correlate backend exceptions or instrumentation with frontend http request logs.
