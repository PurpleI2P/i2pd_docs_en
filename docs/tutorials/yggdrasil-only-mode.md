For a router talking to other routers and reseed servers through Yggdrasil only.  
Start yggdrasil first and make sure you're running from a fresh i2pd installation. Create `~/.i2pd/i2pd.conf`

```
daemon=true # note: remove it if you running on Windows!
ipv4=false
ipv6=false
ssu=false
ntcp2.enabled=false
meshnets.yggdrasil=true  
```

If you use Yggdrasil subnet and want to bind your router to a particular address specify it with
`meshnets.yggaddress=` your local yggdrasil address

Start i2pd, it will also reseed from an yggdrasil reseed server. No clearnet communication is involved. 
