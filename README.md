Problem-Solution-for-Software-Development-Job-Application
=========================================================

This repository is made for job application.

import java.io.*;
import java.util.*;
class Link
{
public String linkvalue;
public String level1; 
public String level2;
public String level3;  
public Link next; // next link in list
// -------------------------------------------------------------
public Link(String level1, String level2, String level3, String linkvalue) // constructor
{
this.level1 = level1;
this.level2 = level2;
this.level3 = level3;
this.linkvalue = linkvalue;
} // set to null)
// -------------------------------------------------------------
public void displayLink() // display ourself
{
String linkdisplayer;
linkdisplayer = level1; 
if (!(level2.equals(""))) linkdisplayer = linkdisplayer + "  >>  " + level2;
if (!(level3.equals(""))) linkdisplayer = linkdisplayer + "  >>  " + level3;
System.out.println("" + linkdisplayer );
}
} // end class Link
////////////////////////////////////////////////////////////////
class LinkList
{
private Link first; // ref to first link on list
/*public static String linkvalue;
public static String level1; 
public static String level2;
public static String level3;  
*/
public LinkList() // constructor
{
first = null; // no items on list yet
}
// -------------------------------------------------------------
public boolean isEmpty() // true if list is empty
{
return (first==null);
}
// -------------------------------------------------------------
// insert at start of list
public void insertFirst(String level1, String level2, String level3, String linkvalue)
{ // make new link
Link newLink = new Link( level1,  level2,  level3, linkvalue);
newLink.next = first; // newLink --> old first
first = newLink; // first --> newLink
}
public void displayList()
{
System.out.print("List (first-->last): ");
Link current = first; // start at beginning of list
while(current != null) // until end of list,
{
current.displayLink(); // print data
current = current.next; // move to next link
}
System.out.println("");
}

public void findCategory(String category)
{
Link current = first; // start at beginning of list
while(!(current.linkvalue.equals(category))) 
{
//System.out.println(""+current.linkvalue);
current = current.next; // move to next link
}
if (current == null) System.out.println("Sorry, category not found ");
else{
System.out.println("The breadcrumb of the requested category is : ");
current.displayLink();
}
}


// -------------------------------------------------------------
} // end class LinkList
////////////////////////////////////////////////////////////////
class BreadCrumbTrailApp
{
public static void main(String[] args) throws IOException
{
 String linkvalue="";
 String level1=""; 
 String level2="";
 String level3="";
 boolean record = false;
 boolean level1identifier = false;
 boolean level2identifier = false;
 boolean level3identifier = false; 
LinkList theList = new LinkList(); // make new list
File file = new File("category.txt");
    if (!file.exists()) {
      System.out.println("File does not exist.");
      return;
    }
    if (!(file.isFile() && file.canRead())) {
      System.out.println(file.getName() + " cannot be read from.");
      return;
    }
    try {
      FileInputStream fis = new FileInputStream(file);
      char current = (char)96;
      while (fis.available() > 0) {
        current = (char) fis.read();
  	if (current=='|' || current=='*' || Character.isUpperCase(current)) record = true;
		if (current == '*') {
		level3identifier=true;
		level2identifier=false;
		level1identifier=false;
		}
		if (current == '|') {
		level2identifier=true;
		level1identifier=false;
		level3identifier=false;
		}
		if (!level2identifier && !level3identifier) level1identifier=true;
		if (current=='\\'){
			//System.out.println(""+linkvalue);
			if (level1identifier){
				level1=linkvalue;
				level2="";
				level3="";
			}
			if (level2identifier){
				level2=linkvalue;
				level3="";
			}
			if (level3identifier){
				level3=linkvalue;				
			}
			theList.insertFirst( level1,  level2,  level3,  linkvalue);
			linkvalue="";
			record=false;
			level1identifier = false;
			level2identifier = false;
			level3identifier = false; 
		}
		if (record  && current!='\\' && current!='*' && current!='|') linkvalue = linkvalue + current;
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
 theList.displayList(); // display list
//theList.findCategory("SAP"); // finding specific category
	
        //System.out.print(current);
		char carryon = (char)89;
		String category="";
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		while(carryon=='Y' || carryon=='y'){
		System.out.print("\n Please enter the category you want to search : ");
		category=br.readLine();
		theList.findCategory(""+category);
		System.out.print("\n Do you want to carry on (y/n) : ");
		category=br.readLine();
		carryon = category.charAt(0);
		}
} // end main()
} // end class BreadCrumbTrailApp
