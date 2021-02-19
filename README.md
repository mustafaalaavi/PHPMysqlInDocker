## مقدمه
سلام
امروز رو درگیر راه اندازی یک اپلیکیشن PHP بر روی داکر بودم .
در این حالت ما چند نیاز اساسی داشتیم
-  ماژول mod_rewrite رو بر روی apache فعال باشد 
- کتابخانه GD بر روی PHP فعال شود
- فایل dump اولیه دیتابیس که وظیفه ساخت دیتابیس خالی‌ما هست بعد از راه اندازی کانتینر به صورت خودکار import شود


### ساختار فایل ها
| نام پوشه  | توضیحات  |
| ------------ | ------------ |
|dump |  dump فایل اولیه دیتابیس را در داخل این فولدر قرار دهید . توجه کنید فایل هم نام با دیتابیس باشد |
| php | این فایل وظیفه نصب apache و کانفیک را داره می باشد  |
|source | کد ها php خود را درون این فایل قرار دهید  |

### docker-compose
```yaml
version: "2"
services:
    api:
        build:
          context: ./php
        ports: 
            - "80:80"
        restart: always
        volumes:
            - ./source:/var/www/html/
        links:
            - mariadb
        networks:
            - default
    mariadb:
        image: mariadb
        restart: always
        ports: 
            - "3306:3306"
        environment:
            MYSQL_ROOT_PASSWORD: 123456
            MYSQL_DATABASE: myDB
            MYSQL_USER: webuser
            MYSQL_PASSWORD: 123456
        volumes:
            - ./dump:/docker-entrypoint-initdb.d
            - phpInDocker:/var/lib/mysql
        networks:
            - default
    phpmyadmin:
        image: phpmyadmin/phpmyadmin
        restart: always
        links: 
            - mariadb:db
        ports:
            - 8080:80
        environment:
            MYSQL_USER: webuser
            MYSQL_PASSWORD: 123456
            MYSQL_ROOT_PASSWORD: 123456
volumes:
    phpInDocker:
```

### اجرا
پس از اعمال تغییرات می‌توانید با دستور زیر کانتینر ها را تولید کنید

`
$ docker-compose up -d
`