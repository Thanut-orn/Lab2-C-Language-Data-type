คำถามท้ายการทดลอง
1.นักศึกษาได้เรียนรู้อะไรบ้างเกี่ยวกับความแตกต่างและคุณสมบัติของชนิดข้อมูลแต่ละประเภทบน ESP32 (เช่น int, float, char, bool, long, long long, unsigned int, byte, double)
-ขนาดหน่วยความจำ (Bytes): แต่ละชนิดใช้พื้นที่ต่างกัน (เช่น byte ใช้ 1 ไบต์, int/long/unsigned int/unsigned long ใช้ 4 ไบต์, double/long long/unsigned long long ใช้ 8 ไบต์)

-ขอบเขตของค่า (Range): แต่ละชนิดมีขีดจำกัดในการเก็บค่า (เช่น int มีทั้งบวก/ลบ, byte และ unsigned ประเภทอื่น ๆ เก็บได้เฉพาะค่าบวกแต่มีช่วงที่ใหญ่กว่าในขนาดเท่ากัน)

-ความแม่นยำ (Precision): โดยเฉพาะชนิดทศนิยม (float ให้ประมาณ 6-7 ตำแหน่ง, double บน ESP32 ให้ถึง 15-17 ตำแหน่ง)

-พฤติกรรมการคำนวณ:
 -Overflow/Underflow: ชนิดจำนวนเต็มจะ "วนกลับ" เมื่อค่าเกินขอบเขต
 -Type Promotion/Casting: การคำนวณข้ามชนิดข้อมูลจะมีการแปลงชนิดข้อมูลโดยอัตโนมัติ (promotion) หรือต้องแปลงด้วยตนเอง (casting) เพื่อให้ได้ผลลัพธ์ที่ต้องการ (เช่น การหารทศนิยม)

-การใช้งานเฉพาะ: เช่น char ใช้เก็บอักขระซึ่งสัมพันธ์กับค่า ASCII, bool ใช้เก็บค่าตรรกะ true/false

2.ความสำคัญของการเลือกใช้ชนิดข้อมูลที่เหมาะสมในการเขียนโปรแกรมคืออะไร?
-ประหยัดหน่วยความจำ: ไมโครคอนโทรลเลอร์มีหน่วยความจำจำกัด การเลือกขนาดที่เล็กที่สุดเท่าที่จำเป็นจะช่วยให้ใช้ทรัพยากรได้อย่างมีประสิทธิภาพ

-คำนวณถูกต้อง: ป้องกัน Overflow/Underflow และมั่นใจว่าผลลัพธ์ทศนิยมมีความแม่นยำตามที่ต้องการ

-โปรแกรมมีประสิทธิภาพ: ลดการแปลงข้อมูลที่ไม่จำเป็นและใช้ฮาร์ดแวร์ได้เต็มที่

-โค้ดชัดเจน: ทำให้ผู้อื่นเข้าใจเจตนาของตัวแปรได้ง่ายขึ้น

3.ถ้านักศึกษาต้องการเก็บค่าเวลา (เป็นมิลลิวินาที) ซึ่งอาจมีค่าสูงถึงหลายพันล้านมิลลิวินาที นักศึกษาควรใช้ชนิดข้อมูลใดบน ESP32
หากต้องการเก็บค่าเวลาเป็นมิลลิวินาทีที่อาจสูงถึงหลายพันล้านมิลลิวินาที ควรใช้ชนิดข้อมูล unsigned long หรือ unsigned long long บน ESP32:

-unsigned long: เพียงพอสำหรับการจับเวลาถึงประมาณ 4.2 พันล้านมิลลิวินาที (49 วัน) ซึ่งเป็นค่าที่ฟังก์ชัน millis() ส่งคืน

-unsigned long long: หากต้องการเก็บค่าที่นานกว่า 4.2 พันล้านมิลลิวินาที (หลายสิบปี) ซึ่งเป็นชนิดข้อมูล 64 บิต

4.อธิบายความแตกต่างระหว่าง float และ double ในแง่ของขนาดหน่วยความจำและความแม่นยำ
-float:
 -ขนาดหน่วยความจำ: 4 ไบต์ (32 บิต)
 -ความแม่นยำ: ประมาณ 6-7 ตำแหน่งทศนิยม (Single Precision)

-double (บน ESP32):
 -ขนาดหน่วยความจำ: 8 ไบต์ (64 บิต)
 -ความแม่นยำ: ประมาณ 15-17 ตำแหน่งทศนิยม (Double Precision) ซึ่งแม่นยำกว่า float มาก

5.อธิบายแนวคิดเรื่อง "Overflow" และ "Underflow" ที่เกิดขึ้นกับชนิดข้อมูลจำนวนเต็ม (เช่น int, byte) พร้อมยกตัวอย่างจากใบงานนี้
Overflow และ Underflow เกิดขึ้นเมื่อค่าของตัวแปรจำนวนเต็มเกินกว่าขีดจำกัดที่ชนิดข้อมูลนั้นๆ เก็บได้:
-Overflow: เมื่อค่าบวกเกินขีดจำกัดสูงสุด
ตัวอย่าง: 2147483647 + 1 (สำหรับ int) จะวนกลับไปเป็น -2147483648
ตัวอย่าง: กำหนด myByte = 256 จะทำให้ myByte เป็น 0 (เพราะเกิน 255)

-Underflow: เมื่อค่าลบต่ำกว่าขีดจำกัดต่ำสุด
ตัวอย่าง: -2147483648 - 1 (สำหรับ int) จะวนกลับไปเป็น 2147483647

6.การทราบขนาดของชนิดข้อมูลด้วย sizeof() มีประโยชน์อย่างไรในการเขียนโปรแกรมสำหรับไมโครคอนโทรลเลอร์ที่มีหน่วยความจำจำกัด?
การทราบขนาดของชนิดข้อมูลด้วย sizeof() มีประโยชน์อย่างยิ่งสำหรับไมโครคอนโทรลเลอร์ที่มีหน่วยความจำจำกัด ดังนี้:

-ประหยัดหน่วยความจำ: ช่วยให้เลือกชนิดข้อมูลที่เล็กที่สุดแต่เพียงพอต่อการใช้งาน ทำให้ประหยัด RAM และ Flash ซึ่งมีอยู่อย่างจำกัด

-ป้องกันข้อผิดพลาด: ช่วยให้เข้าใจขอบเขตของค่าที่ชนิดข้อมูลนั้นเก็บได้ เพื่อหลีกเลี่ยง Overflow/Underflow

-วางแผนการใช้ทรัพยากร: เมื่อออกแบบโครงสร้างข้อมูลหรือส่งข้อมูล จะสามารถคำนวณและวางแผนการใช้หน่วยความจำได้อย่างแม่นยำ
