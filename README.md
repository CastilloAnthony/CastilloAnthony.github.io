![Disclaimer:](https://networks.imdea.org/wp-content/uploads/2021/09/media-file-code-900x500.png "I do not own this picture.")
# Anthony Castillo's Portfolio

Hello, my name is Anthony Castillo! I am a Computer Science student who is looking for a position that will ensure healthy growth and refinement of my current skills while also expanding my knowledge and abilities in a challenging and friendly work environment.

## Degrees & Certificates
- Bachelor of Science Degree in Computer Science, 2024
- Associate of Science Degree in Computer Science, 2022
- Associate of Arts Degree in General Studies with Emphasis in Language and Rationality, 2022
- Certificate of Achievement Computer Programming Specialist, 2022
- Certificate of Achievement California State University General Education Pattern, 2022

## Contact Details
- Phone: (209) 602-1093
- Preferred Email: [anthony1093ca@mail.com](anthony1093ca@mail.com)
- Stanislaus State University Email:  [acastillo52@csustan.edu](acastillo52@csustan.edu)
- MJC Student Email: [anthony856177@my.yosemite.edu](anthony856177@my.yosemite.edu)
- GitHub: [https://github.com/CastilloAnthony](https://github.com/CastilloAnthony)
- LinkedIn: [https://www.linkedin.com/in/anthony-castillo-3b7829149/](https://www.linkedin.com/in/anthony-castillo-3b7829149/)

### Programming Langauges that I'm Comfortable with:
- Python
- C++
- Lua
- SQL
- HTML
- CSS

### Operating Systems and Software that I'm Comfortable with:
- Windows 10
- Raspberry Pi
- Linux
    - Bash Shell programming
- Microsoft Office Suite of Applications
    - i.e. Word, Excel, Powerpoint
- Visual Studio
- Git
- PuTTY
- VMWare
- Adobe Photoshop Elements

### A Quicklook

Below you can see a project that I've been working on for a Softweare Engineering class at Stanislaus State University. This project was written, primarily, in Python, but also using some HTML and CSS. In this project, my classmates and I developed a program that could monitor the latency of any websites that a user is interested in. The program would run the latency monitoring routines in the background while hosting a front-end webportal for user interfacing. The interface would allow users to add or remove websites from their personal list and also display a graphical representation of the latency over time. The code below shows three of the functions that the background server uses to control the program.  

```python
def _checkForRequests(self):
        """This function is responsible for checking the requestsQ and responding to each request as is appropraite.
        """
        # Expected Request Formats # WIP
        #auth {'username':'Christian', 'email':'something@csustan.edu', 'id':uuid.uuid4(), 'password':'??????'}
        #users {'id':uuid.uuid4(), 'username':'Christian', 'email':'something@csustan.edu', 'websitesList':['www.google.com', 'www.csustan.edu'], 'presets':[{"name": "Christian", "presetLists": ["www.google.com", "www.csustan.edu", "www.microsoft.com", "www.nasa.gov"],"timestamp": 1698890950.1513646}, {"name": "Anthony", "presetLists": ["www.google.com", "www.instagram.com", "www.csustan.edu"], "timestamp": 1698890933.333366}]

        #{'id':uuid.uuid4(), 'timestamp':time.time(), 'request_type':'request', 'column':'masterList', 'query':{}}                                                               ### Gets all urls from the master list
        #{'id':uuid.uuid4(), 'timestamp':time.time(), 'request_type':'request', 'column':'pollingData', 'query':'wwww.google.com'}                                               ### Requests the polling data for a specific url
        #{'id':uuid.uuid4(), 'timestamp':time.time(), 'request_type':'request', 'column':'pollingData', 'query':['wwww.google.com', 'www.instgram.com', 'www.csustan.edu']}      ### Requests the polling data for a list of urls
        #{'id':uuid.uuid4(), 'timestamp':time.time(), 'request_type':'request', 'column':'pollingData', 'query':{'url':'wwww.google.com', 'timestamp':{$gte:time.time()-60*3}}}  ### Requests the polling data for a specified url with a timestamp greater tna or equal to the current timestamp - 3 minutes 
        #{'id':uuid.uuid4(), 'timestamp':time.time(), 'request_type':'insert', 'column':'masterList', 'query':'wwww.google.com'}                                                 ### Inserts a url into the masterList
        #{'id':uuid.uuid4(), 'timestamp':time.time(), 'request_type':'remove', 'column':'masterList', 'query':{url:'wwww.google.com'}}                                           ### Removes a url from the master list, be careful with this
        #{'id':uuid.uuid4(), 'timestamp':time.time(), 'request_type':'setting', 'column':'pollingSpeed', 'query':60}                                                             ### Changes the polling speed of the server. Query must be an integer
        # Additonal info: https://www.mongodb.com/docs/manual/reference/operator/query/#std-label-query-projection-operators-top
        while self.__requestsQ.empty() != True:
            newRequest = self.__requestsQ.get()
            if newRequest['request_type'] == 'request': # For requesting any data from the system
                if newRequest['column'] in 'masterList':
                    if newRequest['query'] == {}:
                        masterList = []
                        data = self.requestManyFromDB(newRequest['column'], newRequest['query'])
                        for doc in data:
                            masterList.append(doc['url'])
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':masterList})
                    else:
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':False})
                elif newRequest['column'] in 'pollingData':
                    if isinstance(newRequest['query'], list): # For a list of urls
                        tempData = []
                        for i in newRequest['query']:
                            tempData.append(self.requestManyFromDB(newRequest['column'], newRequest['query']))
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':tempData})
                        del tempData
                    elif isinstance(newRequest['query'], dict):
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.requestManyFromDB(newRequest['column'], newRequest['query'])})
                    elif isinstance(newRequest['query'], str): # For a single url
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.requestManyFromDB(newRequest['column'], newRequest['query'])})
                    else:
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':False})
                elif newRequest['column'] in 'presets':
                    if newRequest['query'] == {}:
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.requestManyFromDB(newRequest['column'], newRequest['query'])})
                    elif isinstance(newRequest['query'], str):
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.requestFromDB(newRequest['column'], newRequest['query'])})
                    elif isinstance(newRequest['query'], list):
                        tempData = []
                        for i in newRequest['query']:
                            tempData.append(self.requestManyFromDB(newRequest['column'], newRequest['query'][i]))
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':tempData})
                        del tempData
                    elif isinstance(newRequest['query'], dict):
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':'Not Yet Implemented'})
                elif newRequest['column'] in 'users':
                    if isinstance(newRequest['query'], dict):
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.requestFromDB(newRequest['column'], newRequest['query'])})#C.A.
                elif newRequest['column'] in 'auth':
                    #self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.requestFromDB(newRequest['column'], newRequest['query'])})
                    data = self.requestFromDB(newRequest['column'], newRequest['query'])
                    if data == None:
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':False})
                    else:
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.requestFromDB(newRequest['column'], newRequest['query'])})
                    #temp = self.requestFromDB(newRequest['column'], )
                    #if newRequest['query'] == '':
                        #if bcrypt.checkpw(newRequest['query']['password'], passwordcheck):
                    #else:
                        #self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':False})
                elif newRequest['column'] in self.__columns: # For all other requests
                    self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':'Not Yet Implemented'})
            elif newRequest['request_type'] == 'insert':
                if newRequest['column'] == 'masterList': # For url insertions into the master list
                    if self.requestFromDB('masterList', {'url':newRequest['query']}) == None:
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.sendToDB(newRequest['column'], {'url':newRequest['query'], 'timestamp':time.time()})})
                    else:
                        self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':False})
                elif newRequest['column'] == 'presets':
                    self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.sendToDB(newRequest['column'], newRequest['query'])})
                elif newRequest['column'] == 'auth':
                    self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.sendToDB(newRequest['column'], newRequest['query'])})
                elif newRequest['column'] == 'users':
                    self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.sendToDB(newRequest['column'], newRequest['query'])})
                elif newRequest['column'] in self.__columns: # For all other insertions, might not be necessary (infact might not be good either)
                    self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.sendToDB(newRequest['column'], newRequest['query'])})
            elif newRequest['request_type'] == 'remove':
                if newRequest['column'] in self.__columns: # For all removals of data, might not be good, but works
                    self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.removeFromDB(newRequest['column'], newRequest['query'], newRequest['changeTo'])}) #C.A. up
            elif newRequest['request_type'] == 'setting': # For changing settings such as the polling speed of the server
                self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self._changeSettings(newRequest['column'], newRequest['changeTo'])})
            elif newRequest['request_type'] == 'update':
                self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.updateInDB(newRequest['column'], newRequest['query'], newRequest['changeTo'])})
            elif newRequest['request_type'] == 'update2':#Electric boogaloo
                self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':self.update2InDB(newRequest['column'], newRequest['query'], newRequest['old'], newRequest['changeTo'])})#C.A.
            #column:str, query:dict, old:dict, changeTo:dict
            else:
                self.__dataQ.put({'id':newRequest['id'], 'timestamp':time.time(), 'data':'Not Implemented'})

    def _clearDataQ(self):
        initial = None
        while not self.__dataQ.empty():
            tempData = self.__dataQ.get()
            print(str(time.ctime())+' - Deleted dataQ object from '+str(time.ctime(tempData['timestamp']))+' with id: '+str(tempData['id']))
        '''
        while not self.__dataQ.empty():
            if initial == None:
                initial = self.__dataQ.get()
                while initial['timestamp'] < (time.time() - 60*1):
                    initial = self.__dataQ.get()
            tempData = self.__dataQ.get()
            print(tempData)
            if tempData == initial:
                self.__dataQ.put(tempData)
                break
            elif tempData['timestamp'] < (time.time() - 60*1):
                continue
            else:
                self.__dataQ.put(tempData)
        '''
        

    def _pollWebsites(self):
        """DEPRECATED, Funciton for requesting the masterlist from the database and for recording the latency for connections to each url in the masterlist. 
        """
        print(str(time.ctime())+' - Polling sites...')
        masterList = self.requestManyFromDB('masterList', {})
        for object in masterList:
            for port in self.__httpPorts:
                try:
                    latencyTimerStart = time.time()
                    temp = socket.create_connection((object['url'], port))
                    temp.close()
                    latencyTimerEnd = time.time()
                    self.sendToDB('pollingData', {'url':object['url'], 'port':port, 'timestamp':time.time(), 'up':True, 'latency':latencyTimerEnd-latencyTimerStart})
                    break
                except:
                    self.sendToDB('pollingData', {'url':object['url'], 'port':port, 'timestamp':time.time(), 'up':False, 'latency':np.nan})
```

For more information on this above project you can visit the github repository [here](https://github.com/CastilloAnthony/CS-4800_Project_Sherlock/). Again, other Projects and their repositories can be found [here](https://github.com/CastilloAnthony?tab=repositories).

Below is a sample of some C++ code that I've written and worked with. In this example, I created a header file for a class object with the intent that it'd be capable of reading a specific file (contigencyLibrary.txt) line by line and importing those lines as urls, folder locations, or file locations. Once the program has pulled the data from the text file, it then attempts to access each url individually. And then wait for user input before closing. Its a simple program intended to automate the opening of specific files, folders, or websites.

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

For more information on this above project you can visit the github repository [here](https://github.com/CastilloAnthony/Contingency). Other Projects and their repositories can be found [here](https://github.com/CastilloAnthony?tab=repositories).
