---
layout: post
title: "github on Mac"
date: 2012-08-16 19:43
comments: true
categories: 
---
when we are coworking on github,there maybe have some conflict problems between partner occasionally.

I assume some reason previously, and find some solutions.

1.Between Master and Branches problem

Because our project was small previously, we was coding on Master.
However,If we have more developer on this Master or project become more complex (For example, there are two developer on this Master at the same time),we will face conflict status.

Now, we must use git's "branch" feature.
As the name suggests,it means coder do his project in his own branch.When his project will be done, he can push it from branch to Master.

This way has advantages as follows:

	1.We can use branch to build website's plugin.We can change plugin easily.

	2.The follow-up project tracking can use branch to distinguish all projects.


Hint! When we use it previously, we must understand it steps.

In Master, it step is :

	commit -> pull ->push

In Branch , it step will be more complex.As follows:

Part I：
	commit

Part II:（先從github的主幹上面下載別人做的東西，跟在dev做的pull即是一樣的道理）
	1) switch to Local/dev: git checkout dev
	2) pull: git pull
	3)switch to Local/[topic] branch: git checkout [topic]
	4) merge Local/dev branch into currently checked out branch (topic): git merge dev

Part III:（把自己的branch與主幹merge並丟上github跟push道理是一樣的)

	1)switch to Local/dev branch: git checkout dev
	2)pull: git pull
	3)merge topic branch into currently checked out branch (Local/dev): git merge [topic]
	4)push (if ready to share): git push
	5)switch back to Local/topic branch to continue development: git checkout [topic]

We can make a bash to combine PartII and PartIII.As follows:

PART II:

	function setWorkingBranch {
	  br=`git branch | grep "*"`
	  workingBranch=${br/* /}
	}
	setWorkingBranch
	echo 'Currently on '$workingBranch
	echo 'Checking out dev...'
	git checkout dev
	echo 'Pulling...'
	git pull
	echo 'Checking out '$workingBranch'...'
	git checkout $workingBranch
	echo 'Merging dev into '$workingBranch'...'
	git merge dev

Part III:

	function setWorkingBranch {
	  br=`git branch | grep "*"`
	  workingBranch=${br/* /}
	}
	setWorkingBranch
	echo 'Currently on '$workingBranch
	echo 'Checking out dev...'
	git checkout dev
	echo 'Pulling...'
	git pull
	echo 'Merging '$workingBranch' into dev...'
	git merge $workingBranch
	echo 'Pushing to origin...'
	git push
	echo 'Checking out '$workingBranch'...'
	git checkout $workingBranch

github on Mac
