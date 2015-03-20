title: "Willin: Setup Nginx with SSL and SPDY"
date: 2015-03-20 10:00:18
categories: Dev
tags:
- Nginx
- Security
- Performance
---

## Download and Setup

### 1. PCRE

```
wget -c http://softlayer-sng.dl.sourceforge.net/project/pcre/pcre/8.36/pcre-8.36.zip
unzip pcre-8.36.zip
cd pcre-8.36

./configure
make
make install
```

### 2. OpenSSL

```
wget -c http://www.openssl.org/source/openssl-1.0.2a.tar.gz
tar zxvf openssl-1.0.2a.tar.gz
cd openssl-1.0.2a

./config
make
make install
```

### 3. Nginx

```
wget -c http://nginx.org/download/nginx-1.7.10.tar.gz
tar zxvf nginx-1.7.10.tar.gz
cd nginx-1.7.10

./configure --with-pcre --with-openssl --with-http_ssl_module --with-http_spdy_module

make
make install
```

Optional

```
ln -s /usr/local/nginx/sbin/nginx /usr/local/bin/nginx
```



## What is Google SPDY ?

SPDY (pronounced speedy) is similar to HTTP, with particular goals of reducing web page load latency and improving web security. SPDY achieves reduced latency through compression (of both request and response HTTP headers), multiplexing (allows for unlimited concurrent streams over a single TCP connection), and prioritization (client side requests are each assigned a priority).

From [SPDY whitepaper](http://dev.chromium.org/spdy/spdy-whitepaper) some numbers:

> *   SPDY sends ~40% fewer packets than HTTP, which means fewer packets affected by loss.
> *   SPDY uses fewer TCP connections, which means fewer chances to lose the SYN packet. In many TCP implementations, this delay is disproportionately expensive (up to 3 s).
> *   SPDY's more efficient use of TCP usually triggers TCP's fast retransmit instead of using retransmit timers.
>
> We discovered that SPDY's latency savings also increased proportionally with increases in RTTs, up to a 27% speedup at 200 ms. The  The reason that SPDY does better as RTT goes up is because SPDY fetches all requests in parallel. If an HTTP client has 4 connections per domain, and 20 resources to fetch, it would take roughly 5 RTs to fetch all 20 items.  SPDY fetches all 20 resources in one RT.

| RTT in ms |HTTP(ms)|  SPDY (TCP)  (ms)|  Speed Up     |
|-----------|--------|------------------|---------------|
|   20      |  1240  |  1087            |   12.34%      |
|   40      |  1571  |  1279            |   18.59%      |
|   60      |  1909  |  1526            |   20.06%      |
|   80      |  2268  |  1727            |   23.85%      |
|   120     |  2927  |  2240            |   23.47%      |
|   160     |  3650  |  2772            |   24.05%      |
|   200     |  4498  |  3293            |   26.79%      |


## Configuration

### dhparam

For DHE key-exchange, generate a `dhparam.pem` file at `/usr/local/nginx/conf/ssl/domaincom/dhparam.pem` with higher strength 2048 bit or 4096 bit key-exchange instead of using Nginx's OpenSSL Ephemeral Diffie-Hellman (DHE) default key of 1024 bit to at least match SSL 2048 bit certificates.

To generate dhparam file you can either use one of the 2 below commands (not both). Both can take quite a long time to generate depending on your server's processing power. The 4096 bit can take between 15-60 minutes to generate.

```
cd /usr/local/nginx/conf/ssl/domaincom
openssl dhparam -out dhparam.pem 2048
```

or

```
cd /usr/local/nginx/conf/ssl/domaincom
openssl dhparam -out dhparam.pem 4096
```

Within Nginx vhost you'd add this line.

```
ssl_dhparam /usr/local/nginx/conf/ssl/domaincom/dhparam.pem;
```

### ssl_ciphers

```
ssl_ciphers 'ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4';
```

or below seems better (From: [http://centminmod.com/nginx_configure_https_ssl_spdy.html](http://centminmod.com/nginx_configure_https_ssl_spdy.html))

```
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA:!CAMELLIA;
```

### Nginx Conf

```
server {
        listen       443 ssl spdy;
        server_name  domain.com;
        add_header Strict-Transport-Security max-age=15768000;

        ssl_dhparam          /path/to/dhparam.pem;
        ssl_certificate      /path/to/crt;
        ssl_certificate_key  /path/to/key;

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA:!CAMELLIA;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;
        ssl_session_timeout 10m;
        ssl_prefer_server_ciphers   on;
        add_header Alternate-Protocol  443:npn-spdy/3;
        add_header Strict-Transport-Security "max-age=31536000; includeSubdomains;";
        #add_header  X-Content-Type-Options "nosniff";
        #add_header X-Frame-Options DENY;

        access_log /home/nginx/domains/domain.com/log/access.log combined buffer=32k;
        error_log /home/nginx/domains/domain.com/log/error.log;

        ......
	}
}
```
