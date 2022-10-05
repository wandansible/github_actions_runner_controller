Ansible role: GitHub Actions Runner Controller
==============================================

Deploy GitHub actions runner controllers on a kubernetes
cluster. This can be used for deploying ephemeral self-hosted
actions runners across multiple repositories or organisations.

Requirements
------------

This role requires the kubernetes.core collection to be installed.

Role Variables
--------------

```
ENTRY POINT: main - Deploy GitHub actions runner controllers

        Deploy GitHub actions runner controllers on a kubernetes
        cluster. This can be used for deploying ephemeral self-hosted
        actions runners across multiple repositories or organisations.

OPTIONS (= is mandatory):

= arc_app_id
        ID of the GitHub app

        type: int

= arc_app_private_key
        Private SSH key generated for the GitHub app

        type: str

- arc_cert_manager_version
        Version of cert-manager to install
        [Default: 1.8.0]
        type: str

- arc_config_dir
        Directory containing configuration for ARC
        [Default: /etc/github-actions-runner-controller]
        type: str

- arc_enterprise_server
        URL for a GitHub enterprise server
        [Default: (null)]
        type: str

- arc_manifests_dir
        Directory containing kubernetes manifests for ARC
        [Default: /etc/github-actions-runner-controller/manifests]
        type: str

- arc_rootless_docker_image
        The rootless docker image used for the sidecar container
        [Default: docker:dind-rootless]
        type: str

- arc_runners
        List of runners to deploy
        [Default: (null)]
        elements: dict
        type: list

        OPTIONS:

        = app_installation_id
            ID of the GitHub app installation

            type: int

        = org
            Name of GitHub organisation

            type: str

        - replicas
            Number of runners to deploy
            [Default: 1]
            type: int

        - repo
            Name of GitHub repository
            [Default: (null)]
            type: str
```

Installation
------------

This role can either be installed manually with the ansible-galaxy CLI tool:

    ansible-galaxy collection install kubernetes.core
    ansible-galaxy install git+https://github.com/wandansible/github_actions_runner_controller,main,wandansible.github_actions_runner_controller
     
Or, by adding the following to `requirements.yml`:

    roles:
      - name: wandansible.github_actions_runner_controller
        src: https://github.com/wandansible/github_actions_runner_controller

    collections:
      - name: kubernetes.core

Roles listed in `requirements.yml` can be installed with the following ansible-galaxy command:

    ansible-galaxy install -r requirements.yml

Example Playbook
----------------

    - hosts: github_actions_runner_controllers
      roles:
         - role: wandansible.github_actions_runner_controller
           become: true
           vars:
             arc_enterprise_server: https://github.example.org
             arc_app_id: 1
             arc_runners:
               - org: exampleorg
                 app_installation_id: 2
             arc_app_private_key: CHANGEME
