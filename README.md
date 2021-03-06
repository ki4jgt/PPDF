# PPDF
PPDF is a NIDS (Non-Indexed Document Storage) database, written in 127 lines of pure Python.

It features:

- Native Python data storage through JSON
- Local database interaction through pre-defined functions
- A fully-working Memcache to avoid unnecessary read-write operations

# Simple to Use
PPDF has 4 main functions, and 2 worker threads. Using it is simple:

    import ppdf
    
    db = ppdf.db()

## Push
    db.push(dict)

Push takes a dictionary of keys, splits them up, and writes each key to its own unique path within the database.

## Pull
    db.pull(list)

Pull takes a list of database keys and returns them and their values in a dictionary.

## Drop
    db.drop(list)

Drop takes a list of keys and removes them from the database.

## Find
    db.find(list)

To be implemented later. This function is dedicated to distributed databases. It broadcasts a message to all network nodes, looking for a particular key.

# Worker Threads
Worker threads prevent data corruption by ensuring PPDF is only accessing one file at a time.

## Writer
The writer begins by clearing any cached values in RAM resembling what you've written. It then proceeds to write your data to the hard drive.

## Reader
The reader begins by looking through the cache for requested data. If it cannot find it there, it opens the document file on-drive.

# Memcache
PPDF has built-in Memcache. This drastically lowers the number of read-write operations to the hard drive, and allows for quicker retrival of data for your application. PPDF's memcache works by saving the last 100 pull operatations into RAM.
