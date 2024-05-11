![Screenshot](https://img.shields.io/badge/ansible-v2.16-blue?logo=ansible&logoColor=yellow)
![Lint](https://github.com/memphis-tools/implement_compiled_softwares_signatures_file_with_ansible/actions/workflows/lint.yml/badge.svg)
![CI](https://github.com/memphis-tools/implement_compiled_softwares_signatures_file_with_ansible/actions/workflows/lint.yml/badge.svg)

# What is it ?

An Ansible playbook.

# What is it for ?

Because the package manager won't see compiled softwares, a dedicated file is set up.

It could be done using Python for example. The demand was to use only Ansible.

The playbook can be run independently, without any real deployment on the host.

# How it works ?

User indicates some pre-defined informations about a compiled software installed on the host:

  - a name
  - a version
  - an internal code
  - a description

These informations are recorded in a local file on the host. The script adds a revision reference (a simple integer digit).

This file regroups all compiled softwares.

User can choose to add, update, or delete an entry.

The software name and version are used to find the entry which match with it. If it already exist, the revision digit reference is incremented.

A compiled softwares signatures historical file is set as well.

# Pre-requisites

**Adapt the ansible.cfg to your needs. Here we suppose that the running user has a ssh public-key access with sudo privilege on remote host.**

# How to use it ?

- Clone the repo:

  git clone https://github.com/memphis-tools/implement_signatures_file.git

  cd implement_signatures_file

- Create a venv, source it and install requirements:

  python -m venv venv

  source venv/bin/activate

  pip install -r requirements.txt

- Update at least the signatures file path in the vars :

  update_compiled_softwares_signatures_file_path

  update_compiled_softwares_signatures_historical_file_path

- Execute: the examples below are just illustrations to try playbook on your local machine without any inventory.

- Run it

## Add signature in file (same syntax /command for add/update).

    ansible-playbook site.yml -i localhost, -t update -e 'soft_name="apache" soft_version="2.2" soft_type="middle" soft_code="httpd_2.2" soft_description="a compiled apache for dev team"'

  **/etc/compiled_softwares_list.json (default path)**

    [
      {
          "name": "apache",
          "type": "middle",
          "version": "2.2",
          "revision": "1",
          "code": "httpd_2.2",
          "description": "a compiled apache for dev team"
      }
    ]

  **/etc/compiled_softwares_historical_list.json (default path)**

    [
      {
          "2024-05-11 21:09:04": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "1",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for dev team"
              }
          ]
      }
    ]

## Update signature in file (same syntax /command for add/update).

    ansible-playbook site.yml -i localhost, -t update -e 'soft_name="apache" soft_version="2.2" soft_type="middle" soft_code="httpd_2.2" soft_description="a compiled apache for developer team"'

  **/etc/compiled_softwares_list.json (default path)**

    [
      {
          "name": "apache",
          "type": "middle",
          "version": "2.2",
          "revision": "2",
          "code": "httpd_2.2",
          "description": "a compiled apache for developer team"
      }
    ]

  **/etc/compiled_softwares_historical_list.json (default path)**

    [
      {
          "2024-05-11 21:09:04": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "1",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for dev team"
              }
          ]
      },
      {
          "2024-05-11 21:10:17": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "2",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for developer team"
              }
          ]
      }
    ]

## Add an other signature in file for example purposes (same syntax /command for add/update).

    ansible-playbook site.yml -i localhost, -t update -e 'soft_name="nginx" soft_version="1.26.0" soft_type="middle" soft_code="nginx_1.26.0" soft_description="a compiled nginx for developer team"'

  **/etc/compiled_softwares_list.json (default path)**

    [
      {
          "name": "apache",
          "type": "middle",
          "version": "2.2",
          "revision": "2",
          "code": "httpd_2.2",
          "description": "a compiled apache for developer team"
      },
      {
          "name": "nginx",
          "type": "middle",
          "version": "1.26.0",
          "revision": "1",
          "code": "nginx_1.26.0",
          "description": "a compiled nginx for developer team"
      }
    ]

  **/etc/compiled_softwares_historical_list.json (default path)**

    [
      {
          "2024-05-11 21:09:04": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "1",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for dev team"
              }
          ]
      },
      {
          "2024-05-11 21:10:17": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "2",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for developer team"
              }
          ]
      },
      {
          "2024-05-11 21:11:59": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "2",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for developer team"
              },
              {
                  "name": "nginx",
                  "type": "middle",
                  "version": "1.26.0",
                  "revision": "1",
                  "code": "nginx_1.26.0",
                  "description": "a compiled nginx for developer team"
              }
          ]
      }
    ]

## Remove a signature in the file

    ansible-playbook site.yml -i localhost, -t 'uninstall' -e 'soft_name="apache" soft_version="2.2"'

  **/etc/compiled_softwares_list.json (default path)**

    [
      {
          "name": "nginx",
          "type": "middle",
          "version": "1.26.0",
          "revision": "1",
          "code": "nginx_1.26.0",
          "description": "a compiled nginx for developer team"
      }
    ]

  **/etc/compiled_softwares_historical_list.json (default path)**

    [
      {
          "2024-05-11 21:09:04": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "1",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for dev team"
              }
          ]
      },
      {
          "2024-05-11 21:10:17": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "2",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for developer team"
              }
          ]
      },
      {
          "2024-05-11 21:11:59": [
              {
                  "name": "apache",
                  "type": "middle",
                  "version": "2.2",
                  "revision": "2",
                  "code": "httpd_2.2",
                  "description": "a compiled apache for developer team"
              },
              {
                  "name": "nginx",
                  "type": "middle",
                  "version": "1.26.0",
                  "revision": "1",
                  "code": "nginx_1.26.0",
                  "description": "a compiled nginx for developer team"
              }
          ]
      },
      {
          "2024-05-11 21:13:46": [
              {
                  "name": "nginx",
                  "type": "middle",
                  "version": "1.26.0",
                  "revision": "1",
                  "code": "nginx_1.26.0",
                  "description": "a compiled nginx for developer team"
              }
          ]
      }
    ]

## Browse the compiled softwares signatures historical file

    ansible-playbook site.yml -i localhost, -t browse -e 'not_before="2024-05-11 21:13:45"'

    ansible-playbook site.yml -i localhost, -t browse -e 'not_after="2024-05-11 21:10:16"'

    ansible-playbook site.yml -i localhost, -t browse -e 'not_before="2024-05-11 21:11:58" not_after="2024-05-11 21:13:45"'

# Links:

About re-using roles in Ansible playbooks: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse.html#re-using-files-and-roles

About conditionals in Ansible : https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html

About Ansible playbooks's tags: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_tags.html
