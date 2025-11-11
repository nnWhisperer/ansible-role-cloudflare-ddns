This is archived. Instead use https://github.com/favonia/cloudflare-ddns image inside a docker compose, more practical. The problem with current repo is it uses unmaintained dockerhub image.


Role Name
=========

**cloudflare-ddns** - Install cloudflare DDNS docker image.

This role helps you push your public IP address on Cloudflare DNS, so your self-hosted sites can be accessed. 

Role Variables
--------------

This role uses the variables listed below, along with default values (see defaults/main.yml).

You can specify the docker container name and the image version to be used. You can also specify the docker volume where the container should store its relevant data

```yml
dns_container_name: "cloudflare-dns"
dns_image_version: "latest"
dns_volume_config: "/tmp"
```

The `dns_domains` variable contains credentials and domain definitions. Credentials are specified like so:

```yaml
dns_domains:
  auth:
    scopedToken: QPExdfoNLwndJPDbt4nK1-yF1z_srC8D0m6-Gv_h
```
This role allows you to set up DDNS for multiple domains:

```yaml
dns_domains:
  domains:
  - name: foo.example.com
    type: A
    proxied: true
    create: true
    zoneId: JBFRZWzhTKtRFWgu3X7f3YLX
  - name: bar.example.com
    type: A
    proxied: true
    create: true
    zoneId: JBFRZWzhTKtRFWgu3X7f3YLY
```

All items declared into `dns_domains` are converted to the necessary configuration file via the *config.yaml.j2* template.

If you want more details on which parameters you can add to `dns_domains`, please see the [container's documentation](https://hub.docker.com/r/oznu/cloudflare-ddns/#!)



Example Playbook
----------------

You can see a full example (with dummy vars too!) below. It configures two sub-domains as A records:

```yaml
- hosts: servers
  vars:
    dns_container_name: "cloudflare-dns"
    dns_image_version: "latest"
    dns_volume_config: "/tmp"
    dns_domains:
      auth:
        scopedToken: QPExdfoNLwndJPDbt4nK1-yF1z_srC8D0m6-Gv_h
      domains:
      - name: foo.example.com
        type: A
        proxied: true
        create: true
        zoneId: JBFRZWzhTKtRFWgu3X7f3YLX
      - name: bar.example.com
        type: A
        proxied: true
        create: true
        zoneId: JBFRZWzhTKtRFWgu3X7f3YLY

  roles:
      - 'laurivan.cloudflare_ddns'
```


License
-------

MIT

Author Information
------------------

This role was created in 2022 by [Laur Ivan](https://www.laurivan.com).
