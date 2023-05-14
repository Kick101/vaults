#### Scope
![[Pasted image 20230512111709.png]]

---
#### Tech
Akamai Fastly Netscaler Nginx+Lua Nginx Kubernetes NGM API Cassandra Canary Solr Varnish Redis SQL "Spark Streaming" Kafka "Apache Hbase"

__Languages__
.net, Node.js, Java, php

---
### Dorking
#### Shodan
https://173.213.4.101/ats/

#### Google Dorks Sensitive
```txt
site:newegg.com "ext:asp | ext:aspx | ext:php | ext:jsp" intext:password
```
- Landpages of products, for mobile view?
- `/LandingPage/OverviewContent4Moblie.aspx?ItemNumber=0BD-001N-002F5&device=site`

```txt
site:newegg.com "ext:doc | ext:docx | ext:odt | ext:pdf" intext:confidential
```
```txt
site:newegg.com "ext:xls | ext:xlsx | ext:csv" intext:credit card
```

---
#### 403
http://e11ssltestm.newegg.ca/
http://e11stgtest.newegg.ca/
http://e11ssltest.newegg.ca/
http://e11stgtest-secure-m.newegg.ca/
http://e11wwwtest.newegg.ca/
http://e11stgtest-secure.newegg.ca/
http://i.promo.newegg.ca/
http://live.stream.newegg.com/
https://logistics.newegg.com/intro/bridge/

#### Blank
https://l.promo.newegg.ca/ats/
https://s.promo.newegg.ca/ats/
https://w.promo.newegg.ca/ats/
https://x.promo.newegg.ca/ats/
https://l.promo-flash.newegg.com/ats/

#### Subs
https://kb.newegg.ca/contact-us
https://partner.newegg.com/
https://developer.newegg.com/
https://promotions.newegg.com
https://images10.newegg.com
https://ec-apis.newegg.com
https://staffing.newegg.com
https://partner.newegg.com
https://vendorportal.newegg.com
https://secure.newegg.com

#### API subs
- https://stagingapis.newegg.com/ [403]
- sandboxapis.newegg.com [403]
- api.newegg.com
- apis.newegg.com
- https://as2.newegg.com [403]
- https://push.newegg.com/ [403]

---
#### secure
- `appId: 107630`
- 


---
### API Testing
#### www.newegg.com
- /api
- /home/api
- /tools/api
- /product/api
- /mktplace/api
- /search/api

#### secure.newegg.com
- /account/api
- /identity/api
- /notifications/api
- /order/api
- /shop/api
- /wishlist/api
- /api
- /returns/api

##### Wishlist
- /wishlist/api/{list-type}/{ID}/wishlistPageDetailInfo
- /wishlist/api/deletewishList/{ID}
- /wishlist/api/28444076/follow
- /wishlist/api/duplicateMyWishlist
- ID -> `WishListNumber:28441376` - JSON Resp

__Variables__
- list-type: 0 (user/private), 2 (public)

#### Notes
__Auth__
- /identity/api/SignIn?ticket=\&timestamp=&nonce=&appId=107630
```json
{"ln_y_pVP7BBFwr89adTOHUMI0N7Z":"anacletobarzetti+neweggB@gmail.com","raNi-cxgnM8kvD-VMx0n":"bVTX1tFvBt2fK8oOj6iqPtx69U79McRq+lzFrMxO63wucpK2XIq0v8riup5zSVggvXs2zoiVhdGNzCXfVTtXBEOGDNCVVubAoyzB5X8GoGnhMU1ag48zzMEYxqx6ScNoBoDzqaS/ST+Me2quoQpA5r1COQrL6rIx1rZwR7XGzC3fIOeieQnVpJkWCrbju6LiJtZUnYjPodumWAPhL8z4gh+pSlR/kdEqUanzbjpZuSL0BeNoC3AY8DvHabuGgdGPp5leFcpou+0+4SekTbHvFFyvUSkJbUcWh3Q1rBR+Sxr9FrYt2ehISkO0+dA7KHtYF1DqUNoG9fW2siTPciaRBQ==","FuzzyChain":"4a82115966aa448fb583ec9c26912819","S":"v0DlxZPhv8KjyLWtWGLp7MLdOVWHTY0cN4cjsd00Xm5A1MoH+ROxM1WEzIZUBC0sPyz0GEsL0rw6hSprREapTZrGUVbevy59AnH/fVM+vPtSlUByKfTLqRJpMttc/EP7BWFkJTFBn9BSRXVnuSdfaArz76ZRV7uT6awVOZTPTGeO4ljNKkvTjAZOS+Gtf/MivXypTKsJc5qa59KYaL2/+wgVz2FDjypVVI2Xutpx4XpvYpEIVT/aZN26kxsxrJfy7Dfj2E9fk2sLGPvL8n96EXksdCVuzHBIrERazg95QvizCAmvlJzOnHi+6RJ/RequlVehxDzrpFCv5FBuiJho8g==","d":"HGe3Z1yZOmHn0jnU7yKYGVUkGQjC20BQm7Y3OTQbUHaQlfEOVSpcSlBvMUfjIII5SE5dF0X2dXH9zxkue+GRmKpL1A/os7AAqf6WOvhKdntAgWYDbPRAaQOqN161rKEMdYEWeogaLwpi7BEItuPEvyMP3Tt8MXxvXnDfnU/Jq6mBf3U89WyqGiJ8CGdB+4MDeDRdi/3Fiw6elUNcQi8WOMy6WKnX5baRgsZ8P5/x1/w//FQ9IfyWJpd2yZK3yxZ2o1kdgvXgz6zcl5l2QTAGkKAGeJKO6RE9Ydr37TFON+LORkCvjtlis2Dp0rYUgyBEuV5WXsrL5YzPGySPo7EdHBM+8kh8Moj4fU49wW+BP3U="}
```

__Allowed Headers__
```http
Content-Type, x-auth-captcha, x-gateway-profileid, x-lastvisittime, x-location, x-login-status, x-server-id, x-serverid, x-servicechaintraceid, x-bswitch, x-client-referrer, Authorization, DNT, X-Mx-ReqToken, Keep-Alive, User-Agent, X-Requested-with, If-Modified-Since, Cache-Control, Content-Type, Authorization, token, x-logintoken, x-accesstoken, X-SessionID, terribleterribledamage
```
