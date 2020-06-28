# encoder_debounce
_This is what I use to sofware debounce the cheap rotary encoders. It seems to work pretty well so far for my applications, but I have no doubt it can be improved upon. It's based on an example I found somewhere (but can't seem to track down), but I modified it a little to suit my own needs._

```
static uint8_t prevNextCode = 0;
static uint16_t store = 0;

// Retuns 0 if invalid, otherwise -1 or 1 to indicate direction
int8_t read_rotary() 
{
  static int8_t rot_enc_table[] = {0,1,1,0,1,0,0,1,1,0,0,1,0,1,1,0};

  prevNextCode <<= 2;
  if (digitalRead(ENCODER_DT)) prevNextCode |= 0x02;
  if (digitalRead(ENCODER_CLOCK)) prevNextCode |= 0x01;
  prevNextCode &= 0x0f;

   // If valid then store as 16 bit data.
   if( rot_enc_table[prevNextCode] ) 
   {
      store <<= 4;
      store |= prevNextCode;
      if ((store&0xff)==0x2b) return -1;
      if ((store&0xff)==0x17) return 1;
   }
   return 0;
}
```
