by Khaydarov MARUFJON      ---- MARVELL IT ACADEMY ----
===============
node-macaddress 
=====

[![Build Status](https://travis-ci.org/scravy/node-macaddress.svg?branch=master)](https://travis-ci.org/scravy/node-macaddress)

Retrieve [MAC addresses](https://en.wikipedia.org/wiki/MAC_address) in Linux, OS X, and Windows.

A common misconception about MAC addresses is that every *host* had *one* MAC address,
while a host may have *multiple* MAC addresses – since *every network interface* may
have its own MAC address.

This library allows to discover the MAC address per network interface and chooses
an appropriate interface if all you're interested in is *one* MAC address identifying
the host system (see `API + Examples` below).

A common misconception about this library is that it reports the mac address of the client that accesses some kind of backend. It does not. Reporting the mac address is not in any way similar to reporting the IP address of the client that accesses your application. This library reports the mac address of the server the application is running on. This useful for example for distributed/scaled applications, for example when generating UUIDs.

Also it seems to be worth noting that this library is not intended to be used in a browser. There is no web api reporting the mac address of the machine (and that is a good thing).

**Features:**

+ works on `Linux`, `Mac OS X`, `Windows`, and on most `UNIX` systems.
+ `node ≥ 0.12` and `io.js` report MAC addresses in `os.networkInterfaces()`
  this library utilizes this information when available.
+ also features a sane replacement for `os.networkInterfaces()`
  (see `API + Examples` below).
+ works with stoneage node versions ≥ v0.8 (...)
+ Promise support

Usage
-----

```
npm install --save macaddress
```

```JavaScript
var macaddress = require('macaddress');
```

API + Examples
--------------

    (async)  .one(iface, callback) → string
    (async)  .one(iface)           → Promise<string>
    (async)  .one(callback)        → string
    (async)  .all()                → Promise<{ iface: { type: address } }>
    (async)  .all(callback)        → { iface: { type: address } }
    (sync)   .networkInterfaces()  → { iface: { type: address } }

---

### `.one([iface], callback)`

Retrieves the MAC address of the given `iface`.

If `iface` is omitted, this function automatically chooses an
appropriate device (e.g. `eth0` in Linux, `en0` in OS X, etc.).

**Without `iface` parameter:**

```JavaScript
macaddress.one(function (err, mac) {
  console.log("Mac address for this host: %s", mac);  
});
```

or using Promise

```JavaScript
macaddress.one().then(function (mac) {
  console.log("Mac address for this host: %s", mac);  
});
```

```
→ Mac address for this host: ab:42:de:13:ef:37
```

**With `iface` parameter:**

```JavaScript
macaddress.one('awdl0', function (err, mac) {
  console.log("Mac address for awdl0: %s", mac);  
});
```
or using Promise

```JavaScript
macaddress.one('awdl0').then(function (mac) {
  console.log("Mac address for awdl0: %s", mac);  
});
```

```
→ Mac address for awdl0: ab:cd:ef:34:12:56
```

---

### `.all(callback)`

Retrieves the MAC addresses for all non-internal interfaces.

```JavaScript
macaddress.all(function (err, all) {
  console.log(JSON.stringify(all, null, 2));
});
```
or using Promise

```JavaScript
macaddress.all().then(function (all) {
  console.log(JSON.stringify(all, null, 2));
});
```

```JavaScript
{
  "en0": {
    "ipv6": "fe80::cae0:ebff:fe14:1da9",
    "ipv4": "192.168.178.20",
    "mac": "ab:42:de:13:ef:37"
  },
  "awdl0": {
    "ipv6": "fe80::58b9:daff:fea9:23a9",
    "mac": "ab:cd:ef:34:12:56"
  }
}
```

---

### `.networkInterfaces()`

A useful replacement of `os.networkInterfaces()`. Reports only non-internal interfaces.

```JavaScript
console.log(JSON.stringify(macaddress.networkInterfaces(), null, 2));
```

```JavaScript
{
  "en0": {
    "ipv6": "fe80::cae0:ebff:fe14:1dab",
    "ipv4": "192.168.178.22"
  },
  "awdl0": {
    "ipv6": "fe80::58b9:daff:fea9:23a9"
  }
}
```

#   g e t - m a c c a d d r e s s - e x p r e s s - j s - 
 
 # get-maccaddress-express-js-
