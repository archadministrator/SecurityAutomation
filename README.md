# 🛡️ Roadmap Security Automation — 6 Tháng (Beginner → Freelance/Junior)

**Xuất phát điểm của bạn:** Python cơ bản, đang học Linux/networking/HTTP/web security, đã làm Bandit.
**Nguyên tắc xuyên suốt:** 70% code — 30% đọc. Mỗi tuần đều phải có ít nhất 1 tool chạy được, push lên GitHub.

---

## 📌 Cách dùng roadmap này
- Mỗi **Giai đoạn = 1 tháng**, chia thành 4 tuần.
- Mỗi giai đoạn có: Kiến thức → Tool → Project chính → ≥7 bài tập thực hành → Tài liệu.
- **Từ Tháng 3**, các project đã đủ chất lượng để nhận job freelance nhỏ (audit script, recon automation, report tự động…).
- Tất cả project đều đẩy lên GitHub với README tử tế — đây chính là **portfolio** để xin việc/freelance.

---

# 🟢 GIAI ĐOẠN 1 — Tháng 1: Python Automation Core + Linux/Network Scripting

### Mục tiêu
Biến Python từ "biết cú pháp" thành "công cụ hack việc" — thao túng file, process, network, subprocess như một pentester thực thụ.

### Kiến thức cần học
- `argparse`, xử lý file/text bằng `regex`, `pathlib`
- `subprocess` để điều khiển tool CLI (nmap, curl…) từ Python
- `socket` cơ bản (TCP connect, banner grabbing)
- `threading`/`concurrent.futures` để scan nhanh nhiều target song song
- Đọc/ghi log, xử lý JSON/CSV (định dạng chuẩn để tool nào cũng đọc được)
- Linux cơ bản phục vụ automation: cron, file permission, pipe/redirect, `grep/awk/sed`

### Tool cần biết
- `nmap` (dùng qua CLI và qua thư viện `python-nmap`)
- `netcat`
- Git/GitHub (bắt buộc, để lưu portfolio)
- VS Code + venv/pip

### 🔨 Project chính: **Mini Network Scanner CLI**
Tool Python nhận input 1 dải IP/subnet → quét port mở → banner grabbing → xuất báo cáo JSON/CSV. Có multithreading, có `--help` tử tế.

### Bài tập thực hành (bắt buộc làm hết)
1. Viết script `port_scanner.py` quét TCP port 1-1024 trên 1 host, in ra port mở (dùng socket thô, không dùng thư viện có sẵn).
2. Thêm multithreading vào bài 1, so sánh thời gian scan trước/sau (đo bằng `time`).
3. Viết script wrap `nmap` qua `subprocess`, parse output ra JSON.
4. Viết script banner grabbing: connect vào port mở, đọc service banner (SSH, HTTP, FTP…) và in ra.
5. Viết script quét cả 1 dải subnet (CIDR, dùng `ipaddress` module) — kết hợp với bài 2.
6. Viết log rotation script: tự động nén file log cũ hơn N ngày trong 1 thư mục (bash + cron, hoặc Python + `schedule`).
7. Viết script "system health check": CPU, RAM, disk, list process lạ (dùng `psutil`) → xuất report .txt.
8. (Bonus) Đóng gói toàn bộ thành CLI tool dùng `argparse` với các flag: `--target`, `--ports`, `--output`, `--threads`.

### Tài liệu tham khảo
- *Black Hat Python* (Justin Seitz) — chương socket & networking
- Python docs: `socket`, `concurrent.futures`, `subprocess`
- OverTheWire Bandit (tiếp tục song song, củng cố Linux)

---

# 🟡 GIAI ĐOẠN 2 — Tháng 2: Web Security Automation (HTTP + Scraping + Vuln Detection cơ bản)

### Mục tiêu
Tự động hoá việc "click chuột" khi test web: gửi request hàng loạt, phát hiện lỗi phổ biến (SQLi, XSS reflected, header thiếu bảo mật) bằng script.

### Kiến thức cần học
- HTTP sâu hơn: headers, cookies, session, redirect chain, response code
- OWASP Top 10 (đọc nhanh, không sa đà lý thuyết — chỉ để biết pattern cần detect)
- Web scraping/crawling (lấy toàn bộ link/form trong 1 site)
- Cách phát hiện lỗi cơ bản bằng payload + so sánh response (error-based detection)
- Cấu trúc request/response, cách parse HTML để tìm form input

### Tool cần biết
- `requests`, `httpx` (Python)
- `BeautifulSoup` / `lxml`
- Burp Suite (Community) — hiểu traffic, dùng Proxy để so sánh với script tự viết
- `ffuf` hoặc `gobuster` cho fuzzing/directory brute-force

