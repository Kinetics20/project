## generate ssl key and cert files:
```bash
openssl req -x509 --nodes -days 365 -newkey rsa:2048 -keyout key.pem -out cert.pem

```


## add description for cert:
```bash
uvicorn 'main:app' --host 0.0.0.0 --port 8002 --ssl-keyfile=./.cert/key.pem --ssl-certfile=./.cert/cert.pem --reload

```
