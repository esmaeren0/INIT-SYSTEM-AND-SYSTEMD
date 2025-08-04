# INIT-SYSTEM-AND-SYSTEMD
Linux systemd and init system

1. Init System Tanımı:
 Linux ve Unix tabanlı işletim sistemlerinde bilgisayar açıldığında ilk çalışan süreçtir.
- Görevi; sistem servislerini (örneğin ağ, zamanlayıcı, kullanıcı girişi vb.) sırayla veya paralel olarak başlatır, yönetir ve sistemi çalışır hale getirir.

--------------------------------------------------------------------------------
2. systemd nedir?
  
systemd, Linux sistemlerde en yaygın kullanılan modern init sistemidir. Servisleri hızlı ve paralel şekilde başlatır, sistem yönetimi ve günlük (log) takibi gibi birçok işlemi tek bir yapı altında toplar.

Systemd görevleri:
     - Servis yönetmi	(Apache, Nginx, PostgreSQL gibi servisleri başlatır/durdurur.)
     - Paralel başlatma	(Sistem açılışını hızlandırmak için servisleri paralel çalıştırır.)
     - Günlük (log) yönetimi	(journalctl ile detaylı log takibi yapmanı sağlar.)
     - Otomatik yeniden başlatma	(Çöken servisleri otomatik yeniden başlatabilir.)
     - Timer & Socket desteği	 (Cron yerine zamanlayıcı (timer), veya istekle çalışan servisler kurar.)
    
-----------------------------------------------------------------------------
3. Systemd'nin önceki init sistemlerden farkı :
- SysVinit, eski dönemlerde kullanılan, servisleri sırayla başlatan basit bir init sistemiydi. Upstart ise bir dönem Ubuntu tarafından kullanılan, olay tabanlı bir init sistemidir. Günümüzde ise çoğu Linux dağıtımında kullanılan systemd, modern yapısıyla servisleri hızlı ve paralel şekilde başlatabilen gelişmiş bir init sistemidir.

---------------------------------------------------------------------------------
4. Systemd Yapısı
   systemd unit (birim) dosyalarıyla çalışır.

   Unit türleri:
   - service (.service) : Yazılım yada scriptin arka planda daemon olarak başlatılmasını sağlar. ( Apache,Nginx,Postgresql servislerini start, stop. restart için)

   ```\
      [Unit]
      Description=Basit servis // basit servis bilgisi 

     [Service] (nasıl çalışacağı bilgisi )
     ExecStart=/usr/local/bin/merhaba.sh // çalışacak coommand ya da script

     [Install] (Otomatik başlatma ayarları)
     WantedBy=multi-user.target //servisin bağlı olduğu target (WantedBy)

 -  timer (.timer): cron'un modern ve entegre hali. Belirli zamanda çalışan service birimlerini tetikler ( backup, temizlik, control). Timer service dosyları ile beraber kullanılır. Timer ne zaman yaopılacağını, service ne yapılacağını belirtir.

   ...timer ve service örneği her saat başı ekrana saati yazdıran bir timer oluşturuldu....

```bash
[Unit]
Description=Saat bildirimi

[Service]
Type=oneshot
ExecStart=/usr/bin/notify-send "Saat: " "$(date '+Saat: %H:%M:%S')" -t 10000 // notify-send masaüstüne bildirim gönderir

---------
[Unit]
Description=Her saat başı saat bildirimi

[Timer]
OnCalendar=hourly
Persistent=false // sistem kapalıyken gelen bildirimler tekrar gelmez.

[Install]
WantedBy=timers.target

------------
sudo systemctl daemon-reload
sudo systemctl enable --now saat_goster.timer
//timer ve service i etkinleştir.


\n```
   
