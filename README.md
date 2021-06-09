# salt-bootstrap

## Preamble

This is the process that I am using to bootstrap the salt deployment to my raspberry pi cluster. I am running this from a WSL2 VM on Windows 10 (in June 2021) so some additional steps are necessary, hopefully these can be elided in the future (or now if you are using a real linux box to bootstrap from)

## Install minions

Salt SSH has way too many outstanding and long running bugs ([6+ years](https://github.com/saltstack/salt/issues/31531)) to be useful for bootstrapping.
Instead bootstrap the minions using the [bootstrap script](https://github.com/saltstack/salt-bootstrap) via

```
curl -o bootstrap-salt.sh -L https://bootstrap.saltproject.io
sudo sh bootstrap-salt.sh -A daisy
```

(`daisy` is the name of my workstation that is serving as the bootstrap salt-master)

Then the salt formula can take it from there.

## Run master

From this diretory start the salt-master

```
salt-master
```

Then from an admin Powershell console you will need to set up port forwarding to the WSL VM. Annoyingly the IP changes each time I reboot.

```
wsl --exec ip addr
```

Note the IP address of eth0

```
netsh interface portproxy add v4tov4 listenport=4505 listenaddress=0.0.0.0 connectport=4505 connectaddress=<IP>
netsh interface portproxy add v4tov4 listenport=4506 listenaddress=0.0.0.0 connectport=4506 connectaddress=<IP>
```

## Accept the keys

```
salt-key -A
```

## Test it

```
salt '*' test.ping
```
