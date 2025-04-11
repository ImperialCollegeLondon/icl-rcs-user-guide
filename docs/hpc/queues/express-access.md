# Express Access

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

Standard access to our systems is unmetered and free-at-the-point-of-use. If you find yourself requiring more than the standard service provides, for example to meet a specific deadline, you may pay for express access.

The [Express Access self-service page](https://selfservice.rcs.imperial.ac.uk/express) page is only accessible if you are on the campus network or following the recommended Remote Access guidance.

## Express Access Charges

The current charges for express access may be found on the [Express Access self-service page](https://selfservice.rcs.imperial.ac.uk/express).

## What to know before creating an Express Account

* The "billing run" for express accounts is carried out on the morning of the 10th of each month for the previous month. So the billing run on the 10th September 2022 would be for express usage during August 2022.
* An express account can only be associated with a single funding source. Once the funding source has run out or expired you will no longer be able to use the express account.
* If you make an advanced purchase of credit for an express account and the funding source expires, then you will no longer have access to that advance purchased credit. When this occurs, please contact us so we can transfer any remaining credit to a new express account.

## Creating an Express Account
You can create an express account account on the [Create a new Express Account page](https://selfservice.rcs.imperial.ac.uk/express/new); you will need to select a funding source of which you have access to, and confirm that you authorise payments from that funding source.

Every express account is assigned a corresponding express group name of the form **exp-XXXXX** where **XXXXX** is replaced by a number. This express group name will have a corresponding Linux group created which is used to control access to the express account. Please read through the following sections on how to manage and submit jobs to the express account.

## Submitting jobs using the express account

To submit job to the express queue you must use:

```console
qsub -q express -P exp-XXXXX
```

where **exp-XXXXX** is the name associated with your express account. Note that when you submit a job to the express queue, the job must confirm to one of the job classes as listed on the express self-service page.

## Managing your Express Account

### Listing your Express Accounts

You can see a list of all the express accounts you have access to by going to the [Your Express Accounts(https://selfservice.rcs.imperial.ac.uk/express/manage/accounts)] page on the self service website.

### Managing user access to your express account

The following steps describe how to control who can submit jobs against your express account (please note that any changes may take a few hours to reflect on the HPC systems):

* Go to the [Your Express Accounts](https://selfservice.rcs.imperial.ac.uk/express/manage/accounts) page on the self service website so view a list of your express accounts.
* Expand the section belonging to the express account that you wish to update.
* Click on the **Manage user access control group** link for the express account you wish to edit.

You should now be presented with a list of users for the express account.

#### To add a user

* Enter the username of the user in question into the Add user (enter username) text box.
* Click the Add button.

If the username is valid, the page should reload showing a list of users including the newly added on.

#### To remove a user

* Enable the Remove tick box for the user you wish to remove from your express account.
* Click the Update button.

The page should reload showing the list of users minus the ones you've requested are removed.

### Making an advanced purchase of credit

You can make an advanced purchase of credit for an express access account:

* Go to the [Your Express Accounts](https://selfservice.rcs.imperial.ac.uk/express/manage/accounts) page on the self service website so view a list of your express accounts.
* Expand the section belonging to the express account that you wish to make an advanced purchase for.
* Click the **Make Advance Purchase** link against the corresponding express account.

### Viewing the usage and billing history of your Express Account

* Go to the [Your Express Accounts](https://selfservice.rcs.imperial.ac.uk/express/manage/accounts) page on the self service website so view a list of your express accounts.
* Expand the section belonging to the express account that you wish to view.
* Click the Usage link to view the usage against the express account.
* Click the Billing History link for a list of charges made against the express account.