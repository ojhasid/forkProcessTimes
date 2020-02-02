/***************************************************************************
* File: forktime.c
* Author: Sujeet Ojha
* Modification History:
			File was modified on January 28, 2020 by Sujeet Ojha
			in order to add extra error conditions for the function fork()
* Procedures:
* main   - driver function that calls the ftimes(int n) function n
		   number of times
* ftimes - function used to gather data regarding the time it takes to start
		   a new process with fork(). This function also stores process elapsed
		   time data into two separate text files:
		   (childElapsedTime.txt and parentElapsedTime.txt)
***************************************************************************/

#include <sys/time.h>
#include <time.h>
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>


/***************************************************************************
* int ftimes(int n)
* Author: Sujeet Ojha
* Date: January 28 2020
* Description: Calls the fork() function n amount of times and calculates the process 
			   elapsed times for both parent and child processes.
			   This function also utilizes two files: (childElapsedTime.txt and parentElapsedTime.txt)
			   in order to store the reference start time and the process elapsed time
			   information for all parent and child processes.
* Parameters:
* n I/P int The number of iterations data regarding the parent and child processes should be gathered
* ftimes O/P void Status code (not currently used)
***************************************************************************/

void ftimes(int n){

	double ntimes = 0;
    struct timeval t0, t1;
    double childStart, childEnd, childDuration, parentStart, parentEnd, parentDuration;
    
    //Initializing objects of type FILE with access mode of "w" to ensure it is used for writing
	FILE *fchild, *fparent;
	
	//Opening the files used to store data
  	fchild = fopen("childElapsedTime.txt","w");
  	fparent = fopen("parentElapsedTime.txt", "w");
    
    //Iterates and calls the fork() function n amount of times
    //Note: n is specified by the driver function, main, and determines the amount of 
    //data points collected
    for (int i = 0; i < n; i++){
   
		gettimeofday(&t0, 0); //Gets the reference start time before fork() is called

		const pid_t a = fork(); //Creates a new process and returns the process ID (PID) of the child process
		
		gettimeofday(&t1, 0); //Gets the reference end time after the function fork() is called

		
   		if (a == 0){ //If the PID relates to one of a child process

			printf("------Child------\n");
			
			childStart = (t0.tv_sec) + (t0.tv_usec)*0.000001; //Converts the reference start time to seconds
			childEnd = (t1.tv_sec) + (t1.tv_usec)*0.000001; //Converts the reference end time to seconds
			childDuration = childEnd - childStart; //Calculates the elapsed time for the child process
			
			printf("PID: %d [PPID: %d]\n", getpid(), getppid());
			printf("Child start time: %0.8f\n", childStart);
			printf("Child end time: %0.8f\n", childEnd);
			printf("Child duration: %0.8f\n\n", childDuration);
			
			//If the file stream is unable to open, output an error statement and exit
			if(fchild == NULL)
  			{
      			printf("Error!");   
      			exit(1);             
   			}
   			else{
   				//Storing the reference start time and child process elapsed time to the file (childElapsedTime.txt)
   				fprintf(fchild,"%0.8f\t%0.8f\n", childStart, childDuration);
   			}
   			
		   break; //Used to stop the process from duplicating			
		}
		else if (a > 0){ //If the PID relates to one of a parent process
		
			printf("------Parent------\n");
			
			parentStart = (t0.tv_sec) + (t0.tv_usec)*0.000001; //Converts the reference start time to seconds
			parentEnd = (t1.tv_sec) + (t1.tv_usec)*0.000001; //Converts the reference end time to seconds
			parentDuration = parentEnd - parentStart; //Calculates the elapsed time for the parent process
			
			printf("PID: %d\n", getpid());
			printf("Parent start time: %0.8f\n", parentStart);
			printf("Parent end time: %0.8f\n", parentEnd);
			printf("Parent duration: %0.8f\n\n", parentDuration);
			
			//Waiting for the child process to finish
			wait(NULL);
			
		}
		
		else{ //If a < 0, fork() could not create a new process, therefore, output an error
			printf("Error! Child process creation has failed!\n");
		
		}

	}
	
	//If the file stream is unable to open, output an error statement and exit
	if(fparent == NULL){
	
      	printf("Error!");   
      	exit(1);             
   	}
   	
   	else{
   		
   		//If statement ignores the first data point with duration (0.0000) that does not have a matching child pair
   		if ((float)parentDuration != (float)0){
   			//Store the reference start time and parent process elapsed time to the file (parentElapsedTime.txt)
   			fprintf(fparent,"%0.8f\t%0.8f\n", parentStart, parentDuration); //
   		}
   		
   	}
	
	//Closing the files after use
	fclose(fchild);
	fclose(fparent);
    
}

/***************************************************************************
* int main()
* Author: Sujeet Ojha
* Date: 28 January 2020
* Description: Driver function that specifies the amount of times the function
			   ftimes() should run. The function has the variable n = 2200,
			   specifying that 2200 samples of parent process and child process
			   data points should be collected.
* Parameters:
* main O/P int Status code (not currently used)
**************************************************************************/

int main(){
	
	int n = 2200; 
	ftimes(n); //Function call with an input of n

}
