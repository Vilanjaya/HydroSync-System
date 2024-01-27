// LCD module connections
sbit LCD_RS at RB2_bit;
sbit LCD_EN at RB3_bit;
sbit LCD_D4 at RB4_bit;
sbit LCD_D5 at RB5_bit;
sbit LCD_D6 at RB6_bit;
sbit LCD_D7 at RB7_bit;

sbit LCD_RS_Direction at TRISB2_bit;
sbit LCD_EN_Direction at TRISB3_bit;
sbit LCD_D4_Direction at TRISB4_bit;
sbit LCD_D5_Direction at TRISB5_bit;
sbit LCD_D6_Direction at TRISB6_bit;
sbit LCD_D7_Direction at TRISB7_bit;

// Text messages
char txt1[] = "Water";
char txt2[] = "Level";
char txt3[] = "Indicator";
char txt4[] = "And Controller";
char mtr1[] = "Motor ";
char mtr2[] = "OFF";
char mtr3[] = "ON";
char wtr1[] = "Level: ";
char wtr2[] = "Very Low";
char wtr3[] = "Low";
char wtr4[] = "Medium";
char wtr5[] = "High";
char wtr6[] = "Full";

void main() {
    int i = 0;
    int c = 16;
    int b = 0;

    // Configuration
    CMCON = 0x07;
    ADCON1 = 0x06;
    TRISA = 0x0F; // Set direction to input
    PORTA = 0x00;
    PORTD = 0x00;
    PORTC = 0x00;
    TRISB = 0x00; // Set direction to output
    TRISC = 0x00; // Set direction to output
    TRISD = 0x80; // Set direction to output

    PORTD.F2 = 1;
    PORTD.F7 = 1;

    Lcd_Init(); // Initialize LCD
    Lcd_Cmd(_LCD_CLEAR); // Clear display
    Lcd_Cmd(_LCD_CURSOR_OFF); // Cursor off
    Lcd_Out(1, 1, txt1); // Write text in the first row
    Lcd_Out(2, 1, txt2); // Write text in the second row
    Delay_ms(500);

    Lcd_Cmd(_LCD_CLEAR); // Clear display
    Lcd_Out(1, 1, txt3); // Write text in the first row
    Lcd_Out(2, 1, txt4); // Write text in the second row
    Delay_ms(500);

    // Moving text
    for (i = 0; i < 15; i++) {
        Lcd_Cmd(_LCD_SHIFT_RIGHT);
        Delay_ms(125);
    }

    i = 0; // Motor Status OFF

    do {
        Lcd_Cmd(_LCD_CLEAR);
        Lcd_Out(1, 1, wtr1);
        Lcd_Out(2, 1, mtr1);

        if (c > 0) {
            PORTD.F2 = 1; // LCD Backlight ON
            c--;
        } else {
            PORTD.F2 = 0; // LCD Backlight OFF
        }

        if (b > 0) {
            PORTD.F0 = 1; // Buzzer ON
            Delay_ms(125);
            PORTD.F0 = 0; // Buzzer OFF
            b--;
        }

        if (PORTD.F7 == 0) // Manual Backlight ON
            c = 16;

        if (PORTA == 0x0F) {
            PORTD.F1 = 1;
            Lcd_Out(1, 8, wtr2);
            Lcd_Out(2, 7, mtr3);
            PORTC = 1;

            if (i == 0) {
                c = 16; // Backlight
                b = 3; // Buzzer
            }

            i = 1;
        } else if (PORTA == 0x0E) {
            Lcd_Out(1, 8, wtr3);

            if (i == 1)
                Lcd_Out(2, 7, mtr3);
            else
                Lcd_Out(2, 7, mtr2);

            PORTC = 3; // LED Bar
        } else if (PORTA == 0x0C) {
            Lcd_Out(1, 8, wtr4);

            if (i == 1)
                Lcd_Out(2, 7, mtr3);
            else
                Lcd_Out(2, 7, mtr2);

            PORTC = 7; // LED Bar
        } else if (PORTA == 0x08) {
            Lcd_Out(1, 8, wtr5);

            if (i == 1)
                Lcd_Out(2, 7, mtr3);
            else
                Lcd_Out(2, 7, mtr2);

            PORTC = 15; // LED Bar
        } else if (PORTA == 0x00) {
            Lcd_Out(1, 8, wtr6);
            Lcd_Out(2, 7, mtr2);
            PORTD.F1 = 0; // Motor OFF

            if (i == 1) {
                c = 16; // Backlight
                b = 3; // Buzzer
            }

            i = 0; // Motor Status Updated
            PORTC = 31; // LED Bar
        } else {
            PORTA = 0x0F;
        }

        Delay_ms(125);

    } while (1); // Endless loop
}
