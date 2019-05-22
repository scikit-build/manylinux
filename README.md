What is this project ?
----------------------

This project is **NOT** the official manylinux repository.

It is a fork of the official upstream repository hosted at https://github.com/pypa/manylinux.

It is used as staging area to maintain and test patches that will be contributed back to the
official repository.

Docker images associated with this repository are manually published into the
[scikitbuild](https://cloud.docker.com/u/scikitbuild/repository/list) dockerhub organization.


Available branches
------------------

* [scikit-build-manylinux1-2019-03-01-91cb02fb8](https://github.com/scikit-build/manylinux/tree/scikit-build-manylinux1-2019-03-01-91cb02fb8): Add Python 3.8.0a2. Integration in upstream was not possible. Read [here](https://github.com/pypa/manylinux/pull/273) for more details.


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


How to be granted push rights ?
-------------------------------

Create an [issue](https://github.com/scikit-build/manylinux/issues)


Questions
---------

If you have questions, create an [issue](https://github.com/scikit-build/manylinux/issues)
