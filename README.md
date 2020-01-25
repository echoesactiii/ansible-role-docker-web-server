# ansible-role-docker-web-server

This sets up a Docker web server on a CentOS 7 host with the following features:

- Uses the [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) container as the front-end proxy, which allows for automatic configuration of virtual hosts by environment variables.
- Uses the [JrCs/docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) container for LetsEncrypt, which allows for automatic issuance & renewals of LetsEncrypt certificates, configured with environment variables. It also automatically updates the vhosts in the `nginx-proxy` container with the configuration for the certificates.
- Automatically pulls a repository from Git container a docker-compose file (or other script) which is used to configure the containers and environment variables.
  - It uses an SSH deploy key to accomplish this.

## Vars & defaults

	docker_web_http_port: 80 # The HTTP port that should be exposed by nginx-proxy
	docker_web_https_port: 443 # The HTTPS port that should be exposed by nginx-proxy
	docker_web_deploy_key: "{{ role_path }}/../../files/deploy_key" # The path to the SSH public key for the deploy key
	docker_web_deploy_key_remote: "/tmp/git.deploy_key" # The path the SSH public key should be copied to on the server
	docker_web_build_command: "cd /opt/web-containers && docker compose up -d" # The build command for the container repository - can be a bash script, python etc
	docker_web_yum_dependencies:
	  # Yum dependencies
	  - python-setuptools
	  - python-pip
	  - git
	  - docker-compose
	docker_web_pip_dependencies:
	  # Pip dependencies
	  - docker

The following vars have no defaults and **must** be specified:

	docker_web_git_repo: "git@github.com:somebody/web-containers.git" # the git repository to build containers from
