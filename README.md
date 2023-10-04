01) Create a field on Account named "Number of Contacts". Populate this field with the number of contacts related to an account.

 public class CountcontactTriggerHandler {
     public static void increment(list<contact>con) {
               set<id>ids=new set<id>();
                   for(contact c:con) {
                      ids.add(c.accountid);
                      }
                    List<account>a=[select id,name,Number_of_Contacts__c,(select id,lastname from contacts) from account where id=:ids];
                for(account ac:a) {
        ac.Number_of_Contacts__c=ac.contacts.size();
        }
     update a;
    
   }
}

trigger CountcontactTrigger on Contact (after insert, after update , after delete) {
    CountcontactTriggerHandler.increment(Trigger.new);

}
