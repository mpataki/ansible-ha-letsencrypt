# ansible-ha-letsencrypt

[Let's Encrypt](https://letsencrypt.org/) is a free service that provides SSL certificates used for end-to-end encryption on the internet. It can be used to provide certs for HTTPS traffic, as well as any other TLS protocol.

This service mainly consumes a valid hostname, such as `my_house.duckdns.org`, and produces SSL certificants using Let's Encrypt. In addition to obtaining the initial certificates, it will also install a cron job to make sure they are automatically renewed.

On renewal, it is common to have to restart services like nginx or mosquitto which may be using the certificates to encrypt their TCP traffic. Variables are also provided which allow for these kinds of pre/post hooks to occur automatically.

## Requirements

Really this should work on any debian based system, but has been tested on a Raspberry Pi running Hassbian.

## Role Variables

- `certbot_hostnames`
  - Ex. `my_home.duckdns.org,grafana.myhome.duckdns.org`
  - This is a comma delineated list of hostnames that the SSL cert will be issued for.
- `certbot_pre_hook`:
  - Ex. `systemctl stop nginx`
  - This is anything shell command that you want to run before an SSL cert update.
- `certbot_post_hook`:
  - Ex. `systemctl restart mosquitto; systemctl start nginx`
  - This is any shell command that you want to run after updating you SSL certs.
- `certbot_email`:
  - Ex. `you@your-domain.com`
  - This is the email that will be pinged when your cert is about to expire (although it should be renewed automatically).

## Dependencies

None.

## Example Playbook

```yml
    - hosts: pi
      vars:
        certbot_hostnames: 'my_home.duckdns.org,grafana.myhome.duckdns.org'
        certbot_pre_hook: 'systemctl stop nginx'
        certbot_post_hook: 'systemctl restart mosquitto; systemctl start nginx'
      roles:
         - role: mpataki.ha-letsencrypt
```

## License

MIT

