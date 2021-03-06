# Official card.io Android release process 

This is probably only useful for maintainers.

Public github address (https://github.com/card-io/card.io-Android-SDK)

1. Make sure you're using NDK r10d!  Ndk 8 is broken for some devices.  

1. Sanity check the develop branch.  

1. Run `ant dist doc`, and review the obfuscated jar and javadocs for any anomalies.  
 
1. Deploy to a device and run a few sanity checks  (don't forget to do a release build!)

1. Run `git checkout -b release/1.2.3 develop` with a release version

1. Update `release_notes.md` and commit.

1. Run `fab sdk_reset`, it will reset the branches

1. Run `fab sdk_release` it will build the sdk and prepare `public` folder, it will also tag the release with a version you have created in step 0. 
```
1. Switch to master and merge release branch
```bash
    git checkout master
    git merge --no-ff release/1.2.3
```
1. Now push the changes to origin repo
```bash
    git push origin master --tags
```
1. Merge master to develop and push
```bash
    git checkout develop
    git merge master
    git push origin develop --tags
```
1. Wait for tests to complete and let's release to public, check the diffs to verify all is good
	1. Check the changes
	```bash
	    cd public/card.io-Android-SDK
	    git diff
	```
	2. Commit the changes
	```bash
	    git commit -am "Update library to 1.2.3"
	```
	3. Tag commit and push
	```bash
	    git tag 1.2.3
	    git push public master --tags
	```

1. Update javadocs
	1. On public repo, checkout special gh branch called gh-pages:
		2. `git checkout gh-pages`
		2. Remove all files except README.md, so they can be replaced with new files.
	1. On private sdk rep, build javadoc and copy to public repo:
		2. `ant doc`
		2. `cp -a javadoc/* <public repo dir>`
	1. On public repo, push the changes:
		2. `git push` 

1. Check github that all is good.

1. Post that a new release is available using the same format previously used to:
	1. Twitter (username=cardio/password=_ask for password_)
	2. Google Groups (send email to card-io-sdk-announce@googlegroups.com) 
