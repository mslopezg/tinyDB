# Database Record Manager - Assignment 3

Manuel Lopez    - *A20411680*
Mohammed Chisti - *A20393790*

#### Building and Testing Instructions

* Head to the assignment 3 directory `cd ***\cs525_s20_g9\assign3_record_manager`
	* (If you can see this you are at the right place)
* Input `make` to compile the assignment (ignore the warnings the gcc is giving for memcpy)
	* This results in **two executable files: test3 and testexpr**
	* **_test3_**: this tests the project using test_assign3_1 file
	* **_testexpr_**: this tests the project using test_expr.c file
* Chose which executable
	* For test3 input `./test3`
	* For testexpr input `./testexpr`
* After testing, cleanup by inputting `make clean`

#### Table and Record Manager Functions

* `extern RC initRecordManager (void *mgmtData)`
	* initializes record manager
* `extern RC shutdownRecordManager ()`
	* shuts down record manager and deallocates table manger structure
* `extern RC createTable (char *name, Schema *schema)`
	* creates table in memory 
	* initializes buffer pool via init and we utilize the FIFO replacement policy
	* creates page file, opens that page file and writes to it and later closes the page file
* `extern RC openTable (RM_TableData *rel, char *name)`
	* table opened from memory
	* pins page
	* initializes schema, allocates memory and sets the copies the pages information there
* `extern RC closeTable (RM_TableData *rel)`
	* grabs the mgmtData and assigns to table struc
	* later shutsdown the buffer pool for table struc
* `extern int getNumTuples(RM_TableData* rel)`
	* gets number of tuples in table

#### Record Functions

* `extern RC insertRecord (RM_TableData *rel, Record *record)`
	* insert a new record at page and slot
	* pins page -> inserts record
	* page is marked dirty and written to disk
* `extern RC deleteRecord(RM_TableData* rel, RID id)`
	* deletes record
	* pins page -> record updated
	* uses tombstone
	* marks page as dirty and written to disk
* `extern RC updateRecord (RM_TableData *rel, Record *record)`
	* updates record
	* pins page -> update
	* marks page as dirty and written to disk
* `extern RC getRecord (RM_TableData *rel, RID id, Record *record)`
	* retrieves record from page and slot

####  Scan Functions

* `extern RC startScan(RM_TableData* rel, RM_ScanHandle* scan, Expr* cond)`
	* scans tuples based on data from RM_ScanHandle
* `extern RC next(RM_ScanHandle* scan, Record* record)`
	* gets next tuple that satisfies cond.
* `extern RC closeScan(RM_ScanHandle* scan)`
	* closes the scan

####  Schema Functions

* `extern int getRecordSize (Schema *schema)`
	* using schema gets record size 
* `extern Schema *createSchema (int numAttr, char **attrNames, DataType *dataTypes, int *typeLength, int keySize, int *keys)`
	* creates new Schema
* `extern RC freeSchema(Schema* schema)`
	* deletes schema 
	* frees the allocated resources

####  Attribute Functions

* `extern RC createRecord(Record** record, Schema* schema)`
	* creates empty record
	* sets tombstone marker to indicate empty
* `extern RC freeRecord (Record *record)`
	* pins page with record
	* deletes record -> updates tombstone 
	* marks dirty and writes to disk
* `extern RC getAttr (Record *record, Schema *schema, int attrNum, Value **value)`
	* gets attribute value for a record
* `extern RC setAttr (Record *record, Schema *schema, int attrNum, Value *value)`
	* sets the attribute value based on int, string etc.