# git clone을 먼저한다. 아래 또는 이 리파지토리. 추후에는 이 리파지토리를 커스텀할 예정

```sh
$git clone https://github.com/Kong/docker-kong
$cd ./compose
```
# 실행. docker 설치가 되어있어야함. (docker-compose로 실행하는 방법)
```sh
$KONG_DATABASE=postgres docker-compose --profile database up
```

# 테스트
1. Add a Service using the Admin API
request
```sh
curl -i -X POST \
  --url http://localhost:8001/services/ \
  --data 'name=example-service' \
  --data 'url=http://mockbin.org'
```
response
```sh
HTTP/1.1 201 Created
Content-Type: application/json
Connection: keep-alive

{
   "host":"mockbin.org",
   "created_at":1519130509,
   "connect_timeout":60000,
   "id":"92956672-f5ea-4e9a-b096-667bf55bc40c",
   "protocol":"http",
   "name":"example-service",
   "read_timeout":60000,
   "port":80,
   "path":null,
   "updated_at":1519130509,
   "retries":5,
   "write_timeout":60000
}
```
2. Add a Route for the Service
request
```sh
   curl -i -X POST \
   --url http://localhost:8001/services/example-service/routes \
   --data 'hosts[]=example.com'
```
response
```sh
HTTP/1.1 201 Created
Content-Type: application/json
Connection: keep-alive

{
   "created_at":1519131139,
   "strip_path":true,
   "hosts":[
      "example.com"
   ],
   "preserve_host":false,
   "regex_priority":0,
   "updated_at":1519131139,
   "paths":null,
   "service":{
      "id":"79d7ee6e-9fc7-4b95-aa3b-61d2e17e7516"
   },
   "methods":null,
   "protocols":[
      "http",
      "https"
   ],
   "id":"f9ce2ed7-c06e-4e16-bd5d-3a82daef3f9d"
}
```

3. Forward your requests through Kong
```sh
curl -i -X GET \
  --url http://localhost:8000/ \
  --header 'Host: example.com'
```

ref.link (cp)
https://docs.konghq.com/gateway/latest/get-started/quickstart/configuring-a-service/
