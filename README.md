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

## Variable interpolation 
- start the variable interpolation w/ a cluster name
- load variables.yaml into a dict(), say, config_vars
- parse the environment off the front of that name (dev-, or production-, )
- load the dictionary config_vars['default'] into vars
- next, if '<ENVIRONMENT_NAME>' in config_vars['environment'], UPDATE vars w/ config_vars['environment']['<ENVIRONMENT_NAME>']
- next, if '<CLUSTER_NAME>' in config_vars['cluster'], UPDATE vars w/ config_vars['cluster']['<CLUSTER_NAME>']

## Secrets rendering
- same 3-level approach: default, environment->env_name, cluster->cluster_name
- secrets have an "arn" and "name", and something to tell us what to pull out of the secret. 
	- fetched from arn
	- stored in secrets under scope (default|environment|cluster) and name
	- will need to experiment w/ fetching secrets to figure out what else we need to store in the yaml about fetching them
- next
