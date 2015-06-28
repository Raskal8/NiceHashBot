# NiceHashBot
NiceHash bot for automatic order management.

- [Features](#features)
- [How to run?](#run)
- [How to compile?](#compile)
- [Tips for programmers](#tips)
- [Create custom order handler](#handler)

# <a name="features"></a> Features

- Create new orders (including when 2FA is turned on)
- Automatically manage orders for:
    * price adjustment - keep price as low as possible and sustain wanted speed, but keep price below max specified
    * refilling - automatically refill order when it is nearly depleted
    * re-creation - automatically create new order if order is removed by the system (timeout or any other reason)
- Ability to adjust max price and speed limit for each monitored order
- Pool manager to easily define new pools, remove them or use for orders
- Console window showing all important events or errors
- Custom order handlers

# <a name="run"></a> Instructions on how to run

- Download binaries from here: https://github.com/nicehash/NiceHashBot/releases
- Extract zip archive
- Run NiceHashBot.exe
- Note: .NET Framework 2.0 or higher is required. No additional installations are needed if you use Windows 7 or later. Install .NET Framework 2.0 if you use Windows XP or lower.

# <a name="compile"></a> Instructions on how to compile

- Use Visual Studio 2010 or later
- Open project in Visual Studio
- Rebuild & run

# <a name="tips"></a> For programmers

You can easily create your own bot software by reusing this code. NiceHashBotLib is the core of everything - modifications of this library should not be needed. Examine NiceHashBot project to see how NiceHashBotLib is used. You can create your own bot with little coding knowledge by just calling certain methods of OrderInstance class such as 'SetMaximalPrice', 'SetLimit' and 'Stop'. With these methods you have full control of what is happening with the order - NiceHashBotLib takes care of evaluating current orders and adjusting lowest possible price.

# <a name="handler"></a> How to create custom order handler?

NiceHashBot allows you to programmatically adjust 'MaxPrice' and 'Limit' for each order by creating custom C# DLL. Take a look at existing example: https://github.com/nicehash/NiceHashBot/blob/master/src/HandlerExample/HandlerClass.cs

DLL has to contain class 'HandlerClass' and public static method 'HandleOrder' inside that class. Method 'HandleOrder' is called two times per second. It gets parameters statistics of order, current maximal price and current speed limit. Inside the method, you should perform all the logic related to calculation of maximal price and speed limit. After you finish with calculation, just assign new numbers to 'MaxPrice' and/or 'NewLimit' or leave them intact, if you do not wish to change them. The provided example shows, how this is done by calling CoinWarz API, deserializing JSON and setting maximal price according to performed profitability calculation.

You can easily create own order handler. You need to have Visual Studio 2010 or later. Make copy of HandlerExample project (optionally you can rename it), modify code inside 'HandleOrder' method. You can add your own classes, methods and properties, just remember that 'HandleOrder' is called twice per second when NiceHashBot is running and you have set this handler to be order handler for your order. When finished with programming, rebuild project to create DLL. Use this DLL as order handler.
