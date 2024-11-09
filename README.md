//CPU SCHUDULING ALGORITHM

//SJF

import java.util.Scanner;

class Sjf {
    public static void main(String[] args) {
        int id[] = new int[20];        // Process IDs
        int arrivalTime[] = new int[20]; // Arrival time for processes (used only if arrival time is considered)
        int burstTime[] = new int[20];    // Execution time (burst time)
        int originalBurstTime[] = new int[20]; // Store original burst time for correct calculation
        int completionTime[] = new int[20];  // Completion time
        int waitingTime[] = new int[20];    // Waiting time
        int turnaroundTime[] = new int[20]; // Turnaround time
        int flag[] = new int[20];          // To check if process is completed
        int total = 0, st = 0;             // st: current time
        int n;                             // Number of processes
        float avgwt = 0, avgta = 0;        // Average waiting time and turnaround time
        
        Scanner sn = new Scanner(System.in);

        System.out.print("Enter the number of processes: ");
        n = sn.nextInt();

        System.out.print("Do you want to include arrival times? (yes/no): ");
        String includeArrival = sn.next();
        
        // Input process details
        for (int i = 0; i < n; i++) {
            System.out.println();
            System.out.print("Enter the process ID of process " + (i + 1) + ": ");
            id[i] = sn.nextInt();
            
            System.out.print("Enter the execution (burst) time of process " + (i + 1) + ": ");
            burstTime[i] = sn.nextInt();
            originalBurstTime[i] = burstTime[i]; // Save the original burst time
            
            if (includeArrival.equalsIgnoreCase("yes")) {
                System.out.print("Enter the arrival time of process " + (i + 1) + ": ");
                arrivalTime[i] = sn.nextInt();
            } else {
                arrivalTime[i] = 0; // If no arrival time is provided, assume arrival time 0
            }
            
            flag[i] = 0; // Process is not completed yet
        }

        // Sort by arrival time if using arrival times
        if (includeArrival.equalsIgnoreCase("yes")) {
            for (int i = 0; i < n; i++) {
                for (int j = i + 1; j < n; j++) {
                    if (arrivalTime[i] > arrivalTime[j]) {
                        // Swap arrival times
                        int temp = arrivalTime[i];
                        arrivalTime[i] = arrivalTime[j];
                        arrivalTime[j] = temp;
                        
                        // Swap process IDs
                        temp = id[i];
                        id[i] = id[j];
                        id[j] = temp;
                        
                        // Swap burst times
                        temp = burstTime[i];
                        burstTime[i] = burstTime[j];
                        burstTime[j] = temp;
                    }
                }
            }
        }

        // SJF Algorithm: Process execution
        while (total < n) {
            int min = Integer.MAX_VALUE;
            int c = n;
            
            // Choose process with minimum burst time that has arrived
            for (int i = 0; i < n; i++) {
                // If the process is ready (arrived) and not completed
                if (arrivalTime[i] <= st && burstTime[i] < min && flag[i] == 0) {
                    min = burstTime[i];
                    c = i;
                }
            }

            // If no process is ready, increment the time
            if (c == n) {
                st++;
            } else {
                burstTime[c]--;
                st++;
                if (burstTime[c] == 0) {
                    completionTime[c] = st;
                    flag[c] = 1;
                    total++;
                }
            }
        }

        // Calculate turnaround time and waiting time for each process
        for (int i = 0; i < n; i++) {
            turnaroundTime[i] = completionTime[i] - arrivalTime[i];  // Turnaround time = Completion time - Arrival time
            waitingTime[i] = turnaroundTime[i] - originalBurstTime[i]; // Use original burst time for correct waiting time
            avgwt += waitingTime[i];
            avgta += turnaroundTime[i];
        }

        // Display results
        System.out.println("\nProcess ID\tArrival Time\tBurst Time\tCompletion Time\tWaiting Time\tTurnaround Time");
        for (int i = 0; i < n; i++) {
            System.out.println(id[i] + "\t\t" + arrivalTime[i] + "\t\t" + originalBurstTime[i] + "\t\t" + completionTime[i] + "\t\t" + waitingTime[i] + "\t\t" + turnaroundTime[i]);
        }

        System.out.printf("\nAverage wait time: %.2f", (avgwt / n));
        System.out.printf("\nAverage turnaround time: %.2f", (avgta / n));

        sn.close();
    }
}


