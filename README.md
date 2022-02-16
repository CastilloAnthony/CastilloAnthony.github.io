# Anthony Castillo's Portfolio

Hello! I am a computer science student who is looking for a position that will ensure healthy growth and refinement of my current skills while also expanding my knowledge and abilities in a challenging an friendly work environment.

## Degrees & Certificates
- Associate's Degree in Computer Science (Pending Completion)
- AA GSE: Language and Rationality (Pending Completion)
- Certificate of Achievement Computer Programming Specialist (Pending Completion)
- Certificate of Achievement CSU GE Pattern (Pending Completion)

## Contact Me Directly At
- Phone: (209) 602-1093
- Email: [anthony1093ca@mail.com](anthony1093ca@mail.com)
- GitHub: [https://github.com/IceWolf777](https://github.com/IceWolf777)
- LinkedIn: [https://www.linkedin.com/in/anthony-castillo-3b7829149/](https://www.linkedin.com/in/anthony-castillo-3b7829149/)

### Programming Langauges that I'm Comfortable with:
- C++
- Lua
- Python

### Operating Systems and Software that I'm Comfortable with:
- Windows 10
- Linux
    - Bash Shell programming
- Raspberry Pi
- Microsoft Office Suite of Applications
    - i.e. Word, Excel, Powerpoint
- VMWare

### A Quicklook

Below is a sample of some code that I've written and worked with. For this example, I created a header file for a class object that was capable of reading a specific file (contigencyLibrary.txt) line by line and importing those lines as urls, file locations, or folder locations. Once the program has pulled the data from the text file, it  then attemptted to access each url individually. And then wait for user input before closing.

```c++
#ifndef H_dictionaryList
#define H_dictionaryList

/*
	I need to create a class which can contain a theoretically infinite number of urls and output that url to what ever function is calling them.
	Anthony Castillo 7/20/2019
*/

#include <iostream>
#include <cassert>
#include <cstdlib>
#include <string>
#include "linkedList.h"
#include "unorderedLinkedList.h"
#include <fstream>

class library
{
public:
	// Accessors
	std::string printList(int); // Will return the info stored in the unorderedLinkedList node #int.
	int length(); // Will return the length of the unorderedLinkedList.

	// Mutators
	void addSitesToListFromFile(); // Pulls strings from a file and inputs it into our lists for easy add-ability

	// Constructors
	library(); // Default constructor
	
	// Deconstructors
	void selfDestruct(); // Default deconstructor

private:
	unorderedLinkedList<string> list1;
};

/*		FUNCTION DEFINITIONS		*/
// Accessors
std::string library::printList(int n) // Will return the info stored in the unorderedLinkedList node numbered n.
{
	return list1.printItem(n);
}; // end printList

int library::length() { return list1.length(); }; // Will return the length of the unorderedLinkedList.

// Mutators

void library::addSitesToListFromFile() // This function is used to pull urls from a file and input them into an unorderedLinkedList
{
	ifstream inData;
	std::string temp; // A temporary storage for the urls we pull from the file

	if (inData.is_open()) // Making sure there is no stream open
		inData.close();

	inData.open("contigencyLibrary.txt"); // Opening the data stream
	if (inData.is_open())
	{
		while (!inData.eof())
		{
			getline(inData, temp); // Grabbing a line and storing it in temp
			list1.insertLast(temp); // Inserting temp into the end of list1
		};
	}
	else
		cout << "The file contigencyLibrary.txt could not be found!" << endl << "No sites could be imported." << endl;
	inData.close(); // Close the opened stream

	return;
}; // end addSitesToListFromFile

// Constructors
library::library()
{
	library::list1.initializeList();
	library::addSitesToListFromFile();
};

// Deconstructors
void library::selfDestruct()
{
	list1.destroyList();
};
#endif
```

For more information on this project, the github repository can be located [here](https://github.com/IceWolf777/Contingency).

