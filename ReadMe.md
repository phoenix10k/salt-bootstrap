## Bootstrapping

Salt SSH has way too many outstanding and long running bugs ([6+ years](https://github.com/saltstack/salt/issues/31531)) to be useful for bootstrapping.
Instead bootstrap the minions using the [bootstrap script](https://github.com/saltstack/salt-bootstrap) via

```
curl -o bootstrap-salt.sh -L https://bootstrap.saltproject.io
sudo sh bootstrap-salt.sh -A <bootstrap_master>
```

Then the salt formula can take it from there.