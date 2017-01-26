# sa-sentry


[![Build Status](https://travis-ci.org/softasap/sa-sentry.svg?branch=master)](https://travis-ci.org/softasap/sa-sentry)

Usage example:

Simple

```YAML

---
- hosts: www

  vars:
    - root_dir: ..


  pre_tasks:
    - debug: msg="Pre tasks section"

  roles:
     - {
         role: "sa-sentry"
       }


  tasks:

    - debug: msg="Tasks section"


```

User precreation

```YAML

my_sentry_users:
  - {
    email: "root@localhost",
    password: "VerySecurePassword",
    role: "superuser" # no-superuser
    }


```

```
  roles:
     - {
         role: "sa-sentry",
         option_sentry_addon_users: true,
         sentry_application_users: "{{my_sentry_users}}"
       }
```


Advanced

```YAML

---
- hosts: www

  vars:
    - root_dir: ..

    - custom_conf_properties:
        - {regexp: "^        'USER':*", line: "        'USER': '{{sentry_database_user}}',"}
        - {regexp: "^SENTRY_PUBLIC =*", line: "SENTRY_PUBLIC = False"}
        - {regexp: "^SENTRY_WEB_HOST =*", line: "SENTRY_WEB_HOST = '{{sentry_bind_to}}'" }
        - {regexp: "^SESSION_COOKIE_SECURE =*", line: "SESSION_COOKIE_SECURE = True" }
        - {regexp: "^SECURE_PROXY_SSL_HEADER =*", line: "SECURE_PROXY_SSL_HEADER = (\"HTTP_X_FORWARDED_PROTOCOL\", \"https\")" }

    - custom_yml_properties:
            - {regexp: "^#?system.secret-key:*", line: "system.secret-key: '{{sentry_secret_key}}'"}

    - custom_certificates_pack:
           - {
                src: "{{role_dir}}/files/nginx/dhparam.pem",
                dest: "/etc/nginx/dhparam.pem",
                remote_src: false
              }



  pre_tasks:
    - debug: msg="Pre tasks section"

  roles:
     - {
         role: "sa-sentry",
         option_create_database: true,
         option_configure_startup: true,
         option_configure_nginx: true,

         sentry_user: sentry,
         sentry_home: "/home/{{sentry_user}}",
         sentry_virtual_env: "/home/{{sentry_user}}/virtual_envs",
         sentry_log_dir: /var/log/sentry,

         sentry_database_name: sentry,
         sentry_database_user: sentry,
         sentry_database_password: sentry,

         sentry_bind_to: "0.0.0.0",


         sentry_conf_properties: "{{custom_conf_properties}}",

         sentry_yml_properties: "{{custom_yml_properties}}",

         sentry_certificates_pack: "{{custom_certificates_pack}}",

         sentry_ssl_key: sentry,
         sentry_ssl_crt: sentry,
         sentry_domain: sentry.softasap.com

       }


  tasks:

    - debug: msg="Tasks section"


```
