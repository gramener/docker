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
        Machine: Linux xxxxxxxxxxxx (long machine name / ID)
        Location: /home/gramex/gramex/LICENSE
        Visit http://127.0.0.1:8888 for a license

4. The message you get has your machine ID. Email the message to
   <license@gramener.com> for a license.
