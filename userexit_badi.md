### How to find a User Exit or BADI from debugger.

When we are trying to find a User Exit or a BADI, we can use some features of the debugger.

On top menu, we click:
Breakpoints -> Breakpoint at -> Breakpoint at Statement

![image](https://github.com/user-attachments/assets/1e3f41ff-c4c3-44cd-86e1-5b25401b576c)

On the below popup, on ABAP Cmnds 

![image](https://github.com/user-attachments/assets/e9767ae2-74a7-41a7-8fe8-8c97cb3e9e33)

We can put breakpoints on the followings statements:
- User exits: CALL CUSTOMER-FUNCTION
- BADI: CALL BADI

Also, for the BADI we can insert on the second tab 

![image](https://github.com/user-attachments/assets/c2863f2c-3f41-4aa8-a320-b81ff5acbee8)

the following class that is triggered before the call of the BADI’s.

- Class name: CL_EXITHANDLER
- Method Name: GET_INSTANCE

> [!NOTE]
> The use of CL_EXITHANDLER-GET_INSTANCE is for Classic BADI.
> 
> The use of CALL BADI is for Kernal BADI.
> 
> Classic BADI:
> * Interface name automatically generated
> * Implementing class name automatically created
> * Uses proxy class (CL_EXITHANDLER)
> * Not using Fallback Class
> * Implementation done in SE19
> 
> Kernal BADI:
> * We have to create interface
> * Implementing class name, we need to give
> * Not uses proxy class
> * Execution time less
> * Uses Fallback Class

