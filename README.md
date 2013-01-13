pipeline-simulator-
===================
ALGORITHM/DISCRIPTION:
First part is the reading of static trace file and storing in the form
hash table
The static file is read line by line in form of a string
The string read is passed in input() function the function take input
in form of string and and divide it into array of sting every time the
white space is encountered
Hence for static file it will divide it into an array of index 5 by using
following inbuilt function in java asr=s.split(" ");
This array of string is converted into integer and stored in the form of
hash table
Now, the 2 current and earlier input of Dynamic trace file it is in form
of hash table but of index 2
Similar to Queue data structure each time the function insdin() is
called the earlier value is popped and new value is inserted in this
manner we keep track of of the last instruction.
The insdin() function calls the time() function which is the basic
implementation of pipeline .the input of the time function are index
in the hass table of current pc ,earlier pc ,32 bit binary string of
memory address of dynamic file from converter() function and 0/1/-1
integers to pass it in the branch predictor.
Time function is basic implementation of pipeline. For current
instruction index in hash table we look at whether the instruction is
load,store,branch,ALU
We keep the counter of total time required and the total number of
instruction in the in the dynamic file
If the instruction is ALU and the above instruction is load and the
destination register of the above load instruction is either of rs or rt
register :
Eg
lw 15,......
ALU 18,15....
In this case the data of register 15 can only be forwarded after the
DM stage hence we require 1 stall the stall function calculate for the
required stall and we add time=time +2 for next instruction rest in all
other stages data can be directly forwarded.
We keep the counter of total correct prediction and total branch
instruction
If the instruction is load we need to assess the memory the 32 bit
binary string of address is passed in the function for calculating the
time required to access the cache.the output of the function is added
to current time.
If the instruction is branch the 12 bit pc binary string to branch
predictor.ang get the prediction from predictor if the prediction is
wrong we add 2 stalls else not.
If the instruction is store and following case is there
Lw 15,.....
Str 18,15,.......
In this case the stalling of 1 clock is required as the r15 is needed in
the ALU stage for str instruction,str instruction also access memory
and the output of the cache is added to the current time.
*Branch predictor consists of Branch predictor class which is a child of Cache.
*Formed 3 arrays, tournament predictor array{tur[i]} of size 1024,Bimodal
array{bm[i]} of size 1024,Gshare array{gs[i]} of size 4096.
*Branch predictor constructor is used to initialize 3 arrays and bhr=000000.
*Initially in tournament predictor both bimodal and gshare predictors are called;
and their predictions are stored in variables predict1 and predict2.
* Based on the value in tournament predictor, if(tur[i]==0 || tur[i]==1), we will
store predict2 value in a variable named mainprediction else we will store
predict1 in mainprediction.
*If both predict1 and predict2 are equal or unequal to dynamic file value
“do nothing” to the tournament predictor array value. If predict2 is equal and
predict1 is unequal to dynamic file value decrement the tournament predictor array
value. If predict1 is equal and predict2 is unequal to dynamic file value
increment the tournament predictor array value.
*BIMODAL PREDICTOR:
*PC given to the bimodal predictor function is made into a substring of 10 bits.
*Using these 10 bits we have to go to an address in bimodal array.
*if(bm[i]==0 || bm[i]==1) then we will store ‘0’ in predict2 else ‘1’ is stored.
*Now we have to update our bimodal predictor array, to update bimodal predictor
first check predict2 and the value in the dynamic file. If both are equal
increment the value in bimodal predictor array, if it is ‘3’ leave it as it is. If
they are unequal decrement the value in bimodal predictor array, if it is ‘0’
leave it as it is.
*BHR:
*BHR consists of the history of branches in the instruction file.
*We have to update it everytime after each instruction.
*For updating ,we have to remove the end bit, right shift the BHR and add a new
bit at left most position which the dynamic value.
*GSHARE PREDICTOR:
*First take end ‘6’ bits of ‘12’ bits PC.
*We have to XOR end ‘6’ bits of PC and BHR. This XOR value is included at the end
of first ‘6’ bits of PC.
*Using this value we have to check a address in gshare array.
*if(gs[i]==0 || gs[i]==1), we will store ‘0’ in predict1 else store ‘1’ in
predict1.
*Now we have to update our gshare predictor array, to update gshare predictor
first check predict1 and the value in the dynamic file. If both are equal
increment the value in gshare predictor array, if it is ‘3’ leave it as it is. If
they are unequal decrement the value in gshare predictor array, if it is ‘0’ leave
it as it is.
To make the cache L1 I take 2 array one is integer array in1[] of size 512 which is used to
determine the index of L1 cache and I take another array node1 l1[] of node1of size 1024 ,a node 1
contain string and int index, array l1 contain the tag value of address,.initially In in 1[i]=2*i, and l1[]
contain “man” in string part of node1 of array l1
similarly for L2 cache
To make the cache L2 I take 2 array one is integer array in1[] of size 2048 which is used to
determine the index of L2 cache and I take another array node1 l2[] of node2 of size 16384 ,a node
1 contain string and int index, array l2 contain the tag value of address,.initially In in 1[i]=8*i, and
l2[] contain “man” in string part and 0 in index part of node1 of array l1
now for L1 ,
index value is of 9-bit and tag value is of 17-bit
i convert the hexa decimal address to 32 bit binary address string ,so we a address string of 32 bit, in
function find I break the 32 bit string according to our required tag1 string which is a substring of
address from 0 to 18 and ins1 which contain substring 18 to 27 having 9 bit string .ins1 we convert
into integer value which give the index of address
similarly thing we do for L2 also in which our index value is of 11 bit and tag value is of 15 bit
initially I take time counter 0
or time=0
for L1
int readl1(int a,String s);
int a is the index value of address which I convert int integer
String s is of 17bit tag value which I take in the form of string
z=in1[a];
l1[z]
initially l2 contain man in all its node it man will be compared to input string s;
if both colomn l1[z] and l1[z+1] contain man s will over rite on l1[z] and there is a miss,will go to L2
if l1[z] is equal to s then there will be a hit
ifl 1[z] is not equal to s and l1[z+1] is equal to man then s over rite l1[z+1] and it is again a miss,it
will go to L2
if l 1[z] is not equal to s and 1[z+1] is not equal to s ,then it will a miss and s over rite l1[z+1] ,will go
to L2
initially we keep t=0;
when we call readl1()
time=time+1
when there is call of readl2();
time=time+8
which will satisfy the FIFO PROPERY
when there is a miss in L1 it will go to L2 to Check whether this value is present in L2 or not if it Will
present in L2,
and in case of miss in L2, time= time+ 200 in time counter
i calculate the number total of miss and hit in both cases L1 and L2
miss rate =1-(number of hit/total number of instruction pass )
