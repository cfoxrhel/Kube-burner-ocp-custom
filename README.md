# Kube-burner-ocp-custom

after you untar it you'll see 'kubeburner/setup.txt'


note you 'll have to change the image repos to the quay repos for it to work for you in the below files.

templates/deployment.yaml:      - image: {repo url}/pause:3.1

templates/deployment_patch_add_pod_2.yaml:      - image: {repo ulr}/pause:3.1

archive/app-deployment.yml:        image: {repo url}/cloud-bulldozer/perfapp:latest
