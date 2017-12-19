###Ldap 서버 설치

#### 패키지 설치

```bash
yum install -y openldap-servers openldap-clients
```

#### repo 추가

```bash
 vi /etc/yum.repos.d/CentOS-Base.repo
 
[AN:core]
name=AnNyung 2 Core Repository
mirrorlist=http://annyung.oops.org/mirror.php?release=2&arch=$basearch&repo=core
gpgcheck=1
gpgkey=http://mirror.oops.org/pub/AnNyung/2/RPM-GPG-KEY-AnNyung-2
exclude=php* whois httpd*
```

#### ldap-auth-utils 설치 및 초기화

```
yum clean all
yum install -y ldap-auth-utils ldap-auth-utils-passwd
yum install -y perl
rm -rf /etc/openldap/slapd*.conf
rm -rf /etc/openldap/slapd.d
ldap_auth_init
```

#### LDAP client 설정

```bash
vi /etc/openldap/ldap.conf
URI ldapi:///
BASE DC=oops,DC=org
```

#### account / group 확인

```bash
#ldap account 확인
ldapsearch -Y EXTERNAL  "(objectClass=posixAccount)" dn

#ldap group 확인
ldapsearch -Y EXTERNAL  "(objectClass=posixGroup)" dn
```

#### LDAP 관리자 암호 변경

````bash
export CHGPASSWD=$(slappasswd -s 'asdf!asdf')
cat <<EOF > ldapmodify -Y EXTERNAL -H ldapi:///
dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootPW
olcRootPW: ${CHGPASSWD}
dn: olcDatabase={2}bdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: ${CHGPASSWD}

unset CHGPASSWD
````

#### Account 암호 변경

```bash
#ssoadmin account 암호 변경
ldap_passwd -u Admin ssoadmin@oops.org

#ssomanager account 암호 변경
ldap_passwd -u Admin ssomanager@oops.org

#replica account 암호 변경
ldap_passwd -u Admin replica@oops.org
```
출처 : [안녕리눅스3 UserG Guide](https://joungkyun.gitbooks.io/annyung-3-user-guide/chapter2-3-auth-integrate-openldap-1.html)

