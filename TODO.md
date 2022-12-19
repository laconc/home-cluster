# TODO

* Document CIDR ranges
* Set up a second instance of Blocky and Redis. Consider enabling logging.
* Consider DNSSEC and other such protection mechanisms
* Can we default to DoH when accessing the authoritative servers?
* Add initContainer for pdns-auth to set up default zone; `pdnsutil create-zone 64f.dev`
* Annotate the pieces used for this setup in the readme
* Update chart metadata
* Encrypt the drives
* Encrypt secrets at rest
* Set up Loki
* Set up `alertmanager` alerts
* Fix the Longhorn PrometheusRules
* Enable mTLS between pods
* Enable ufw in the nodes
* Set NetworkPolicies
* Enable a dependency manager in Github
* Consider backing up to the cloud
* Automate the manual commands in the README, likely via Helmfile hooks
* Look into running all the images as non-root
