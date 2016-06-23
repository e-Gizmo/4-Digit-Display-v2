# 4-Digit-Display-v2
Arduino Compatible

<http://www.e-gizmo.com/oc/index.php?route=product/product&product_id=561&search=4+digit>

![Imgur](http://i.imgur.com/S5JTymd.jpg)

#### Wiring Diagram


#### Sample Code
```arduino
/*
    4 Digit Display v 2.0
 
 This sample program functions 
 as a 4 digit 7 segment display 
 counter.
 
 Codes by:
 e-Gizmo Mechatronix Central
 http://www.e-gizmo.com
 
 */
// Data, Strobe, and clock LD1207
const int STB = 11;
const int CLK = 12;
const int DIN = 13;

char Num[10] = {0xBF, 0x06, 0xDB, 0x4F, 0xE6, 0x6D, 0xFD, 0x07, 0xFF, 0x67};	//0 

byte Digit0;
byte Digit1;
byte Digit2;
byte Digit3;

void setup() {
  pinMode(STB, OUTPUT);
  pinMode(CLK, OUTPUT);
  pinMode(DIN, OUTPUT);
  
  digitalWrite(STB, HIGH);
  digitalWrite(CLK, HIGH);
  digitalWrite(DIN, HIGH);
  
  SET_DISP_MODE();
  SET_DATA_MODE();
  DISP_OFF();
  DISP_ON();
  SET_DISP_CTRL(4 | 0x08);
}  

void loop() {
  SEND_DATA(0x00, Digit0, HIGH);
  SEND_DATA(0x02, Digit1, LOW);
  SEND_DATA(0x04, Digit2, HIGH);
  SEND_DATA(0x06, Digit3, LOW);
  delay(1000);
  Digit0++;
  Digit1++;
  Digit2++;
  Digit3++;
  SEND_DATA(0x00, Digit0, LOW);
  SEND_DATA(0x02, Digit1, HIGH);
  SEND_DATA(0x04, Digit2, LOW);
  SEND_DATA(0x06, Digit3, HIGH);
  delay(1000);
  Digit0++;
  Digit1++;
  Digit2++;
  Digit3++;
  if(Digit0 == 10) {
    Digit0 = 0;
    Digit1 = 0;
    Digit2 = 0;
    Digit3 = 0;
  }    
}

void SET_DISP_MODE(void) {
  digitalWrite(STB, LOW);  
//  WRITE_COMMAND(0x00);    //4 Grids, 12 Segments
//  WRITE_COMMAND(0x01);    //5 Grids, 11 Segments
//  WRITE_COMMAND(0x02);    //6 Grids, 10 Segments
  WRITE_BYTE(0x03);    //7 Grids, 9 Segments    (default)
  digitalWrite(STB, HIGH);  
}  

void SET_DATA_MODE(void) {
  digitalWrite(STB, LOW);  
  WRITE_BYTE(0x40);    //Increment Address after Data has been Written    (default)
//  WRITE_COMMAND(0x44);    //Fixes Address
  digitalWrite(STB, HIGH);  
}

void SEND_DATA(byte addr, byte data, boolean LED) {
  digitalWrite(STB, LOW);  
  WRITE_BYTE(0xC0 | addr);    //addr = 00H to 0DH
  WRITE_BYTE(Num[data]);
  WRITE_BIT(LED);
  digitalWrite(STB, HIGH);  
}

void SET_DISP_CTRL(byte inten) {
  digitalWrite(STB, LOW);  
  WRITE_BYTE(0x80 | inten);    //inten = 0 to 7
  digitalWrite(STB, HIGH);  
}  

void DISP_ON(void) {
  digitalWrite(STB, LOW);  
  WRITE_BYTE(0x80 | 0x08);
  digitalWrite(STB, HIGH);  
}  

void DISP_OFF(void) {
  digitalWrite(STB, LOW);  
  WRITE_BYTE(0x80 & ~0x08);
  digitalWrite(STB, HIGH);  
}  

void WRITE_BYTE(byte db_data)
{
      digitalWrite(DIN, db_data & 1);
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(DIN, db_data & 2);
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(DIN, db_data & 4);
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(DIN, db_data & 8);
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(DIN, db_data & 16);
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(DIN, db_data & 32);
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(DIN, db_data & 64);
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(DIN, db_data & 128);
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
}

void WRITE_BIT(boolean db_bit)
{
      digitalWrite(DIN, db_bit);  
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
      digitalWrite(CLK, LOW);  
      digitalWrite(CLK, HIGH);  
}


```
