```
 docker network create -d macvlan \
  --subnet=10.27.1.0/24 \
  --ip-range=10.27.1.16/28 \
  --gateway=10.27.1.1  \
  -o parent=enp6s18 macnet16
```