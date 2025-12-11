# Mini-CPU
To open .dig file, use the Digital program available at https://github.com/hneemann/Digital

**Description**

This CPU is designed by using **Digital program**. The process starts with loading a program into a **256 x 14-bit RAM** (Program RAM, *pRAM*), where the instructions begin execution from address 0x00 until a stop instruction is encountered. After execution, the CPU waits for a signal to transmit the results stored in a **256 x 8-bit RAM** (Result RAM, *rRAM*), displaying values at addresses 0x00 to 0x0F via an **8-bit output** and **two 7-segment displays**.

**valid (1 bit):** Indicates the program has been fully executed and results are ready to be sent.

**done (1 bit):** Signals completion of result transmission.

**output (8 bits):** Displays hexadecimal results stored in *rRAM* after receiving the result signal.

| Opcode | Instruction | Definition |
| :--- | :--- | :--- |
| 000000 | NOPE | ไม่ต้องทำงานอะไร ข้ามไปทำงานคำสั่งถัดไป |
| 000001 | accA ← Operand | นำค่า 8 bits ของ operand ไปเก็บไว้ใน accA |
| 000010 | accB ← Operand | นำค่า 8 bits ของ operand ไปเก็บไว้ใน accB |
| 000011 | accA ← accB | นำค่าจาก accB ไปเก็บไว้ใน accA |
| 000100 | accB ← accA | นำค่าจาก accA ไปเก็บไว้ใน accB |
| 000101 | regC ← accA | นำค่าจาก accA ไปเก็บไว้ใน regC |
| 000110 | accA ← regC | นำค่าจาก regC ไปเก็บไว้ใน accA |
| 000111 | regD ← accA | นำค่าจาก accA ไปเก็บไว้ใน regD |
| 001000 | accA ← regD | นำค่าจาก regD ไปเก็บไว้ใน accA |
| 001001 | regC ← M | นำค่าจาก input M ไปเก็บไว้ใน regC |
| 001010 | regC ← N | นำค่าจาก input N ไปเก็บไว้ใน regC |
| 001011 | regD ← M | นำค่าจาก input M ไปเก็บไว้ใน regD |
| 001100 | regD ← N | นำค่าจาก input N ไปเก็บไว้ใน regD |
| 001101 | regC ← M <br> regD ← N | นำค่าจาก input M ไปเก็บไว้ใน regC <br> นำค่าจาก input N ไปเก็บไว้ใน regD |
| 001110 | accA ← regC <br> accB ← regD | นำค่าจาก regC ไปเก็บไว้ใน accA <br> นำค่าจาก regD ไปเก็บไว้ใน accB |
| 001111 | accA ← pRAM_adr[Operand] | นำค่าจาก pRAM ใน address ที่ระบุโดย operand ไปเก็บไว้ใน accA โดยจัดเก็บเฉพาะ 8 bits ด้านขวาเท่านั้น |
| 010000 | accA ← rRAM_adr[Operand] | นำค่าจาก rRAM ใน address ที่ระบุโดย operand ไปเก็บไว้ใน accA |
| 010001 | rRAM_adr[Operand] ← accA | นำค่าจาก accA ไปเก็บไว้ใน rRAM ใน address ที่ระบุโดย operand |
| 010010 | Jump to address Operand | ข้ามไปทำคำสั่งใน address ตามที่ระบุใน Operand แบบไม่มีเงื่อนไข |
| 010011 | Jump to address Operand if eq | ข้ามไปทำคำสั่งใน address ตามที่ระบุใน Operand ถ้า equal flag เป็น 1 |
| 010100 | Jump to address Operand if gr | ข้ามไปทำคำสั่งใน address ตามที่ระบุใน Operand ถ้า greater flag เป็น 1 |
| 010101 | Jump to address Operand if le | ข้ามไปทำคำสั่งใน address ตามที่ระบุใน Operand ถ้า lesser flag เป็น 1 |
| 010110 | Jump to address Operand if eq or gr | ข้ามไปทำคำสั่งใน address ตามที่ระบุใน Operand ถ้า equal flag หรือ greater flag เป็น 1 |
| 010111 | Jump to address Operand if eq or le | ข้ามไปทำคำสั่งใน address ตามที่ระบุใน Operand ถ้า equal flag หรือ lesser flag เป็น 1 |
| 100000 | accA ← accA + accB | นำค่าจาก accA มาบวกกับ accB แล้วไปเก็บที่ accA (ไม่ต้องสนใจกรณีผลบวกเกินขอบเขต) คำนวณแบบ 2’s complement |
| 100001 | accA ← accA - accB | นำค่าจาก accA มาลบกับ accB แล้วไปเก็บที่ accA (ไม่ต้องสนใจกรณีผลลบเกินขอบเขต) คำนวณแบบ 2’s complement |
| 100010 | accA ← accA * accB | นำค่าจาก accA[3..0] มาคูณกับ accB[3..0] แล้วไปเก็บที่ accA (คิด 4 bits ดังนั้นจะมองเป็นเลขบวกอย่างเดียว) |
| 100011 | accA ← accA / accB | นำค่าจาก accA คิดแบบ binary ไม่ดู signed bit มาหารกับ accB แล้วไปเก็บที่ accA |
| 100100 | accA ← accA % accB | นำค่าจาก accA คิดแบบ binary ไม่ดู signed bit มา mod กับ accB แล้วไปเก็บที่ accA |
| 100101 | accA ← accA ^ accB | นำค่าจาก accA[2..0] มายกกำลัง accB[2..0] แล้วไปเก็บที่ accA (ไม่ต้องสนใจกรณีผลลัพธ์เกินขอบเขต) |
| 101000 | accA ← NOT(accA) | กลับบิตของ accA แล้วเก็บไว้ที่ accA |
| 101001 | accA ← accA AND accB | นำค่าจาก accA มา bitwise AND กับ accB แล้วไปเก็บที่ accA |
| 101010 | accA ← accA OR accB | นำค่าจาก accA มา bitwise OR กับ accB แล้วไปเก็บที่ accA |
| 101011 | accA ← accA XOR accB | นำค่าจาก accA มา bitwise XOR กับ accB แล้วไปเก็บที่ accA |
| 101100 | accA ← accA << accB | นำค่าจาก accA มา logical shift left ตามค่าของ accB[2..0] แล้วไปเก็บที่ accA โดย shift left ไม่เกิน 7 bits |
| 101101 | accA ← accA <<< accB | นำค่าจาก accA มา rotate logical shift left ตามค่าของ accB[2..0] แล้วไปเก็บที่ accA โดย shift left ไม่เกิน 7 bits |
| 101110 | accA ← accA >> accB | นำค่าจาก accA มา logical shift right ตามค่าของ accB[2..0] แล้วไปเก็บที่ accA โดย shift right ไม่เกิน 7 bits |
| 101111 | accA ← accA >>> accB | นำค่าจาก accA มา rotate logical shift right ตามค่าของ accB[2..0] แล้วไปเก็บที่ accA โดย shift right ไม่เกิน 7 bits |
| 110000 | accA CMP accB | เทียบค่า accA กับ accB คำนวณแบบ 2’s complement <br> - ถ้า accA == accB ค่า equal flag จะเป็น 1 <br> - ถ้า accA > accB ค่า greater flag จะเป็น 1 <br> - ถ้า accA < accB ค่า lesser flag จะเป็น 1 |
| 110001 | isPrime(accA) | ตรวจสอบว่า accA เป็นจำนวนเฉพาะหรือไม่ <br> - ถ้า accA เป็นจำนวนเฉพาะ ค่า equal flag จะเป็น 1 <br> - กรณีนี้ไม่กระทบกับ greater flag และ lesser flag <br> ให้คิด accA แบบเลขฐาน 2 ปกติ ไม่มีเลขลบ |
| 110010 | accB \| accA | ทดสอบว่า accA ที่เป็นตัวตั้งหารด้วย accB ลงตัวหรือไม่ <br> - ถ้าลงตัวให้ equal flag เป็น 1 <br> - กรณีนี้ไม่กระทบกับ greater flag และ lesser flag <br> ให้คิด accA และ accB แบบเลขฐาน 2 ปกติ ไม่มีเลขลบ <br> เช่น accA = 20 และ accB = 5 จะได้ว่า 5\|20 คือ 20 หารด้วย 5 ลงตัว จะทำให้ equal flag เป็น 1 |
| 110011 | rRAM[0x0E:0x0F] ← LCM(...) | คำนวณหาค่าคูณร่วมน้อย LCM(m,n) โดย <br> m คือค่าใน rRAM ตำแหน่งตาม operand[7:4] <br> n คือค่าใน rRAM ตำแหน่งตาม operand[3:0] <br> ให้คิดตัวเลขแบบ binary เป็นจำนวนเต็มบวกเท่านั้น <br> นำผลลัพธ์ที่ได้เก็บไว้ที่ rRAM[0x0E] และ rRAM[0x0F] <br> โดยค่า most significant บิต result[15:8] เก็บที่ rRAM[0x0E] <br> โดยค่า least significant บิต result[7:0] เก็บที่ rRAM[0x0F] |
| 110100 | rRAM[0x0A:0x0B] ← FAC(...) | คำนวณหา Factorial(n) โดย n คือค่าใน accA เฉพาะบิต [2:0] ดังนั้นค่าของ n จะอยู่ระหว่าง 0 - 7 เท่านั้น <br> นำผลลัพธ์ที่ได้เก็บไว้ที่ rRAM[0x0A] และ rRAM[0x0B] <br> โดยค่า most significant บิต result[15:8] เก็บที่ rRAM[0x0A] <br> โดยค่า least significant บิต result[7:0] เก็บที่ rRAM[0x0B] <br> (ในคำสั่งนี้ให้ใช้ การวนลูปผ่าน ASM ไม่อนุญาตให้ใช้ ROM) |
| 110101 | rRAM[0x09] ← max( pRAM[accA:accB] ) | คำนวณหาค่าที่มากที่สุด ของค่าใน pRAM[7:0] (คิดเฉพาะ 8 บิตด้านขวาเท่านั้น) ตำแหน่งที่ระบุโดย accA ถึง accB <br> นำคำตอบ ที่ได้ไปเก็บที่ rRAM[0x09] ให้ใช้การเปรียบเทียบแบบ 2’s complement <br> (ในกรณีนี้โจทย์จะตั้งให้ accA <= accB เสมอ) |
| 111111 | STOP | หยุดการทำงาน ไม่ต้องทำคำสั่งถัดไป แล้วรอสัญญาณ result |

**This project was developed as part of the CEDT Final Project 2025: Digital Computer Logic (2110252)**
