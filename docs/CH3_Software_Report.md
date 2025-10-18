🧩 **Drone Detector 5KA — Этап CH3 · Software Environment / Hailo Integration**

📍 **Цель этапа**
Развернуть и протестировать официальное окружение Hailo SDK 4.20.0 на Raspberry Pi 5 (Debian 12 Bookworm aarch64) с модулем Hailo-8 PCIe (Waveshare rev 01) и камерой Sony IMX500.  
Результат — полностью рабочий пайплайн инференса YOLOv12n (1 класс: “drone”) через rpicam-apps → HailoRT → постпроцесс JSON.

---

⚙️ **Аппаратная и системная конфигурация**
- Плата: Raspberry Pi 5 (Debian 12 Bookworm aarch64, ядро 6.12.47+rpt-rpi-2712)  
- AI-модуль: Hailo-8 PCIe (Waveshare rev 01)  
- Камера: Raspberry Pi AI Camera Sony IMX500  
- Сеть: TP-Link_D265_5G / 58338933  
- SSH-доступ: MacBook (Pro) → pi-drone  
- Рабочая директория: `/home/nemez/project_root/`

---

✅ **Выполнено**
1. Установка SDK — `sudo apt update && sudo apt install -y hailo-all`  
2. Проверка устройства — `hailortcli fw-control identify` → Device OK  
3. Сервис `hailort.service` active (running)  
4. Бенчмарк `yolov8s_h8.hef` → 157 FPS · 7.9 ms  
5. Python-окружение: torch 2.4.1, ultralytics 8.3.217, opencv-python-headless 4.10.0, onnx 1.16.2  
6. Подготовлена структура проекта (project_root с configs/, models/, logs/)  
7. Тест камера IMX500 + Hailo postprocess → рабочий инференс  
8. Бенчмарк кастомной модели → ≈ 258 FPS · 4.56 ms latency · 1.05 W power

---

📄 **Критически важные данные**

| Параметр | Значение |
|:--|:--|
| HailoRT версия | 4.20.0 |
| Сервис | hailort.service (active) |
| HEF-модель | /home/nemez/models/best_drone_yolov12n_h8.hef |
| Postproc конфиг | /home/nemez/project_root/configs/hailo_yolo_drone.json |
| Камера тюнинг | /usr/share/libcamera/ipa/rpi/pisp/imx500.json |
| Бенчмарк FPS | ≈ 258 FPS @ 4.56 ms |
| Python venv | /home/nemez/project_root/venv |
| Отчёт CH3 | /home/nemez/project_root/logs/CH3_status.txt |

⚠️ **Важно**
- postprocess-файл должен указывать корректный путь к HEF  
- перед детектором проверить `hailort.service` (active)  
- устройство IMX500 должно быть инициализировано (`/dev/media*`)  
- GPIO/LED — настраивается на этапе CH4  
- структура проектных папок должна сохраняться точно для CI/CD  

---

🧱 **Коммит в репозиторий**

cd ~/Desktop/project_root
git add docs/CH3_Software_Report.md configs/.yaml configs/.json logs/CH3_status.txt
git add models/best_drone_yolov12n_h8.hef
git commit -m "docs: add CH3 software environment and Hailo integration report"
git tag -a v0.0.3-CH3 -m "Software environment validated on Pi5 + Hailo-8"
git push && git push origin v0.0.3-CH3



