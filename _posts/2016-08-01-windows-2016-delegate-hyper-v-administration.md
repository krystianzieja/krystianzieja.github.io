Adding users to Hyper-V Administrators local group is not the most secure way to delegate Hyper-V administration to others. That would allow members of this group to modify virtual switches and host settings. By using Authorization Manager we can create much more detailed delegation.

We start from launching Microsoft Management Console and add Authorization Manager snap-in.

![MMC]({{ site.url }}/images/w2016-authorization-manager-00.png)

![Add Snap-In]({{ site.url }}/images/w2016-authorization-manager-01.png)

Then we open InitialStore.xml for Hyper-V.

![Open store]({{ site.url }}/images/w2016-authorization-manager-02.png)

Next under Definitions we define new task definition. We provide a name and click Add to specify allowed operations.

![New Task Definition]({{ site.url }}/images/w2016-authorization-manager-03.png)

We select "Create Virtual Machine" from Operations tab. This would allow selected users to create virtual machines.

![Add Operations]({{ site.url }}/images/w2016-authorization-manager-04.png)

We confirm our choices.

![Confirm]({{ site.url }}/images/w2016-authorization-manager-05.png)

After we defined our Task Definition, we need to create a Role Definition that would use it.

![Role Definition]({{ site.url }}/images/w2016-authorization-manager-06.png)

The last step is to assign users to our new role, we do it under Role Assignment.

First we add our new role.

![Role Definition]({{ site.url }}/images/w2016-authorization-manager-07.png)

Then we select "Assign Users and Roles"

![Assign Users and Groups]({{ site.url }}/images/w2016-authorization-manager-08.png)
