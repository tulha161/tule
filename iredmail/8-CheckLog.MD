# Check maillog để kiểm tra trạng thái gửi mail.

- File log mail mặc định được tại `/var/log/maillog` => log này thu thập tổng hợp các hoạt động liên quan đến email
- File log iredAPD `/var/log/iredapd/iredapd.log` => log này là log của policy server, có thê check log này để focus thông tin lien quan đến  blacklist blocked


## 1. Check log gửi/nhận mail thành công : 
- Toàn bộ log trong quá trình gửi một email từ `tulha@tulha.fun` -> `tulha@vccloud.vn` : 
```
Dec 28 15:16:27 mail postfix/submission/smtpd[322086]: connect from mail.tutrangcubeoxam.online[127.0.0.1]
Dec 28 15:16:27 mail postfix/submission/smtpd[322086]: Anonymous TLS connection established from mail.tutrangcubeoxam.online[127.0.0.1]: TLSv1.3 with cipher TLS_AES_256_GCM_SHA384 (256/256 bits) key-exchange X25519 server-signature RSA-PSS (4096 bits) server-digest SHA256
Dec 28 15:16:27 mail postfix/submission/smtpd[322086]: 4JNS6g6J65z9p1j: client=mail.tutrangcubeoxam.online[127.0.0.1], sasl_method=LOGIN, sasl_username=tulha@tulha.fun
Dec 28 15:16:27 mail postfix/cleanup[322102]: 4JNS6g6J65z9p1j: message-id=<d2c8e458b9005fe1e75b5fc94367325d@tulha.fun>
Dec 28 15:16:27 mail postfix/qmgr[2688]: 4JNS6g6J65z9p1j: from=<tulha@tulha.fun>, size=557, nrcpt=1 (queue active)
Dec 28 15:16:27 mail roundcube: <k0cigoea> User tulha@tulha.fun [123.30.170.251]; Message <d2c8e458b9005fe1e75b5fc94367325d@tulha.fun> for tulha@vccloud.vn; 250: 2.0.0 Ok: queued as 4JNS6g6J65z9p1j
Dec 28 15:16:27 mail postfix/submission/smtpd[322086]: disconnect from mail.tutrangcubeoxam.online[127.0.0.1] ehlo=2 starttls=1 auth=1 mail=1 rcpt=1 data=1 quit=1 commands=8
Dec 28 15:16:28 mail postfix/10025/smtpd[322109]: connect from mail.tutrangcubeoxam.online[127.0.0.1]
Dec 28 15:16:28 mail postfix/10025/smtpd[322109]: 4JNS6h47X1z9p1k: client=mail.tutrangcubeoxam.online[127.0.0.1]
Dec 28 15:16:28 mail postfix/cleanup[322102]: 4JNS6h47X1z9p1k: message-id=<d2c8e458b9005fe1e75b5fc94367325d@tulha.fun>
Dec 28 15:16:28 mail postfix/10025/smtpd[322109]: disconnect from mail.tutrangcubeoxam.online[127.0.0.1] ehlo=1 mail=1 rcpt=1 data=1 quit=1 commands=5
Dec 28 15:16:28 mail postfix/qmgr[2688]: 4JNS6h47X1z9p1k: from=<tulha@tulha.fun>, size=1911, nrcpt=1 (queue active)
Dec 28 15:16:28 mail amavis[2718260]: (2718260-03) Passed CLEAN {RelayedInternal}, ORIGINATING/MYNETS LOCAL [127.0.0.1]:39084 ESMTP/ESMTP <tulha@tulha.fun> -> <tulha@vccloud.vn>, (), Queue-ID: 4JNS6g6J65z9p1j, Message-ID: <d2c8e458b9005fe1e75b5fc94367325d@tulha.fun>, mail_id: OaOSMxXnfknq, b: es0WVIFtO, Hits: 0.997, size: 557, queued_as: 4JNS6h47X1z9p1k, Subject: "test logging", From: <tulha@tulha.fun>, User-Agent: Roundcube_Webmail, helo=localhost, Tests: [ALL_TRUSTED=-1,FROM_FMBLA_NEWDOM=1.498,FROM_SUSPICIOUS_NTLD=0.498,FROM_SUSPICIOUS_NTLD_FP=0.001], autolearn=no autolearn_force=no, autolearnscore=1.997, dkim_new=dkim:tutrangcubeoxam.online, 651 ms
Dec 28 15:16:28 mail postfix/amavis/smtp[322103]: 4JNS6g6J65z9p1j: to=<tulha@vccloud.vn>, relay=127.0.0.1[127.0.0.1]:10026, delay=0.89, delays=0.17/0.04/0.01/0.66, dsn=2.0.0, status=sent (250 2.0.0 from MTA(smtp:[127.0.0.1]:10025): 250 2.0.0 Ok: queued as 4JNS6h47X1z9p1k)
Dec 28 15:16:28 mail postfix/qmgr[2688]: 4JNS6g6J65z9p1j: removed
Dec 28 15:16:28 mail postfix/smtp[322145]: Anonymous TLS connection established to captmx-re30.bizflycloud.vn[103.69.195.135]:25: TLSv1.2 with cipher ADH-AES256-GCM-SHA384 (256/256 bits)
Dec 28 15:16:33 mail postfix/smtp[322145]: 4JNS6h47X1z9p1k: to=<tulha@vccloud.vn>, relay=captmx-re30.bizflycloud.vn[103.69.195.135]:25, delay=5.4, delays=0.01/0.03/0.28/5, dsn=2.0.0, status=sent (250 2.0.0 Ok: queued as 95FA2DB2C9)
Dec 28 15:16:33 mail postfix/qmgr[2688]: 4JNS6h47X1z9p1k: removed
```



