#### Scope
![[Pasted image 20230512111709.png]]

---
#### Tech
- AkamaiGHost

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

#### Blank
https://l.promo.newegg.ca/ats/
https://s.promo.newegg.ca/ats/
https://w.promo.newegg.ca/ats/
https://x.promo.newegg.ca/ats/

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