### 🔨 Project chính: **Web Recon + Basic Vuln Scanner**
Tool nhận 1 URL → crawl toàn bộ link/form nội bộ → check security headers → test payload SQLi/XSS cơ bản trên từng form → xuất report HTML/Markdown.

### Bài tập thực hành
1. Viết script check security headers (CSP, X-Frame-Options, HSTS…) cho 1 danh sách domain, xuất bảng pass/fail.
2. Viết crawler đơn giản: từ 1 URL, lấy hết `<a href>` nội bộ (cùng domain), tránh crawl vòng lặp.
3. Viết script tự động tìm tất cả `<form>` trên 1 trang, liệt kê input field + method (GET/POST).
4. Viết script gửi payload XSS cơ bản (`<script>alert(1)</script>`) vào từng input tìm được ở bài 3, kiểm tra reflected trong response.
5. Viết script test SQLi cơ bản bằng payload lỗi (`'`, `" OR 1=1--`) và so sánh độ dài/response code để phát hiện bất thường.
6. Viết script check thư mục/file nhạy cảm (`.git/`, `.env`, `/admin`, `backup.zip`…) bằng wordlist tự tạo.
7. Test toàn bộ tool trên **DVWA** hoặc **OWASP Juice Shop** (self-host bằng Docker) — không test web thật khi chưa được phép.
8. (Bonus) Xuất report tự động dạng Markdown/HTML có mức độ nghiêm trọng (Low/Medium/High) cho từng lỗi tìm thấy.

### Tài liệu tham khảo
- OWASP Testing Guide (chỉ đọc phần liên quan tới payload/detect)
- PortSwigger Web Security Academy (làm lab thay vì đọc lý thuyết)
- Docker: DVWA, OWASP Juice Shop (môi trường luyện tập hợp pháp)

---

# 🟠 GIAI ĐOẠN 3 — Tháng 3: Recon/OSINT Automation ⚡ *(Bắt đầu freelance được từ đây)*

### Mục tiêu
Đây là mảng automation **dễ bán nhất** cho khách hàng bug bounty/pentest: tool recon càng nhanh, càng gọn, càng dễ có job nhỏ trên Upwork/Fiverr hoặc cộng đồng bug bounty VN.

### Kiến thức cần học
- Subdomain enumeration (passive + active)
- OSINT: Shodan, Censys, crt.sh, Wayback Machine API
- Cách chain nhiều tool lại (pipeline: subdomain → alive check → screenshot → tech detect)
- API authentication cơ bản (API key, rate limit handling)
- Cách viết report chuyên nghiệp (khách hàng freelance quan tâm report hơn cả tool)

### Tool cần biết
- `subfinder`, `httpx`, `nuclei` (ProjectDiscovery suite — cực kỳ phổ biến trong automation thực chiến)
- Shodan API, Censys API, crt.sh
- `dnspython` (Python)

### 🔨 Project chính: **Recon Automation Pipeline** (đây sẽ là sản phẩm freelance đầu tiên của bạn)
Script/CLI nhận domain → tự động: subdomain enum → check alive (httpx) → lấy tech stack → chạy nuclei templates cơ bản → xuất report tổng hợp (JSON + Markdown) trong 1 lệnh duy nhất.

### Bài tập thực hành
1. Viết script gọi `crt.sh` API lấy toàn bộ subdomain của 1 domain, loại trùng.
2. Viết wrapper Python gọi `subfinder` qua subprocess, merge kết quả với bài 1.
3. Viết script check subdomain nào đang "alive" (HTTP 200/redirect) bằng `httpx` hoặc `requests` + threading.
4. Viết script tích hợp Shodan API: nhập IP/domain → lấy thông tin port mở, banner, org.
5. Viết script chain toàn bộ pipeline trên thành 1 lệnh: `python recon.py -d target.com -o report/`.
6. Viết script tự động chạy `nuclei` với templates CVE cơ bản trên danh sách subdomain alive, parse kết quả.
7. Viết script xuất report Markdown đẹp: tổng số subdomain, số alive, top tech stack, lỗi tìm thấy — có thể gửi thẳng cho khách.
8. (Bonus) Thêm tính năng gửi report qua Telegram/Discord webhook khi scan xong.

### 💰 Bắt đầu kiếm tiền từ đây
- Đăng tool lên GitHub, viết blog/case-study ngắn về cách nó hoạt động → dùng làm portfolio.
- Nhận job nhỏ dạng "recon cho bug bounty program", "subdomain monitoring", giá rẻ (5-20$ / lần chạy hoặc theo giờ) trên Fiverr/Upwork, hoặc nhóm bug bounty Việt Nam (Facebook, Discord).
- Tham gia public bug bounty (HackerOne/Bugcrowd) dùng chính tool bạn xây — vừa kiếm tiền vừa có case study thật.

### Tài liệu tham khảo
- ProjectDiscovery docs (subfinder, httpx, nuclei)
- Shodan/Censys API docs
- Bug bounty methodology của Jason Haddix ("The Bug Hunter's Methodology")

