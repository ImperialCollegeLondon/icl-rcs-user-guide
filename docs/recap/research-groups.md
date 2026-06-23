# Managing Research Groups

## Viewing your Research Groups

When you first login to [ReCAP](./access.md) you should see the home page with a *"Welcome to the Imperial Research Computing Access Portal"* message.

## Viewing details of a Research Group

To see the research groups you are a member of:

1. Click on **Groups** on the menu bar
2. Next click on **Groups** from the drop-down list that appears

You should then be presented with a tabular list of the research groups of which you are a member. Each row is a group you are part of, and contains details on:

* **ID**: the backend numerical ID of the research group.
* **PI**: the research group owner and manager.
* **Title**: the name of the research group.
* **Status**: text note on whether the allocation is in active use or not.

To see the more details on a specific research group, click on its corresponding ID number. This will take you to a page where you should see:

* A text summary describing the research group.
* A list of users within the research group.
* Any allocations associated with the research group.
* Additional information such reference tickets.

## User Management

### Roles

Users in a research group can be assigned one of the following role types:

* **User** - the standard role which grants a user access to the resources and allocations of the research group. The do not have the permissions to add or remove other users.
* **Manager** - a manager has the access of a standard user while also having the ability to add and remove other users from a research group.

Due to the elevated privileges of the Manager role, only grant it to trusted individuals.

### Adding users to your Research Group

!!! warning

    Users MUST be added to your research group before they can be added to any of its allocation.

To add users to your research group:

1. Navigate to the details page of the research group in question.
2. Click the **Add Users** button on the top of the **Users** section.
3. Either:
    * With **Exact Username only** selected, enter the usernames of each person you want to add into the search box, separating usernames by a space character or using a new line.
    * With **All Fields** selected, enter a search criteria for an individual such as their username. Note that this option can only search for single users.
4. Click the **Search** button.

After clicking search, you will be presented with two lists. One list is all the allocations associated with your research group, while the other list shows the users matching your search criteria and that can be added to the various allocations. In the list of users shown, decide which of them you wish to add to your research group by selecting the checkbox next to the corresponding user, and decide what role they should have for the research group (see above).

By default, ReCAP automatically selects all allocations, which means users added to a research group will be given access to all available allocations. If these users should not have access to certain allocations, you can remove their access by unticking the box next to the relevant allocation.

!!! note
    There is no restriction to the number of research groups an individual can be a part of, nor is there a restriction on the number of individuals that can be in a research group.

### Removing users from your Research Group

Navigate to the details of a research group and click the **Remove Users** button at the top of the Users section. On the page that appears, select the tick box next to each user you would like to remove from your research group and then click the **Remove selected users from Project** button.

Note that when you remove a user from your research group, they will also be removed from any allocation within that research group.

### Change roles of existing users

You can change the role of an existing user by clicking the action icon in the row corresponding to their name. This will take you to a new form where you can change the role and apply the changes by clicking the **Update** button.

## Managing the Allocations of a Research Group

All allocations exist under a research group, thus the management of allocations is an extension of managing a research group. Since different types of allocations, please view the [dedicated documentation](allocations.md) for further details on managing them.
