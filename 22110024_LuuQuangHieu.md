# Lab #1,22110024, Luu Quang Hieu, INSE330380E_03FIE
# Task 1: Software buffer overflow attack

**Question 1**: 
- Compile both C programs and shellcode to executable code. 
- Conduct the attack so that when C executable code runs, shellcode willc also be triggered. 
  You are free to choose Code Injection or Environment Variable approach to do. 
- Write step-by-step explanation and clearly comment on instructions and screenshots that you have made to successfully accomplished the attack.

**Answer 1**:
## 1. Compile both C programs and shellcode to executable code
- First, we create two file C (vuln.c and shell.c) to start:
  
  ![image](https://github.com/user-attachments/assets/e341e894-a03a-491d-9aa1-41f64ffd7f6c)

- Second, Run the Docker to run the img4lab, we use `$> docker build -t img4lab .` and then we run it by `$> docker run -it --privileged -v $HOME/Seclabs:/home/seed/seclabs img4lab`to run Dockerfile.

![image](https://github.com/user-attachments/assets/c69f6435-7ccd-41cf-8bd6-a0637ede3155)

- After run the docker file, we use `$> gcc vuln.c -o vuln.out -fno-stack-protector -z execstack -mpreferred-stack-boundary=2` and `$> gcc shell.c -o shell.out -fno-stack-protector -z execstack -mpreferred-stack-boundary=2` to compile the vuln.c and shell.c to executable code

![image](https://github.com/user-attachments/assets/94640075-1bec-40c9-8461-efa69d9a8da5)


## 2. Conduct the attack so that when C executable code runs, shellcode willc also be triggered. You are free to choose Code Injection or Environment Variable approach to do.

- Step 1 : Load vuln.out in gdb by `$> gdb vuln.out`
  
 ![image](https://github.com/user-attachments/assets/fcd5b11b-0baa-465c-ae4b-1558dc4e7b56)

- Step 2 : Use `gdb-peda$ disas main` to view all the destination and then set the breakpoint after the strcpy instruction by `gdb-peda$ b * the_destination`
   
 ![image](https://github.com/user-attachments/assets/6f574153-19ef-4c52-a8a4-2e6ade8b08c2)

- Step 3 : Run program in gdb with injecting argument. We insert the hex code of shell.c into the argument and make the buffer overflow by `A*16`

![image](https://github.com/user-attachments/assets/802113c4-f110-4633-8d18-2b252d6a5a2d)

![image](https://github.com/user-attachments/assets/57725192-53a2-476b-8cde-ad1cb267c525)

- Step 4:  Watch the stack memory from esp (stack top) and Set Identify the return address while watching out the stack, then replace \xff\xff\xff\xff with the value of esp by `gdb-peda$ set *0xffffd728 = 0xffffd720`

![image](https://github.com/user-attachments/assets/0b59a844-a41b-4bcf-9c6a-3c2e4f29ec94)

- Step 5 : Continue executing program by `gdb-peda$ continue`.

![image](https://github.com/user-attachments/assets/2697dd9d-57cd-4cd8-811c-6bc0bc812679)

- Step 6: Then quit the gdb.

![image](https://github.com/user-attachments/assets/63b8ba4d-8e5b-43ca-b547-0e459c0539a9)


# Task 2. Attack on the database of bWapp 
- Install bWapp (refer to quang-ute/Security-labs/Web-security) by `docker pull raesene/bwapp` and `docker pull raesene/bwapp`
  ![image](https://github.com/user-attachments/assets/bb70d882-3b1c-42ac-be84-07b9d773f681)
  
  ![image](https://github.com/user-attachments/assets/0b35adfb-e1f7-4074-a400-7890024953b1)

And then Enter localhost:8025/install.php on the browser address bar to initialize the database.
You can now login with credential: username: bee, password: bug

![image](https://github.com/user-attachments/assets/9fad052a-74e3-4dd7-9468-0a888687b0f0)

- Install sqlmap by  `git clone https://github.com/sqlmapproject/sqlmap.git`

![image](https://github.com/user-attachments/assets/d3aa30de-4382-4a98-8206-a54f670fc3d9)

- Write instructions and screenshots in the answer sections. Strictly follow the below structure for your writeup.

**Question 1**: Use sqlmap to get information about all available databases 

**Answer 1**:
- Step 1 : Use `python sqlmap.py -u "http://localhost:8025/portal.php?id=1" --dbs --level=3 --risk=3` to perform an SQL Injection attack to detect and list databases on a specific website.
  
![image](https://github.com/user-attachments/assets/4dc5402b-bcc5-408f-aa5e-497926c3f973)

Get the cookies from local Web : vcsq4ms2lgmv1fd55p5iqnfmi7

![image](https://github.com/user-attachments/assets/39f3ff2b-4136-4675-93ec-7eb4c2d36204)




