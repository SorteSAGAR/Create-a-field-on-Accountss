01) Create a field on Account named "Number of Contacts". Populate this field with the number of contacts related to an account.

trigger UpdateNumberOfContacts on Contact (after insert, after update, after delete) {
    // Create a set to store Account IDs that need to be updated
    Set<Contact> accupdate = new Set<Contact>();
     if (Trigger.isInsert || Trigger.isUpdate) {
        for (Contact contact : Trigger.new) {
            accupdate.add(contact.AccountId);
        }
    }
    if (Trigger.isDelete) {
        for (Contact contact : Trigger.old) {
            accupdate.add(contact.AccountId);
        }
    }
    // Update the Number of Contacts field on related Accounts
    List<Account> accupdate = new List<Account>();
    for (Id accountId : accupdate) {
        Account acc = new Account(Id = accountId);
        acc.Number_of_Contacts__c = [SELECT COUNT() FROM Contact WHERE AccountId = :accountId];
        accupdate.add(acc);
    }
    // Update the Accounts
    update accupdate;
}
