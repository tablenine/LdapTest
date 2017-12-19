### LDAPS 사용

#### Self sign 인증서

+ **ldap-auth-utils**에서 제공하는 **ldap_ssl** 명령을 이용하며 간단하게 self sign 인증서를 생성할 수 있습니다. 
+ **ldap_ssl** 명령을 **-c** 옵션과 함께 실행을 하면 */etc/openldap/certs/pki*에 **ldap.crt**와 (key 암호가 제거된)**ldap.key** 파일이 생성이 됩니다.

```bash
ldap_ssl -c
Country Name (2 letter code) [XX]:KR
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:Seoul
Organization Name (eg, company) [Default Company Ltd]:OOPS.ORG
Organizational Unit Name (eg, section) []:Cert Team
Common Name (eg, your name or your server's hostname) []:54bfcb28cc30
Email Address []:admin@domain.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
Success create /etc/openldap/certs/pki/ldap.csr


* 3. Make Self signed certificate

Signature ok
subject=/C=KR/L=Seoul/O=OOPS.ORG/OU=Cert Team/CN=54bfcb28cc30/emailAddress=admin@domain.com
Getting Private key
Success create /etc/openldap/certs/pki/ldap.crt
```

#### Ldap 설정

+ ldap_ssl을 이용한 등록

  ```bash
  ldap_ssl -a -p /etc/openldap/certs/pki/ldap.key -s /etc/openldap/certs/pki/ldap.crt
  ```

+ 직접 등록

+ SSL설정 제거

  ``` bash
  ldap_ssl -r
  ```

+ 재시작

+ 확인

  ``` bash
  ldapsearch -H ldaps:/// -D "cn=manager,dc=oops,dc=org" -W
  ```
  ​
