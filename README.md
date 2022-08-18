![1](https://user-images.githubusercontent.com/111442374/185382815-cbef3fd2-deae-4a71-97ba-4ee033150922.jpg)
![2](https://user-images.githubusercontent.com/111442374/185382825-fcd9d932-fbc4-4c60-a1ed-603adaca7c6e.jpg)
![3](https://user-images.githubusercontent.com/111442374/185382830-8ab86b8d-5061-4624-bc3e-6c8ae31b8178.jpg)
![4](https://user-images.githubusercontent.com/111442374/185382832-7f6bd755-7169-4f04-bc41-98a0a175bcff.jpg)
![5](https://user-images.githubusercontent.com/111442374/185382843-eba70d4c-3f55-49bd-b0cd-90e615357bc8.jpg)
![6](https://user-images.githubusercontent.com/111442374/185382849-8971a485-9a38-4f1b-85e7-267e1ea1db4c.jpg)
![7](https://user-images.githubusercontent.com/111442374/185382858-420f6c82-ec08-4251-aa8e-76c0ef952c0d.jpg)
![8](https://user-images.githubusercontent.com/111442374/185382864-e2c6a62b-9936-4fc0-9056-4f7309cb1976.jpg)
![9](https://user-images.githubusercontent.com/111442374/185382872-678af3ca-dea7-40e6-8d5c-66dd11e70cd9.jpg)
![10](https://user-images.githubusercontent.com/111442374/185382877-cea113f9-0f31-4b0e-ab78-469fbfebad5d.jpg)
![11](https://user-images.githubusercontent.com/111442374/185382775-c904252f-5c09-4401-86e3-764b02b3593b.jpg)
![12](https://user-images.githubusercontent.com/111442374/185382782-04ef5e3e-1ab1-4ba6-8da1-eb00a810db3f.jpg)
![13](https://user-images.githubusercontent.com/111442374/185382790-05510b6d-91e9-4598-a51e-997ed0ce4603.jpg)
![14](https://user-images.githubusercontent.com/111442374/185382794-d762b8cc-7e4b-4f40-a4c8-473572cc426b.jpg)
![15](https://user-images.githubusercontent.com/111442374/185382801-45e5cf5d-7ed0-4fcf-9744-c8b35bab9446.jpg)
![16](https://user-images.githubusercontent.com/111442374/185382807-7b3af5b2-c7f0-4069-8202-5f0ea37bf46e.jpg)

####Micro C code for this Assignment

// PIC16F877A Configuration Bit Settings

// 'C' source line config statements

// CONFIG

#pragma config FOSC = HS        // Oscillator Selection bits (HS oscillator)

#pragma config WDTE = OFF       // Watchdog Timer Enable bit (WDT disabled)

#pragma config PWRTE = OFF      // Power-up Timer Enable bit (PWRT disabled)

#pragma config BOREN = OFF      // Brown-out Reset Enable bit (BOR disabled)

#pragma config LVP = OFF        // Low-Voltage (Single-Supply) In-Circuit Serial Programming Enable bit (RB3 is digital I/O, HV on MCLR must be used for programming)

#pragma config CPD = OFF        // Data EEPROM Memory Code Protection bit (Data EEPROM code protection off)

#pragma config WRT = OFF        // Flash Program Memory Write Enable bits (Write protection off; all program memory may be written to by EECON control)

#pragma config CP = OFF         // Flash Program Memory Code Protection bit (Code protection off)

// #pragma config statements should precede project file includes.

// Use project enums instead of #define for ON and OFF.

#define  _XTAL_FREQ 20000000

#include <xc.h>

void __interrupt() isr(void) //ISR

{

    if (INTF==1) //Check if the interrupt is On
    
    {
    
    INTF=0; //Clear the interrupt
    
    if(RB2==0 && RB1==0){
      
    RC2=0; // Motor 1 is OFFF
    
    RC1=1; // Motor 2 is ON
    
    __delay_ms(250);  //MOTOR 2 is kept ON for 500ms
    
    RC1=0;    //Motor 2 is off
    
     }
     
    }
    
}

 void main(void)
 
{
    
 TRISB0 = 1; //Pin 0 of the PortB is the SWITCH 3
 
 TRISB1 = 1; //Pin 1 of the PortB is the SWITCH 2
 
 TRISB2 = 1; //Pin 2 of the PortB is the SWITCH 1
 
 TRISC2 = 0; //OUTPUT for MOTOR 1
 
 TRISC1 = 0; //Output for MOTOR 2
 
 INTF = 0; 
 
 RC2=0; //MOTOR 1 OFF
 
 //OPTION_REG = 0b00000000;
 
 GIE=1;   //Enable global interrupt-Enables all unmasked interrupt
 
 PEIE=1;  //Peripheral Interrupt enable-Enables all unmasked peripheral interrupts
 
 INTE =1; //enable RB0 interrupt -  Enables the RB0 external interrupt
 
 while(1)
 
 {
 
 RC1=0; //MOTOR 2 OFF
 
 if (RB2==0 && RB1==1 && RB0==1 )
 
 {
 
    RC2=1; //Turn ON Motor 1
    
 } else if(RB2==0 && RB1==0 && RB0==1)
 
 {
 
   RC2=1;  //Turn ON Motor 1 
   
 } else {
 
    RC2=0;  //Turn OFF Motor 1 since there is no condition than above, other than the interrupt 
    
 }
  
}

 return; 
 
}
