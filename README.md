# Домашнее задание к занятию 2 «Работа с Playbook»

## Подготовка к выполнению

1. * Необязательно. Изучите, что такое [ClickHouse](https://www.youtube.com/watch?v=fjTNS2zkeBs) и [Vector](https://www.youtube.com/watch?v=CgEhyffisLY).
2. Создайте свой публичный репозиторий на GitHub с произвольным именем или используйте старый.
3. Скачайте [Playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.
4. Подготовьте хосты в соответствии с группами из предподготовленного playbook.

## Основная часть

1. Подготовьте свой inventory-файл `prod.yml`.
2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает [vector](https://vector.dev). Конфигурация vector должна деплоиться через template файл jinja2.
3. При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.
4. Tasks должны: скачать дистрибутив нужной версии, выполнить распаковку в выбранную директорию, установить vector.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
```
Ошибок не было
```
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
```
yaha@yahawork:~/netology/ansible-02/playbook$ ansible-playbook site.yml -i inventory/prod.yml --check

PLAY [Install Clickhouse] ********************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************************************
[DEPRECATION WARNING]: Distribution centos 9 on host clickhouse-01 should use /usr/libexec/platform-python, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release will default to 
using the discovered platform python for this host. See https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation warnings can be
 disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ****************************************************************************************************************************************************************************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
failed: [clickhouse-01] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 1000, "group": "yaha", "item": "clickhouse-common-static", "mode": "0644", "msg": "Request failed", "owner": "yaha", "response": "HTTP Error 404: Not Found", "secontext": "unconfined_u:object_r:user_home_t:s0", "size": 246310036, "state": "file", "status_code": 404, "uid": 1000, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ****************************************************************************************************************************************************************************************************************
ok: [clickhouse-01]

TASK [Install clickhouse packages] ***********************************************************************************************************************************************************************************************************
ok: [clickhouse-01]

TASK [Create database] ***********************************************************************************************************************************************************************************************************************
skipping: [clickhouse-01]

PLAY [Install Vector] ************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Download vector packages] **************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Install vector packages] ***************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Vector create systed unit] *************************************************************************************************************************************************************************************************************
[WARNING]: The value "1000" (type int) was converted to "'1000'" (type string). If this does not look like what you expect, quote the entire value to ensure it does not change.
ok: [clickhouse-02]

TASK [Change vector systemd unit] ************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

PLAY RECAP ***********************************************************************************************************************************************************************************************************************************
clickhouse-01              : ok=3    changed=0    unreachable=0    failed=0    skipped=1    rescued=1    ignored=0   
clickhouse-02              : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
```
yaha@yahawork:~/netology/ansible-02/playbook$ ansible-playbook site.yml -i inventory/prod.yml --diff

PLAY [Install Clickhouse] ********************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************************************
[DEPRECATION WARNING]: Distribution centos 9 on host clickhouse-01 should use /usr/libexec/platform-python, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release will default to 
using the discovered platform python for this host. See https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation warnings can be
 disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ****************************************************************************************************************************************************************************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
failed: [clickhouse-01] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 1000, "group": "yaha", "item": "clickhouse-common-static", "mode": "0644", "msg": "Request failed", "owner": "yaha", "response": "HTTP Error 404: Not Found", "secontext": "unconfined_u:object_r:user_home_t:s0", "size": 246310036, "state": "file", "status_code": 404, "uid": 1000, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ****************************************************************************************************************************************************************************************************************
ok: [clickhouse-01]

TASK [Install clickhouse packages] ***********************************************************************************************************************************************************************************************************
ok: [clickhouse-01]

TASK [Create database] ***********************************************************************************************************************************************************************************************************************
ok: [clickhouse-01]

PLAY [Install Vector] ************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Download vector packages] **************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Install vector packages] ***************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Vector create systed unit] *************************************************************************************************************************************************************************************************************
[WARNING]: The value "1000" (type int) was converted to "'1000'" (type string). If this does not look like what you expect, quote the entire value to ensure it does not change.
ok: [clickhouse-02]

TASK [Change vector systemd unit] ************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

PLAY RECAP ***********************************************************************************************************************************************************************************************************************************
clickhouse-01              : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
clickhouse-02              : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


```
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
```
yaha@yahawork:~/netology/ansible-02/playbook$ ansible-playbook site.yml -i inventory/prod.yml --diff

PLAY [Install Clickhouse] ********************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************************************
[DEPRECATION WARNING]: Distribution centos 9 on host clickhouse-01 should use /usr/libexec/platform-python, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release will default to 
using the discovered platform python for this host. See https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation warnings can be
 disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ****************************************************************************************************************************************************************************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
failed: [clickhouse-01] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 1000, "group": "yaha", "item": "clickhouse-common-static", "mode": "0644", "msg": "Request failed", "owner": "yaha", "response": "HTTP Error 404: Not Found", "secontext": "unconfined_u:object_r:user_home_t:s0", "size": 246310036, "state": "file", "status_code": 404, "uid": 1000, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ****************************************************************************************************************************************************************************************************************
ok: [clickhouse-01]

TASK [Install clickhouse packages] ***********************************************************************************************************************************************************************************************************
ok: [clickhouse-01]

TASK [Create database] ***********************************************************************************************************************************************************************************************************************
ok: [clickhouse-01]

PLAY [Install Vector] ************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Download vector packages] **************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Install vector packages] ***************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

TASK [Vector create systed unit] *************************************************************************************************************************************************************************************************************
[WARNING]: The value "1000" (type int) was converted to "'1000'" (type string). If this does not look like what you expect, quote the entire value to ensure it does not change.
ok: [clickhouse-02]

TASK [Change vector systemd unit] ************************************************************************************************************************************************************************************************************
ok: [clickhouse-02]

PLAY RECAP ***********************************************************************************************************************************************************************************************************************************
clickhouse-01              : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
clickhouse-02              : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


```
9. Подготовьте README.md-файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги. Пример качественной документации ansible playbook по [ссылке](https://github.com/opensearch-project/ansible-playbook).
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-02-playbook` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---