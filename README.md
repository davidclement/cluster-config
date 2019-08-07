# EKS Cluster Config Repo

Goal: wire a github repo to a set of eks clusters for the baseline cluster config (RBAC, cluster autoscaler, etc)

## Components
- API Gateway POST receiver for webhook
- Lambda webhook receiver
- Lambda cluster configurator
- This Repo:
	- variables.yaml --> variables available to interpolate in manifest templates
	- secrets.yaml --> secrets to fetch for interpolating into manifests, ideally k8s secrets
	- default dir: for default manifests
	- evnironment/<environment name> dirs: for environment specific manifest (versions) - if a manifest by same name already exists from default, replaced w/ this one
	- cluster/<cluster name> dirs: for cluster-specific manifest (versions) - if a manifest by same name already exists from default or environment, replaced w/ this one

