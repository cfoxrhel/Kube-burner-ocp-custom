# Kube-burner-ocp-custom

after you untar it you'll see 'kubeburner/setup.txt'


note you 'll have to change the image repos to the quay repos for it to work for you in the below files.

templates/deployment.yaml:      - image: {repo url}/pause:3.1

templates/deployment_patch_add_pod_2.yaml:      - image: {repo ulr}/pause:3.1

archive/app-deployment.yml:        image: {repo url}/cloud-bulldozer/perfapp:latest


# Contents of setup.txt file

-> all commands done after untaring file and staying in the same directory this readme file is located
-> copying the kube-burner-ocp to '~/'
-> note can have subdirectory under '~/' like i use 'cfworkspace'

cp ./kube-burner-ocp-custom/kube-burner-ocp ~/

-> set alias to bash
  
echo "alias kube-burner-ocp='~/kube-burner-ocp'" >> ~/.bash_profile
source ~/.bash_profile

-> if you are good with 400 namespaces/pods set across your workernodes then no change needed
-> if you desire to change it from 400 to something else do the following.
  
vi ./kube-burner-ocp-custom/node-density-heavy-cfox-v1/node-density-heavy.yml

-> the files edited would be the '400' in the below area for 'jobIterations'
jobs:
  - name: api-intensive-v1
    jobIterations: 400

-> once set to run the command you can choose to let it do the default garbage collection of the namespaces at the end or have that be a seperate command.
-> the below commands sequence has the --gc disabled and then the namespace delete command done seperate (giving time to watch the api traffic stabilize)
-> to run the kube-burner-ocp command taking the custom files you have to be in the directory that houses the '*.yml' file and the 'templates' directory

-> switch to the required directory
  
cd kube-burner-ocp-custom/node-density-heavy-cfox-v1/

-> cluster access
  
kubeconfig {cluster FQDN}

-> command with automatic cleanup disabled
  
kube-burner-ocp node-density-heavy --gc=false

-> checks namespace count, it should match the value set for 'jobIterations'
  
oc get projects | grep api-intensive-v1 | wc -l

-> deletes the namespaces created

oc delete namespace api-intensive-v1-{0..400}
