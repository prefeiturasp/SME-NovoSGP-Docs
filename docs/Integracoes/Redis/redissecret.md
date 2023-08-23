```js title='Modelo de Secret'
{
    "ConfiguracaoCache": {
        "UtilizaRedis": true
    },
    "Redis": {
        "EndPoint": "localhost:6379",
        "Prefixo": "DEV_",
        "Proxy": "Twemproxy",
        "SyncTimeout": "5000"
    },
    "ThreadPool": {
        "CompletionPortThreads": 50,
        "WorkerThreads": 50,
    },
}
```