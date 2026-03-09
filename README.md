# slipstream-evilcu
نصب اسان و مرحله به مرحله تانل اضطراری slipstream 

1 - توخارج و توی دایرکتوری روت ابن دستور رو بزنید تا فایل فشرده رو دانلود کنه تو پوشه روت 

wget https://github.com/evilrezaa/slipstream-evilcu/releases/download/sos-frpbypass/slip-evilcu-kharej.tar.gz


2 - سرویس ایجاد شده رو ویرایش کنید

nano /etc/systemd/system/slipstream.service

بجای پورت 3389 پورت اینباند پنل رو بزنید و بجای t.example.com هم دامنه ای که با رکورد ns دارید رو بزنید 
بعد Ctrl X و اینتر 

بعد کل این دستورات پایینی رو کپی کنید و توی دایرکتوری روت بزنید تا سرت هارو بسازه اینا فقط تو این سرور کاربرد دارن نیازی به هیچی نیست دامنه خودتون رو بزنید آخرش


openssl req -x509 -newkey rsa:2048 -nodes \
-keyout key.pem \
-out cert.pem \
-days 3650 \
-subj "/CN=t.example.com"


بعد این دستورات رو بزنید


sudo systemctl daemon-reload
sudo systemctl enable slipstream
sudo systemctl start slipstream
sudo systemctl status slipstream


بعد اکتیو و رانینگ رو باس ببینین .
حالا پورت 53 رو باید ریدایرکت کنید به پورت این سرویس ... اگه قبلاً سرویس های dnstt یا slipstream رو روی سرور خارج نصب کردین ترجیحاً حذف کنید یا اگه بلدین غیرفعال کنید ریدایرکت های قبلی رو اگرم بلد نیستی برو ریبیلد کن .
حالا پورت 53 رو به این سرویس ریدایرکت میکنیم دستورات زیر رو بزنید 


iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-port 5300
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-port 5300


این پایینی هارو هم بزنید و بر اساس رم سرور اون عدد مکس رو تغییر بدین 


1 GB RAM: 131072
2-4 GB RAM: 262144
8 GB+ RAM: 524288


sysctl -w net.netfilter.nf_conntrack_max=262144
sysctl -w net.netfilter.nf_conntrack_udp_timeout=15
sysctl -w net.netfilter.nf_conntrack_udp_timeout_stream=60



# تنظیمات خارج تمام شد




# الان توی ایران هم این پایینی هارو انجام بدین 

 - تو ایران و توی دایرکتوری روت ابن دستور رو بزنید تا فایل فشرده رو دانلود کنه تو پوشه روت

 - 
wget https://github.com/evilrezaa/slipstream-evilcu/releases/download/sos-frpbypass/slip-evilcu-iran.tar.gz


2 - سرویس ایجاد شده رو ویرایش کنید


nano /etc/systemd/system/slipstream.service


و بجای پورت 3389 پورت اینباند پنل رو بزنید و بجای t.example.com هم دامنه ای که با رکورد ns دارید رو بزنید 

بعد Ctrl X و اینتر البته تو ایران میتونید هر پورتی رو میخواید بزنید و پورتی هست که توی برنامه گوشی یا سیستم بهش وصل میشه کاربر .

دیگه کاری لازم نیست فقط چک کنید که فایروال سرور ایران اجازه اتصال رو به اون پورت بده میتونید تو سایت و از سکوریتی گروپ چک کنید
بعد اینارو تو ایران بزنید


sudo systemctl daemon-reload
sudo systemctl enable slipstream
sudo systemctl start slipstream
sudo systemctl status slipstream


تو وضعیت باید INFO Connection ready رو ببینید و اگه نبود ینی اتصال برقرار نیست . البته چک کنید شاید برقرارم باشه ایرانه دیگه .

خلاصه اگه نبود باید تو nano /etc/systemd/system/slipstream.service ریسولورز رو تغییر بدین اونجا که 8.8.8.8 نوشته 

بعد هر تغییر در سرویس پاید

sudo systemctl daemon-reload
sudo systemctl restart slipstream

بزنید .
موفق باشد .
by Evil
