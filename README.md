01) Create a field on Account named "Number of Contacts". Populate this field with the number of contacts related to an account.

 trigger UpdateContactss on Contact (before insert,after update, after delete) {
    // Create a set to store Account IDs that need to be updated 
     Set<Id> accountIdsToUpdate = new Set<Id>();

if (Trigger.isInsert || Trigger.isUpdate) {
        for (Contact contact : Trigger.new) {
            accountIdsToUpdate.add(contact.AccountId);
        }
    }
    if (Trigger.isDelete) {
        for (Contact contact : Trigger.old) {
            accountIdsToUpdate.add(contact.AccountId);
        }
    }

// Update the Number of Contacts field on related Accounts
    List<Account> accountsToUpdate = new List<Account>();
    for (Id accountId : accountIdsToUpdate) {
        Account acc = new Account(Id = accountId);
        acc.Number_of_Contact__c = [SELECT COUNT() FROM Contact WHERE AccountId = :accountId];
        accountsToUpdate.add(acc);
    }

// Update the Accounts
    update accountsToUpdate;
}
