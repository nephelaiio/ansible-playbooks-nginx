# nephelaiio.playbooks-nginx

[![Build Status](https://travis-ci.org/nephelaiio/ansible-playbooks-nginxproxy.svg?branch=master)](https://travis-ci.org/nephelaiio/ansible-playbooks-nginxproxy)

Ansible playbook to install nginx server and configure vhosts

## Playbook descriptions

The following lists the group targets and descriptions for every playbook

| playbook  | description                                    | target      |
| ---       | ---                                            | ---         |
| proxy.yml | install nginx server and configure proxy vhost | nginx_proxy |

## Playbook variables

The following parameters are available/required for playbook invocation

### [proxy.yml](proxy.yml):
| required | variable                          | description                                                 | default                                |
| ---      | ---                               | ---                                                         | ---                                    |
| *yes*    | nginx_proxy_url                   | target awx url                                              | _undefined_                            |
| *yes*    | nginx_proxy_aws_access_key_id     | an ec2 key id with route53 management rights                | lookup('env', 'AWS_ACCESS_KEY_ID')     |
| *yes*    | nginx_proxy_aws_access_key_secret | an ec2 key secret                                           | lookup('env', 'AWS_SECRET_ACCESS_KEY') |
| no       | nginx_proxy_upstream_servers      | the set of proxy upstream servers (*)                       | _undefined_                            |
| no       | nginx_proxy_upstream_group        | the inventory group for upstream servers (*)                | _undefined_                            |
| no       | nginx_proxy_pdns_url              | pdns api url for dns record update                          | _undefined_                            |
| no       | nginx_proxy_pdns_api_key          | pdns api key for dns record update                          | _undefined_                            |
| no       | nginx_proxy_headers               | complete set of proxy headers to define (override defaults) | _undefined_                           |
| no       | nginx_proxy_headers_extra         | set of extra proxy headers to define (augment defaults)     | {}                                     |

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
git checkout https://galaxy.ansible.com/nephelaiio/ansible-playbooks-nginxproxy nginxproxy
ansible-playbook -i inventory/ nginxproxy/proxy.yml
```

## License

This project is licensed under the terms of the [MIT License](/LICENSE)
