# การทดลองที่ 2 เรื่อง การเตรียมสภาพแวดล้อมสำหรับการพัฒนา Web Application ด้วย Django
## วัตถุประสงค์เชิงพฤติกรรม
เพื่อให้ผู้เรียนสามารถ
  1. ใช้งานคำสั่งพื้นฐานเพื่อติดตั้งซอฟต์แวร์บนระบบปฏิบัติการลินุกซ์ได้อย่างถูกต้อง
  2. ติดตั้งซอฟต์แวร์ที่จำเป็นสำหรับการพัฒนา Web App ด้วย Django ได้ครบถ้วน
  3. สร้าง virtual environment สำหรับการพัฒนา Web App ด้วย Django ได้อย่างถูกต้อง
  4. ลบ virtual environment ที่ไม่ต้องการใช้งานได้ตามที่กำหนด
  5. ใช้งาน virtual environment เพื่อการพัฒนา Web App ด้วย Django ได้อย่างถูกต้อง
## ทฤษฎีที่เกี่ยวข้อง
### 1. ความรู้เบื้องต้นเกี่ยวกับ Django
#### 1.1 Django คืออะไร?
Django (อ่านว่า แจง-โก้ JANG-oh) ([ฟังคลิปเสียงได้ที่นี่](http://red-bean.com/~adrian/django_pronunciation.mp3)) เป็น Software framework ที่ใช้สำหรับการพัฒนาโปรแกรมประยุกต์ที่ทำงานบนเว็บ (Web Application Framework) โดยตั้งชื่อตามนักกีต้าร์แจส ชื่อ [Django Reinhardt](https://en.wikipedia.org/wiki/Django_Reinhardt)

Django เริ่มต้นพัฒนาในปีค.ศ. 2003 โดยนักพัฒนา 2 คน (Adrian Holovaty และ Simon Willison) ซึ่งทำงานในทีม World Online ของหนังสือพิมพ์ Lawrence Journal-World สำหรับใช้ในการสร้างและดูแลเว็บไซต์ข่าวออนไลน์ท้องถิ่นหลายเว็บไซต์ เพื่อให้สามารถปรับปรุงข่าวต่างๆ ได้ทันสถานการณ์ และในปี 2005 World Online ได้เปิดเผยรหัสต้นฉบับ Django เนื่องจาก Django เองก็พัฒนามาได้โดยอาศัยซอฟต์แวร์เปิดเผยรหัสต้นฉบับหลายๆ ตัว เช่น Apache, Python, และ PostgrsSQL เป็นต้น

ปัจจุบัน Django ดูแลโดย Django Software Foundation (DSF) ซึ่งได้รับเงินสนับสนุนจากบริษัทต่างๆ (Ref : https://www.djangoproject.com/foundation/corporate-members/)
#### 1.2 Design Pattern ของ Django
Design Pattern ของ Django จะอ้างอิงมาจาก Model-View-Control (MVC) Design Pattern

[รูปที่ 2.1 MVC Design Pattern](https://drive.google.com/file/d/14bv7WV3eqhET5hs6PFhs2sl7t90iDRhK/view?usp=sharing)

  * Model : ส่วนที่ใช้เชื่อมต่อกับฐานข้อมูลที่เก็บข้อมูลต่างๆ สำหรับการทำงานของ application
  * View : ส่วนที่ทำหน้าแสดงผลลัพธ์จากข้อมูลที่ได้รับจาก Model (โดยผ่านทาง Controller) นอกจากนี้ View ยังสามารถตรวจสอบการเปลี่ยนแปลงสถานะของข้อมูลใน Model ได้โดยตรงผ่านกลไกที่เรียกว่า Observer อีกด้วย
  * Controller : เป็นตัวกลางระหว่าง Model และ View โดยมีหน้าที่ในการประมวลผล request ที่เข้ามาจากผู้ใช้ (CRUD : Creat-Read-Update-Delete) และติดต่อกับ Model เพื่อนำข้อมูลตามที่ request ส่งให้กับ View เพื่อแสดงผล

สำหรับ Django นั้นจะมีชื่อเรียก Design Pattern ที่ใช้ในการออกแบบว่า Model-Template-View (MTV) โดยในส่วนของ C : Controller นั้นจะถูกจัดการโดยตัว framework เอง (ผู้ใช้ไม่มีส่วนจัดการโดยตรง)
  * Model : มีหน้าที่เช่นเดียวกับ Model ใน MVC Design Pattern แต่จะเข้าถึงฐานข้อมูลโดยใช้ Object-Relational Mapping (ORM)
  * Template : ทำหน้าที่แสดงผลให้กับผู้ใช้งาน ในรูปแบบ HTML โดยการรวม HTML ที่เตรียมไว้เข้ากับข้อมูลที่ได้จาก Model
  * View : ทำหน้าที่ประมวลผลข้อมูลต่างๆ ของ application และจัดการการสื่อสารข้อความต่างๆ โดย View จะรับ user input ซึ่งก็คือ HTTP request แล้วเข้าถึง Model เพื่อนำข้อมุลส่งต่อไปให้แก่ Template

[รูปที่ 2.2 MTV Design Pattern](https://drive.google.com/file/d/1P5qAdhWN2DjuWqDVjWpPKdgsrSC_27DW/view?usp=sharing)

สำหรับการนำ Django มาใช้งานนั้น ควรเลือกใช้รุ่นที่เป็น LTS (Long Term Support) เนื่องจากจะได้รับการสนับสนุนเป็นเวลา 28 เดือนจาก DSF

[รูปที่ 2.3 เวอร์ชันของ Django และระยะเวลาการสนับสนุน](https://static.djangoproject.com/img/release-roadmap.688d8d65db0b.png)

เมื่อพิจารณาจากรูปที่ 2.3 ในการทดลองนี้จะเลือกใช้ Django version ล่าสุด คือ 2.2 ซึ่งจะต้องใช้ Python version 3.5 ขึ้นไป (Ref : https://docs.djangoproject.com/en/2.2/faq/install/#faq-python-version-support)
### 2. ซอฟต์แวร์ต่างๆ ที่จำเป็น
#### 2.1 Python
Python เป็นภาษาโปรแกรมเชิงวัตถุ (Object-Oriented Programming : OOP) ถูกพัฒนาขึ้นโดย Guido van Rossom เมื่อปีค.ศ. 1989 และได้มีการพัฒนามาอย่างต่อเนื่อง ปัจจุบัน Python อยู่ภายใต้ Python Software Foundation (PSF) (Ref : https://docs.python.org/2.0/ref/node92.html)

การทำงานของภาษา Python จะทำงานโดยผ่าน Interpreter (ตัวแปรภาษารูปแบบหนึ่ง) ซึ่งคุณลักษณะเด่นที่สำคัญของ Python มีดังนี้
  * ใช้ syntax ที่สวยงามทำให้โปรแกรมที่เขียนอ่านได้ง่าย
  * ใช้งานได้ง่ายทำให้สามารถสร้างโปรแกรมได้ง่าย จึงเหมาะกับการพัฒนาต้นแบบ (prototype development)
  * มี library จำนวนมากที่สนับสนุนการทำงานพื้นฐานต่างๆ ในการเขียนโปรแกรม เช่น การค้นหาตัวอักษร การเชื่อมต่อกับเว็บเซิฟร์เวอร์ และการอ่าน-เขียนไฟล์ เป็นต้น
  * มี Interactive mode ทำให้ง่ายต่อการทดสอบการทำงานของ code สั้นๆ (code snippet)
  * สามารถเพิ่มเติมความสามารถได้ด้วยการเพิ่ม module ต่างๆ ที่เขียนด้วยภาษาโปรแกรมอื่นๆ ที่จะต้องถูกแปลงโดย compiler เช่น C หรือ C++ เป็นต้น
  * ใช้งานได้ทุก platform ไม่ว่าจะเป็น macOS, Windows, Linux, และ Unix รวมทั้ง mobile platform คือ Android และ iOS
  * ใช้งานได้ฟรี หมายถึง ไม่ต้องเสียค่าใช้จ่ายในการนำ Python ไปใช้งาน และสามารถแก้ไขดัดแปลงแล้วแจกจ่ายต่อได้อีกด้วย

จากการจัดอันดับโดย IEEE Spectrum เมื่อปีค.ศ. 2018 (Ref : https://spectrum.ieee.org/at-work/innovation/the-2018-top-programming-languages) ภาษา Python เป็นภาษาที่ได้รับความนิยมเป็นอันดับ 1

[รูปที่ 2.4 ผลการจัดอันดับภาษาโปรแกรมโดย IEEE Spectrum](https://spectrum.ieee.org/static/interactive-the-top-programming-languages-2018)
#### 2.2 pip
pip เป็นซอฟต์แวร์สำหรับการจัดการ packages ให้กับ Python ได้แก่ การติดตั้งและปรับปรุง packages ต่างๆ (Ref : https://pypi.org/project/pip/)
#### 2.3 virtualenv
virtualenv ถูกออกแบบและนำมาใช้งานเพื่อสร้างสภาพแวดล้อมเสมือน (Virtual Environment) สำหรับการพัฒนาด้วย Python ซึ่งจะทำให้นักพัฒนาไม่จำเป็นต้องติดตั้ง Python packages ต่างๆ ที่ต้องใช้ในการพัฒนางานทั้งหมดไว้ในสภาพแวดล้อมรวม

ตัวอย่างเช่น นักพัฒนาทำ Project A ให้กับลูกค้าโดยใช้ Python 2 ร่วมกับ Django 1.1 โดยส่งมอบงานให้ลูกค้าเรียบร้อยแต่ยังคงอยู่ในระยะเวลาการดูแลต่อไปอีก 1 ปี ต่อมานักพัฒนาคนดังกล่าวได้รับงานพัฒนา Project Z ให้กับลูกค้าอีกคน โดยต้องใช้ Python 3 ร่วมกับ Django 2 หากไม่มีการใช้งาน virtualenv นักพัฒนาก็จะต้องทำการ upgrade ซอฟต์แวร์ต่างๆ ที่เคยใช้ในการพัฒนา Project A (ได้แก่ Python 2 และ Django 1.1) เป็นซอฟต์แวร์เวอร์ชันใหม่เพื่อการพัฒนา Project Z ซึ่งจะส่งผลให้การดูแล Project A มีปัญหาได้

virtualenv จะช่วยให้นักพัฒนาสามารถติดตั้ง packages ที่แตกต่างกันสำหรับแต่ละโครงการได้ ซึ่งจะทำให้นักพัฒนามีสภาพแวดล้อมในการพัฒนาที่แยกออกจากกันสำหรับแต่ละโครงการ จึงไม่ต้องกังวลว่าการติดตั้ง packages ใน virtualenv ตัวหนึ่ง จะเกิดผลกระทบกับ packages ที่ติดตั้งในอีก virtualenv

โดยปกติ virtual environment ที่ถูกสร้างขึ้นด้วย virtualenv จะใช้ Python version เดียวกับที่ virtualenv ทำงานอยู่
## แหล่งข้อมูลเพิ่มเติม
  * https://www.djangoproject.com/
  * https://packaging.python.org/guides/installing-using-pip-and-virtualenv/
  * https://virtualenv.pypa.io/en/stable/
  * https://virtualenv.pypa.io/en/stable/userguide/
## ขั้นตอนการทดลอง
### 1. การติดตั้งซอฟต์แวร์ต่างๆ
#### 1.1 การปรับปรุงระบบปฏิบัติการ Ubuntu ให้ทันสมัย
  1. เปิดโปรแกรม Terminal
  2. ป้อนคำสั่ง

`sudo apt update`

  3. ระบบจะสอบถาม password ของ root ให้ใส่ password เดียวกับที่ใช้ login (หมาเหตุ เวลาพิมพ์ password จะไม่แสดงในหน้าจอ terminal)
  4. ระบบจะทำการตรวจสอบกับ repository ที่เก็บซอฟต์แวร์เพื่อหาว่ามีซอฟต์แวร์ใดบ้างที่มีการ update
  5. เมื่อเสร็จเรียบร้อย ให้ป้อนคำสั่ง

`sudo apt upgrade`

  6. หากมีซอฟต์แวร์ที่มีการ update ระบบจะแจ้งว่าต้อง upgrade ซอฟต์แวร์ใดบ้าง โดยให้ตอบ Y และรอจนการ upgrade เสร็จสมบูรณ์
  7. สามารถดูตัวอย่างได้จาก [clip VDO](https://drive.google.com/open?id=1LIXmXwu5UpFasKGn5WzI9hERU9EM1yYU)
#### 1.2 การติดตั้ง Python version 3
  1. ปกติ Ubuntu จะติดตั้ง Python version 3 มาเรียบร้อยแล้ว (รวมทั้งมีการติดตั้ง Python version 2 ด้วย เนื่องจากซอฟต์แวร์บางตัวยังจำเป็นต้องใช้งาน) โดยสามารถตรวจสอบได้ด้วยการป้อนคำสั่ง

`python3 --version` หรือ `python --version`

  2. หากยังไม่มีการติดตั้ง Python version 3 สามารถทำการติดตั้งได้ด้วยการป้อนคำสั่ง

`sudo apt install python3`

:question:[คำถามข้อที่ 1](https://github.com/engedu/comLab03-Lab02/issues/1)


#### 1.3 การติดตั้ง pip
  1. เปิดโปรแกรม Terminal และป้อนคำสั่ง

`sudo apt install python3-pip`

  2. ตรวจสอบการติดตั้ง โดยป้อนคำสั่ง

`pip3 --version`
#### 1.4 การติดตั้ง virtualenv
  1. เปิดโปรแกรม Terminal และป้อนคำสั่ง

`sudo pip3 install virtualenv`

  2. ตรวจสอบ package ต่างๆ ที่ถูกติดตั้งแล้ว โดยป้อนคำสั่ง

`pip3 freeze`

  3. ให้ทำการจับภาพผลลัพธ์ที่แสดงใน Terminal และบันทึกเป็นไฟล์ชื่อ “result2.1.png”

:camera:*ใส่ link ไปยังรูป “result2.1.png” ที่นี่*
### 2. การใช้งาน Virtual Environment
#### 2.1 การใช้งาน virtualenv เบื้องต้น
  1. 
  2. 
#### 2.2 การสร้าง Virtual Environment สำหรับ django2.2
  1. สร้าง Virtual Environment ชื่อ django2.2
  2. เปิดการทำงาน (activate) Virtual Environment ชื่อ django2.2
  3. ตรวจสอบ packages ต่างๆ ที่ติดตั้ง แล้วทำการจับภาพผลลัพธ์ที่แสดงใน Terminal และบันทึกเป็นไฟล์ชื่อ “result2.2-01.png”

:camera:*ใส่ link ไปยัง “result2.2-01.png” ที่นี่*

:question:[คำถามข้อที่ 2](https://github.com/engedu/comLab03-Lab02/issues/2)


  4. ติดตั้ง Library สำหรับ Python ที่ชื่อ Requests (แนะนำให้ติดตั้งเสมอเมื่อพัฒนา app ที่ต้องติดต่อผ่าน http) โดยป้อนคำสั่งดังนี้

`pip3 install requests`

  5. ตรวจสอบ packages ต่างๆ ที่ติดตั้ง แล้วทำการจับภาพผลลัพธ์ที่แสดงใน Terminal และบันทึกเป็นไฟล์ชื่อ “result2.2-02.png”

:camera:*ใส่ link ไปยัง “result2.2-02.png” ที่นี่*

  6. ติดตั้ง Django2.2 โดยป้อนคำสั่งดังนี้ (คำสั่งนี้จะเป็นการติดตั้ง latest official release version)

`pip3 install Django`

  7. ตรวจสอบ packages ต่างๆ ที่ติดตั้ง แล้วทำการจัดภาพผลลัพธ์ที่แสดงใน Terminal และบันทึกเป็นไฟล์ชื่อ “result2.2-03.png”

:camera:*ใส่ link ไปยัง “result2.2-03.png” ที่นี่*
#### 2.3 การสร้าง Virtual Environment สำหรับ django1.11
การทดลองนี้จะเป็นการสร้าง Virtual Environment ชื่อ django1.11 โดยกำหนด version ของ Python ที่ต้องการ
  1. ตรวจสอบตำแหน่งที่ python2.7 ถูกติดตั้งในระบบปฏิบัติการ โดยป้อนคำสั่งดังนี้

`whereis python`

:question:ข้อที่ 3 ตำแหน่งที่ python2.7 ถูกติดตั้งในระบบปฏิบัติการ คือ

:white_check_mark:ตอบ 

  2. สร้าง Virtual Environment ชื่อ django1.11 พร้อมกำหนด version ของ Python ที่ต้องการใช้เป็น python2.7 โดยป้อนคำสั่งดังนี้

`virtualenv --python=PATH ENV`
        
*PATH คือ ตำแหน่งที่ python2.7 ถูกติดตั้ง*

*ENV คือ ชื่อ Virtual Environment ที่ต้องการสร้าง*

  3. เปิดการทำงาน (activate) Virtual Environment ชื่อ django1.11
  4. ตรวจสอบ packages ต่างๆ ที่ติดตั้ง แล้วทำการจับภาพผลลัพธ์ที่แสดงใน Terminal และบันทึกเป็นไฟล์ชื่อ “result2.2-04.png”

:camera:*ใส่ link ไปยัง “result2.2-04.png” ที่นี่*

  5. ติดตั้ง Library สำหรับ Python ที่ชื่อ Requests (แนะนำให้ติดตั้งเสมอเมื่อพัฒนา app ที่ต้องติดต่อผ่าน http)
  6. ติดตั้ง Django1.11
  7. ตรวจสอบ packages ต่างๆ ที่ติดตั้ง แล้วทำการจับภาพผลลัพธ์ที่แสดงใน Terminal และบันทึกเป็นไฟล์ชื่อ “result2.2-05.png” (ให้เห็นชื่อ package Django==1.11)

:camera:*ใส่ link ไปยัง “result2.2-05.png” ที่นี่*

  8. ให้ผู้เรียนบันทึก Clip VDO ขั้นตอนการลบ Virtual Environment django1.11 (ให้ผู้เรียน upload clip VDO ไว้ใน Google Drive@KMITL ของตนเอง)

:camera:*ใส่ link ของ Clip VDO  ที่นี่*
