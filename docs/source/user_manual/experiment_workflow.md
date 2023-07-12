# Experiment Workflow 

This document will lay out all of the steps that will need to happen in order to run an experiment through the "experimental pipeline".
It will outline what both the user and operator will do, along with any actions that can be automated on devices. 

### Actors:

This outline will reference four different actors that will work throughout the "experimental pipeline".

> * User: A user is an external person who will create projects and experiments using Discover's software and hardware.
> * Operator: An operator is a Discover-affiliated individual who will be responsible for ensuring that experiments are properly run on the hardware.
> * Portal: In this instance, the Discover Portal is both the software used to manage experiments and the server the software is running on.
> * Site-Server: The Site-Server is a server that is dedicated to a specific Discover site.

### 1. Experiment Creation:
> * User: The user will create an experiment in the Discover Portal. They will provide a Resource Definition, GitHub link (based on our templates), cloud storage link,
name, and description.
> * Operator: None
> * Portal: The Portal will create a new directory pertaining to that individual experiment, giving it a unique experiment ID.
The experiment mode and status will both be set to idle.
> * Site-Server: None

### 2. Initiate Development:
> * User: When ready, the user will initiate development on their experiment. This signals to the Discover team that the experiment is ready to run on the hardware.
> * Operator: None
> * Portal: The Portal will set the mode to Development and the status to deploying. The portal will then run `cd /experiments/<experiment_site>/ && git clone <experiment_GitHub_link> && mv <experiment_repo_name> <experiment_id>`
to properly clone the code. Then it will run `cp /<experiment_path_on_web_server>/config.yaml /experiments/<experiment_site>/<experiment_id>` to get the configuration file in the right place.
Next, it will use rsync to copy over the experiment with `rsync -avz /experiments/<experiment_site>/<experiment_id> <user>@<site-server>:/experiments/<experiment_id>`.
Finally, it will use ssh to instruct the Site-Server to build the code into a Docker container image with `ssh <user>@<site-server> ./package-builder.py --pkg <experiment_id>`.
> * Site-Server: The Site-Server will receive the experimental code, build a Docker container image, generate a docker-compose file, and push the image to the local repository, as instructed by the Portal.

### 3. Experiment Setup:
> * User: None
> * Operator: After the Docker container image has been built, the operator will change the mode to TESTBED, while keeping the status at deploying.
> * Portal: None
> * Site-Server: The Site-Server will use rsync to move a generated docker-compose file to the correct piece of hardware with `rsync -avz /experiments/<experiment_id> <user>@<end-node>:~/<experiment_id>`.

### 4. Experiment Run:
> * User: None
> * Operator: The operator will keep the mode at TESTBED, but change the state to ready. Then, they will run the experiment under the control module with `./control-module.py --exp <experiment_id>`. This control module will not only ensure the experiment runs properly, but will also get experimental results off of the container and clean up the Docker engine, along with putting the results back on the Site-Server.
This should start up a docker container using the compose file, which will automatically pull the container image from the local repository on the Site-Server. Then, the experiment will run.
> * Portal: None
> * Site-Server: None

### 5. Experiment Finished:
> * User: None
> * Operator: The operator will keep the mode at TESTBED, but change the state to submitted.
> * Portal: The change in the state will signal to the portal that the experiment is finished. It will run a rsync pull to get the experiment's results back from the Site-Server, with `rysnc -avz <user>@<site-server>:/experiments/experiment_id> /experiments/<experiment_site>/<experiment_id>`. Then, the results will automatically be uploaded via the user's cloud storage link.
> * Site-Server: The Site-Server will take actions as directed by the Portal.
