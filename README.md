# bitflyer-api-dotnet-client

bitFlyer APIs Client Library for .NET https://lightning.bitflyer.jp/docs

Install
---
* PM> Install-Package [BitFlyer.Apis](https://www.nuget.org/packages/BitFlyer.Apis)

Private API needs your API Key and Secret. If you use Private API, I recommend you to clone this repository and use it.

Quick Start
---
### HTTP Public API

```csharp
var ticker = await PublicApi.GetTicker(ProductCode.BtcJpy);
```

### HTTP Private API

You can create API Key and API Secret here.
https://lightning.bitflyer.jp/developer

```csharp
var api = new PrivateApi("Your API Key", "Your API Secret");
var result = await api.SendChildOrder(new SendChildOrderParameter
{
    ProductCode = ProductCode.FxBtcJpy,
    ChildOrderType = ChildOrderType.Limit,
    Side = Side.Buy,
    Price = 10000,
    Size = 0.01,
    MinuteToExpire = 10000,
    TimeInForce = TimeInForce.GoodTilCanceled
});
```

### Realtime API

```csharp
class Program
{
    static async Task Main(string[] args)
    {
        var api = new RealtimeApi();
        
        await api.Subscribe<Ticker>(RealtimeChannel.TickerFxBtcJpy, OnReceive, OnConnect, OnError);
        
        Console.ReadKey();
    }
    
    static void OnConnect()
    {
        Console.WriteLine("connected.");
    }
    
    static void OnReceive(Ticker data)
    {
        Console.WriteLine(data);
    }
    
    static void OnError(string message, Exception ex)
    {
        Console.WriteLine(message);
        if (ex != null)
        {
            Console.WriteLine(ex);
        }
    }
}
```

### Realtime API (Private)

```csharp
class Program
{
    static async Task Main(string[] args)
    {
        var key = "Your API Key";
        var secret = "Your API Secret";
        var api = new RealtimeApi();
        
        await api.Subscribe<ChildOrderEvents[]>("child_order_events", OnReceive, OnConnect, OnError, key, secret);
        
        Console.ReadKey();
    }
    
    static void OnConnect()
    {
        Console.WriteLine("connected.");
    }
    
    static void OnReceive(ChildOrderEvents[] data)
    {
        Console.WriteLine(data[0]);
    }
    
    static void OnError(string message, Exception ex)
    {
        Console.WriteLine(message);
        if (ex != null)
        {
            Console.WriteLine(ex);
        }
    }
}
```
---
