Download Link: https://assignmentchef.com/product/solved-solvedspell-check
<br>
Your task is to write a Multi-Threaded (ie. concurrent) Client Server C/C++ application for performing spell checking of text files using the dictionary in /usr/dict/words.

After the file is processed by the server, the client should display all miss-spelt words and any near words as suggested replacements. (An example output is shown below.)“`cppLine No. Mis-spelt word Suggestion7 enogh enough12 nothng nothing230 happenned happened234 glob globe563 poeple people1034 lathgter laughter1455 kowing knowing1788 wuold would1951 quate quite2145 javascript no suggestion2344 winkle wrinkleTotal processing time = 3.723 sec“`File processing should be done by the server on a line by line basis. Each time a line of text is read it should be preprocessed by converting all uppercase letters to lowercase and removing leading or tailing punctuation characters. A word is to be defined as a sequence alphabetic characters as well as the characters ‘ ‘ ‘ (apostrophe) and ‘-‘(hyphen). Thus, words like “co-operate” and “it’s” will be processed as whole words. (Note: If you are not sure how to extract words from a line of text you can use the example code in sscanf_eg.cpp.) The total processing time is the time from the commencement of the program (or client) until its termination. To search for words in the dictionary just use a linear search.

To determine if the two words are the same (or different) use the int WordCmp(char*,char*) function below. It returns 0 if the words are the same or a number 0 if the words are different. (Note: the greater the return value the greater the difference between the words.) To obtain a suggestion word for a miss-spelt word use the word with the minimum difference. (Note: if there is more than one word with the minimum difference for a miss-spelt word then use the one found first as the suggestion word.)“`cppint WordCmp(char*Word1,char*Word2){if(strcmp(Word1,Word2)==0)return 0;if(Word1[0]!=Word2[0]))return 100;float AveWordLen = (strlen(Word1) + strlen(Word2)) / 2.0;return int(NumUniqueChars(Word1,Word2) / AveWordLen * 100);}“`where: the `NumUniqueChars()` is the number of chars in Word1 that are not in Word2 plus the number of chars in Word2 that are not in Word1. You may improve this compare function if you wish.

**You should complete this program by performing the following steps.**

Step 1: Implement a single-process program without sockets or threads.

Step 2: Convert the program in step 1 into a client-server application using TCP sockets.

Step 3: Convert the server program in step 2 into a multithreaded program capable of processing lines ofinput file data in parallel from an input queue and record all miss-spelt words, their suggestionword and line number in a global linked list. You may use buffer.cc to help with this step. Themutexes and condition vars should provide mutual exclusion to the shared global data to preventthe threads from updating it simultaneously. When processing is complete the contents of thelinked list are sent to the client for displaying on the user’s screen.

###Algorithms:

####SINGLE THREADED“`cppopen input filewhile !eofread line from filepreprocess linefor each word in lineopen dictionaryfor each word in dictionarycompare wordsif same found=true;break;if word difference is minimumkeep dictionary word as a suggestion wordclose dictionaryif !foundprint line no. word and suggestion on screenend forclose input fileprint processing time“`####CLIENT:

1. get filename and number of slaves etc. from command line2. gethostbyname (to obtain IP_Address of the Server)3. contact Server (using connect + IP_Address from above)4. send filename and number of threads to server &amp; wait for miss-spelt words to return.5. take miss-spelt words from Server and print them6. close socket.

####SERVER:

1. setup socket2. accept client connection4. read filename and number of threaded slaves.5. open file6. create threaded slaves7. keep filling input buffer with preprocessed lines of text. (see Producer below)8. wait for threads to complete9. send miss-spelt words to client10. close socket &amp; file, goto 2

####PRODUCER:“`cppwhile(1)read line from fileif no data left set data_finished &amp; break;if queue full pthread_cond_waitelseput item in input queue. . .“`####CONSUMER THREAD:“`cppwhile(1)if data_finished break;if input queue empty pthread_cond_waitelsetake item from queueprocess item. . .“`####NOTES:

(i) For the server to work correctly you will have to implement a number of mutexes and conditionvariables appropriately to make the global data accesses mutually exclusive. Remember with threadspreemption can occur at any point in the program, even halfway through calculations.

(ii) Using the following attribute ensures that threads will be bound to kernel LWPs in a 1:1 manner – ie. the OS uses time slicing (this ensures that each thread will get CPU time allocated, &amp; overcomes potential Solaris thread scheduling problems):“`cpppthread_attr_setscope(&amp;attr, PTHREAD_SCOPE_SYSTEM);“`

(iii) Compile using the -mt &amp; -lpthread options [ref. cc man page entries](By default, -mt appends -lthread ie. Solaris rather than POSIX!)