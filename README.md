# is-process-alive
Small, generic program to check if a particular service is still running

## Why?
Sometimes you just want to keep tabs on a service and automatically restart it if it dies for any reason.

## Dependencies
Make sure you have `nmap` and `mail` before using. That is all.

## Running
Run as a service by creating a `.config` file in `/etc/init`
or simply:

```
./run <PORT>
```

## TODO
Not generic enough to apply to any other process besides the Pizza My Mind server yet.

- Have script read "message to be emailed in case of error" from a specific file instead of having it be hard-coded.
- Have a different file where recipient email addresses are stored.
- Have a way to set "frequency" at which port is pinged.
- Have a way to set `service name` or `restart procedure` so that service being "watched" can be restarted.
