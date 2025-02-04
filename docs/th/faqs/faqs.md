# คำถามที่พบบ่อย

## SubQuery คืออะไร?

SubQuery เป็นโปรเจ็กต์โอเพ่นซอร์สที่ช่วยให้นักพัฒนาสามารถทำการ index เปลี่ยนแปลง และสืบค้นข้อมูลเชนของ Substrate เพื่อขับเคลื่อนแอปพลิเคชันของตนได้

SubQuery ยังให้บริการโฮสติ้งโปรเจ็กต์ระดับโปรดักชันฟรีสำหรับนักพัฒนา เพื่อลดความรับผิดชอบในการจัดการโครงสร้างพื้นฐาน และปล่อยให้นักพัฒนาได้ทำในสิ่งที่พวกเขาทำได้ดีที่สุด - นั่นคือ การพัฒนาระบบ

## วิธีที่ดีที่สุดในการเริ่มต้นใช้งาน SubQuery คืออะไร?

วิธีที่ดีที่สุดในการเริ่มต้นใช้งาน SubQuery คือทดลองทำตาม [Hello World tutorial](../quickstart/helloworld-localhost.md) ของเรา วิธีที่ดีที่สุดในการเริ่มต้นใช้งาน SubQuery คือทดลองทำตาม [บทแนะนำ Hello World](../quickstart/helloworld-localhost.md) ของเรา นี่คือขั้นตอนง่ายๆในการดาวน์โหลดเทมเพลตเริ่มต้น สร้างโปรเจ็กต์ จากนั้นใช้ Docker เพื่อเรียกใช้โหนดบน localhost ของคุณและรัน query อย่างง่าย

## ฉันจะมีส่วนร่วมหรือให้คำติชมกับ SubQuery ได้อย่างไร?

เรารักการมีส่วนร่วมและข้อเสนอแนะจากชุมชน ในการสนับสนุนโค้ด ให้ fork repository ที่สนใจ และทำการเปลี่ยนแปลง จากนั้นส่ง PR หรือ Pull Request อ้อ อย่าลืมทดสอบด้วยล่ะ! รวมถึงตรวจสอบหลักเกณฑ์การสนับสนุน (TBA) ของเราด้วย

