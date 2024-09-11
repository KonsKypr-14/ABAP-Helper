### Watchpoint - What is it and how to activate it.

Another useful option is the Watchpoint button.

When debugging ABAP code, you can use watchpoints to track the value of individual ABAP variables.

The ABAP debugger stops as soon as the value of a watched variable has changed. Furthermore, you can specify conditions for watchpoints. The debugger then additionally checks whether this condition is fulfilled.

For example. We have a LOOP, and we want to know when the SY-TABIX reach 100.

1. Click on Watchpoint button
![image](https://github.com/user-attachments/assets/2d5f6f80-78b9-4614-a531-e81214620d8d)

2. On the field Variable insert the variable that you want to check.
![image](https://github.com/user-attachments/assets/ca6c2a4f-8e8e-4ea8-8af1-b9787ab407f3)

   a) If you do not insert Free Condition Entry, then it will stop every time the value is changed.

   b) If you insert Free Condition Entry, then it will stop only when this condition is fulfilled.

3. Enter the condition on the bottom.

![image](https://github.com/user-attachments/assets/c7192efe-969d-4ac5-907b-f71856ff1853)

4. If the watchpoint is valid inside the scope of the Program Name, a success message will appear.
![image](https://github.com/user-attachments/assets/426d6ae7-08aa-4c22-bcf7-d94c1bc0a166)

5. After running the report, if the condition is fulfilled, the debugger will stop after the value reached.
![image](https://github.com/user-attachments/assets/39cd7e1f-c621-46e8-916f-43f58bcac94e)
