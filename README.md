Gramex setup
============

Follow these instructions to install the Gramener Visualisation Server.

Installation
------------

Install Docker on a Linux system. See the instructions for your OS at
<https://docs.docker.com/installation/>

Build the container:

    sudo docker build --tag gramener/gramex:0.17rc https://github.com/gramener/docker.git#:gramex-0.17rc-postgresql

Run the instance:

    sudo docker run -i -t gramener/gramex:0.17rc

This instance has a Linux and PostgreSQL user named `gramex` with password
`gramex`

Test the setup
--------------

1. Start PostgreSQL. (If prompted, the password is `gramex`.)

        sudo service postgresql start

2. Create a database. The response should be `CREATE DATABASE`.

        psql -c 'CREATE DATABASE test'

3. Run `python gramex/gramex.pyc`. You should get the following license message:

        No license file was found.
        Machine: Linux xxxxxxxxxxxx (long machine name / ID)
        Location: /home/gramex/gramex/LICENSE
        Visit http://127.0.0.1:8888 for a license

4. The message you get has your machine ID. Email the message to
   <license@gramener.com> for a license.
