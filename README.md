//Exp_1  LED_BLINK


#include<reg51.h>
#define Del 3000
sfr LED_PORT2=0xA0;

	void delay(unsigned int x)
	{
	unsigned int i,j;
	for(i=0;i<x;i++)
	
	for(j=0;j<=100;j++);
	
	}
	
	void main(void)
	{
	while(1)			     // do it continously
	{
	LED_PORT2=0xff;		 //LED ON
	delay(Del);
	LED_PORT2=0x00;     //LED OFF
	delay(Del);
	}
	}

EXP 1B
BCD COUNTER

#include<reg51.h>
#define Del 2000
void delay(unsigned int x) // delay function
{
unsigned int i,j;
for(i=0;i<x;i++)
for(j=0;j<=100;j++);
}
void main(void) {
unsigned char count[10]={0xff,0xfe,0xfd,0xfc,0xfb,0xfa,0xf9,0xf8,0xf7,0xf6};
unsigned int x;
P1=0x00; // Make P1 as output port
while(1) // do it continuosly
{
for(x=0;x<10;x++)
{
P1=count[x];
delay(Del);
}
}
}


EXP 1C
HEX COUNTER


#include <reg51.h>
# define del 2000
void delay (unsigned int x)//delay function {
unsigned int i,j;
for(i=0; i<=x; i++)
for(j=0;j<=100;j++);
}
void main(void)
{
unsigned char count[16] =
{0XC0,0XF9,0XA4,0XB0,0X99,0X92,0X82,0XF8,0X80,0X90,0X88,0X83,0XC6,0XA1,0X86,0X8E};
unsigned int x;
P1 =0X00;
while(1)
{
for(x=0 ; x<16 ;x++)
{
p1 = count[x];
delay(del);
}
}
} 

Exp_2
//Seven segment  display

#include<reg51.h>
#define Del 500
void delay(unsigned int x)
// delay function
{
unsigned int i;
for(i=0;i<x;i++);
}
void main(void)
{
while(1)
{
P2=0xeb;// display1
P3=0x7F;
delay(Del); P2=0x4c; // display 2
P3=0xbf;
delay(Del);
P2=0x49;// display 3
P3=0xdf;
delay(Del);
P2=0x2b;// display 4
P3=0xef;
delay(Del);// do it continuosly
}
} 

Exp_3
//Square waveform generation
#include<reg51.h>
void delay()
{
inti,j;
for(i=0;i<100;i++)
for(j=0;j<100;j++);
}
void main()
{
while(1)
{
P2=0x00; // logic0 of sqaure wave
delay(); P2=0x0ff; // logic 1 of sqaure wave
delay();
}
}



//Triangular waveform generation
#include< reg51.h>
unsigned char d;
void main(void)
{
while(1)
 {
for(d=0; d<255; d++)
 { P2 = d;
 }
for(d=255; d>0; d--)
 {
 P2 = d;
 }
 }
} 

Exp_4

//Stepper Motor
#include <reg51.h>
void delay(unsigned char);
void main()
{
while(1)
{
P2=0x0C;
delay(100);
P2=0x06;
delay(100);
P2=0x03;
delay(100);
P2=0x09;
delay(100);
}
}

void delay(unsigned char del)
{
unsigned int i,j;
for(i=0;i<1275;i++)
 for(j=0;j<del;j++);
}

Exp_5

//Internal memory organisation
//RAM -RAM
ORG 0000H
MOV R0, #20H
MOV R1, #40H
MOV R2, #05H
BACK: MOV A, @R0
MOV @R1, A
INC R0
INC R1
DJNZ R2, BACK
END 



//RAM-EX RAM


ORG 0000H
MOV R0, #20H
MOV DPTR, #040H
MOV R2, #05H
BACK: MOV A, @R0
MOVX @DPTR, A
INC R0
INC DPTR
DJNZ R2, BACK
END 


//ROM- RAM


ORG 0000H
MOV R0, #20H
MOV R1, #05H 
MOV DPTR, #0234H
BACK: CLR A
MOVC A, @A+DPTR
MOV @R0, A
INC R0
INC DPTR
DJNZ R1, BACK
ORG 0234H
DB 2H, 4H, 6H, 8H, 10H
END 


EXP_6

//INTERFACING PIC18F WITH BUZZER LED RELAY

#include <stdio.h>

#include <stdlib.h>

#include<PIC18F4550.h>


void delay()

{
unsigned int j;

for(j=0; j<30000; j++)

{

}

}


void main()

{

unsigned int key, i;

key=0;


TRISB=0x00;

TRISDbits.RD2=0;

TRISAbits.RA4=0;

TRISDbits.RD0=1;

TRISDbits.RD1=1;


while(1)

{

if(PORTDbits.RD0==0)

{

key=0;

}

if(PORTDbits.RD1==0)

{
key=1;
}
if(key==0)
{
LATDbits.LATD2=0;
LATAbits.LATA4=0;
for(i=0;i<8;i++)
{
LATB=0x01<<i;
delay();
}
}
if(key==1)
{
LATDbits.LATD2=1;
LATAbits.LATA4=1;
for(i=0;i<8;i++)
{
LATB=0x80>>i;
delay();
}
}
}
}


