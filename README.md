# Speedtest Implementation for .NET
This is a very simple implementation for testing users newtork speeds. It uses [speedtest.net](http://www.speedtest.net/)'s servers and logic.
There is no official API for [speedtest.net](http://www.speedtest.net/) but i've extracted some information from developer console.

[Speedtest.net](http://www.speedtest.net/) has a list of ISP's test servers. You can access via this url. When you send a POST request to this url, it returns a serverlist as json array that nearest to you.
I think it locates you by your IP address.
```
http://www.speedtest.net/api/js/servers?engine=js
```

You can select a server from the list for testing. I've selected first item, you can see in code. [Speedtest.net](http://www.speedtest.net/) may select server by preffered parameter or randomizely.
There is an example json object that contains server properties.
```
{
  "url": "http://hiztesti.isimkayit.com/speedtest/upload.php",
  "lat": "40.8520",
  "lon": "29.8780",
  "distance": 16,
  "name": "Kocaeli",
  "country": "Turkey",
  "cc": "TR",
  "sponsor": "IsimKayit.com",
  "id": "6297",
  "preferred": 0,
  "host": "hiztesti.isimkayit.com:8080"
}
```



## Calculating Download Speed
You must combine host parameter and url parameters in below for calculating download speed. First parameter should be a GUID string. Second parameters should be chunksize in bytes for your download.
The server will send a octet-stream that sized by your preferred size. [Speedtest.net](http://www.speedtest.net/) prefers 25000000 (approximately 23,4MB).

Note : GUID should be different for each request.
```
download?nocache={0}&size={1}
````

Then you should have an url lik below for example. This means, when you send a GET request to this url, server will send you 25 million bytes.
```
http://hiztesti.isimkayit.com:8080/download?nocache=ef43485f-1bbc-4507-88a7-eb4ef8e536cb&size=25000000
```

You can use stopwatch for counting download time. For example; if download finished in 15 seconds and your download size is 25000000. You can use this formula to calculate your speed in MB/s.
```
speed = (25000000 / 15) / (1024 * 1024) // get speed in MB/s
```

I prefer Mbits to show download and upload speed. For this you should multiply your result with 8. Because 1 Byte = 8 bits.



## Calculating Upload Speed
This is almost same with upper logic. You must combine host parameter and url parameters in below for calculating upload speed. There is one parameter. It should be GUID string.

Note : GUID should be different for each request.
```
upload?nocache={0}
```

Then you should have an url like below for example. 
```
http://hiztesti.isimkayit.com:8080/upload?nocache=ef43485f-1bbc-4507-88a7-eb4ef8e536cb
```

You can send POST request to this url but before, you must add some parameters to your request.
```
Content-Length = 4000000 // Length is in bytes. You can increase or decrease it. I prefer 4MB, so 4000000. It is enough.
Content-Type = application/octet-stream
```

You can use stopwatch for counting upload time. For example; if upload finished in 100 seconds and your upload size is 4000000. You can use this formula to calculate your speed in MB/s.
```
speed = (4000000 / 100) / (1024 * 1024) // get speed in MB/s
```

I prefer Mbits to show download and upload speed. For this you should multiply your result with 8. Because 1 Byte = 8 bits.



# License
MIT © [Hasan Cemre Ok](https://github.com/hasancemreok)
