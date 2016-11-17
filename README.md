# sa-sentry


[![Build Status](https://travis-ci.org/softasap/sa-sentry.svg?branch=master)](https://travis-ci.org/softasap/sa-sentry)

Usage example:

```

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

