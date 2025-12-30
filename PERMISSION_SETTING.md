# Running Local Code on Ubuntu via Jenkins

> **Disclaimer:** This setup is intended **only for testing purposes**.  
> The group `rnd` has full read, write, and execute permissions.  
> After testing, **delete the group** to avoid permission conflicts in future work with Jenkins and GitHub.

**Note:** Directory paths may differ based on your system setup. Adjust them accordingly.

This documentation provides step-by-step instructions to set up a shared folder, configure user permissions, and run local code on Ubuntu via Jenkins.

---

## 1. Create a Group for Collaboration

Create a Linux group that allows multiple users to share files with full permissions (Read, Write, Execute).

```bash
# Create a new group named 'rnd'
sudo groupadd rnd
```

## 2. Add Users to the Group

Add the users vikas and jenkins to the newly created group. This gives them shared access to folders assigned to this group.

```bash
# Add user 'vikas' to group 'rnd'
sudo usermod -aG rnd vikas

# Add user 'jenkins' to group 'rnd'
sudo usermod -aG rnd jenkins

```

## 3. Assign Folder Permissions

Set the Jenkins workspace folder to use this group and grant full access.

```bash
# Change the group ownership of the Jenkins workspace folder
sudo chown -R :rnd /home/jenkins/jenkins-practice

# Grant full read, write, and execute permissions to the group
sudo chmod -R 777 /home/jenkins/jenkins-practice

# (Optional) Ensure new files created inside inherit the group automatically
# This sets the setgid bit so all new files belong to 'rnd'
sudo chmod g+s /home/jenkins/jenkins-practice
```

## 4. Verify Group Creation and Membership

Check that the group was created and users are members:

```bash
# List all groups on the system
cat /etc/group

# Check which groups a user belongs to
groups vikas
groups jenkins

# Check details of a specific group
getent group rnd

# Expected output:
# rnd:x:1004:vikas.kumar,jenkins
```

## 5. Delete a Group (Optional / Cleanup)

After testing, you can remove the group to prevent permission conflicts in future projects.

```bash
# Check if the group exists
grep groupname /etc/group

# Delete the group
sudo groupdel groupname

# Verify deletion
getent group groupname
# Note: Deleting a group does not remove files owned by that group.
# You may need to reassign files to another group if necessary.
```

## 6. Summary

- Group rnd allows multiple users to share files and folders.

- Folder permissions ```777``` give full access to owner, group, and others (use cautiously).

- Setting the setgid bit ensures new files inherit the group automatically.

- Jenkins and local users can collaborate in the jenkins-practice folder.

- Always delete the testing group after use to avoid security or permission issues.

## 7. Notes for Future Use

- Adjust directory paths if your Jenkins workspace is in a different location.

- For production environments, avoid 777; use more restrictive permissions like 770.

- This setup is ideal for testing pipelines or scripts locally with Jenkins before deployment.
