# slipstream-evilcu
# ظاهراً خار cpu ایرانو پیوند میزنه با htop چک کنید اگه چسبیده بود ریبوت کنید سرور رو 
نصب اسان و مرحله به مرحله تانل اضطراری slipstream 

1 - توخارج و توی دایرکتوری روت این دستور رو بزنید تا فایل فشرده رو دانلود کنه تو دایرکتوری روت 

```bash
cd /root
wget https://github.com/evilrezaa/slipstream-evilcu/releases/download/sos-frpbypass/slip-evilcu-kharej.tar.gz
```

بعدش این‌دستور رو برای استخراج بزنید این دستور فایلهای لازم‌رو تو مسیر های خودشون قرار میده .
```bash
tar -xzvf slip-evilcu-kharej.tar.gz -C /
```

2 - سرویس ایجاد شده رو ویرایش کنید
```bash
nano /etc/systemd/system/slipstream.service
```
بجای پورت 3389 پورت اینباند پنل رو بزنید و بجای t.example.com هم دامنه ای که با رکورد ns دارید رو بزنید 
بعد Ctrl X و اینتر 

بعد کل این دستورات پایینی رو کپی کنید و توی دایرکتوری روت بزنید تا سرت هارو بسازه اینا فقط تو این سرور کاربرد دارن نیازی به هیچی نیست دامنه خودتون رو بزنید آخرش

```bash
openssl req -x509 -newkey rsa:2048 -nodes \
-keyout key.pem \
-out cert.pem \
-days 3650 \
-subj "/CN=t.example.com"
```

بعد این دستورات رو بزنید


```bash
sudo systemctl daemon-reload
sudo systemctl enable slipstream
sudo systemctl start slipstream
sudo systemctl status slipstream
```


بعد اکتیو و رانینگ رو باس ببینین .
حالا پورت 53 رو باید ریدایرکت کنید به پورت این سرویس ... اگه قبلاً سرویس های dnstt یا slipstream رو روی سرور خارج نصب کردین ترجیحاً حذف کنید یا اگه بلدین غیرفعال کنید ریدایرکت های قبلی رو اگرم بلد نیستی برو ریبیلد کن .
حالا پورت 53 رو به این سرویس ریدایرکت میکنیم دستورات زیر رو بزنید 

```bash
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-port 5300
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-port 5300
```

این پایینی هارو هم بزنید و بر اساس رم سرور اون عدد مکس رو تغییر بدین 


1 GB RAM: 131072

2-4 GB RAM: 262144

8 GB+ RAM: 524288


```bash
sysctl -w net.netfilter.nf_conntrack_max=262144
sysctl -w net.netfilter.nf_conntrack_udp_timeout=15
sysctl -w net.netfilter.nf_conntrack_udp_timeout_stream=60
```



# تنظیمات خارج تمام شد




# الان توی ایران هم این پایینی هارو انجام بدین 

 - تو ایران و توی دایرکتوری روت این دستور رو بزنید تا فایل فشرده رو دانلود کنه تو دایرکتوری روت
 - 

```bash
cd /root
wget https://github.com/evilrezaa/slipstream-evilcu/releases/download/sos-frpbypass/slip-evilcu-iran.tar.gz
```


بعدش این‌دستور رو برای استخراج بزنید این دستور فایلهای لازم‌رو تو مسیر های خودشون قرار میده .
```bash
tar -xzvf slip-evilcu-iran.tar.gz -C /
```

2 - سرویس ایجاد شده رو ویرایش کنید

```bash
nano /etc/systemd/system/slipstream.service
```

و بجای پورت 3389 پورت اینباند پنل رو بزنید و بجای t.example.com هم دامنه ای که با رکورد ns دارید رو بزنید 

بعد Ctrl X و اینتر البته تو ایران میتونید هر پورتی رو میخواید بزنید و پورتی هست که توی برنامه گوشی یا سیستم بهش وصل میشه کاربر .

دیگه کاری لازم نیست فقط چک کنید که فایروال سرور ایران اجازه اتصال رو به اون پورت بده میتونید تو سایت و از سکوریتی گروپ چک کنید
بعد اینارو تو ایران بزنید


```bash
sudo systemctl daemon-reload
sudo systemctl enable slipstream
sudo systemctl start slipstream
sudo systemctl status slipstream
```


تو وضعیت باید INFO Connection ready رو ببینید و اگه نبود ینی اتصال برقرار نیست . البته چک کنید شاید برقرارم باشه ایرانه دیگه .

خلاصه اگه نبود باید تو nano /etc/systemd/system/slipstream.service ریسولورز رو تغییر بدین اونجا که 8.8.8.8 نوشته 

بعد هر تغییر در سرویس پاید

```bash
sudo systemctl daemon-reload
sudo systemctl restart slipstream
```

بزنید .

یه کرون تب کم ایجاد کنید تا هر چند دقیقه تانلو ریستارت کنه تا کرش نکنه اگرم دیدین cpu چسبید ریبوت کنید . فقط ایران
```bash
crontab -e
```
```bash
*/10 * * * * systemctl restart slipstream
```
هر ده دقیقه تانلو ری‌ستارت میکنه . فقط تو ایران لازمه
موفق باشد .
by Evil
