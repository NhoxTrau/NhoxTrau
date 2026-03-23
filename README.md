# Hi there, I'm **Pham Minh Thien** 👋

## 🛡️ **Cybersecurity Enthusiast | Third-year Student | PTIT**

### 🎓 About Me
- 👨‍🎓 **Full Name**: Pham Minh Thien
- ✨ **Student ID**: N22DCAT059
- ⚖️ **Major**: Information Security
- ☕ **GPA**: 3.21 / 4.0
- 🎯 **Competitions**: Participated in **CTF**, **ICPC**, and cybersecurity challenges
- 📧 **Email**: thienpham5301@gmail.com
- 🔍 **Current Focus**: Network Security
- 🌱 **Learning**: Networking

---

## 🛡️ Skills & Technologies

### 🖥️ Operating Systems
![Linux](https://img.shields.io/badge/-Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Windows](https://img.shields.io/badge/-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)

### 🌐 Network Security
![TCP/IP](https://img.shields.io/badge/-TCP%2FIP-FF6B6B?style=for-the-badge)
![OSI Model](https://img.shields.io/badge/-OSI%20Model-4ECDC4?style=for-the-badge)
![IDS](https://img.shields.io/badge/-IDS%2FIPS-45B7D1?style=for-the-badge)

**Capabilities:**
- OSI Model and TCP/IP protocol analysis
- Network traffic analysis and intrusion detection
- Firewall configuration and network segmentation

### 🛠️ Application Security
**Focus Areas:**
- OSI Model and TCP/IP protocol analysis
- Network traffic analysis and intrusion detection
- Firewall configuration and network segmentation

### ⚡ Programming Languages & Tools
![Python](https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![C++](https://img.shields.io/badge/-C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Bash](https://img.shields.io/badge/-Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)
![PowerShell](https://img.shields.io/badge/-PowerShell-5391FE?style=for-the-badge&logo=powershell&logoColor=white)

---

## 📊 GitHub Stats

<div align="center">
  <img height="180em" src="https://github-readme-stats.vercel.app/api?username=NhoxTrau&show_icons=true&theme=dark&include_all_commits=true&count_private=true&hide_border=true&bg_color=0d1117&title_color=58a6ff&text_color=c9d1d9&icon_color=58a6ff"/>
  <img height="180em" src="https://github-readme-stats.vercel.app/api/top-langs/?username=NhoxTrau&layout=compact&langs_count=8&theme=dark&hide_border=true&bg_color=0d1117&title_color=58a6ff&text_color=c9d1d9"/>
</div>

## 🔥 Contribution Streak
<div align="center">
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=NhoxTrau&theme=dark&hide_border=true&background=0d1117&stroke=58a6ff&ring=58a6ff&fire=ff7b00&currStreakNum=c9d1d9&sideNums=c9d1d9&currStreakLabel=58a6ff&sideLabels=58a6ff&dates=8b949e" alt="GitHub Streak" />
</div>

## 📈 Activity Graph
<img src="https://github-readme-activity-graph.vercel.app/graph?username=NhoxTrau&theme=github-dark&hide_border=true&bg_color=0d1117&color=58a6ff&line=58a6ff&point=ff7b00" />

---

## 🚀 Featured Projects

### 🔐 **AES Encryption Tool (128/192-bit)**
[![Repository](https://img.shields.io/badge/-View%20Repository-000?style=for-the-badge&logo=github)](https://github.com/ch1lL9uy/MAT-MA-HOC-CO-SO)

**Tech Stack:** `C++` `Cryptography` `File Security`

**Description:** Advanced AES encryption implementation supporting both 128-bit and 192-bit key lengths for local file protection. Features secure key generation, file integrity verification, and cross-platform compatibility.

**Key Features:**
- Multi-key length support (128/192-bit)
- Secure random key generation
- File integrity checksums
- Command-line interface

### 🎯 **CTF Challenge Solutions**
[![Repository](https://img.shields.io/badge/-View%20Repository-000?style=for-the-badge&logo=github)](https://github.com/thienpham5301/ctf-writeups)

**Categories:** `Reverse Engineering` 

**Description:** Collection of detailed writeups and solutions from various CTF competitions including local and international events.

---

## 🧠 Ghi chú nhanh: DDoS Attack Detection and Mitigation using ML

Bạn đang hỏi về repo này:  
https://github.com/thesaajii/Ddos-attack-detection-and-mitigation-using-ML

### Vì sao bạn “không chọn được thư mục” đó?
- Trong repo hiện tại (`NhoxTrau/NhoxTrau`) không có sẵn thư mục của repo `thesaajii/...`.
- Muốn thao tác trực tiếp thư mục đó, bạn cần clone repo đó về máy hoặc mở đúng repository trong IDE/tool.

### Họ chọn features như thế nào?
- Họ **không dùng bước feature selection riêng** (không có lọc bằng PCA, SelectKBest, v.v.).
- Cách làm chính: lấy dữ liệu từ `FlowStatsfile.csv`, rồi dùng **toàn bộ cột trừ cột nhãn cuối cùng** để train.
- Trong code: `X_flow = flow_dataset.iloc[:, :-1]` và `y_flow = flow_dataset.iloc[:, -1]`.

### Pipeline train mô hình AI trong repo đó
1. Thu thập flow stats từ SDN (Ryu/Mininet), ghi CSV.
2. Tiền xử lý: loại dấu `.` ở một số cột ID/IP để ép kiểu số.
3. Tách tập train/test với `train_test_split(..., test_size=0.25, random_state=0)`.
4. Huấn luyện mô hình:
   - Nhóm classic ML: Decision Tree, KNN, Random Forest (và có script Logistic Regression, Naive Bayes, SVM).
   - Nhóm deep learning: MLP, CNN (ở thư mục `CNN-MLP-OPT-Controller`).
5. Đánh giá bằng confusion matrix + accuracy.
6. Lưu model (`.pkl`/`.h5`) và dùng để dự đoán traffic thời gian thực.

### Các features được sử dụng (flow-level)
Các cột dùng làm đầu vào (tùy script, thường là toàn bộ cột thống kê flow):
- `timestamp`
- `datapath_id`
- `flow_id`
- `ip_src`, `tp_src`
- `ip_dst`, `tp_dst`
- `ip_proto`, `icmp_code`, `icmp_type`
- `flow_duration_sec`, `flow_duration_nsec`
- `idle_timeout`, `hard_timeout`, `flags`
- `packet_count`, `byte_count`
- `packet_count_per_second`, `packet_count_per_nsecond`
- `byte_count_per_second`, `byte_count_per_nsecond`

Nhãn (label) là cột cuối trong `FlowStatsfile.csv` (0: traffic hợp lệ, 1: DDoS theo logic dự đoán trong controller scripts).
