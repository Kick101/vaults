__Domain Admins__
- Administrator
- svc_admin

__Forests__
- moneycorp.local
-  eu.eurocorp.local 

__Domains & DC__
- moneycorp.local :  mcorp-dc.moneycorp.local [172.16.1.1]
- \*dollarcorp.moneycorp.local :  dcorp-dc.dollarcorp.moneycorp.local [172.16.2.1]
- us.dollarcorp.moneycorp.local :  us-dc.us.dollarcorp.moneycorp.local [172.16.9.1]
- eurocorp.local : eurocorp-dc.eurocorp.local [ 172.16.15.1]
- eu.eurocorp.local : 

#### Pwned Users
- student30/dcorp-std30 [Admin]
- student30/dcorp-adminsrv [Admin]
- ciadmin/dcorp-ci [Admin] (jenkins)
- srvadmin/dcorp-mgmt
- svcadmin/dcorp-mgmt [DA]

##### DOLLARCORP.MONEYCORP.LOCAL
- Domain SID: S-1-5-21-719815819-3726368948-3917688648
```txt
Username : srvadmin
aes256_key: 145019659e1da3fb150ed94d510eb770276cfbd0cbd834a4ac331f2effe1dbb4
rc4_hmac_nt: a98e18228819e8eec3dfa33cb68b0728

Username : appadmin
Password : *ActuallyTheWebServer1
aes256:       68f08715061e4d0790e71b1245bf20b023d08822d2df85bff50a0e8136ffe4cb
rc4_hmac_nt: d549831a955fee51a43c83efb3928fa7

Username : websvc
Password : AServicewhichIsNotM3@nttoBe
aes256_hmac : 2d84a12f614ccbf3d716b8339cbbe1a650e5fb352edc8e879470ade07e5412d7
rc4_hmac_nt : cc098f204c5887eaa8253e7c2749156f

Username : svcadmin
Password : *ThisisBlasphemyThisisMadness!!
aes256_hmac       6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011
rc4_hmac_nt: b38ff50264b74508085d82c69794a4d8

Username: Administrator (DSRM)
ntlm: a102ad5753f4c441e3af31c97fad86fd
   
Username : DCORP-MGMT$
Password : 4?PhChKP(`?yW`E8=VM2QI13O!i*3Q?WVB"X)=>Il3=AczJ0^T!X]r&:&yG41`*/$^4+EeZ07?zF2Z3`:[Jd*F/z_P`p6B9XH^g$*mXIQMXY(Sc?3\A6ICrX
rc4: 0878da540f45b31b974f73312c18e754

Username : dcorp-std30$
aes256 :  a11df8c7e74acfdfc117808c9690b6d64b884f0e2159ad2d8f49d6106438c6c8

Username : dcorp-dc$
aes256 key:  0695c2f48dc8ee0410182ef09177a10f0aa16b3cb2e9f98ad1c85c34964c48a5
rc4_hmac_nt: 9c87133f864ed68b56e6a09c7755ef80

Username : dcorp-adminsrv$
aes256key :  e9513a0ac270264bb12fb3b3ff37d7244877d269a97c7b3ebc3f6f78c382eb51

Username : dcorp-appsrv$
aes256 : e9513a0ac270264bb12fb3b3ff37d7244877d269a97c7b3ebc3f6f78c382eb51

Username: dcorp\krbtgt
ntlm- 0: 4e9815869d2090ccfca61c1fe0d23986
lm  - 0: ea03581a1268674a828bde6ab09db837
aes256 key:  154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848
```

##### MONEYCORP.LOCAL
- Domain SID:  S-1-5-21-335606122-960912869-3279953914
```txt
Username: mcorp\krbtgt
ntlm: a0981492d5dfab1ae0b97b51ea895ddf
```

##### EU.EUROCORP.LOCAL
- Domain SID: S-1-5-21-3665721161-1121904292-1901483061-1105
```txt
Username : dbadmin
aes256_hmac:    ef21ff273f16d437948ca755d010d5a1571a5bda62a0a372b29c703ab0777d4f
rc4_hmac_nt: 0553b02b95f64f7a3c27b9029d105c27
```

##### Trust Keys
```txt
DOLLARCORP.MONEYCORP.LOCAL -> MONEYCORP.LOCAL
aes256_hmac       9835fba12ffb0e836259dc68da27b14a538a73f35cbf5a69644e7e0f446c06dc
aes128_hmac       1a9407d1d9a63c7320aab9c4147b6923
rc4_hmac_nt       e2f785b90d3a0e627e171a90f403f260

DOLLARCORP.MONEYCORP.LOCAL -> EUROCORP.LOCAL
aes256_hmac       4b3d9c78a58e13dba8366da47a490c00dfacbfe0d5b82a710fb463ca3238a093
aes128_hmac       fa4231195868e53ca9299bcbe0f14c16
rc4_hmac_nt       652c7cebf1529f0e9fe172c0d4272ba6
```






