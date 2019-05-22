What is this project ?
----------------------

This project is **NOT** the official manylinux repository.

It is a fork of the official upstream repository hosted at https://github.com/pypa/manylinux.

It is used as staging area to maintain and test patches that will be contributed back to the
official repository.

Docker images associated with this repository are manually published into the
[scikitbuild](https://cloud.docker.com/u/scikitbuild/repository/list) dockerhub organization.

**Motivation**: Since integration of Python 3.8 pre-release in upstream images was not possible,
these custom images provide support for it. Read [here](https://github.com/pypa/manylinux/pull/273) for more details.

Available branches
------------------

* [scikit-build-manylinux1-2019-03-01-91cb02fb8](https://github.com/scikit-build/manylinux/tree/scikit-build-manylinux1-2019-03-01-91cb02fb8): Add Python 3.8.0a2.

* [scikit-build-manylinux2010-2019-05-06-1a8986532](https://github.com/scikit-build/manylinux/tree/scikit-build-manylinux2010-2019-05-06-1a8986532): Add Python 3.8.0a4.


What is the branch naming convention ?
--------------------------------------

Each branch is named following the pattern `scikit-build-manylinux1-YYYY-MM-DD-SHA{9}`

where:

* `manylinux1` is the version of the forked project
* `YYYY-MM-DD` is the date of the last official commit associated with the branch.
* `SHA{9}` are the first seven characters of the last official commit associated with the branch.


How to update the version of manylinux ?
----------------------------------------

1. Clone this repository and add a remote to the official project

```
git clone git://github.com/scikit-build/manylinux
cd manylinux
git remote add upstream git://github.com/pypa/manylinux
git fetch upstream
```

2. Create a new branch following the convention

```
DATE=$(git show -s --format=%ci upstream/master | cut -d" " -f1)
echo "DATE [${DATE}]"

manylinuxSHA=$(git show -s --format=%h upstream/master)
echo "manylinuxSHA: [${manylinuxSHA}]"

BRANCH_NAME=scikit-build-manylinux1-${DATE}-${manylinuxSHA}
echo "BRANCH_NAME [${BRANCH_NAME}]"

git checkout -b ${BRANCH_NAME} ${manylinuxSHA}
```

3. Cherry-pick the scikit-build specific commits from last branch. Resolve conflict as needed.

   - the commit hash included in each branch name is the manylinux *upstream* hash, so
     scikit-build specific commits may be viewed with the git range (`..`) operation:
```
          git log --pretty=format:"%h" ${manylinuxSHA}..scikit-build-manylinux1-YYYY-MM-DD-${manylinuxSHA}
```

4. Test the changes

5. Publish the branch. (directly in this repo if you have push rights, or on a fork)


How to rebuild the docker images ?
----------------------------------

* for `manylinux2010`, locally build `manylinux2010_centos-6-no-vsyscall` image

```
cd docker/glibc
docker build --rm -t quay.io/pypa/manylinux2010_centos-6-no-vsyscall .
```

* for both `manylinux1` and `manylinux2010`

```
docker/build_scripts/prefetch.sh openssl curl

COMMIT=$(git rev-parse HEAD | head -c 9)
echo "COMMIT [${COMMIT}]"

PLATFORM=x86_64
docker build --rm -t scikitbuild/manylinux2010_${PLATFORM}:${COMMIT} -f docker/Dockerfile-${PLATFORM} docker/

docker push scikitbuild/manylinux2010_${PLATFORM}:${COMMIT}
```

* for `manylinux1`

```
docker/build_scripts/prefetch.sh openssl curl

COMMIT=$(git rev-parse HEAD | head -c 9)
echo "COMMIT [${COMMIT}]"

PLATFORM=i686
docker build --rm -t scikitbuild/manylinux2010_${PLATFORM}:${COMMIT} -f docker/Dockerfile-${PLATFORM} docker/

docker push scikitbuild/manylinux2010_${PLATFORM}:${COMMIT}
```


How to be granted push rights ?
-------------------------------

Create an [issue](https://github.com/scikit-build/manylinux/issues)


Questions
---------

If you have questions, create an [issue](https://github.com/scikit-build/manylinux/issues)
