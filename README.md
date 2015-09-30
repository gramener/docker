Docker setups
=============

This repo has Dockerfiles to install Gramex

Installation
------------

1. Install Docker on a Linux system. See the instructions for your OS at
   <https://docs.docker.com/installation/>
2. Build the container: `sudo docker build --tag gramener/gramex:0.17rc https://github.com/gramener/docker.git#:gramex-0.17rc-postgresql`
3. Run the instance: `sudo docker run -i -t gramener/gramex:0.17rc`

This instance has a Linux and PostgreSQL user named `gramex` with password
`gramex`

Test the setup
--------------

1. Run `sudo service postgresql start`. (If prompted, the password is `gramex`)
2. Run `psql -c 'CREATE DATABASE test'`. The response should be `CREATE DATABASE`
3. Run `python gramex/gramex.pyc`. You should get the following license message:

    No license file was found.
    Machine: Linux xxxxxxxxxxxx 3.13.0-48-generic #80-Ubuntu SMP Thu Mar 12 11:16:15 UTC 2015 x86_64 x86_64
    Location: /home/gramex/gramex/LICENSE
    Visit http://127.0.0.1:8888 for a license
