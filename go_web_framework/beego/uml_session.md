``` plantuml
@startuml

interface Store {
    CRUD()
}

interface Provider {
    SessionRead(sid) Store
    SessionGC()
}
Provider ..> Store

class Manager {
    provider Provider
    config *ManagerConfig
    sessionID() string
}
Manager *--> Provider

class redis.Provider {
    poollist *redis.Client
    SessionRead(sid) Store
}
note top of redis.Provider
    1. SessionRead()方法从redis中读取session数据, 解析到内存里, 交给SessionStore操作.
    2. GC()为空, 通过设置redis过期时间来实现。
end note
redis.Provider ..|> Provider
class redis.SessionStore
redis.SessionStore ..|> Store
entity redisPool <<redis.Client>>
redis.Provider --> redisPool

class mem.MemProvider {
   
}
mem.MemProvider ..|> Provider
class mem.MemSessionStore
mem.MemSessionStore ..|> Store

@enduml
```