---

# 🔵 GIAI ĐOẠN 4 — Tháng 4: Vulnerability Scanning & Exploitation Automation

### Mục tiêu
Từ "phát hiện" nâng lên "khai thác có kiểm soát" + viết custom detection thay vì chỉ dùng tool có sẵn — đây là điểm khác biệt giữa junior thường và junior giỏi.

### Kiến thức cần học
- Cách viết custom Nuclei template (YAML)
- CVE research cơ bản: đọc advisory, hiểu PoC, tái tạo bằng script
- API security testing (REST API: auth bypass, IDOR, rate limit)
- Cách viết exploit PoC an toàn (chỉ chứng minh, không phá hoại) — dùng cho report

### Tool cần biết
- Nuclei (viết custom template)
- Postman/Insomnia + `httpx`/`requests` cho API testing
- `sqlmap` (hiểu cách nó hoạt động, wrap bằng script để tự động chạy hàng loạt)
- Burp Suite Extender/API cơ bản (biết là được, không bắt buộc chuyên sâu)

### 🔨 Project chính: **Custom Vulnerability Scanner + API Security Checker**
Tool tự viết detection logic riêng (không chỉ chạy tool có sẵn) cho: IDOR cơ bản, broken auth, thiếu rate-limit, exposed API endpoint — chạy hàng loạt trên danh sách endpoint.

### Bài tập thực hành
1. Viết custom Nuclei template YAML để detect 1 lỗi cụ thể (ví dụ: exposed `.env` hoặc header thiếu).
2. Viết script test IDOR cơ bản: đổi ID trong URL/API (`/api/user/1` → `/api/user/2`) và so sánh response giữa 2 account khác quyền.
3. Viết script test rate-limit: gửi N request liên tục vào 1 endpoint login, kiểm tra có bị chặn không.
4. Viết script tự động test JWT: decode token, thử đổi `alg` sang `none`, thử test weak secret bằng wordlist nhỏ.
5. Viết script wrap `sqlmap` chạy hàng loạt trên danh sách URL có tham số, gom kết quả về 1 report.
6. Viết script phát hiện API endpoint "ẩn" từ file JS của 1 website (regex tìm path trong file `.js`).
7. Test toàn bộ trên **OWASP Juice Shop / VAmPI (Vulnerable API)** — môi trường hợp pháp để luyện API security.
8. (Bonus) Viết report tự động đánh giá theo CVSS cơ bản (severity score) cho từng lỗi phát hiện.

### Tài liệu tham khảo
- Nuclei template writing guide (ProjectDiscovery docs)
- VAmPI, OWASP API Security Top 10
- HackTricks (tra cứu kỹ thuật, không đọc tuần tự — dùng như "dictionary")

---

# 🟣 GIAI ĐOẠN 5 — Tháng 5: SOC/DevSecOps Automation (SIEM, Log, CI/CD Security)

### Mục tiêu
Mở rộng sang mảng automation "phòng thủ" — rất nhiều công ty trả tiền cho automation loại này (SOC automation, security trong CI/CD), giúp bạn không chỉ làm offensive mà còn defensive → tăng cơ hội job.

### Kiến thức cần học
- Log analysis: parse log Apache/Nginx/Auth log để phát hiện brute-force, scan
- Khái niệm SOAR cơ bản (Security Orchestration, Automation, Response) — không cần dùng SIEM to lớn, tự làm mini version
- CI/CD security: SAST/DAST tự động trong pipeline (GitHub Actions)
- Alerting: webhook, email, Slack/Discord integration

### Tool cần biết
- `Elastic/Grafana` (mức cơ bản, biết đọc dashboard) — không cần setup phức tạp, có thể dùng file log local
- GitHub Actions (viết security pipeline: chạy `bandit`, `semgrep`, `trivy`)
- `semgrep`, `bandit` (SAST cho Python), `trivy` (scan container image)

### 🔨 Project chính: **Mini SOC Automation Toolkit**
1) Script phân tích log (auth.log/access.log) → phát hiện brute-force/scan pattern → tự động gửi cảnh báo Slack/Discord.
2) Pipeline GitHub Actions tự động chạy SAST (`bandit`/`semgrep`) + scan dependency (`trivy`) mỗi lần push code, fail build nếu có lỗi nghiêm trọng.

