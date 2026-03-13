# slipstream-evilcu
# سعی کردم مصرف cpu رو کنترل کنم
# تانل اضطراری slipstream کاربر بالا جواب نمیده عمو جون
## پنل رو سرور خارج نصب میشه
## اگه یهو تانل قطع شد به هیچی دست نزنید خودش اوکی میشه
## ممکنه پورتی که انتخاب کردید تو فایروال سرور ایران بسته شده باشه توصیه میکنم از همون پیشفرض 3389 استفاده کنید
### اگه دسترسی ssh به سرور قطع شد از سایتی که سرور رو خریدین سرور رو ریبوت کنید البته خیلی کم پیش میاد

# دستورات سرور خارج


تو سرور خارج و توی دایرکتوری root این دستور رو بزنید تا فایل فشرده رو دانلود کنه

```bash
cd /root
wget https://github.com/evilrezaa/slipstream-evilcu/releases/download/gfy-dpi/slip-evilcu-kharej2.tar.gz
```

بعدش این‌دستور رو برای استخراج بزنید این دستور فایلهای لازم‌ رو تو مسیر های خودشون قرار میده
```bash
tar -xzvf slip-evilcu-kharej2.tar.gz -C /
```

سرویس ایجاد شده رو ویرایش کنید
```bash
nano /etc/systemd/system/slipstream.service
```
بجای پورت 3389 پورت اینباند پنل رو بزنید و بجای t.example.com هم دامنه ای که با رکورد ns دارید رو بزنید 
بعد Ctrl+x و Enter 

