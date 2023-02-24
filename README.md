# READERS WRITERS PROBLEM STARVE FREE

This problem is about synchronizing 2 processes which are Readers and Writers
- **Readers -** The type of processes which read data from shared memory location
- **Writers -** The type of processes which write data to shared memory location 

Various solution to this problem exist but majority of them lead to either readers or writers starving.

## Starve Free Solution

The solution can be made starve free by using another semaphore which stops the read process which arrive after the an write process.

### Explanation

In the original solution discussed in the class the writers would starve as continous readers would come. The solution solves this problem using another semaphore readhold on which the write processes wait. Once a write process is waiting on the semaphore then the new process that arrive will also wait on the semaphore readhold. Thus the previous processes which were executing will complete execution and signal the semaphore wrt which in turn will execute the write process. Thus an starve free solution is achieved.

The solution can be better understood through an example:

Consider a 3 read processes arrive one after another and then a write process arrives then an additional 2 read process arrive. Then until 3 read processes are executed the write process will be blocked and the read process that will arrive will blocked by the semaphore which is blocking the write process and then the write process will be executed until then the 2 read processes will be blocked.

### Implementation


#### Initialisation


``` 
// Initialisation

semaphore mutex(1);
semaphore write(1);
semaphore readhold(1);
integer counter(0); 
```

#### Reader Pseudocode


``` 
// Reader Part

wait(readhold)
wait(mutex)
counter++
if(counter==1) wait(wrt)
signal(mutex)
signal(readhold)

/*
Read
Section
*/

wait(mutex)
counter--
if(counter==0) signal(wrt)
signal(mutex) 
```

#### Writer Psudocode


``` 
// Writer Part

wait(readhold)
wait(wrt)

/*
Write
Section
*/

wait(wrt)
wait(readhold) 
```

All the pseudocode is given in the file [Pseudocode.txt](/Pseudocode.txt). Additionally the pseudocode for semaphore implementation is also mentioned.