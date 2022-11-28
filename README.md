# az-tf-github

# Secrets
+ 5 Secrets configured in github repo settings which is used in github action pipeline.
<img width="842" alt="image" src="https://user-images.githubusercontent.com/19683199/165492270-56a0e30b-1d65-4e06-b80f-fae2cb79f790.png">


# Terraform Var File 
+ 01-var.json

# Github Action File
+ .github/workflows/deploy.yml

# Github Action File succesfully processed will be in processing/passed
<img width="1684" alt="image" src="https://user-images.githubusercontent.com/19683199/165535369-2be73be6-4503-4e04-97b5-90f37f597087.png">

# Github Action File failed will be in processing/failed
<img width="1684" alt="image" src="https://user-images.githubusercontent.com/19683199/165535551-1ea686eb-7dc2-42d2-b49a-c693a9c9ea0f.png">

# Azure Resource group created
<img width="1514" alt="image" src="https://user-images.githubusercontent.com/19683199/164457790-f959f6f1-c953-43e6-b6bd-4b7e3a50b829.png">


# V2 Changes

RECAP of what github actions does to 2 repos:
+ Workflow reads from `SunnyOswal/az-tf` repo.
+ Workflow writes to `SunnyOswal/az-tf-github` repo.

SUMMARY OF WHAT WE WILL DO TO MAKE ABOVE WORK:
+ Create key pair 1 :
  + Public key of this key pair will be added to `az-tf` repo's `Deploy keys`
  + Private key of this key pair will be added to `az-tf-github` repo as secret so that pipeline can clone `az-tf` repo.

+ Create key pair 2 :
  + Public key of this key pair will be added to `az-tf-github` repo's `Deploy keys`
  + Private key of this key pair will be added to `az-tf-github` repo as secret so that pipeline can write to `az-tf-github` repo.


## Create key pair 1 for `az-tf` repo
+ Open Terminal.
+ Paste the text below, substituting in your GitHub email address.
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
+ After following as per prompt, you will see below message:
`Your public key has been saved in /home/XXXX/.ssh/id_ed25519.pub`

+ Get public key by running below command (public key path from above step). Keep this public key handy as will be used in below steps.
```
cat /home/XXXX/.ssh/id_ed25519.pub
```

+ Get private key by running below command (public key path from above step). Keep this private key handy as will be used in below steps.
```
cat /home/XXXX/.ssh/id_ed25519
```

## Add Public key to `az-tf` repo
+ From your repository, click Settings.
+ In the sidebar, click Deploy Keys, then click Add deploy key.
+ Provide a title `PublicKey`, paste in your public key from previous `Generating a new SSH key` step and click `Add key`
+ DO NOT CHECK THE CHECKBOX AS WE ONLY WANT TO READ FROM `az-tf` repo
![image](https://user-images.githubusercontent.com/19683199/204340698-77051f35-21d6-4518-b2ad-94453ac7819a.png)

## Add Private key to `az-tf-github` repo SECRET (AZ_TF_PRIVATE_KEY) so that it can clone az-tf repo
![image](https://user-images.githubusercontent.com/19683199/204345298-31422bbb-9596-44cb-bfe8-2f1ff2655ad7.png)

## Create key pair 2 for `az-tf-github` repo
+ Open Terminal.
+ Paste the text below, substituting in your GitHub email address.
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
+ This time just pass some other path like i passed `/home/XXXX/.ssh/id_ed25519_v2` as previous key already stored in default path
```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/XXXX/.ssh/id_ed25519): /home/XXXX/.ssh/id_ed25519_v2
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/XXXX/.ssh/id_ed25519_v2
Your public key has been saved in /home/XXXX/.ssh/id_ed25519_v2.pub
The key fingerprint is:
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
...
.....
......
.......
```

+ After following as per prompt, you will see below message:
`Your public key has been saved in /home/XXXX/.ssh/id_ed25519_v2.pub`

+ Get public key by running below command (public key path from above step):
```
cat /home/XXXX/.ssh/id_ed25519_v2.pub
```

## Add Public key to `az-tf-github` repo
+ From your repository, click Settings.
+ In the sidebar, click Deploy Keys, then click Add deploy key.
+ Provide a title `PublicKey`, paste in your public key from previous `Generating a new SSH key` step and click `Add key`
+ CHECK THE CHECKBOX AS WE WANT TO WRITE TO `az-tf-github` repo
![image](https://user-images.githubusercontent.com/19683199/204341524-b1746fc8-1490-4311-b646-fc9d4ca2976f.png)


## Add Private key to `az-tf-github` repo SECRET (AZ_TF_GITHUB_PRIVATE_KEY) so that it can write to az-tf-github repo.

## Total secrets (you can remove the `REPO_TOKEN` as no longer required):
![image](https://user-images.githubusercontent.com/19683199/204347817-c95e4f70-becd-4937-9879-d755ede7350b.png)
