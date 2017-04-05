# Base image Ubuntu
---
- Build base image command : `docker build -t toixtran/ubuntu:16.04 .`
- User/pass : `root/toor`
- From : ubuntu:16.04
- Packages : `python, vim, wget, curl, git-all, openssh-server, supervisor`
- Services : supervisor, openssh (permit root login)