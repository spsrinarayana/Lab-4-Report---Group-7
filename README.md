# Lab-4-Report---Group-7
The submission for the Lab 4 Report of Group 7 for MicroComputers, Y2 S1, Faculty of Engineering, SLIIT 
                    EN21475368 - Sri Narayana S. N. B. M. S. P.
                    EN21488306 - Nilaweera W. N. R. T. R.
                    EN21474132 - Rathnayaka M. H. M.

## Introduction
      In this lab, the main objective is to create a water level controlling system of a water tank using PIC16F877A.  so, in our real implementation we have used water detecting sensors, PCB, leds .....

## PCB Design
       ![image](https://user-images.githubusercontent.com/47419680/179459428-0e6e42df-8826-4595-a07d-1b5b051771f2.png)




## Image of the Real Implementation
      

## Results
## Code

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



// The PINS We Used In PIC16F877A
//      Switch 1 is connected to RB2
//      Switch 2 is connected to RB1
//      Switch 3 is connected to RB0
//      Motor 1 is connected to RC0
//      Motor 2 is connected to RC1



#include <xc.h>
#include <htc.h> 

#define _XTAL_FREQ 20000000

void __interrupt() isr(void) {      // When RB0=1 interrupt function executed
    if(RB2 == 1 && RB1 == 1) {      // also with RB0=1 other 2 switches also to be Logic high to proceed
        RC0 = 0;                    //Motor1 is turned off
        RC1 = 1;                    //Motor2 is turn on and again off after 1/2 second
        __delay_ms(500);
        RC1 = 0;        
    }
    INTF = 0;
    
}
void main(void){
    GIE = 1;            //Global Interrupt Enable
    INTE = 1;           //RB0/INT pin interrupt Enable
    INTF = 0;           //Interrupt Flag to be logic low. if interrupt pin at logic HIGH the flag become High to call interrupt
    PEIE = 1 ;          //Peripheral Interrupt Enable
    INTEDG = 1;         
    
    TRISB0 = 1;     // SWITCH 3 , set as an Input
    TRISB1 = 1;     // SWITCH 2 , set as an Input
    TRISB2 = 1;     // SWITCH 1 , set as an Input
    TRISC0 = 0;     // MOTOR 1 , set as an Output
    TRISC1 = 0;     // MOTOR 2 , set as an Output
    PORTB = 0X00;   // all portB set to Logic Low
    PORTC = 0X00;   // all portC set to Logic Low
      
    while(1){                       //Infinity While loop running inside the main Program Until Interrupt Occurs
        if(RB2 == 1 && RB0==0){     // If condition Run when Switch1 at logic High, Switch 3(RB0 surely equal to zero here inside the main fuction)
            RC0 = 1;                // if RB2=1, Motor 1 is ON and Motor 2 is off
            RC1 = 0;                //       the same combination for the both ON/OFF fro the Switch 2 (RB1),
        }else{                      // If the above combinations not true 
            RC0 = 0;                //      Both Motors are Turned off
            RC1 = 0;
        }
                                
    }
    return;
}
