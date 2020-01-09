# contactlist-class

import java.util.*;

/**
 * The ContactList class implements a Contact List in that it serves as a list of Contacts
 * optimized for searching for Contacts by various aspects of personal information. It is
 * possible to add/remove Contacts from a ContactList as well.
 *
 * @author Asma Awad
 * @version 1.0
 */
public class ContactList implements Iterable<Contact> {

    private ArrayList<Contact> contacts;

    public ContactList() {
        contacts = new ArrayList<>();
    }
    public ContactList(Contact[] c) {
        int n = c.length;
        Contact temp;

        //sorts the array of Contacts provided before converting it into an ArrayList
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (c[j].compareTo(c[j + 1]) > 0) {
                    temp = c[j];
                    c[j] = c[j + 1];
                    c[j + 1] = temp;
                }
            }
        }
        contacts = new ArrayList<>(Arrays.asList(c));
    }

    /**
     * findByLastName searches for a Contact in the ContactList by last name
     * @param last the last name being used to search through the ContactList
     * @return the Contact found that matches the last name, and null otherwise
     */
    public Contact findByLastName(String last) {
        int low = 0, high = size() - 1, mid;

        while (low <= high) {
            mid = (low + high) / 2;

            if (last.equals(contacts.get(mid).getLast()))
                return contacts.get(mid);

            else if (last.compareTo(contacts.get(mid).getLast()) < 0)
                high = mid - 1;

            else
                low = mid + 1;
        }
        return null;
    }

    /**
     * findByPhoneNumber searches for a Contact in the ContactList by phone number
     * @param phone the phone number being used to search through the ContactList
     * @return the Contact found that matches the phone number, and null otherwise
     */
    public Contact findByPhoneNumber(String phone) {
        for (Contact c : contacts) {
            if (c.getPhone().equals(phone))
                return c;
        }
        return null;
    }

    /**
     * findAllByLastInitial searches for Contacts in the ContactList by last initial
     * @param ch the last initial being used to search through the ContactList
     * @return a ContactList of Contacts with the last initial provided
     */
    public ContactList findAllByLastInitial(char ch) {
        ContactList cl = new ContactList();
        for (Contact c : contacts) {
            if (c.getLast().charAt(0) == ch)
                cl.add(c);
        }
        return cl;
    }

    /**
     * findAllByCity searches for Contacts in the ContactList by city
     * @param city the city being used to search through the ContactList
     * @return a ContactList of Contacts with the city provided
     */
    public ContactList findAllByCity(String city) {
        ContactList cl = new ContactList();
        for (Contact c : contacts) {
            if (c.getCity().equals(city))
                cl.add(c);
        }
        return cl;
    }

    /**
     * add adds a Contact into the ContactList in sorted order if it isn't already there
     * @param c the Contact to be added into the ContactList
     * @return whether or not the Contact was successfully added into the ContactList
     */
    public boolean add(Contact c) {
        //determines whether the ContactList already contains the specified contact
        int low = 0, high = size() - 1, mid;

        while (low <= high) {
            mid = (low + high) / 2;

            if (c.equals(contacts.get(mid)))
                return false;

            else if (c.compareTo(contacts.get(mid)) < 0)
                high = mid - 1;

            else
                low = mid + 1;
        }

        //adds in the Contact at the first point where it hits a lexicographically greater Contact
        int count = 0;
        for (Contact con : contacts) {
            if (con.compareTo(c) > 0) {
                contacts.add(count, c);
                return true;
            }
            count++;
        }

        //adds in the Contact in the case that it would be the last element
        contacts.add(c);
        return true;
    }

    /**
     * size provides the size of the ContactList
     * @return the number of Contacts in the ContactList
     */
    public int size() {
        return contacts.size();
    }

    /**
     * remove removes a Contact from the ContactList
     * @param obj the object to be removed from the ContactList
     * @return the Contact that was removed
     */
    public Contact remove(Object obj) {
        //ensure that the Object is a Contact before removing it from the ContactList
        if (!(obj instanceof Contact))
            return null;
        //cast the Object to a Contact once it passes the instanceof check
        Contact c = (Contact) obj;

        int index = contacts.indexOf(c);
        //call the ArrayList's builtin remove method
        return contacts.remove(index);
    }

    /**
     * get provides the Contact at the given index in the ContactList, provided the index is within bounds
     * @param index the index of the ContactList to be accessed
     * @return the Contact at the given index in the ContactList
     * @throws IndexOutOfBoundsException when the provided index is out of the bounds of the ContactList
     */
    public Contact get(int index) {
        if (index < 0 || index >= size())
            throw new IndexOutOfBoundsException(index + " doesn't exist or isn't a valid index");

        return contacts.get(index);
    }

    /**
     * equals determines whether or not two ContactLists are equal
     * @param obj the object being compared to the ContactList the method is called on
     * @return whether or not the ContactLists are equal
     */
    public boolean equals(Object obj) {
        //ensure that the Object is a ContactList before comparing it to another ContactList
        if (!(obj instanceof ContactList))
            return false;
        //cast the Object to a ContactList once it passes the instanceof check
        ContactList other = (ContactList) obj;

        if (contacts.size() != other.size())
            return false;

        //use iterators to iterate through each ContactList
        Iterator<Contact> iter = contacts.iterator();
        Iterator<Contact> iter2 = other.iterator();
        Contact c1, c2;

        while (iter.hasNext()) {
            c1 = iter.next();
            c2 = iter2.next();

            //if a difference in the ContactLists is found, return false
            if (!(c1.equals(c2)))
                return false;
        }
        return true;
    }

    /**
     * toString provides a formatted representation of the ContactList
     * @return a String representation of the ContactList
     */
    public String toString() {
        StringBuilder s = new StringBuilder();
        for (Contact contact : contacts) {
            s.append(contact.toString()).append("\n");
        }
        return s.toString();
    }

    /**
     * iterator provides an iterator to iterate through a ContactList
     * @return an iterator over Contacts
     */
    public Iterator<Contact> iterator() {
        return contacts.iterator();
    }
}
