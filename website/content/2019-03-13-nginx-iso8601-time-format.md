+++
title = "Nginx - logging milliseconds in ISO8601 format"
date = 2019-03-13
+++

Nginx has two variables for time that you can use in your logs.

* `$msec` - Unix timestamp with millisecond precision
  * E.g.: `2019-03-05T04:02:30+00:00`
* `$time_iso8601` - ISO8601 format time with second precision
  * E.g.: `1551758550.627`

See: [http://nginx.org/en/docs/http/ngx_http_log_module.html#log_format](http://nginx.org/en/docs/http/ngx_http_log_module.html#log_format)

Having milliseconds is really useful for debugging, but Unix timestamps are not easy for humans to read.

**Solution** - Log in [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) format with millisecond precision (which is a part of the specification)

E.g.: `2019-03-05T04:02:30.627+00:00`

## Problem

How do we log time in ISO8601 format with millisecond precision within Nginx if we don't have that available?

A quick internet search will show many others having the same issue without a simple solution.

**Option 1:** Log both variables, then use some other program to merge the milliseconds from `$msec` into the `$time_iso8601`. (E.g.: Logstash)

**Option 2:** Patch Nginx to have millisecond support in `$time_iso8601` (looks like it was proposed a few years ago without progress)

**Option 3:** Merge `$msec` and `$time_iso8601` *within Nginx*

## Option 3

After much effort, I finally found the right regex that would work within a default Nginx install.

``` Nginx
   map $time_iso8601 $time_iso8601_p1 {
     ~([^+]+) $1;
   }
   map $time_iso8601 $time_iso8601_p2 {
     ~\+([0-9:]+)$ $1;
   }
   map $msec $millisec {
     ~\.([0-9]+)$ $1;
   }

  log_format json escape=json '{'
    '"timestamp":"$time_iso8601_p1.$millisec+$time_iso8601_p2",'
    '"status":"$status"'
    '}';
```

Result (prettified through `jq`):

``` JSON
{
  "timestamp": "2019-03-05T04:02:30.627+00:00",
  "status": "200"
}
```