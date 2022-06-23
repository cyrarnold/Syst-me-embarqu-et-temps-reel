En nous inspirant du code du fichier  system.1 implémentons la mise en oeuvre du système en utilisant les threads

#include <stdlib.h>
#include <signal.h>
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

long count_time = 0;
int t1_isready = 0;
int t2_isready = 0;
int t3_isready = 0;
int t4_isready = 0;
int A_isready = 0;
int B_isready = 0;
int C_isready = 0;
//pthread_t task1, task1, task3, task4;
pthread_t taskA, taskB, taskC;
pthread_t task[4];


char * message = "fin de la tâche";

void handle_alarm(int number){
  count_time++;
  alarm(1);	        
}

void * routine_taskA(void * arg)
{
        
	char * message = "i am task b \n";
	return (void *) (message);
}


void * routine_taskB(void * arg)
{
        
	char * message = "i am task b \n";
	return (void *) (message);
}

void * routine_taskC(void * arg)
{
	
	char * message = "i am task b \n";
	return (void *) (message);
}

void * routine_task(void * arg)
{
	
	char * message = "i am task b \n";
	return (void *) (message);
}

int main(int argc, char *argv[]){
  void *ptr;
  if (signal(SIGALRM, handle_alarm) == SIG_ERR)
    fprintf(stderr, "Signal %d non capture\n", SIGALRM);
  alarm(1);

  // creation des threads 
  printf("start task iiiiii A %ld\n", count_time);
  for(int i=0; i<4; i++){
        if (pthread_create(& task[i], NULL, routine_task, NULL) != 0) {
                fprintf(stderr, "Erreur dans pthread_create\n");
                exit(EXIT_FAILURE);
        }else{
		fprintf(stderr, "thread créé pour task%d \n", i+1);
	}
  }
  while(count_time < 24){
    if(count_time==0)  fprintf(stderr, "debut de la tâche1 \n");
    if(count_time==2){ 
    	pthread_join(task[0], & ptr);
    	fprintf(stderr, "-> %s\n", message );
	fprintf(stderr, "debut de la tâche2 \n"); 
    }
    if(count_time==7){ 
    	pthread_join(task[1], & ptr);
    	fprintf(stderr, "-> %s\n", message );
	fprintf(stderr, "debut de la tâche3 \n"); 
    }
    if(count_time==13){ 
    	pthread_join(task[2], & ptr);
    	fprintf(stderr, "-> %s\n", message );
	fprintf(stderr, "debut de la tâche4 \n"); 
    }
    if(count_time==23){ 
    	pthread_join(task[3], & ptr);
	fprintf(stderr, "-> %s\n", message);
    }
    pause();
  }
  fprintf(stderr, "le temps d'execution de tout les threads est %lds \n", count_time-1);
  return 0;
