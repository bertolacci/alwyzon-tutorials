Authors: Giacinto Bertolacci <https://github.com/bertolacci>
Published at: 2021-04-28T20:50Z
Tags: Debian, Ubuntu, Linux, Beginner's Guides


# How to Create Superusers on Debian and Ubuntu

UNIX derivatives, including Linux, are designed as multi-user systems. That is, one computer can be used by multiple different users at the same time. Each user is identified by its username, such as `root`, `nginx` or `johndoe`. The UNIX way to do things actually involves user accounts not only for "real" human users, but also uses users to segreate resources for different services. For example, one user for `nginx` and a separate user for `postgres`. This guide will show you, how you can create additional superuser for your administrators, or, how to replace the pre-created superuser with your own one.


## Step 1 – Create a new User

Creating new users on Debian and Ubuntu is really easy, thanks to the `adduser` utility. This utility launches an interactive dialog to configure all properties of the new users. To create a new user named `johndoe`, just call:

```
sudo adduser johndoe
```

After running this command, you should see an output such as the following:

```
% sudo adduser johndoe 
Adding user `johndoe' ...
Adding new group `johndoe' (1001) ...
Adding new user `johndoe' (1001) with group `johndoe' ...
Creating home directory `/home/johndoe' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for johndoe
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] 
```

... answer those questions and once done, the new user will be created for you.

## Step 2 – Assign it to the `sudo` Group

By default, the `sudo` utility is configured to grant all users in the `sudo` group superuser permissions. So, to grant the newly created user superuser permissions, you just need to add it to the `sudo` group:

```
sudo usermod -aG sudo johndoe
```

That's it. The new user should now have been granted superuser permission and can run commands with `root`-privileges by prefixing commands with `sudo <cmd>`. For example, `sudo apt update && sudo apt upgrade` to load and install all available software updates.

## (Optional) Step 3 – Delete an Old User

If you would like to get rid of any user on Debian or Ubuntu, such as the precreated `alwyzon` user on any [virtual server from Alwyzon](https://www.alwyzon.com/), you just need to run:

```
sudo deluser alwyzon
```

But, before doing so, make sure to check if you can actually access and use the previously created `johndoe` user to not get locked out of your server. 


## Conclusion

As shown above, user management is really easy on Linux. All you need are those three commands:

```
sudo adduser newuser
sudo usermod -aG sudo newuser
sudo deluser olduser
```
