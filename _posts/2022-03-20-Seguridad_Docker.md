---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: Seguridad en Docker - PrácticasTasca
---

# Seguridad en Docker - PrácticasTasca

Un escáner de seguridad de contenedores ayudará a encontrar todas las vulnerabilidades dentro de los contenedores y a monitorearlas regularmente contra cualquier ataque, problema o error nuevo.

### Practica 1

Una de ellas es [trivy](https://aquasecurity.github.io/trivy/v0.25.3/). Por ejemplo, descarga https://github.com/christophetd/log4shell-vulnerable-app que contiene, entre otras, la vulnerabilidad [log4shell](https://www.cvedetails.com/cve/CVE-2022-23307/). Genera la imagen y luego escanéala con trivy

Instalación de trivy:

```bash
wget https://github.com/aquasecurity/trivy/releases/download/v0.25.3/trivy_0.25.3_Linux-64bit.deb
sudo dpkg -i trivy_0.25.3_Linux-64bit.deb
```

En caso de que queramos escanear un repositorio de github con trivy:

```bash
trivy repo https://github.com/knqyf263/trivy-ci-test                                                                                                                                                                                 1 ⨯
Enumerating objects: 25, done.
Counting objects: 100% (25/25), done.
Compressing objects: 100% (18/18), done.
Total 25 (delta 4), reused 20 (delta 2), pack-reused 0
2022-04-28T16:32:00.865+0200    INFO    Number of language-specific files: 2
2022-04-28T16:32:00.865+0200    INFO    Detecting cargo vulnerabilities...
2022-04-28T16:32:00.866+0200    INFO    Detecting pipenv vulnerabilities...

Cargo.lock (cargo)
==================
Total: 9 (UNKNOWN: 2, LOW: 0, MEDIUM: 1, HIGH: 2, CRITICAL: 4)

+-----------+-------------------+----------+-------------------+---------------+--------------------------------------------+
|  LIBRARY  | VULNERABILITY ID  | SEVERITY | INSTALLED VERSION | FIXED VERSION |                   TITLE                    |
+-----------+-------------------+----------+-------------------+---------------+--------------------------------------------+
| ammonia   | CVE-2019-15542    | HIGH     | 1.9.0             | 2.1.0         | Uncontrolled recursion leads               |
|           |                   |          |                   |               | to abort in HTML serialization             |
|           |                   |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-15542      |
+           +-------------------+----------+                   +---------------+--------------------------------------------+
|           | CVE-2021-38193    | MEDIUM   |                   | 2.1.3, 3.1.0  | An issue was discovered                    |
|           |                   |          |                   |               | in the ammonia crate                       |
|           |                   |          |                   |               | before 3.1.0 for Rust....                  |
|           |                   |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-38193      |
+-----------+-------------------+----------+-------------------+---------------+--------------------------------------------+
| openssl   | CVE-2016-10931    | HIGH     | 0.8.3             | 0.9.0         | SSL/TLS MitM vulnerability                 |
|           |                   |          |                   |               | due to insecure defaults                   |
|           |                   |          |                   |               | -->avd.aquasec.com/nvd/cve-2016-10931      |
+-----------+-------------------+----------+-------------------+---------------+--------------------------------------------+
| rand_core | CVE-2020-25576    | CRITICAL | 0.4.0             | 0.3.1, 0.4.2  | An issue was discovered                    |
|           |                   |          |                   |               | in the rand_core crate                     |
|           |                   |          |                   |               | before 0.4.2 for Rust....                  |
|           |                   |          |                   |               | -->avd.aquasec.com/nvd/cve-2020-25576      |
+-----------+-------------------+          +-------------------+---------------+--------------------------------------------+
| smallvec  | CVE-2019-15551    |          | 0.6.9             | 0.6.10        | An issue was discovered                    |
|           |                   |          |                   |               | in the smallvec crate                      |
|           |                   |          |                   |               | before 0.6.10 for Rust....                 |
|           |                   |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-15551      |
+           +-------------------+          +                   +               +--------------------------------------------+
|           | CVE-2019-15554    |          |                   |               | An issue was discovered                    |
|           |                   |          |                   |               | in the smallvec crate                      |
|           |                   |          |                   |               | before 0.6.10 for Rust....                 |
|           |                   |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-15554      |
+           +-------------------+          +                   +---------------+--------------------------------------------+
|           | CVE-2021-25900    |          |                   | 0.6.14, 1.6.1 | An issue was discovered                    |
|           |                   |          |                   |               | in the smallvec crate                      |
|           |                   |          |                   |               | before 0.6.14 and 1.x...                   |
|           |                   |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-25900      |
+           +-------------------+----------+                   +---------------+--------------------------------------------+
|           | RUSTSEC-2018-0018 | UNKNOWN  |                   | 0.6.13        | smallvec creates uninitialized             |
|           |                   |          |                   |               | value of any type                          |
|           |                   |          |                   |               | -->osv.dev/vulnerability/RUSTSEC-2018-0018 |
+-----------+-------------------+          +-------------------+---------------+--------------------------------------------+
| tempdir   | RUSTSEC-2018-0017 |          | 0.3.7             |               | `tempdir` crate has been                   |
|           |                   |          |                   |               | deprecated; use `tempfile` instead         |
|           |                   |          |                   |               | -->osv.dev/vulnerability/RUSTSEC-2018-0017 |
+-----------+-------------------+----------+-------------------+---------------+--------------------------------------------+
Pipfile.lock (pipenv)
=====================
Total: 20 (UNKNOWN: 0, LOW: 0, MEDIUM: 7, HIGH: 10, CRITICAL: 3)

+---------------------+------------------+----------+-------------------+------------------------+---------------------------------------+
|       LIBRARY       | VULNERABILITY ID | SEVERITY | INSTALLED VERSION |     FIXED VERSION      |                 TITLE                 |
+---------------------+------------------+----------+-------------------+------------------------+---------------------------------------+
| babel               | CVE-2021-42771   | HIGH     | 2.6.0             | 2.9.1                  | CVE-2021-20095 CVE-2021-42771         |
|                     |                  |          |                   |                        | python-babel: Relative path           |
|                     |                  |          |                   |                        | traversal allows attacker             |
|                     |                  |          |                   |                        | to load arbitrary locale...           |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2021-42771 |
+---------------------+------------------+          +-------------------+------------------------+---------------------------------------+
| celery              | CVE-2021-23727   |          | 4.3.0             | 5.2.2                  | celery: stored command                |
|                     |                  |          |                   |                        | injection vulnerability may           |
|                     |                  |          |                   |                        | allow privileges escalation           |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2021-23727 |
+---------------------+------------------+          +-------------------+------------------------+---------------------------------------+
| django              | CVE-2019-6975    |          | 2.0.9             | 1.11.19, 2.0.12, 2.1.7 | python-django: memory exhaustion in   |
|                     |                  |          |                   |                        | django.utils.numberformat.format()    |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2019-6975  |
+                     +------------------+----------+                   +------------------------+---------------------------------------+
|                     | CVE-2019-3498    | MEDIUM   |                   | 1.11.18, 2.0.10, 2.1.5 | python-django: Content spoofing       |
|                     |                  |          |                   |                        | via URL path in default 404 page      |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2019-3498  |
+                     +------------------+          +                   +------------------------+---------------------------------------+
|                     | CVE-2021-33203   |          |                   | 2.2.24, 3.1.12, 3.2.4  | django: Potential directory           |
|                     |                  |          |                   |                        | traversal via ``admindocs``           |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2021-33203 |
+---------------------+------------------+          +-------------------+------------------------+---------------------------------------+
| djangorestframework | CVE-2020-25626   |          | 3.9.2             | 3.11.2                 | django-rest-framework: XSS            |
|                     |                  |          |                   |                        | Vulnerability in API viewer           |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2020-25626 |
+---------------------+------------------+----------+-------------------+------------------------+---------------------------------------+
| httplib2            | CVE-2021-21240   | HIGH     | 0.12.1            | 0.19.0                 | python-httplib2: Regular              |
|                     |                  |          |                   |                        | expression denial of                  |
|                     |                  |          |                   |                        | service via malicious header          |bash
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2021-21240 |
+                     +------------------+----------+                   +------------------------+---------------------------------------+
|                     | CVE-2020-11078   | MEDIUM   |                   | 0.18.0                 | python-httplib2: CRLF injection       |
|                     |                  |          |                   |                        | via an attacker controlled            |
|                     |                  |          |                   |                        | unescaped part of uri for...          |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2020-11078 |
+---------------------+------------------+          +-------------------+------------------------+---------------------------------------+
| jinja2              | CVE-2020-28493   |          | 2.10.1            | 2.11.3                 | python-jinja2: ReDoS                  |
|                     |                  |          |                   |                        | vulnerability in the urlize filter    |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2020-28493 |
+---------------------+------------------+----------+-------------------+------------------------+---------------------------------------+
| py                  | CVE-2020-29651   | HIGH     | 1.8.0             | 1.10.0                 | python-py: ReDoS in the py.path.svnwc |
|                     |                  |          |                   |                        | component via mailicious input        |
|                     |                  |          |                   |                        | to blame functionality...             |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2020-29651 |
+---------------------+------------------+          +-------------------+------------------------+---------------------------------------+
| pygments            | CVE-2021-20270   |          | 2.3.1             | 2.7.4                  | python-pygments: Infinite loop        |
|                     |                  |          |                   |                        | in SML lexer may lead to DoS          |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2021-20270 |
+                     +------------------+          +                   +                        +---------------------------------------+
|                     | CVE-2021-27291   |          |                   |                        | python-pygments: ReDoS                |
|                     |                  |          |                   |                        | in multiple lexers                    |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2021-27291 |
+---------------------+------------------+----------+-------------------+------------------------+---------------------------------------+
| pyyaml              | CVE-2019-20477   | CRITICAL |               5.1 | 5.2b1                  | PyYAML: command execution             |
|                     |                  |          |                   |                        | through python/object/apply           |
|                     |                  |          |                   |                        | constructor in FullLoader             |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2019-20477 |
+                     +------------------+          +                   +------------------------+---------------------------------------+
|                     | CVE-2020-14343   |          |                   |                    5.4 | PyYAML: incomplete                    |
|                     |                  |          |                   |                        | fix for CVE-2020-1747                 |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2020-14343 |
+                     +------------------+          +                   +------------------------+---------------------------------------+
|                     | CVE-2020-1747    |          |                   | 5.3.1                  | PyYAML: arbitrary command             |
|                     |                  |          |                   |                        | execution through python/object/new   |
|                     |                  |          |                   |                        | when FullLoader is used               |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2020-1747  |
+---------------------+------------------+----------+-------------------+------------------------+---------------------------------------+
| sqlparse            | CVE-2021-32839   | HIGH     | 0.3.0             | 0.4.2                  | python-sqlparse: ReDoS via regular    |
|                     |                  |          |                   |                        | expression in StripComments filter    |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2021-32839 |
+---------------------+------------------+          +-------------------+------------------------+---------------------------------------+
| urllib3             | CVE-2019-11324   |          | 1.24.1            | 1.24.2                 | python-urllib3: Certification         |
|                     |                  |          |                   |                        | mishandle when error should be thrown |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2019-11324 |
+                     +------------------+          +                   +------------------------+---------------------------------------+
|                     | CVE-2021-33503   |          |                   | 1.26.5                 | python-urllib3: ReDoS in the          |
|                     |                  |          |                   |                        | parsing of authority part of URL      |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2021-33503 |
+                     +------------------+----------+                   +------------------------+---------------------------------------+
|                     | CVE-2019-11236   | MEDIUM   |                   | 1.24.3                 | python-urllib3: CRLF injection        |
|                     |                  |          |                   |                        | due to not encoding the               |
|                     |                  |          |                   |                        | '\r\n' sequence leading to...         |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2019-11236 |
+                     +------------------+          +                   +------------------------+---------------------------------------+
|                     | CVE-2020-26137   |          |                   | 1.25.9                 | python-urllib3: CRLF injection        |z
|                     |                  |          |                   |                        | via HTTP request method               |
|                     |                  |          |                   |                        | -->avd.aquasec.com/nvd/cve-2020-26137 |
+---------------------+------------------+----------+-------------------+------------------------+---------------------------------------+
```

Como podemos obsevar trivy nos ha escaneado el repositorio y nos ha extraido todas las vulnerabilidades de este.

### Practica 2

Realiza un testeo de una imagen de Wordpress (no uses tags recientes, pues no tendrán tantas vulnerabilidades)

Primero nos descargamos una imagen de WordPress antigua:

```bash
docker pull wordpressweb/boats
```

Y ahora escanearemos la imagen con trivy

```
trivy image wordpressweb/boats

2022-04-28T16:46:49.201+0200    INFO    Detected OS: alpine
2022-04-28T16:46:49.201+0200    INFO    Detecting Alpine vulnerabilities...
2022-04-28T16:46:49.202+0200    INFO    Number of language-specific files: 1
2022-04-28T16:46:49.202+0200    INFO    Detecting python-pkg vulnerabilities...
2022-04-28T16:46:49.202+0200    WARN    This OS version is no longer supported by the distribution: alpine 3.7.0
2022-04-28T16:46:49.202+0200    WARN    The vulnerability detection may be insufficient because security updates are not provided

wordpressweb/boats (alpine 3.7.0)
=================================
Total: 38 (UNKNOWN: 0, LOW: 3, MEDIUM: 19, HIGH: 12, CRITICAL: 4)

+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
|        LIBRARY        | VULNERABILITY ID | SEVERITY | INSTALLED VERSION | FIXED VERSION |                 TITLE                 |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| expat                 | CVE-2018-20843   | HIGH     | 2.2.5-r0          | 2.2.7-r0      | expat: large number of                |
|                       |                  |          |                   |               | colons in input makes parser          |
|                       |                  |          |                   |               | consume high amount...                |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-20843 |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2019-15903   |          |                   | 2.2.7-r1      | expat: heap-based buffer              |
|                       |                  |          |                   |               | over-read via crafted XML input       |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-15903 |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| libbz2                | CVE-2019-12900   | CRITICAL | 1.0.6-r6          | 1.0.6-r7      | bzip2: out-of-bounds write            |
|                       |                  |          |                   |               | in function BZ2_decompress            |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-12900 |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| libcrypto1.0          | CVE-2018-0732    | HIGH     | 1.0.2o-r0         | 1.0.2o-r1     | openssl: Malicious server             |
|                       |                  |          |                   |               | can send large prime to               |
|                       |                  |          |                   |               | client during DH(E) TLS...            |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0732  |
+                       +------------------+----------+                   +---------------+---------------------------------------+
|                       | CVE-2018-0734    | MEDIUM   |                   | 1.0.2q-r0     | openssl: timing side channel attack   |
|                       |                  |          |                   |               | in the DSA signature algorithm        |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0734  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2018-0737    |          |                   | 1.0.2o-r1     | openssl: RSA key generation           |
|                       |                  |          |                   |               | cache timing vulnerability            |
|                       |                  |          |                   |               | in crypto/rsa/rsa_gen.c               |
|                       |                  |          |                   |               | allows attackers to...                |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0737  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2018-5407    |          |                   | 1.0.2q-r0     | openssl: Side-channel vulnerability   |
|                       |                  |          |                   |               | on SMT/Hyper-Threading                |
|                       |                  |          |                   |               | architectures (PortSmash)             |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-5407  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2019-1547    |          |                   | 1.0.2t-r0     | openssl: side-channel weak            |
|                       |                  |          |                   |               | encryption vulnerability              |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1547  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2019-1559    |          |                   | 1.0.2r-r0     | openssl: 0-byte                       |
|                       |                  |          |                   |               | record padding oracle                 |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1559  |
+                       +------------------+----------+                   +---------------+---------------------------------------+
|                       | CVE-2019-1563    | LOW      |                   | 1.0.2t-r0     | openssl: information                  |
|                       |                  |          |                   |               | disclosure in PKCS7_dataDecode        |
|                       |                  |          |                   |               | and CMS_decrypt_set1_pkey             |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1563  |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| libressl2.6-libcrypto | CVE-2018-0732    | HIGH     | 2.6.3-r0          | 2.6.5-r0      | openssl: Malicious server             |
|                       |                  |          |                   |               | can send large prime to               |
|                       |                  |          |                   |               | client during DH(E) TLS...            |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0732  |
+                       +------------------+----------+                   +               +---------------------------------------+
|                       | CVE-2018-0495    | MEDIUM   |                   |               | ROHNP: Key Extraction Side Channel    |
|                       |                  |          |                   |               | in Multiple Crypto Libraries          |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0495  |
+-----------------------+------------------+----------+                   +               +---------------------------------------+
| libressl2.6-libssl    | CVE-2018-0732    | HIGH     |                   |               | openssl: Malicious server             |
|                       |                  |          |                   |               | can send large prime to               |
|                       |                  |          |                   |               | client during DH(E) TLS...            |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0732  |
+                       +------------------+----------+                   +               +---------------------------------------+
|                       | CVE-2018-0495    | MEDIUM   |                   |               | ROHNP: Key Extraction Side Channel    |
|                       |                  |          |                   |               | in Multiple Crypto Libraries          |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0495  |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| libssl1.0             | CVE-2018-0732    | HIGH     | 1.0.2o-r0         | 1.0.2o-r1     | openssl: Malicious server             |
|                       |                  |          |                   |               | can send large prime to               |
|                       |                  |          |                   |               | client during DH(E) TLS...            |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0732  |
+                       +------------------+----------+                   +---------------+---------------------------------------+
|                       | CVE-2018-0734    | MEDIUM   |                   | 1.0.2q-r0     | openssl: timing side channel attack   |
|                       |                  |          |                   |               | in the DSA signature algorithm        |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0734  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2018-0737    |          |                   | 1.0.2o-r1     | openssl: RSA key generation           |
|                       |                  |          |                   |               | cache timing vulnerability            |
|                       |                  |          |                   |               | in crypto/rsa/rsa_gen.c               |
|                       |                  |          |                   |               | allows attackers to...                |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0737  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2018-5407    |          |                   | 1.0.2q-r0     | openssl: Side-channel vulnerability   |
|                       |                  |          |                   |               | on SMT/Hyper-Threading                |
|                       |                  |          |                   |               | architectures (PortSmash)             |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-5407  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2019-1547    |          |                   | 1.0.2t-r0     | openssl: side-channel weak            |
|                       |                  |          |                   |               | encryption vulnerability              |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1547  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2019-1559    |          |                   | 1.0.2r-r0     | openssl: 0-byte                       |
|                       |                  |          |                   |               | record padding oracle                 |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1559  |
+                       +------------------+----------+                   +---------------+---------------------------------------+
|                       | CVE-2019-1563    | LOW      |                   | 1.0.2t-r0     | openssl: information                  |
|                       |                  |          |                   |               | disclosure in PKCS7_dataDecode        |
|                       |                  |          |                   |               | and CMS_decrypt_set1_pkey             |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1563  |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| libxml2               | CVE-2018-14404   | HIGH     | 2.9.7-r0          | 2.9.8-r1      | libxml2: NULL pointer dereference     |
|                       |                  |          |                   |               | in xmlXPathCompOpEval()               |
|                       |                  |          |                   |               | function in xpath.c                   |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-14404 |
+                       +------------------+----------+                   +               +---------------------------------------+
|                       | CVE-2018-14567   | MEDIUM   |                   |               | libxml2: Infinite loop caused         |
|                       |                  |          |                   |               | by incorrect error detection          |
|                       |                  |          |                   |               | during LZMA decompression             |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-14567 |
+                       +------------------+          +                   +               +---------------------------------------+
|                       | CVE-2018-9251    |          |                   |               | libxml2: infinite loop in             |
|                       |                  |          |                   |               | xz_decomp function in xzlib.c         |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-9251  |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| openssl               | CVE-2018-0732    | HIGH     | 1.0.2o-r0         | 1.0.2o-r1     | openssl: Malicious server             |
|                       |                  |          |                   |               | can send large prime to               |
|                       |                  |          |                   |               | client during DH(E) TLS...            |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0732  |
+                       +------------------+----------+                   +---------------+---------------------------------------+
|                       | CVE-2018-0734    | MEDIUM   |                   | 1.0.2q-r0     | openssl: timing side channel attack   |
|                       |                  |          |                   |               | in the DSA signature algorithm        |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0734  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2018-0737    |          |                   | 1.0.2o-r1     | openssl: RSA key generation           |
|                       |                  |          |                   |               | cache timing vulnerability            |
|                       |                  |          |                   |               | in crypto/rsa/rsa_gen.c               |
|                       |                  |          |                   |               | allows attackers to...                |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-0737  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2018-5407    |          |                   | 1.0.2q-r0     | openssl: Side-channel vulnerability   |
|                       |                  |          |                   |               | on SMT/Hyper-Threading                |
|                       |                  |          |                   |               | architectures (PortSmash)             |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-5407  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2019-1547    |          |                   | 1.0.2t-r0     | openssl: side-channel weak            |
|                       |                  |          |                   |               | encryption vulnerability              |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1547  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2019-1559    |          |                   | 1.0.2r-r0     | openssl: 0-byte                       |
|                       |                  |          |                   |               | record padding oracle                 |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1559  |
+                       +------------------+----------+                   +---------------+---------------------------------------+
|                       | CVE-2019-1563    | LOW      |                   | 1.0.2t-r0     | openssl: information                  |
|                       |                  |          |                   |               | disclosure in PKCS7_dataDecode        |
|                       |                  |          |                   |               | and CMS_decrypt_set1_pkey             |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-1563  |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| python2               | CVE-2019-9636    | CRITICAL | 2.7.14-r4         | 2.7.15-r2     | python: Information                   |
|                       |                  |          |                   |               | Disclosure due to urlsplit            |
|                       |                  |          |                   |               | improper NFKC normalization           |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-9636  |
+                       +------------------+          +                   +               +---------------------------------------+
|                       | CVE-2019-9948    |          |                   |               | python: Undocumented local_file       |
|                       |                  |          |                   |               | protocol allows remote attackers      |
|                       |                  |          |                   |               | to bypass protection mechanisms       |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-9948  |
+                       +------------------+----------+                   +---------------+---------------------------------------+
|                       | CVE-2018-1060    | HIGH     |                   | 2.7.15-r0     | python: DOS via regular expression    |
|                       |                  |          |                   |               | catastrophic backtracking in          |
|                       |                  |          |                   |               | apop() method in pop3lib...           |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-1060  |
+                       +------------------+          +                   +               +---------------------------------------+
|                       | CVE-2018-1061    |          |                   |               | python: DOS via regular expression    |
|                       |                  |          |                   |               | backtracking in difflib.IS_LINE_JUNK  |
|                       |                  |          |                   |               | method in difflib                     |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-1061  |
+                       +------------------+          +                   +---------------+---------------------------------------+
|                       | CVE-2018-14647   |          |                   | 2.7.15-r2     | python: Missing salt initialization   |
|                       |                  |          |                   |               | in _elementtree.c module              |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-14647 |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+
| sqlite-libs           | CVE-2019-8457    | CRITICAL | 3.23.1-r0         | 3.25.3-r1     | sqlite: heap out-of-bound             |
|                       |                  |          |                   |               | read in function rtreenode()          |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-8457  |
+                       +------------------+----------+                   +---------------+---------------------------------------+
|                       | CVE-2018-20346   | HIGH     |                   | 3.25.3-r0     | CVE-2018-20505 CVE-2018-20506         |
|                       |                  |          |                   |               | sqlite: Multiple flaws in sqlite      |
|                       |                  |          |                   |               | which can be triggered via...         |
|                       |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2018-20346 |
+-----------------------+------------------+----------+-------------------+---------------+---------------------------------------+

Python (python-pkg)
===================
Total: 2 (UNKNOWN: 0, LOW: 0, MEDIUM: 1, HIGH: 1, CRITICAL: 0)

+---------+------------------+----------+-------------------+---------------+---------------------------------------+
| LIBRARY | VULNERABILITY ID | SEVERITY | INSTALLED VERSION | FIXED VERSION |                 TITLE                 |
+---------+------------------+----------+-------------------+---------------+---------------------------------------+
| Jinja2  | CVE-2019-10906   | HIGH     |              2.10 | 2.10.1        | python-jinja2: str.format_map         |
|         |                  |          |                   |               | allows sandbox escape                 |
|         |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2019-10906 |
+         +------------------+----------+                   +---------------+---------------------------------------+
|         | CVE-2020-28493   | MEDIUM   |                   | 2.11.3        | python-jinja2: ReDoS                  |
|         |                  |          |                   |               | vulnerability in the urlize filter    |
|         |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2020-28493 |
+---------+------------------+----------+-------------------+---------------+---------------------------------------+
```

### Practica 3

En la siguiente práctica primero deberemos crear una BOM el cual luego analizaremos con [DependencyTracker](https://yosoymark0911.github.io/2022/03/17/Dependecy-Track.html), para esto primero deberemos instalr [syft](https://github.com/anchore/syft/) y el [Grype](https://github.com/anchore/grype/) una vez con los programas instalados generaremos nuestro ficheros BOM, para ello deberemos ejecutar los siguientes comandos:

```bash
syft wordpressweb/boats -o cyclonedx-json > Syft-BOM
```

```bash
grype wordpressweb/boats -o cyclonedx > grype-bom 
```

Y ahora pasaremos estos ficheros por el DependecyTraker:

![image-20220428175352523](/assets/img/image-20220428175352523.png)