### Bài tập thực hành
1. Viết script parse `auth.log`/`access.log` bằng regex, đếm số lần login fail theo IP trong khoảng thời gian.
2. Thêm logic: nếu 1 IP fail > N lần trong M phút → tự động gửi alert (Discord/Slack webhook).
3. Viết script phát hiện pattern port scan/dir brute-force từ access log (nhiều request 404 liên tiếp từ 1 IP).
4. Viết GitHub Actions workflow tự động chạy `bandit` trên 1 repo Python, fail nếu có lỗi severity cao.
5. Thêm `semgrep` custom rule vào workflow trên để detect 1 lỗi code cụ thể (hardcoded secret, ví dụ).
6. Viết script scan Docker image bằng `trivy`, parse output, chỉ show lỗi CRITICAL/HIGH.
7. Viết dashboard đơn giản (có thể chỉ là HTML tự generate hoặc Grafana) hiển thị thống kê alert theo ngày.
8. (Bonus) Tích hợp toàn bộ thành 1 pipeline: code push → SAST/dependency scan → nếu pass → auto-deploy; nếu fail → alert Slack.

### Tài liệu tham khảo
- Semgrep rule writing docs
- GitHub Actions security hardening guide
- Trivy docs (Aqua Security)

---

# 🔴 GIAI ĐOẠN 6 — Tháng 6: Tổng hợp thành Framework + Portfolio + Định vị Freelance

### Mục tiêu
Gộp toàn bộ 5 tháng thành **1 sản phẩm hoàn chỉnh** đủ sức làm portfolio chủ lực, đồng thời chuẩn hoá cách bán dịch vụ freelance.

### Kiến thức cần học
- Thiết kế tool dạng modular/plugin (để dễ maintain, dễ show là "kỹ sư thật" chứ không phải "script kiddie")
- Cách viết README/report chuyên nghiệp, cách định giá dịch vụ freelance
- Cơ bản packaging: Docker hoá tool để khách dễ chạy

### Tool cần biết
- Docker (đóng gói toàn bộ pipeline recon+scan thành 1 image)
- Click/Typer (CLI framework Python chuyên nghiệp hơn argparse)
- GitHub Pages/Notion (làm trang portfolio)

### 🔨 Project chính: **"All-in-One Security Automation Framework"**
Gộp: Recon (Tháng 3) + Vuln Scan (Tháng 4) + Report engine (Tháng 2) + Alerting (Tháng 5) thành 1 CLI tool modular, có Docker image, có README chuẩn, có demo video/GIF.

### Bài tập thực hành
1. Refactor toàn bộ code cũ thành cấu trúc module rõ ràng (`recon/`, `scanner/`, `report/`, `alert/`).
2. Viết CLI chuyên nghiệp bằng `Typer`/`Click` thay cho `argparse` cũ, có subcommand (`tool recon`, `tool scan`, `tool report`).
3. Viết Dockerfile đóng gói toàn bộ tool, test chạy bằng `docker run`.
4. Viết bộ test cơ bản (`pytest`) cho các module chính, đảm bảo tool không "sập" khi target lỗi/không phản hồi.
5. Viết README chuẩn portfolio: mô tả, kiến trúc, demo GIF, hướng dẫn cài đặt, use-case thực tế.
6. Chạy toàn bộ framework trên 1 target hợp pháp (CTF box, HackTheBox, chương trình bug bounty cho phép recon) và viết 1 case-study/blog post kể lại quá trình.
7. Tạo hồ sơ freelance: profile Upwork/Fiverr hoặc bài giới thiệu trên LinkedIn/nhóm cộng đồng, gắn kèm link GitHub + case study.
8. (Bonus) Thêm tính năng "plugin system" — cho phép thêm module scan mới mà không sửa code lõi (dễ show tư duy kiến trúc phần mềm).

### Tài liệu tham khảo
- Typer docs, Docker docs
- "Clean Code" (áp dụng nhẹ cho phần refactor, không cần đọc hết sách)
- Cách định giá freelance security: xem case study trên r/AskNetsec, r/cybersecurity

---

## 🎯 Tổng kết lộ trình kiếm tiền

| Mốc | Trạng thái |
|---|---|
| Cuối Tháng 2 | Có 2 tool nền tảng (network scanner, web vuln scanner) trên GitHub |
| **Cuối Tháng 3** | **Có thể nhận job freelance nhỏ** (recon automation, subdomain monitoring) |
| Cuối Tháng 4 | Portfolio đủ mạnh để apply job junior security/pentest automation |
| Cuối Tháng 5 | Mở rộng sang SOC/DevSecOps → tăng loại job có thể nhận (không chỉ offensive) |
| **Cuối Tháng 6** | Có 1 framework hoàn chỉnh làm sản phẩm chủ lực + hồ sơ freelance sẵn sàng |

### Lưu ý quan trọng
- **Chỉ test trên môi trường hợp pháp**: máy ảo tự dựng (DVWA, Juice Shop, VAmPI, HackTheBox, TryHackMe), hoặc chương trình bug bounty có cho phép recon/scan. Không tự ý quét/khai thác hệ thống không được uỷ quyền.
- Mỗi project xong đều **commit + viết README** ngay, đừng để cuối tháng 6 mới làm — portfolio xây dần mới tự nhiên.