EXP_7(A)

//LCD 8 BIT

#include <stdio.h>
#include <stdlib.h>
#include<pic18f4550.h>

#define RS LATCbits.LATC0
#define E LATCbits.LATC1
#define LCDPORT LATB

void delay()
{
for(int i=0; i<30000; i++)
{
}
}

void sendCommand(unsigned char command)
{
LCDPORT=command;
RS=0;
delay();
E=1;
delay();
E=0;
delay();
}
void sendData(unsigned char data)
{
LCDPORT=data;
RS=1;
delay();
E=1;
delay();
E=0;
delay();
}

void main()
{
TRISB=0x00;
TRISCbits.RC0=0;
TRISCbits.RC1=0;
sendCommand(0x38);
sendCommand(0x01);
sendCommand(0x0F);
sendCommand(0x06);
sendCommand(0x80);
sendData('E');
sendData('&');
sendData('T');
sendData('C');
sendData(' ');
sendData('S');
sendData('C');
sendData('O');
sendData('E');
sendCommand(0xC0);
sendData('P');
sendData('U');
sendData('N');
sendData('E');
sendData('-');
sendData('4');
sendData('1');
}


EXP_8
//TIMER_INTEERUPT

#include<p18f4550.h>

void timerInit(void)
{
T0CON = 0b00000111;
TMR0H = 0xED;
TMR0L = 0xB0;
}

void Interrupt_Init(void)
{
IPEN = 1;
INTCON = 0b11100000;
TMR0IP = 0;
}

void interrupt low_priority timerinterrupt(void)
{
if(TMR0IF == 1)
{
TMR0ON = 0;
TMR0IF = 0;
LATB =~LATB;
TMR0H = 0xED;
TMR0L = 0xB0;
TMR0ON = 1;
}
}

void main(void)
{
TRISB = 0x00;
LATB = 0xFF;

Interrupt_Init();
timerInit();
TMR0ON = 1;
while(1);
}


EXP_9
// SERIAL COMMUNICATION CODE
#include<p18F4550.h>
#include<stdio.h>

void InitUART()
{
TRISCbits.RC6 = 0;                        //TX pin set as output
TRISCbits.RC7 = 1;                        //RX pin set as input

SPBRG = 77;

TXSTA = 0b00100000;
RCSTA = 0b10010000;
}

void SendChar(unsigned char data)
{
while(TXIF == 0);
TXREG = data;
}

unsigned char GetChar(void)
{
while(!RCIF);
return RCREG;
}

void main(void)
{
unsigned char key;
InitUART();

while(1)
{
key=GetChar();
SendChar(key);
}
}



Experiment no . 10
//Generation of PWM signal for DC motor control.

#include<stdio.h>
#include <p18f4550.h>
#define RS LATCbits.LATC0
#define E LATCbits.LATC1
#define LCDPORT LATB
void delay()
{
unsigned int i;
for(i=0;i<30000;i++)
{
}
}
void sendCommand(unsigned char command)
{
LCDPORT=command;
delay();
RS=0;
delay();
E=1;
delay();
E=0;
delay();
}
void sendData(unsigned char data)
{
LCDPORT=data;
delay();
RS=1;
delay();
E=1;
delay();
E=0;
delay();
}
void InitLCD(void)
{
sendCommand(0x38);
sendCommand(0x01);
sendCommand(0x0F);
sendCommand(0x06);
}
void ADCInit()
{
TRISEbits.RE2 = 1;
ADCON0 = 0b00011101;
ADCON1 = 0b00000111;
ADCON2 = 0b10101110;
}
unsigned short Read_ADC()
{
GODONE = 1;
while(GODONE == 1 );
return ADRES;
}
void DisplayResult(unsigned short ADCVal)
{
unsigned char i,text[16];
unsigned short tempv;
tempv = ADCVal;
ADCVal = (5500/1024)*tempv;
sprintf(text,"%04dmv",ADCVal);
sendCommand(0x80);
for(i=0;i<6;i++)
{
sendData(text[i]);
}
sendCommand(0xC0);
for(i=0;i<10;i++)
{
if(tempv & 0x200)
{
sendData('1');
}
else
{
sendData('0');
}
tempv=tempv<<1;
}
}
void main()
{
unsigned short Ch_result;
TRISB = 0x00;
TRISCbits.RC0 = 0;
TRISCbits.RC1 = 0;
ADCInit();
InitLCD();
while(1)
{
Ch_result = Read_ADC();
DisplayResult(Ch_result);
delay();
delay();
}
}
