# Топики в Kafka

## Примеры данных в топиках


### Топик src.clients

```
{
  "topic": "src.clients",
  "key": "CL001",
  "value": {
    "type": "CLIENT",
    "data": {
      "client_id": "CL001",
      "gender": "F",
      "registration_date": "2023-01-15"
    }
  },
  "offset": 100,
  "partition": 0,
  "timestamp": 1629153600000,
  "headers": {
    "contentType": "application/json"
  }
}
```

### Топик src.orders

```
{
  "topic": "src.orders",
  "key": "CL001",
  "value": {
    "type": "ORDER",
    "data": {
      "order_id": "ORD001",
      "client_id": "CL001",
      "item_id": "ITEM001",
      "status": "done"
    }
  },
  "offset": 50,
  "partition": 1,
  "timestamp": 1629153610000,
  "headers": {
    "contentType": "application/json"
  }
}
```

### Топик src.items

```
{
  "topic": "src.items",
  "key": "IT001",
  "value": {
    "type": "ITEM",
    "data": {
      "item_id": "ITEM001",
      "item_name": "Smartphone",
      "item_price": 129999.00
    }
  },
  "offset": 200,
  "partition": 2,
  "timestamp": 1629153620000,
  "headers": {
    "contentType": "application/json"
  }
}
```

### Топик src.subscriptions

```
{
  "topic": "src.subscriptions",
  "key": "CL001",
  "value": {
    "type": "SUBSCRIPTION",
    "data": {
      "subscription_id": "SUB001",
      "client_id": "CL001",
      "subsc_status": "Active"
    }
  },
  "offset": 150,
  "partition": 0,
  "timestamp": 1629153630000,
  "headers": {
    "contentType": "application/json"
  }
}
```

### Топик src.favorites

```
{
  "topic": "src.favorites",
  "key": "CL001",
  "value": {
    "type": "FAVORITE",
    "data": {
      "item_id": "IT001",
      "client_id": "CL001"
    }
  },
  "offset": 250,
  "partition": 1,
  "timestamp": 1629153640000,
  "headers": {
    "contentType": "application/json"
  }
}
```

### Топик src.tracker

```
{
  "topic": "src.tracker",
  "key": "CL001",
  "value": {
    "type": "VISIT",
    "data": {
      "track_id": "TRK001",
      "client_id": "CL001",
      "visit_date": "2023-06-12T10:30:00Z",
      "visit_platform": "Web"
    }
  },
  "offset": 300,
  "partition": 2,
  "timestamp": 1629153650000,
  "headers": {
    "contentType": "application/json"
  }
}
```

В этих примерах добавлены следующие технические поля:

`offset`: уникальный идентификатор сообщения в партиции.

`partition`: номер партиции, к которой принадлежит сообщение.

`timestamp`: время создания сообщения в миллисекундах с начала эпохи Unix.

`headers`: дополнительные заголовки сообщения, например, тип содержимого.
