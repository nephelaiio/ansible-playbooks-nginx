# nephelaiio.playbooks-nginx

[![Build Status](https://travis-ci.org/nephelaiio/ansible-playbooks-nginx.svg?branch=master)](https://travis-ci.org/nephelaiio/ansible-playbooks-nginx)

Ansible playbook to install nginx server and configure vhosts

## Playbook descriptions

The following lists the group targets and descriptions for every playbook

| playbook  | description                                    | target      |
| ---       | ---                                            | ---         |
| proxy.yml | install nginx server and configure proxy vhost | nginx_proxy |

## Playbook variables

The following parameters are available/required for playbook invocation

### [proxy.yml](proxy.yml):
| required | variable                          | default                                        | description                                                 |   |
| ---      | ---                               | ---                                            | ---                                                         |   |
| *yes*    | nginx_proxy_url                   | _undefined_                                    | target awx url                                              |   |
| *yes*    | nginx_proxy_aws_access_key_id     | lookup('env', 'AWS_ACCESS_KEY_ID')             | an ec2 key id with route53 management rights                |   |
| *yes*    | nginx_proxy_aws_access_key_secret | lookup('env', 'AWS_SECRET_ACCESS_KEY')         | an ec2 key secret                                           |   |
| no       | nginx_proxy_upstream_servers      | _undefined_                                    | the set of proxy upstream servers (*)                       |   |
| no       | nginx_proxy_upstream_group        | _undefined_                                    | the inventory group for upstream servers (*)                |   |
| no       | nginx_proxy_pdns_url              | _undefined_                                    | pdns api url for dns record update                          |   |
| no       | nginx_proxy_pdns_api_key          | _undefined_                                    | pdns api key for dns record update                          |   |
| no       | nginx_proxy_headers               | _undefined_                                    | complete set of proxy headers to define (override defaults) |   |
| no       | nginx_proxy_headers_extra         | {}                                             | set of extra proxy headers to define (augment defaults)     |   |
| no       | nginx_proxy_upstream_name         | "{{ nginx_proxy_url / urlsplit('hostname') }}" | nginx config upstream name                                  |   |
| no       | nginx_proxy_upstream_scheme       | "{{ nginx_proxy_url / urlsplit('scheme) }}"    | nginx config upstream protocol                              |   |

(*) Strictly one of these variables must be defined

## Data Types

### Upstream Servers
```{yaml}
nginx_proxy_upstream_servers:
  - address: 10.50.1.1
    port: 80
    weight: 1
    health_check: max_fails=1 fail_timeout=3s
```

### Upstream Group
```{yaml}
nginx_proxy_upstream_group: nginx_proxy_upstream
```

### Proxy Headers
```{yaml}
nginx_proxy_headers_extra:
  header_referer:
    name: Referer
    value: https://ipa.example.com
```

## Example Invocation

```
git checkout https://galaxy.ansible.com/nephelaiio/ansible-playbooks-nginx nginx
ansible-playbook -i inventory/ nginx/proxy.yml
```

## License

This project is licensed under the terms of the [MIT License](/LICENSE)
