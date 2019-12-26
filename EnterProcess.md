## Activity
```
@startuml
start

:Browser checks local DNS cache;
if (is in cache) then
    :return focus to browser;
else
    :search for DNS;
    :update cache;
endif

:Check for page in cache;
if (is in cache) then
    :load page from cache;
else
    :Send HTTP request;
    :Get http response;
    :Parse HTTP response body;
endif
:Create DOM tree;
:Run JS & CSS Interpreter;
:Compile page;

stop
@enduml
```


## Sequence
```
@startuml

actor User
boundary "Browser" as GUI
entity "DNS Server" as DNS
entity Server

activate User
    User -> GUI : Enter query
deactivate User
activate GUI
    GUI -> GUI : Check local DNS cache
activate DNS
    GUI -> DNS : Search for DNS
    DNS -> GUI : Return IP
deactivate DNS
    GUI -> GUI : Check local cache for page
activate Server
    GUI -> Server : Send HTTP request
    Server -> GUI : Return HTTP response
    deactivate Server
    GUI -> GUI : Parse response
    GUI -> GUI : Compile page
    GUI -> User : Show page
    deactivate GUI
@enduml
```
## Component

```
@startuml

frame Browser {
    URL --> [Browser]
    [Browser] --> Rendered_page
}

frame Server {
    [Browser] --> HTTP_request
    HTTP_response -> [Browser]
    HTTP_request -> [Server]
    [Server] --> HTTP_response
}

frame DNS {
    [Browser] --> DNS_request
    DNS_request -->[DNS_provider]
    [DNS_provider] --> DNS_response
    DNS_response --> [Browser]
}

@enduml
```
