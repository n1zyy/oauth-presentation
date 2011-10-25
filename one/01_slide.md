!SLIDE
# Oh, auth? #
## OAuth! ##

!SLIDE bullets incremental

# Authenticates Components #

* Conductor
* Warehouse
* Image Factory
* Config Server (TBD)

!SLIDE bullets incremental
# 2-legged OAuth #

* Completely unlike 3-legged OAuth
* Username = "consumer key"
* Password = "consumer secret"

!SLIDE
# OAuth signature in headers #

     GET /foo HTTP/1.1
     HOST: localhost
     Auth oauth_consumer_key="1bcd57de-58be-4eec-9321-2744b12049d5",
       oauth_nonce="JTIMw8znaConhbxdPaJ3RGlwYo9byqSJphf9NCV2M",
       oauth_signature="KtQNtAaCTgi4lSa8%2FoaZgF2ltKE%3D",
       oauth_signature_method="HMAC-SHA1",
       oauth_timestamp="1319479861", oauth_version="1.0"

!SLIDE bullets incremental
# Setup #

* aeolus-configure does it all!
  * Uses UUID pairs per-service
  * Conductor: src/config/settings.yml
  * iwhd: users.js or command-line
  * Factory: imagefactory.conf

!SLIDE
# src/config/settings.yml #
    @@@ makefile
    :iwhd:
      :url: http://localhost:9090
      :oauth:
        :consumer_key: 16f316f4-b8da-439d-b319-d91d48dcf85d
        :consumer_secret: 6f446eb5-301c-4904-9812-d8a20436471d
    :imagefactory:
      :url: https://localhost:8075/imagefactory
      :oauth:
        :consumer_key: 39b9e7be-2c2f-44f9-a530-418e1eaab605
        :consumer_secret: 1a8fc3ad-83ab-44b5-a0e6-92fc64255ba1


!SLIDE bullets
# iwhd #

* Old way: -o -U key:secret
* New way: -o -u /etc/iwhd/users.js

!SLIDE commandline

    $ ps aux | grep iwhd
    iwhd -c /etc/iwhd/conf.js -d localhost:27017 \
    -l /var/log/iwhd.log -v -o \
    -U 2014e046-e2e1-4112-b734-f5e7d7a56f2e:01f1576e-6bf7-455f-afb0-f5368eec9047

!SLIDE
# imagefactory.conf #
* Two pairs of keys
  * warehouse_key and warehouse_secret used for Factory -> iwhd
  * clients block used for Conductor -> Factory


!SLIDE
# imagefactory.conf #

    {
      "warehouse": "http://localhost:9090/",
      "warehouse_key": "16f316f4-b8da-439d-b319-d91d48dcf85d",
      "warehouse_secret": "6f446eb5-301c-4904-9812-d8a20436471d",
      "ec2_ami_type": "s3",
      "clients": {
        "39b9e7be-2c2f-44f9-a530-418e1eaab605": "1a8fc3ad-83ab-44b5-a0e6-92fc64255ba1"
        },
      "proxy_ami_id": "ami-id"
    }
