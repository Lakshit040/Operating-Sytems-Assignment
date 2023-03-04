# Operating-Sytems-Assignment
LAKSHIT SHARMA 20115058 3rd Year Minor


Readers Writers Problem


First of all I will be showing, why in the standard solution writers starve

Standard Solution of Readers Writers Problem

Semaphores Used ->

wrt = 1, mutex = 1

Shared Variables ->

counter = 0

////////////////

Reader's Process 

while(true){

	wait(mutex);
	counter++;

	if(counter == 1) wait(wrt);

	signal(mutex);

	/*
	CRITICAL SECTION
	*/

	wait(mutex);
	counter--;

	if(counter == 0) signal(wrt);

	signal(mutex);

}

//////////////////

Writer's Process ->

while(true){

	wait(wrt);

	/*
	CRITICAL SECTION
	*/

	signal(wrt);

}


Case of Starvation: {

Reader in CS and Writer tries to access CS and it gets blocked but another Reader tries to get inside CS and it successfully gets inside, hence Writer starved. 

}

Proof:

When Reader R1 is in CS,

mutex = 1, counter = 1, wrt = 0

Now, let say W1 comes, but as wrt == 0, so it will be blocked... 

Now, Reader R2 comes

mutex becomes 0
counter becomes 2
if condition went false
mutex become 1

So, R2 gets access of CS, and similarly if more Readers come now, so Writer W1 has to starve.
Hence Proved that standard solution is not starvation free.


**************************************************************


Corrected Solution:

Semaphores Used -> 

wrt = 1, mutex = 1, ls = 1

Shared Variables ->

counter = 0

///////////////////

Reader's Process ->

while(true){

	wait(ls);
	wait(mutex);
	counter++;

	if(counter == 1) wait(wrt);

	signal(mutex);
	signal(ls);

	/*
	CRITICAL SECTION
	*/

	wait(mutex);
	counter--;

	if(counter == 0) signal(wrt);

	signal(mutex);

}

Writer's Process ->

while(true){

	wait(ls);
	wait(wrt);

	/*
	CRITICAL SECTION
	*/

	signal(wrt);
	signal(ls);

}

Changes in Code ->>

One more binary semaphore introduced (ls) with inital value = 1

One set of wait and signal command get's introduced in both Reader and Writer, which has nothing to do with the old reader or writer logic.

You can think one more layer of protection has given such that it works exactly same but without starvation.

Proof of Mutual Exclusion >>

Case 1: Reader R1 in CS then Writer W1 tries to go in CS

As R1 is in CS,

ls = 1, mutex = 1, counter = 1, wrt = 0

Now W1 comes,

ls becomes 0,
now as wrt was already 0, so wait(wrt) operation will block W1
Hence, mutual exclusion was present.


Case 2: Writer W1 in CS then Reader R1 tries to go in CS

As W1 is in CS,

ls = 0, wrt = 0

Now R1 comes,

Clearly as ls was 0, so R1 get's blocked here!!
Hence, mutual exclusion was present.

Case 3: Writer W1 in CS then Writer W2 tries to go in CS

As W1 is in CS,

ls = 0, wrt = 0

Now W2 comes, 

Clearly as ls was 0, so W2 get's blocked here!!
Hence, mutual exclusion was present.

Case 4: Reader R1 in CS then R2 tries to go in CS

As we all know that multiple readers are allowed to be in CS at that same time....


Proof of Starvation Free >>>>

As in the Standard solution, writers has to starve.
So, I will be showing that in this corrected solution writers will not starve.

Take the case when R1 is in CS, then W1 comes but get's blocked.

Now another Reader R2 comes and if it can get access to CS, W1 has to starve but it will not be the case here...

Let's see why??

Initially, 
mutex = wrt = ls = 1
counter = 0;

R1 comes->

ls becomes 0,
mutex becomes 0
counter becomes 1
wrt becomes 0
mutex becomes 1
ls becomes 1

R1 reaches CS 

Now, W1 comes

but as ls was 0, due to wait(ls) it get's blocked.

Now, R2 comes

And, now as ls was already 0 so R2 will be blocked.


So, we can see that R2 will not get chance to be in CS that the same time as R1 while having W1 blocked, hence W1 will not starve.

Hence PROVED!!

Neither Writers nor Readers will starve.
