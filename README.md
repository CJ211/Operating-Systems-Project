# Operating-Systems-Project
import java.util.*;
class process
{
	int at,bt=0,wt,tt,flag=0;
	int pno;
}


class mlfq {
		public static void main(String args[])
		{
			Scanner sc= new Scanner(System.in);
			int n;
			System.out.println("Enter the no of processes");
			n=sc.nextInt();
			process[] p= new process[n];
			int[] initbt=new int[n];
			int i,j;
			for(i=0;i<n;i++)
			{
				p[i]= new process();
				System.out.println("Enter the process id");
				p[i].pno=sc.nextInt();
				System.out.println("Enter the arrival time");
				p[i].at=sc.nextInt();
				System.out.println("Enter the burst time");
				p[i].bt=sc.nextInt();
			}
			//Needed for waiting time calculation
			for(i=0;i<n;i++)
				initbt[i]=p[i].bt;
		
			//sorting by arrival time
			for(i=0;i<n-1;i++)
			{
		
				for(j=0;j<n-1-i;j++)
				{
					if(p[j].at>p[j+1].at)
					{
						process t=p[j];
						p[j]=p[j+1];
						p[j+1]=t;
					}
				}
			}
	
		int tq1=2;//time quantum 1
		int tq2=2;//time quantum 2
		int curt=0;//elapsed time or current time
		for(i=0;i<n;i++)
		{
			if(p[i].flag!=1)//as long as the process hasn't finished execution
			{	
				if(p[i].bt<tq1)
				{
					curt=curt+p[i].bt;//incrementing current time by burst time
					p[i].bt=0;//as execution finishes burst time becomes zero
				}
			else
			{	
				p[i].bt=p[i].bt-tq1;//reducing the burst time after allottment
				curt=curt+tq1;//incrementing current time by time quantum
			}
			
			if(p[i].bt<=0)//finally if the process has completely executed.
			{
				p[i].flag=1;
				p[i].tt=curt-p[i].at;//calculating turnaround time for completed process
				p[i].wt=p[i].tt-initbt[i];//calculating waiting time for completed process
				System.out.println("QUEUE 1"+"\t"+"Completed process:"+p[i].pno+"\t"+"Burst Time:"+p[i].bt+"\t"+"Waiting Time:"+p[i].wt+"\t"+"Turnaround Time:"+p[i].tt);
			}
			
			}
				
		}	
		
		for(i=0;i<n;i++)
		{
			if(p[i].flag!=1)//as long as the process hasn't finished execution
			{
				if(p[i].bt<tq2)
				{
					curt=curt+p[i].bt;//incrementing current time by burst time
					p[i].bt=0;//as execution finishes burst time becomes zero
				}
				else
				{	
					p[i].bt=p[i].bt-tq2;//reducing the burst time after allottment
					curt=curt+tq2;//incrementing current time by time quantum
				}
				
				if(p[i].bt<=0)
				{
					p[i].flag=1;
					p[i].tt=curt-p[i].at;//calculating turnaround time for completed process
					p[i].wt=p[i].tt-initbt[i];//calculating waiting time for completed process
					System.out.println("QUEUE 2"+"\t"+"Completed process:"+p[i].pno+"\t"+"Burst Time:"+p[i].bt+"\t"+"Waiting Time:"+p[i].wt+"\t"+"Turnaround Time:"+p[i].tt);
				}
			
			}
		}
		
			int k;
			process selb=new process();
			for(k=0;(k<n) && (p[k].flag!=1) ;k++)
			{
				selb.bt=999;
				for(i=0;i<n;i++)//finding the process with least burst time
				{

					if((p[i].at<=curt) && (p[i].flag!=1))
					{
			
						if(p[i].bt<selb.bt)
						{
							selb=p[i];
						}
					}	

				}
				/*for(j=0;j<n;j++)//updating the flag as it is found.
				{
					System.out.print("hi");
					if(selb.pno==p[j].pno)
					{
						p[j].flag=1;
						break;
					}
				}*/
				
				if(selb.bt!=999)
				{
					selb.flag=1;
					curt=curt+selb.bt;
					selb.bt=0;
					selb.tt=curt-selb.at;//calculating turnaround time for completed process
				selb.wt=selb.tt-initbt[selb.pno];//if all the processes are allotted id's from 0 in order
				System.out.println("QUEUE 3"+"\t"+"Completed process:"+selb.pno+"\t"+"Burst Time:"+selb.bt+"\t"+"Waiting Time:"+selb.wt+"\t"+"Turnaround Time:"+selb.tt);
				
				}
			}
	
			float turn=0,wait=0;
		for(i=0;i<n;i++)
		{
			turn=turn+p[i].tt;
			wait=wait+p[i].wt;
		}
	
		turn=turn/n;
		wait=wait/n;
		System.out.println("The current time after completion of all processes "+curt);
		System.out.println("The average Turnaround time is "+turn);
		System.out.println("The average Waiting time is "+wait);

	
      }
}






	/*
	
	OUTPUT
	
	Enter the no of processes
	4
	Enter the process id
	0
	Enter the arrival time
	0
	Enter the burst time
	7
	Enter the process id
	1
	Enter the arrival time
	2
	Enter the burst time
	4
	Enter the process id
	2
	Enter the arrival time
	4
	Enter the burst time
	1
	Enter the process id
	3
	Enter the arrival time
	5
	Enter the burst time
	4
	QUEUE 1	Completed process:2	Burst Time:0	Waiting Time:0	Turnaround Time:1
	QUEUE 2	Completed process:1	Burst Time:0	Waiting Time:5	Turnaround Time:9
	OUEUE 2	Completed process:3	Burst Time:0	Waiting Time:4	Turnaround Time:8
	QUEUE 3	Completed process:0	Burst Time:0	Waiting Time:9	Turnaround Time:16
	The current time after completion of all processes 16
	The average Turnaround time is 8.5
	The average Waiting time is 4.5
	*/