หากต้องการแสดงความคิดเห็น โปรดติดต่อเราที่ hello@subquery.network หรือไปที่ [ช่อง discord](https://discord.com/invite/78zg8aBSMG)

## การโฮสต์โปรเจ็กต์ของฉันใน SubQuery Projects มีค่าใช้จ่ายเท่าไหร่?

การโฮสต์โปรเจ็กต์ของคุณใน SubQuery Projects นั้นฟรี - นั่นเป็นวิธีการตอบแทนชุมชนของเรา หากต้องการเรียนรู้วิธีโฮสต์โครงการของคุณกับเรา โปรดดูบทแนะนำ [Hello World (SubQuery hosted)](../quickstart/helloworld-hosted.md)

## Deployment slots คืออะไร?

Deployment slots เป็นฟีเจอร์ใน [SubQuery Projects ](https://project.subquery.network) ซึ่งเทียบเท่ากับสภาพแวดล้อมสำหรับการพัฒนา ตัวอย่างเช่น ในองค์กรซอฟต์แวร์ใดๆ โดยปกติจะมีสภาพแวดล้อมสำหรับการพัฒนาและสภาพแวดล้อมสำหรับการผลิตเป็นอย่างน้อย (ไม่นับรวม localhost) โดยทั่วไปแล้วอาจจะมีสภาพแวดล้อมอื่นๆเพิ่มเติมได้ เช่น การจัดเตรียมและการวางแผนก่อนการผลิต หรือแม้กระทั่ง QA จะรวมอยู่ด้วย ขึ้นอยู่กับความต้องการขององค์กรและการตั้งค่ากระบวนการการพัฒนา

ปัจจุบัน SubQuery มีสอง slot ที่พร้อมใช้งาน คือ slot สำหรับ staging และ production ซึ่งช่วยให้นักพัฒนา deploy SubQuery ของตนกับ staging environment และเมื่อทุกอย่างเป็นไปด้วยดี ก็สามารถ"โปรโมตเป็น production" ได้เพียงคลิกปุ่ม

## ข้อดีของ staging slot คืออะไร?

ประโยชน์หลักของการใช้ staging slot คือช่วยให้คุณสามารถเตรียม new release ของโปรเจ็กต์ SubQuery โดยยังไม่ต้องเปิดเผยต่อสาธารณะ คุณสามารถรอให้ staging slot ทำการ index ข้อมูลทั้งหมดใหม่โดยไม่กระทบต่อแอปพลิเคชันที่ใช้งานจริงของคุณ คุณสามารถรอให้ staging slot ทำการ index ข้อมูลทั้งหมดใหม่ก่อน โดยไม่กระทบต่อแอปพลิเคชันที่ใช้งานจริงของคุณ

Staging slot จะไม่แสดงต่อสาธารณะใน [Explorer](https://explorer.subquery.network/) และมี URL เฉพาะที่มองเห็นได้เฉพาะคุณเท่านั้น และแน่นอน สภาพแวดล้อมที่แยกจากกันทำให้คุณสามารถทดสอบโค้ดใหม่ได้โดยไม่กระทบต่อการใช้งานจริง

## Extrinsics คืออะไร?

หากคุณคุ้นเคยกับบล็อคเชนอยู่แล้ว คุณสามารถเปรียบ extrinsics ได้กับธุรกรรม หรืออธิบายให้เป็นทางการมากขึ้น extrinsic เป็นส่วนของข้อมูลที่มาจากนอก chain และถูกรวมอยู่ในบล็อก โดย extrinsics มีสามประเภท ได้แก่ inherent extrinsics, signed transactions และ unsigned transactions

Inherent extrinsics คือชิ้นส่วนของข้อมูลที่ไม่ได้ลงนามและแทรกเข้าไปในบล็อกโดยผู้เขียนบล็อกเท่านั้น

Signed transaction extrinsics คือธุรกรรมที่มีลายเซ็นของบัญชีที่ออกธุรกรรม โดยพวกเขาพร้อมที่จะจ่ายค่าธรรมเนียมเพื่อให้ธุรกรรมรวมอยู่ใน chain

Unsigned transactions extrinsics คือธุรกรรมที่ไม่มีลายเซ็นของบัญชีที่ออกธุรกรรม ซึ่ง Unsigned transactions extrinsics นั้นควรใช้ด้วยความระมัดระวัง เนื่องจากไม่มีใครจ่ายค่าธรรมเนียมให้ เพราะมีการลงนาม ด้วยเหตุนี้ คิวธุรกรรมจึงขาดตรรกะทางเศรษฐกิจในการป้องกันการสแปม

สำหรับข้อมูลเพิ่มเติม คลิก [ที่นี่](https://substrate.dev/docs/en/knowledgebase/learn-substrate/extrinsics)

## Endpoint  สำหรับเครือข่าย Kusama คืออะไร?

network.endpoint สำหรับเครือข่าย Kusama คือ `wss://kusama.api.onfinality.io/public-ws`

## Endpoint  สำหรับเครือข่าย Polkadot mainnet คืออะไร?

network.endpoint สำหรับเครือข่าย Polkadot คือ `wss://polkadot.api.onfinality.io/public-ws`

## ฉันจะพัฒนาสคีมาของโปรเจคของฉันได้อย่างไร

A known issue with developing a changing project schema is that when lauching your Subquery node for testing, the previously indexed blocks will be incompatible with your new schema. In order to iteratively develop schemas the indexed blocks stored in the database must be cleared, this can be achieved by launching your node with the `--force-clean` flag. ยกตัวอย่างเช่น:

```shell
subql-node -f . --force-clean --subquery-name=<project-name>
```

Note that it is recommended to use `--force-clean` when changing the `startBlock` within the project manifest (`project.yaml`) in order to begin reindexing from the configured block. If `startBlock` is changed without a `--force-clean` of the project then the indexer will continue indexing with the previously configured `startBlock`.