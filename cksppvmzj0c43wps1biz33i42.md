## Sync Code to multiple Git repositories


Have you ever been into a situation where you have multiple Git repositories and you have to manually sync code to each of these repositories?

I have been a big fan of [BitBucket](https://bitbucket.org/factorsense/programming/src/master/) for its simple and amazingly intuitive UI. However, being an old school guy, I have not yet been able to take a break from [Github](https://github.com/mayanksrivastav84/Programming) as well. And I have a thing for being organized, so I always love to keep all of my repositories sync-ed up.

As a programmer, most of my code which I write is in Python and hence it becomes very important to keep all my code in a repository so that I can use it in future when needed and also share it with others.

I maintain my code repositories in three locations namely github, bitbucket and Gitlab. The only reason I made three repositories is that I wanted to explore all three platforms. There is no difference of content between these repositories.

I normally write my Python program in Visual Studio Code and commit it to my code repositories. Earlier, I had to manually commit my code to all the repositories individually. But with the process below, I can do it in one go.

Let’s assume that we don’t have an existing version of code cloned into our local machines. As a first step, I will launch Visual Studio Code and clone my BitBucket repository.

![Clone the BitBucket repository](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788296193/MTBz-Ts1V.png)*Clone the BitBucket repository*

Next I would open my cloned folder on my local machine. Visual Studio code comes bundled with a source control provider and would automatically detect the git repository.

Let’s first try to get a list of remote repositories which are being tracked. Please type the below command to get the list. You will see only the bitbucket repository in this case:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788297838/ocqaB9lzI.png)

```
git remote -v
origin  [https://factorsense@bitbucket.org/factorsense/programming.git](https://factorsense@bitbucket.org/factorsense/programming.git) (fetch)
origin  [https://factorsense@bitbucket.org/factorsense/programming.git](https://factorsense@bitbucket.org/factorsense/programming.git) (push)
```


As the last step let’s configure visual studio code to push the changes to both the repositories using the below commands

```
git remote set-url --add --push origin [https://github.com/mayanksrivastav84/Programming.git](https://github.com/mayanksrivastav84/Programming.git)

git remote set-url --add --push origin https://factorsense@bitbucket.org/factorsense/programming.git
```


This will add entries of the repositories to remote.origin.pushurl and now the code will be pushed to both the destinations. We can check it by running the following command

```
git remote show origin
```


You will get a similar output with two push URL’s

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788299509/aJ_Igo0Ow.png)

To make sure this works, I would make a dummy change and try to push to changes and see if it gets committed to both my github and bitbucket repositories

In my case, I committed a small dummy change as below and I can see the same committed to both bitbucket and github repositories.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788301071/Dz-mGPSXK.png)

Let’ see the change in bitbucket and voila, I can see the change committed.

![BitBucket repository](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788302648/5puJywC3g.png)*BitBucket repository*

Let’s see if I can see the change in my github repository and voila, I can see the change there too.

![GitHub Repository](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788304693/NwPvs09Kv.png)*GitHub Repository*

So now, I don’t have to worry about keeping all my repositories in sync. Once I save my code and commit the changes, it will reflect in all the repositories. Before closing this article, let’s add a third repository to the push URL, in my case its the GitLab repository.

```
git remote set-url --add --push origin [https://gitlab.com/mayanksrivastav84/programming.git](https://gitlab.com/mayanksrivastav84/programming.git)
```
