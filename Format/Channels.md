# Definice CAN a Ethernet kanálů
## CAN
```json
{
    "channels":[
        {
            "name":"can0",
            "bitrate":250000
        },
        {
            "name":"can1",
            "bitrate":500000
        }
    ]
}
```
## Ethernet
Kanály ethernetu nejsou definovány zvlášť, ale jsou specifikovány v [routách](Routes.md).