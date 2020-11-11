# Hackathon HOWTO

## Helpful links

* [Devex docs setup](https://docs.google.com/document/d/1cP0Bby1we9sI7NDMJHgwqLVoz-IRmmrmF_2VwfyPUVM/)
* [Handout doc](https://docs.google.com/document/d/1I1EmMvgd-EDkF47owUWf0AcixVBie5En9ynYKvE8STo/edit)

## Start up the cluster

* Follow the instructions [here](https://docs.google.com/document/d/1mnHIHaoP6-yeNITU_4QdkISfMhiuXpxdxKFgQuNaG9E/).

## Set up the cluster

1. Copy `setup/playbooks/group_vars/all/cluster.yml.template` sideways to `cluster.yml`, then edit this new file, embedding token & URL information from the cluster that was set up.

1. (OPTIONAL) Install OpenShift Container Storage

    * You must have at least 3 16 vCPU nodes in the cluster to support OCS.  Remember that it will consume a significant quantity of CPU and memory - you will be best served by dedicating an extra three nodes.

    * Follow the instructions here https://access.redhat.com/documentation/en-us/red_hat_openshift_container_storage/4.5/html/deploying_openshift_container_storage_using_amazon_web_services/deploy-using-dynamic-storage-devices-aws to install the operator and create a storage cluster.

    * Update the storage classes, so that the new OCS storage class is the default.

1. Deploy the Devex lab docs

    * Execute shell commands:

        ```sh
        cd setup/playbooks
        ansible-playbook Deploy_Docs.yml
        ```

    * This generates a bunch of documentation, at URLs:

        * https://username-distribution-devex.apps.cluster-9ebb.9ebb.example.opentlc.com/
        * http://docs-devex.apps.cluster-9ebb.9ebb.example.opentlc.com/workshop/intro/lab/intro
        * http://docs-devex.apps.cluster-9ebb.9ebb.example.opentlc.com/workshop/essentials/lab/applicationbasics
        * http://docs-devex.apps.cluster-9ebb.9ebb.example.opentlc.com/workshop/innovation/lab/rbac
        * http://docs-devex.apps.cluster-9ebb.9ebb.example.opentlc.com/workshop/innovation/lab/camelkocpserverlesseventing
    
    * Adjust, copy and paste these into the appropriate places in the [handout doc](#helpful-links).

    * Test they work!

1. Deploy Etherpad
    
    * Perform shell tasks:
    
        ```sh
        cd setup/etherpad
        ./deploy.sh
        ```

    * This creates an etherpad interface here: https://etherpad-etherpad.apps.cluster-9ebb.9ebb.example.opentlc.com/

    * Create a pad, named something sensible "hackathon-2020".

    * Import the starting content from `HACKATHON-starter.etherpad`.
        
        * If this fails, copy and paste from the fallback textfile `HACKATHON-starter-fallback.txt`, and update headers as appropriate for better formatting.

    * Update the Etherpad with the cluster URL.

    * Copy and paste the Etherpad starting URL into the appropriate place in the [handout doc](#helpful-links).

1. Generate groups, projects, etc.

    ```sh
    ansible-playbook Deploy_Groups.yml
    ```

1. Deploy Advanced Course 

    This deploys the operators, etc.  AMQ Broker, Fuse Online are per-project.  All others are shared.

    ```sh
    ansible-playbook Deploy_Advanced_Course.yml
    ```
