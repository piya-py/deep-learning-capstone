# deep-learning-capstone
# Capstone Project: CIFAR-10 Image Classification (Track A)

เป้าหมายในการ design,train และ evaluate a Convolutional Neural Network (CNN) จากเริ่มต้น (Scratch) บนชุดข้อมูล CIFAR-10 เพื่อให้ได้ผลลัพธ์ที่ดีกว่าค่า Baseline (10.0%) และผ่านเกณฑ์เป้าหมาย (>65.0%)

## สรุปผลการทดลอง (Project Summary & Results)

* **ชุดข้อมูล (Dataset):** CIFAR-10 (ภาพสีขนาด 32x32 Pixels จำนวน 10 หมวดหมู่)
* **การแบ่งชุดข้อมูล (Dataset Split):** Train: 16,000 ภาพ | Validation: 4,000 ภาพ | Test: 10,000 ภาพ
* **ความแม่นยำเริ่มต้น (Baseline Accuracy):** 10.0% (Majority Class / Random Guessing)
* **เกณฑ์เป้าหมาย (Target Accuracy):** > 65.0%
* **ความแม่นยำบน Validation Set:** **65.85%**
* **ความแม่นยำบน Test Set สุทธิ (Final Test Accuracy):** **`66.37%`** (ผ่านเกณฑ์เป้าหมาย: **YES**)


## Model Architecture & Training Hyperparameters

* **โครงสร้างโมเดล (Architecture):** Custom CNN สร้างด้วย PyTorch (`Conv2d`, `BatchNorm2d`, `ReLU`, `MaxPool2d`, `Dropout`, `Linear`)
* **ตัวปรับแต่ง (Optimizer):** Adam (`lr = 2e-3`, `weight_decay = 1e-4`)
* **จำนวนรอบการเทรน (Epochs):** 20 Epochs
* **การเพิ่มข้อมูล (Data Augmentation):** `RandomHorizontalFlip`, `RandomCrop(32, padding=4)`
* **การป้องกัน Overfitting (Regularization):** `Dropout(0.5)`
* **สุ่มค่าเริ่มต้น (Random Seed):** กำหนดค่าคงที่ (`SEED = 42`) เพื่อให้ผลการรันเหมือนเดิมทุกครั้ง


## ตารางการทดลองและปรับจูนโมเดล (Ablation Study)

| การทดลอง (Exp #) | การตั้งค่า / การเปลี่ยนแปลง | Val Accuracy | Train-Val Gap | ข้อสังเกตสำคัญ |
| :---: | :--- | :---: | :---: | :--- |
| **Exp 0 (Baseline)** | 8 Epochs, Subset 10k, LR 1e-3, Dropout 0.25 | 55.50% | ~0.5% | โมเดลเริ่มต้น สับสนหมวดหมู่สัตว์สูง |
| **Exp 1 (Hyper-tuning)** | 15 Epochs, Subset 10k, LR 2e-3, Dropout 0.5 | 64.10% | ~0.6% | ปรับเพิ่ม LR และ Dropout ช่วยดัน Accuracy ขึ้นอย่างชัดเจน (+8.6%) |
| **Exp 2 (Final Model)** | **20 Epochs, Subset 20k**, LR 2e-3, Dropout 0.5 | **65.85%** | **~0.4%** | ขยายชุดข้อมูลเป็น 2 เท่า ทำให้โมเดลเรียนรู้ได้ดีขึ้น ทะลุเป้าหมาย >65% |


## ผลการจำแนกรายคลาสและการวิเคราะห์ข้อผิดพลาด (Failure Analysis)

* **หมวดหมู่ที่ทำได้ดีเยี่ยม (Best Recall):** `horse` (81.94%), `automobile` (78.59%), `truck` (78.59%), และ `frog` (75.82%)
* **หมวดหมู่ที่เป็นข้อจำกัด (Failure Case):** `cat` (44.11%) และ `deer` (56.14%) มักจะถูกทำนายสับสนเป็น `dog` หรือ `frog` เนื่องจากภาพมีขนาดเล็กมาก (32x32) ทำให้ลักษณะทางกายภาพของสัตว์สี่ขาแยกได้ยาก
