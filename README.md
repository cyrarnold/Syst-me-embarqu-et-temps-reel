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


3 Implementation de solu

#include <stdlib.h>
#include <signal.h>
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

long count_time = 0;
int A_isready = 0;
int B_isready = 0;
int C_isready = 0;
int D_isready = 0;
pthread_t taskA, taskB, taskC, taskD;

long n = 10; // variable globale N

// Question 4
/*long n1 = 20; 
long n2 = 30;
long n3 = 40;
long n4 = 50;
long n5 = 60;*/

void handle_alarm(int number){
  count_time++;
  alarm(4);	        
}

static int aleatoire (int maximum)
{
	double d;
	d = (double) maximum * rand();
	d = d / (RAND_MAX + 1.0);
	return ((int) d);
}


void * routine_taskA(void * arg)
{
        
	long num = n;
	sleep(aleatoire(2));
        n = ++num;
	return (void *) (n);
}

void * routine_taskB(void * arg)
{
        
	long num = n;
	sleep(aleatoire(1));
        n = ++num;
	return (void *) (num);
}

void * routine_taskC(void * arg)
{
	
	long num = n;
	sleep(aleatoire(3));
        n = ++num;
	return (void *) (num);
}
void * routine_taskD(void * arg)
{

        long num = n;
        sleep(aleatoire(1));
        n = ++num;
        return (void *) (num);
}



int main(int argc, char *argv[]){
  void *ptr;
  if (signal(SIGALRM, handle_alarm) == SIG_ERR)
    fprintf(stderr, "Signal %d non capture\n", SIGALRM);
  alarm(4);
  while(count_time <= 23){
    if(count_time==2) A_isready = 1;
    if(count_time==7) B_isready = 1; 
    if(count_time==13) C_isready = 1;
    if(count_time==23) D_isready = 1;
    
    if(A_isready){ 
    	printf("start task A %ld\n", count_time);
    	if (pthread_create(& taskA, NULL, routine_taskA, NULL) != 0) {
		fprintf(stderr, "Erreur dans pthread_create\n");
		exit(EXIT_FAILURE);
	}	
    	
    }
    if(B_isready){ 
    	printf("start task B %ld\n", count_time-2);
    	if (pthread_create(& taskB, NULL, routine_taskB, NULL) != 0) {
		fprintf(stderr, "Erreur dans pthread_create\n");
		exit(EXIT_FAILURE);
	}
    }
    if(C_isready){ 
    	printf("start task C %ld\n", count_time-7);
    	if (pthread_create(& taskC, NULL, routine_taskC, NULL) != 0) {
		fprintf(stderr, "Erreur dans pthread_create\n");
		exit(EXIT_FAILURE);
	}
    	
    }

    if(D_isready){
        printf("start task D %ld\n", count_time-13);
        if (pthread_create(& taskD, NULL, routine_taskC, NULL) != 0) {
                fprintf(stderr, "Erreur dans pthread_create\n");
                exit(EXIT_FAILURE);
        }

    }


    
    if(A_isready){
    	pthread_join(taskA, & ptr);
    	fprintf(stderr, "-> %ld\n", n);
    }
    
    if(B_isready){
    	pthread_join(taskB, & ptr);
    	fprintf(stderr, "-> %ld\n", n);
    }
    if(C_isready){
    	pthread_join(taskC, & ptr);
	fprintf(stderr, "-> %ld\n", n);
    }

    if(D_isready){
        pthread_join(taskD, & ptr);
        fprintf(stderr, "-> %ld\n", n);
    }

    A_isready = 0; B_isready = 0; C_isready = 0, D_isready = 0;
    pause();
  }
  fprintf(stderr, "valeur finale de n-> %ld count_time -> %ld\n", n, count_time-1);
  return 0;
}
