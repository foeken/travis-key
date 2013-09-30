# Travis Key

Bash script to quickly create an [encrypted rsa key to use on Travis](http://about.travis-ci.org/docs/user/travis-pro/#How-can-I-encrypt-files-that-include-sensitive-data).


## Installation

Clone the repo and make sure `travis-key` is available on your `$PATH`

## Usage

Inside of your github repo run (you will be prompted for your github password):

```
travis-key -u <username> -o <org> -r <repo> -k <path/to/id_rsa>
```

The `-o` is only required if it is different from the `-u` value.

If successful, it will generate:

```
.travis/
  id_rsa.enc
  secret
```

## Required travis.yml settings to decrypt the key on Travis and use it

Inside of your `.travis.yml`

```
# Decrypt our ssh key to allow travis to commit built packages into github
- secret=`openssl rsautl -decrypt -inkey ~/.ssh/id_rsa -in .travis/secret`
- openssl aes-256-cbc -k "$secret" -in .travis/id_rsa.enc -d -a -out ~/.ssh/id_rsa
- ssh-add -D
- chmod 600 ~/.ssh/id_rsa
- ssh-add ~/.ssh/id_rsa
```
