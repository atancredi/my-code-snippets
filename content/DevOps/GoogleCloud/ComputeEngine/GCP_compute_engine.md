# Use VMs on Google Cloud Platform's Compute Engine

## Connect to a VM

0. Before connecting to a VM from the terminal, a test connection (for authorization) must be made via browser to enable the SSH keys.


1. Ensure that the current user is the right one on the CLI with
```bash
gcloud auth list
```

2. If the active user is not the right one, then activate it with
```bash
gcloud config set account 'ACCOUNT'
```

3. Run the command to start the VM
```bash
gcloud compute ssh 'VMNAME' --project 'PROJECTNAME'
```

## Managing files
From the GCP's [docs](https://cloud.google.com/compute/docs/instances/transfer-files?hl=it)

### Transfer a file from the host to the VM
```bash
gcloud compute scp LOCAL_FILE_PATH VM_NAME:REMOTE_DIR --project PROJECTNAME
```

### Transfer a folder from the VM to the host
```bash
gcloud compute scp --recurse VM_NAME:REMOTE_DIR LOCAL_FILE_PATH --project PROJECTNAME
```

## Manage private GitHub repos
1. Generate a `github_rsa` key from github.
2. Run these commands to make it effective
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/github_rsa
```
- it's possible that the rsa file has permissions on it, so with sudo user run `chmod a+rw github_rsa` if any problem with permissions.


## Monitor your VM
- For CPU and memory usage
```bash
top
```

- For NVIDIA GPUs usage
```bash
watch -n0.1 nvidia-smi
```

