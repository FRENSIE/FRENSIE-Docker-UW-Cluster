# FRENSIE-Docker-UW-Cluster
=====

## Dependencies/Requirements

1. [Docker](https://www.docker.com/)
2. [git](https://git-scm.com/)
3. [UW HPC account](http://chtc.cs.wisc.edu/HPCuseguide.shtml)
4. [MCNP License](https://rsicc.ornl.gov/Default.aspx) - optional

## Installing Dependencies

### Installing Docker
1. run `sudo apt-get install docker`
2. run `sudo groupadd docker`
3. run `sudo gpasswd -a $USER docker`
4. restart system

**Note:** Steps 2-4 will allow docker to be run without superuser privileges.

### Installing Git
1. run `sudo apt-get install git`

## Getting UW HPC Account
Follow the instructions on the UW HPC website for creating an account. When
requesting your account you must also request access to the frensie group. All
frensie related software will be stored on the cluster at /home/group/frensie.

## Building FRENSIE on UW HPC Clone (first build)
1. create a working directory where the docker files will be built (e.g. software/uw-clone)
2. move to uw-clone directory
3. run `git clone https://github.com/FRENSIE/FRENSIE-Docker-UW-Cluster.git`
4. run `ln -s FRENSIE-Docker-UW-Cluster src`
5. run `rn mkdir build`
6. move to the build directory
7. run `cp ../src/scripts/frensie-docker.sh ../src`
8. set the UW_CLUSTER_USER_NAME variable to the user name on the UW cluster
9. set the UW_CLUSTER_USER_DIR variable to the user directory on the UW cluster
   (absolute path)
10. run `./frensie.sh` to configure the Docker files
11. run `make` (this may take a while as all of FRENSIE's software dependencies
    and FRENSIE will be built)

## Building FRENSIE on UW HPC Clone after FRENSIE Update
1. move to the build directory
2. run `make clean-docker-update` (quicker) or run `make clean-docker`
   (requires full rebuild of all images, which is slow)
3. run `make`

## Installing FRENSIE Dependencies on UW HPC
**Note:** Skip these steps if the dependencies have already been installed on
the UW HPC cluster.
1. run `cd path-to-build/frensie-update` where path-to-build is the path to the
   FRENSIE-Docker-UW-Cluster build directory
2. run `./run-frensie-update.sh` to load the frensie-update image
3. run `cd /home/group/frensie/software`
4. run `tar -czvf frensie-deps.tar.gz`
5. run `scp frensie-deps.tar.gz UW_HPC_USER_NAME@ace-service-1.chtc.wisc.edu:/home/group/frensie/software/.`
   where UW_HPC_USER_NAME is the appropriate user HPC cluster user name
6. run `exit`
7. run `ssh -X UW_HPC_USER_NAME@ace-service-1.chtc.wisc.edu` where
   UW_HPC_USER_NAME is the appropriate user HPC cluster user name
8. run `cd /home/group/frensie/software/`
9. run `tar -xvf frensie-deps.tar.gz`

## Installing FRENSIE on UW HPC
1. run `cd path-to-build/frensie-update` where path-to-build is the path to the
   FRENSIE-Docker-UW-Cluster build directory
2. run `./run-frensie-update.sh` to load the frensie-update image
3. run `cd UW_CLUSTER_USER_DIR` where UW_CLUSTER_USER_DIR is the appropriate
   UW cluster user directory
4. run `cd frensie_rel` or `cd frensie_db` depending on whether FRENSIE was
   built for release or for debugging
5. run `tar -czvf frensie.tar.gz bin lib frensie-env.sh`
6. run `scp frensie.tar.gz UW_HPC_USER_NAME@ace-service-1.chtc.wisc.edu:UW_CLUSTER_USER_DIR/.` where UW_HPC_USER_NAME is the appropriate HPC cluster user name
   and UW_CLUSTER_USER_DIR is the appropriate UW cluster user directory
7. run `exit`
8. run `ssh -X UW_HPC_USER_NAME@ace-service-1.chtc.wisc.edu` where
   UW_HPC_USER_NAME is the appropriate user HPC cluster user name
9. run `cd UW_CLUSTER_USER_DIR` where UW_CLUSTER_USER_DIR is the appropriate
   UW cluster user directory
10. run `mkdir frensie_rel` or `mkdir frensie_db` depending on whether FRENSIE
    was built for release or for debugging
11. run `mv frensie.tar.gz FRENSIE_DIR/.` where FRENSIE_DIR is either
    frensie_rel or frensie_db depnding on whether FRENSIE was built for
    release or for debugging
12. run `cd FRENSIE_DIR/.` where FRENSIE_DIR is either
    frensie_rel or frensie_db depnding on whether FRENSIE was built for
    release or for debugging
13. run `tar -xvf frensie.tar.gz`
14. add the following lines to your .bashrc file on the UW Cluster:
    `if [ -r $CRIMP_ENV ]`
    `then`
    `   source UW_CLUSTER_USER_DIR/FRENSIE_DIR/frensie-env.sh`
    `fi`
    where UW_CLUSTER_USER_DIR is the appropriate UW cluster user directory
    and FRENSIE_DIR is either frensie_rel or frensie_db depnding on whether
    FRENSIE was built for release or for debugging

## Other Useful Docker Commands

1. `docker images`: run to see which docker images that have been created
2. `docker rmi -f image-id`: run to remove a docker image with id image-id
3. `docker rmi -f repo:tag`: run to remove a docker image with alias repo:tag
