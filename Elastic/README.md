# Setup
## 8.14 with VM
point
- don't trust auto-config
- configure and check step by step
- when you get an error, suspect permissions at first
```zsh
# Download and install the Debian package manually
# https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.14.3-amd64.deb
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.14.3-amd64.deb.sha512
$ shasum -a 512 -c elasticsearch-8.14.3-amd64.deb.sha512 
$ sudo dpkg -i elasticsearch-8.14.3-amd64.deb

# Permission
$ sudo passwd root
> C-

$ sudo usermod -aG elasticsearch user
$ sudo usermod -aG kibana user

$ sudo chown -R root:elasticsearch /etc/default/elasticsearch
$ sudo chown -R root:elasticsearch /etc/elasticsearch
$ sudo chown -R root:elasticsearch /usr/share/elasticsearch
$ sudo chown -R root:elasticsearch /var/lib/elasticsearch
$ sudo chown -R root:elasticsearch /var/log/elasticsearch
$ sudo chown -R root:kibana /etc/kibana
$ sudo chown -R root:kibana /usr/share/kibana
$ sudo chown -R root:kibana /var/log/kibana
$ sudo chown -R root:kibana /run/kibana

## damn but useful
$ sudo chmod 777 -R /etc/default/elasticsearch
$ sudo chmod 777 -R /etc/elasticsearch
$ sudo chmod 777 -R /usr/share/elasticsearch
$ sudo chmod 777 -R /var/lib/elasticsearch
$ sudo chmod 777 -R /var/log/elasticsearch
$ sudo chmod 777 -R root:kibana /etc/kibana
$ sudo chmod 777 -R root:kibana /usr/share/kibana
$ sudo chmod 777 -R root:kibana /var/log/kibana
$ sudo chmod 777 -R root:kibana /run/kibana

# 1. intentional no security seup at first
$ vi /etc/elasticsearch/elasticsearch.yml
xpack.security.enabled: true
xpack.security.enrollment.enabled: true
xpack.security.http.ssl.enabled: false
xpack.security.transport.ssl.enabled: false

## paswword change
$ /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic -i
> C-
$ /usr/share/elasticsearch/bin/elasticsearch-reset-password -u kibana_system -i
> C-

## test elasticsearch start
$ /usr/share/elasticsearch/bin/elasticsearch

## test kibana start
$ /usr/share/kibana/bin/kibana

# 2. TLS ... Encrypt internode communications with TLS
## https://www.elastic.co/guide/en/elasticsearch/reference/current/security-basic-setup.html
$ cd /usr/share/elasticsearch
$ /usr/share/elasticsearch/bin/elasticsearch-certutil ca
$ /usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12

# store certificates
$ mkdir /etc/elasticsearch/my-certs
$ mv /usr/share/elasticsearch/elastic-certificates.p12 /etc/elasticsearch/my-certs
$ mv /usr/share/elasticsearch/elastic-stack-ca.p12 /etc/elasticsearch/my-certs

## permission
$ sudo chown -R root:elasticsearch /etc/elasticsearch
$ sudo chmod 777 -R /etc/elasticsearch

## config
$ vi /etc/elasticsearch/elasticsearch.yml
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.client_authentication: required
xpack.security.transport.ssl.keystore.path: /etc/elasticsearch/my-certs/elastic-certificates.p12
xpack.security.transport.ssl.truststore.path: /etc/elasticsearch/my-certs/elastic-certificates.p12

## run config binary
$ /usr/share/elasticsearch/bin/elasticsearch-keystore add xpack.security.transport.ssl.keystore.secure_password
> C-
$ /usr/share/elasticsearch/bin/elasticsearch-keystore add xpack.security.transport.ssl.truststore.secure_password
> C-

## permission
$ sudo chown -R root:elasticsearch /etc/elasticsearch/elasticsearch.keystore
$ sudo chmod 777 -R /etc/elasticsearch/elasticsearch.keystore

## test elasticsearch start
/usr/share/elasticsearch/bin/elasticsearch

# 3. HTTPS ... Encrypt HTTP client communications for Elasticsearch
$ cd /usr/share/elasticsearch

## run config binary
$ /usr/share/elasticsearch/elasticsearch-certutil http
> Generate a CSR? [y/N]N
> Use an existing CA? [y/N]y
> CA Path: /etc/elasticsearch/my-certs/elastic-stack-ca.p12
> Password for elastic-stack-ca.p12: [C....]
> For how long should your certificate be valid? [5y]
> Generate a certificate per node? [y/N]N
> Enter all the hostnames that you need, one per line.
hoge-host
piyo-host
[ENTER]
>Is this correct [Y/n]Y
> Enter all the IP addresses that you need, one per line.
XXX.XXX.XXX.XXX
YYY.YYY.YYY.YYY
ZZZ.ZZZ.ZZZ.ZZZ
[ENTER]
>Is this correct [Y/n]Y
> Do you wish to change any of these options? [y/N]N
> Provide a password for the "http.p12" file: [<ENTER> for none] [HOGEEEEEE]
> What filename should be used for the output zip file? [/usr/share/elasticsearch/elasticsearch-ssl-http.zip]

$ unzip elasticsearch-ssl-http.zip
$ sudo chown -R root:elasticsearch /usr/share/elasticsearch
$ sudo chmod 777 -R /usr/share/elasticsearch

## store certificates
$ mv /usr/share/elasticsearch/elasticsearch/http.p12 /etc/elasticsearch/my-certs

## config
$ vi /etc/elasticsearch/elasticsearch.yml
xpack.security.http.ssl.enabled: true
xpack.security.http.ssl.keystore.path: /etc/elasticsearch/my-certs/http.p12

## run config binary
$ su root
$ /usr/share/elasticsearch/bin/elasticsearch-keystore add xpack.security.http.ssl.keystore.secure_password
> [HOGEEEEEE]
$ exit

## Permission
$ sudo chown -R root:elasticsearch /etc/elasticsearch
$ sudo chmod 777 -R /etc/elasticsearch

## Test
$ /usr/share/elasticsearch/bin/elasticsearch

# 4. HTTPS ... Encrypt HTTP client communications for Kibana
## config
$ vi /etc/kibana/kibana.yml
elasticsearch.hosts:[ "https://XXX.XXX.XXX.XXX:9200"] # localhost is banned for registration of certification
elasticsearch.ssl.certificateAuthorities: ["/usr/share/elasticsearch/kibana/elasticsearch-ca.pem"]

## Test
$ /usr/share/kibana/bin/kibana
```

# Ref
- https://qiita.com/tsgkdt/items/7f2cfc457a75d17c5a7f
- https://www.elastic.co/guide/en/elasticsearch/reference/5.4/docs-delete-by-query.html
- http://wpress.biz/blog/2016/09/22/logstash%E3%80%80%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E3%81%A9%E3%81%93%E3%81%BE%E3%81%A7%E8%AA%AD%E3%81%BF%E8%BE%BC%E3%82%93%E3%81%A0%E3%81%8B%E3%82%92%E4%BF%9D%E6%8C%81%E3%81%99%E3%82%8Bsin/
- https://qiita.com/henatyokotraveler/items/a8b1b8ce01c5b63a7f1f
- https://www.elastic.co/guide/en/elasticsearch/reference/1.7/docs-delete-by-query.html
- https://www.elastic.co/guide/jp/elasticsearch/reference/current/gs-delete-index.html
- https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-flush.html
- https://www.elastic.co/jp/blog/a-practical-introduction-to-logstash
