# Advanced Linux Commands
## File Permissions and Access Rights
### chmod command
    The chmod command allows you to modify file permissions. You can use both symbolic and numeric representations to assign permissions to the user, group, and others.

### How it works
1. Create an empty file:
![Empty_file](./1.Empty%20file.png)

2. Check the file permissions:
![file_permission](./2.File%20Permission.png)

3. Update the file permissions using chmod:
![X](./3.Permission%20X.png)

4. Using the numeric representation:
![N](./4.Numeric%20Permission.png)

![](./5.%20Note%20Permission.png)

### chown command
    The chown command allows you to change the ownership of files, directories, or symbolic links to a specified username or group.

1. Create a User
![](./6.%20john%20user@Yinusa01.png)

2. Grant the user sudo privileges:
![](./7.sudo%20permission.png)

3. Switch to the new user:
![](./8.switch%20user.png)

4. Modify user account, by changing user's password:
![](./9.John%20Pwdch@Yinusa02.png)

5. Switch between users:
![](./10.ubuntu.png)

6. Create a group:
![](./11.Create%20devloper.png)

7. Verify the group:
![](./12.verify%20group.png)

8. Add the user to the group and verify:
![](./13.%20Add%20and%20verify%20user.png)

9. Change group ownership of a directory:
![](./14.%20Group%20to%20Directory.png)

![](./15.%20Permission%20change.png)

## Side Task
    Create a group on the server and name it devops
![](./16.%20Group%20create.png)
**Create 5 users ["mary", "mohammed", "ravi","tunji", "sofia" ], and ensure each user belong to the devops group**

![](./17.%20new%20users.png)

**Create a folder for each user in the /home directory. For example /home/mary**
![](18.%20Folder.png)

**Ensure that the group ownership of each created folder belongs to "devops"**
![](19.Group%20Ownwership.png)

**Verify the group ownership of each created folder belongs to "devops"**
![](20.%20Verify%20Group.png)