بعد کل این دستورات پایینی رو کپی کنید و توی دایرکتوری روت بزنید تا سرت هارو بسازه اینا فقط تو این سرور کاربرد دارن نیازی به هیچی نیست
فقط اون آخرش دامنه خودتون رو بزنید

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
sudo systemctl restart slipstream
sudo systemctl status slipstream
```
بعد اکتیو و رانینگ رو باس ببینین با Ctrl+C خارج میشه


حالا پورت 53 رو باید ریدایرکت کنید به پورت این سرویس ... اگه قبلاً سرویس های dnstt یا slipstream رو روی سرور خارج نصب کردین ترجیحاً حذف کنید یا اگه بلدین ، ریدایرکت های قبلی رو غیرفعال کنید
اگرم بلد نیستی برو ریبیلد کن خودتم اذیت نکن

حالا پورت 53 رو به پورت این سرویس [ 5300 ] ریدایرکت میکنیم دستورات زیر رو بزنید 

```bash
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-port 5300
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-port 5300
```
بعد از ریبوت حذف میشن و باید دوباره بزنید

این پایینی هارو هم بزنید و بر اساس رم سرور اون عدد مکس رو تغییر بدین 


1 GB RAM: 131072

2-4 GB RAM: 262144

8 GB+ RAM: 524288


```bash
sysctl -w net.netfilter.nf_conntrack_max=262144
sysctl -w net.netfilter.nf_conntrack_udp_timeout=15
sysctl -w net.netfilter.nf_conntrack_udp_timeout_stream=60
```
بعد از ریبوت حذف میشن و باید دوباره بزنید

# کنترل مصرف cpu

خب برای کنترل مصرف cpu یه سرویس اضافه کردم تا اگه مصرفش بالای 70 درصد رفت سرویس رو ری‌استارت کنه 
البته تو سرور خارج زیاد مهمم نیست چون مصرف cpu خارج آنچنان بالا نمیره و مشکل اصلی ایرانه
بهرحال اگه خواستید تو خارج هم فعالش کنید 
دستورات فعال‌سازی
```bash
sudo systemctl daemon-reload
sudo systemctl enable slipstream-cpu-monitor
sudo systemctl start slipstream-cpu-monitor
sudo systemctl status slipstream-cpu-monitor
```

# تنظیمات خارج تمام شد

# دستورات سرور ایران

# الان توی ایران هم این پایینی هارو انجام بدین 

تو سرور ایران و توی دایرکتوری root این دستور رو بزنید تا فایل فشرده رو دانلود کنه

```bash
cd /root
wget https://github.com/evilrezaa/slipstream-evilcu/releases/download/gfy-dpi/slip-evilcu-iran2.tar.gz
```

دانلود از آپلودسنتر ایرانی برای سهولت استفاده
```bash
cd /root
wget --content-disposition https://netnest.org/api/storage/share/ddb93aaaf7693b246c0c892b7f7dcd9a6a2ecafe440661d8e5f8d8bcd903b61c/download/
```
اگه لینک بالا خطا داد این دوتا نیم سرور پایین‌تر رو به این فایل پایین اضافه کنید
```bash
nano /etc/resolv.conf
```
```bash
nameserver 217.218.127.127
nameserver 217.218.155.155
```
اگه لینک بالا کار نکرد این روش پایینو انجام بدین 
چون الان سرور های ایران به نت دسترسی ندارن فایل slip-evilcu-iran2.tar.gz رو از releases دانلود کنید و با sftp بزاریدش توی دایرکتوری root یا اگه
به دایرکتوری روت دسترسی sftp نداد اول فایل رو توی دایرکتوری قابل دسترس آپلود کنید و بعد با دستور mv یا cp فایل رو به root منتقل کنید نمونه دستور انتقال 
```bash
sudo mv /home/cloud-admin/slipstream-evilcu-iran2.tar.gz /root/
```
یا اگه بلدین تو هر دایرکتوری که استخراج کنید فایلها به مسیر های خودشون میرن . فایل توی تلگرام و چنل evilcu موجوده
بعد از اینکه فایل رو با هر روشی انداختین داخل سرور بعدش دستور استخراج پایین رو بزنید 

بعدش این‌ دستور رو برای استخراج بزنید این دستور فایلهای لازم‌ رو تو مسیر های خودشون قرار میده
```bash
tar -xzvf slip-evilcu-iran2.tar.gz -C /
```

سرویس ایجاد شده رو ویرایش کنید

```bash
nano /etc/systemd/system/slipstream.service
```

و بجای پورت 3389 پورت اینباند پنل رو بزنید و بجای t.example.com هم دامنه ای که با رکورد ns دارید رو بزنید 

بعد Ctrl X و اینتر البته تو ایران میتونید هر پورتی رو میخواید بزنید و پورتی هست که توی برنامه گوشی یا سیستم بهش وصل میشه کاربر

دیگه کاری لازم نیست فقط چک کنید که فایروال سرور ایران اجازه اتصال رو به اون پورت بده میتونید تو سایت و از سکوریتی گروپ چک کنید
بعد اینارو تو ایران بزنید


```bash
sudo systemctl daemon-reload
sudo systemctl enable slipstream
sudo systemctl start slipstream
sudo systemctl restart slipstream
sudo systemctl status slipstream
```


تو وضعیت باید INFO Connection ready رو ببینید و اگه نبود ینی اتصال برقرار نیست . البته چک کنید شاید برقرارم باشه ایرانه دیگه

خلاصه اگه نبود باید تو nano /etc/systemd/system/slipstream.service ریسولورز رو تغییر بدین اونجا که 8.8.8.8 نوشته 

بعد هر تغییر در فایل etc/systemd/system/slipstream.service باید این دستورات پایین رو حتماً بزنید

```bash
sudo systemctl daemon-reload
sudo systemctl restart slipstream
```

# کنترل مصرف cpu
خب برای کنترل مصرف cpu یه سرویس اضافه کردم تا اگه مصرفش بالای 70 درصد رفت سرویس رو ری‌استارت کنه 
بنظرم تو ایران خیلی میتونه کمک کنه و دیگه مجبور نیستین زیاد ریبوت کنید
فعال‌سازی با دستورات زیر
```bash
sudo systemctl daemon-reload
sudo systemctl enable slipstream-cpu-monitor
sudo systemctl start slipstream-cpu-monitor
sudo systemctl status slipstream-cpu-monitor
```
باید اکتیو و رانینگ باشه

یه کرون تب کم ایجاد کنید تا هر چند دقیقه تانلو ریستارت کنه تا کرش نکنه اگرم دیدین بازم cpu چسبید ریبوت کنید . فقط ایران
```bash
crontab -e
```
```bash
*/10 * * * * systemctl restart slipstream
```
هر ده دقیقه تانلو ری‌ستارت میکنه . فقط تو ایران لازمه

# پیشنهادی
با توجه به تجربه ای که این مدت داشتم با mtu پایین بهتر جواب میده البته تست کنید ببینید بهترینش
چی میتونه باشه ، با دستور زیر میتونید mtu رو ست کنید ، فقط توجه کنید اسم اینترفیس سرورتون چیه
معمولاً eth0 هست یا ens3
```bash
sudo ip link set dev ens3 mtu 700
```
ترجیحاً با این دستورات پایین موقتاً ipv6 سیستم رو غیرفعال کنید
```bash
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.ens3.disable_ipv6=1
```
بعد از ریبوت به حالت پیش‌فرض برمیگرده نگران نباشید 

by Evil
