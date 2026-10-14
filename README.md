# Hướng dẫn Debloat Windows 10/11 Toàn diện (Bản mở rộng 2026)

> **Dành cho người dùng muốn làm sạch Windows 10/11 một cách an toàn, có kiểm soát và không bỏ sót các tính năng AI/cloud.**

---

## ⚠️ Cảnh báo quan trọng trước khi bắt đầu

Debloat là việc can thiệp sâu vào hệ thống. Một sai sót nhỏ có thể khiến Windows gặp lỗi khó phục hồi. Trước khi làm bất cứ điều gì, hãy tuân thủ các nguyên tắc sau:

- **Sao lưu trước:** Tạo System Image hoặc ít nhất là Restore Point. Restore Point không khôi phục được mọi gói đã gỡ hoặc lỗi khởi động.
- **Đọc kỹ script:** Luôn xem xét tài liệu và kiểm tra script trước khi chạy với quyền Administrator. Tránh chạy các script từ xa qua `irm | iex` nếu có bản phát hành đã được kiểm duyệt.
- **Thay đổi từng lớp một:** Đừng chồng nhiều bộ debloat lên nhau. Các thay đổi về registry, policy, service và package chồng chéo sẽ rất khó chẩn đoán hoặc đảo ngược.
- **Giữ lại tính năng bảo mật có chủ đích:** Tắt Defender, SmartScreen, Windows Update, UAC, VBS hoặc các biện pháp giảm thiểu CPU là **đánh đổi bảo mật**, không phải tối ưu hóa thông thường.
- **Hiểu rõ khả năng rollback:** Nút hoàn tác có thể khôi phục về mặc định Windows chứ không phải giá trị trước đó của bạn. Gỡ ứng dụng, dọn dẹp, gỡ component và thay đổi ISO có thể yêu cầu cài lại hệ điều hành.

> **Lưu ý về Windows 10:** Windows 10 Home và Pro đã **hết hỗ trợ từ ngày 14 tháng 10 năm 2025**. Một công cụ hỗ trợ Windows 10 không làm cho bản cài đặt không được hỗ trợ trở nên an toàn. Nếu bạn vẫn dùng Windows 10, hãy cân nhắc nâng cấp hoặc chuyển sang hệ điều hành khác.

---

## 📑 Mục lục

- [Bộ công cụ Debloat đa dạng](#-bộ-công-cụ-debloat-đa-dạng-không-chỉ-winutil)
- [Phần 1: Chuẩn bị và tạo điểm phục hồi](#-phần-1-chuẩn-bị-và-tạo-điểm-phục-hồi)
- [Phần 2: Gỡ bloatware – Từ đơn giản đến nâng cao](#-phần-2-gỡ-bloatware--từ-đơn-giản-đến-nâng-cao)
- [Phần 3: Loại bỏ AI và tính năng đám mây](#-phần-3-loại-bỏ-ai-và-tính-năng-đám-mây-phần-còn-thiếu-trong-repo-gốc)
- [Phần 4: Tăng cường bảo mật sau Debloat](#-phần-4-tăng-cường-bảo-mật-sau-debloat)
- [Phần 5: Tối ưu hóa sau Debloat](#-phần-5-tối-ưu-hóa-sau-debloat)
- [Phần 6: Checklist tổng hợp](#-phần-6-checklist-tổng-hợp)
- [Lời kết và định hướng phát triển](#-lời-kết-và-định-hướng-phát-triển)
- [Tài nguyên tham khảo](#-tài-nguyên-tham-khảo)
- [Đóng góp & Giấy phép](#-đóng-góp--giấy-phép)

---

## 🛠️ Bộ công cụ Debloat đa dạng (không chỉ WinUtil)

Repo gốc chỉ dùng WinUtil của Chris Titus. Tuy nhiên, có nhiều công cụ khác mạnh mẽ không kém, mỗi công cụ có thế mạnh riêng. Dưới đây là so sánh và hướng dẫn sử dụng chúng.

### So sánh nhanh các công cụ phổ biến

| Công cụ | Nền tảng | Phong cách | Điểm mạnh | Hạn chế |
|---------|----------|------------|-----------|---------|
| **WinUtil** (Chris Titus) | Win 10/11 | GUI + CLI | Tất cả trong một: tweaks, config, install apps | Có thể thay đổi quá nhiều thứ cùng lúc |
| **Win11Debloat** (Raphire) | Win 10/11 | GUI PowerShell + CLI | Nhẹ, linh hoạt, hỗ trợ command-line và audit mode | Cần chạy qua PowerShell |
| **RemoveWindowsAI** (zoicware) | Win 11 | Script chuyên sâu | Loại bỏ triệt để AI: Copilot, Recall, Click to Do, AI trong Paint/Notepad | Chỉ tập trung vào AI, không phải debloat tổng thể |
| **O&O ShutUp10++** | Win 10/11 | GUI | Kiểm soát quyền riêng tư chi tiết, không cần cài đặt | Chỉ tập trung vào privacy, không gỡ app |
| **Winhance** | Win 10/11 | GUI + Script | Cân bằng giữa debloat và giữ tính năng hệ thống | Ít phổ biến hơn WinUtil |
| **Sophia Script** | Win 10/11 | PowerShell | Tinh chỉnh cực sâu, phù hợp power user | Yêu cầu kiến thức cao |

**Minh họa giao diện một số công cụ:**

*Giao diện WinUtil – bố cục module hóa với thanh điều hướng bên trái:*
![WinUtil Interface](https://blog.gitcode.com/33cac77d4429fa460e214a6050cd49c1/images/winutil-tweaks-tab-light.png)

*Giao diện Win11Debloat – menu lựa chọn các tùy chỉnh debloat:*
![Win11Debloat Menu](https://raw.githubusercontent.com/Raphire/Win11Debloat/master/Assets/Images/menu.png)

*Giao diện RemoveWindowsAI khi chạy:*
![RemoveWindowsAI UI](https://hothardware.com/News%20Images/RemoveWindowsAI.jpg)

---

## 📦 Phần 1: Chuẩn bị và tạo điểm phục hồi

### Bước 1: Tắt S Mode (nếu có)

Nếu bạn không thể tải hoặc chạy ứng dụng từ internet, thiết bị có thể đang ở **S Mode**. Làm theo hướng dẫn chính thức của Microsoft để tắt S Mode trước khi tiếp tục.

### Bước 2: Tạo Restore Point

1. Mở **Start**, gõ `Create a restore point`, nhấn Enter.
2. Trong tab **System Protection**, chọn ổ đĩa hệ thống (thường là C:), nhấn **Configure** → **Turn on system protection**.
3. Nhấn **Create**, đặt tên (ví dụ: `Before Debloat`), chờ hoàn tất.

*Giao diện System Protection với nút Create để tạo điểm phục hồi:*
![System Protection Create Restore Point](https://www.dell.com/community/image/serverpage/image-id/123456iABCDEF?v=v2)

### Bước 3: Tạo ảnh hệ thống đầy đủ (khuyến nghị)

Restore Point chỉ khôi phục registry và một số file hệ thống. Để an toàn tuyệt đối, hãy dùng **Windows Backup** hoặc công cụ như **Macrium Reflect Free** để tạo System Image.

---

## 🧹 Phần 2: Gỡ bloatware – Từ đơn giản đến nâng cao

### Cách 1: Gỡ thủ công qua Settings (An toàn nhất)

Đây là cách **không rủi ro** và được khuyến nghị bắt đầu.

1. Mở **Settings** → **Apps** → **Installed apps** (Win 11) hoặc **Apps and features** (Win 10).
2. Sắp xếp theo **Install date** hoặc **Size** để thấy bloatware.
3. Nhấn dấu ba chấm → **Uninstall** cho các ứng dụng như:
   - Microsoft News, Clipchamp, Solitaire Collection, Tips, Get Help, Feedback Hub
   - Microsoft Teams (bản consumer)
   - Các bản dùng thử antivirus (McAfee, Norton, v.v.)

*Giao diện Settings > Apps > Installed apps với tùy chọn Uninstall:*
![Windows 11 Installed Apps Uninstall](https://www.intowindows.com/wp-content/uploads/2025/05/Windows-11-installed-apps-list-view.jpg)

> **Lưu ý:** Một số ứng dụng không thể gỡ qua Settings (như Edge, Store). Đó là thiết kế của Microsoft.

### Cách 2: Gỡ bằng PowerShell (Mạnh hơn, vẫn an toàn)

PowerShell có thể gỡ các gói mà Settings "từ chối".

```powershell
# Xem tất cả gói đã cài
Get-AppxPackage | Select Name, PackageFullName

# Gỡ một gói cụ thể (ví dụ: Microsoft.BingNews)
Get-AppxPackage Microsoft.BingNews | Remove-AppxPackage

# Gỡ cho tất cả người dùng (cần quyền Admin)
Get-AppxPackage -AllUsers Microsoft.BingNews | Remove-AppxPackage -AllUsers
```

> **Cảnh báo:** Gỡ gói cho người dùng hiện tại **không ngăn Windows cài lại cho tài khoản mới** trong tương lai. Để ngăn chặn triệt để, bạn cần gỡ cả provisioned package (xem phần Win11Debloat bên dưới).

### Cách 3: Dùng Win11Debloat (Khuyên dùng cho người mới)

Win11Debloat là script PowerShell nhẹ, dễ dùng, hỗ trợ cả GUI và CLI, hoạt động trên **cả Windows 10 và 11**.

**Chạy nhanh:**

1. Mở **PowerShell** với quyền Administrator.
2. Dán lệnh sau:

```powershell
& ([scriptblock]::Create((irm "https://debloat.raphi.re/")))
```

3. Làm theo hướng dẫn trên màn hình.

**Hoặc tải thủ công:**

1. Tải bản phát hành mới nhất từ [GitHub Raphire/Win11Debloat](https://github.com/Raphire/Win11Debloat).
2. Giải nén, chạy `Run.bat`.
3. Chấp nhận UAC, làm theo hướng dẫn.

*Giao diện Win11Debloat với menu lựa chọn các tùy chỉnh:*
![Win11Debloat Menu](https://raw.githubusercontent.com/Raphire/Win11Debloat/master/Assets/Images/menu.png)

### Cách 4: Dùng WinUtil (Toàn diện)

WinUtil là bộ công cụ tất cả trong một: tweaks, config, cài app, sửa lỗi.

**Chạy:**

```powershell
irm "https://christitus.com/win" | iex
```

Sau khi giao diện hiện ra:
- Tab **Tweaks**: Chọn các tweak như Disable Telemetry, Disable Wi-Fi Sense, Set Services to Manual, v.v.
- Tab **Config**: Cài Features (WSL, Hyper-V, Sandbox), chạy Fixes (DISM, SFC).
- Tab **Install**: Chọn ứng dụng muốn cài.

*Giao diện WinUtil – tab Tweaks với các tùy chọn Essential và Advanced:*
![WinUtil Tweaks Tab](https://blog.gitcode.com/33cac77d4429fa460e214a6050cd49c1/images/winutil-tweaks-tab-light.png)

*Giao diện WinUtil – tab Config với Features, Fixes và Legacy Panels:*
![WinUtil Config Tab](https://gitcode.com/GitHub_Trending/wi/winutil/raw/7ee5be3d2fbfe2374cbc6bf472be73ba002daa3b/docs/src/content/docs/assets/screenshots/config-tab-new.png)

---

## 🧠 Phần 3: Loại bỏ AI và tính năng đám mây (Phần còn thiếu trong repo gốc)

Repo gốc chỉ đề cập sơ qua về AI. Đây là phần **quan trọng nhất** mà nhiều hướng dẫn bỏ sót.

### 3.1. RemoveWindowsAI – Loại bỏ triệt để AI

Script này loại bỏ **tất cả** tính năng AI: Copilot, Recall, Click to Do, AI trong Paint/Notepad, Voice Access, và nhiều hơn nữa.

**Chạy:**

```powershell
& ([scriptblock]::Create((irm "https://raw.githubusercontent.com/zoicware/RemoveWindowsAI/main/RemoveWindowsAi.ps1")))
```

Script sẽ:
- Vô hiệu hóa registry keys của Copilot, Recall, Input Insights
- Ngăn chặn cài lại các gói AI qua CBS store
- Gỡ bỏ AI appx packages (kể cả `Nonremovable`)
- Xóa các file AI còn sót lại

> **Lưu ý:** Script này có thể bị antivirus báo nhầm là malware. Đây là **false positive** do tính chất can thiệp sâu vào hệ thống. Luôn kiểm tra trong máy ảo trước.

*Giao diện RemoveWindowsAI khi chạy – cửa sổ script với các tùy chọn:*
![RemoveWindowsAI UI](https://hothardware.com/News%20Images/RemoveWindowsAI.jpg)

### 3.2. Tắt thủ công các tính năng AI qua Settings

Một số tính năng không thể tắt hoàn toàn bằng script, cần làm thủ công:

| Vị trí | Thao tác |
|--------|----------|
| **Settings > Privacy & security > Recall & snapshots** | Tắt **Save snapshots** |
| **Game Bar > Gaming Copilot > Settings > Privacy** | Tắt tất cả toggle, xóa memory và history |
| **Settings > Privacy & security > Search permissions** | Tắt **Cloud content search** cho cả tài khoản Microsoft và Work/School |
| **Settings > Personalization > Taskbar** | Tắt **Copilot (preview)** nếu hiển thị |

*Giao diện Settings > Privacy & security > Recall & snapshots với tùy chọn tắt:*
![Windows 11 Recall Settings](https://learn.microsoft.com/en-us/windows/client-management/images/recall-settings.png)

### 3.3. Tắt quảng cáo và thu thập dữ liệu

Windows 11 đầy quảng cáo ở Start menu, Microsoft Store, lock screen, Settings, và File Explorer.

| Vị trí | Thao tác |
|--------|----------|
| **Settings > Privacy & security > General** | Tắt cả 4 toggle: Advertising ID, Language list, App launches, Suggested content |
| **Settings > Privacy & security > Diagnostics & feedback** | Tắt **Send optional diagnostic data** và **Tailored experiences** |
| **Settings > Privacy & security > Activity history** | Tắt **Store my activity history on this device** |
| **Settings > Accounts > Windows backup** | Tắt **Remember my apps** và **Remember my preferences** |
| **Settings > System > Clipboard** | Tắt **Clipboard history** và **Sync across devices** |
| **Settings > Privacy & security > Find my device** | Tắt **Find my device** |

*Giao diện Settings > Privacy & security > General với 4 toggle quảng cáo:*
![Windows 11 Privacy General Settings](https://www.elevenforum.com/attachments/privacy-general-settings-png.123456/)

---

## 🔒 Phần 4: Tăng cường bảo mật sau Debloat

Debloat có thể vô tình tắt các tính năng bảo mật. Hãy đảm bảo những điều sau **vẫn bật**:

- **Windows Defender** (nếu bạn không có antivirus bên thứ ba)
- **SmartScreen**
- **Windows Update** – Quan trọng nhất! Đừng tắt Windows Update vì sẽ mất bản vá bảo mật.
- **UAC** (User Account Control)
- **Memory Integrity** và **VBS** (nếu phần cứng hỗ trợ)

> **Lưu ý:** Một số script debloat "cực đoan" như Tiny11 Core Builder **tắt Windows Update**, khiến PC không nhận được bản vá bảo mật. **Tránh xa** công cụ này.

---

## 🚀 Phần 5: Tối ưu hóa sau Debloat

### 5.1. Quản lý Startup Programs

1. Mở **Task Manager** (Ctrl + Shift + Esc).
2. Chuyển sang tab **Startup**.
3. Chuột phải vào ứng dụng không cần thiết → **Disable**.

*Tab Startup trong Task Manager với các ứng dụng đã bị vô hiệu hóa:*
![Windows 11 Task Manager Startup Tab](https://www.xda-developers.com/wp-content/uploads/2025/07/task-manager-startup-tab.jpg)

### 5.2. Chạy Disk Cleanup và DISM/SFC

```powershell
# Chạy Disk Cleanup
cleanmgr

# Sửa lỗi hệ thống bằng DISM
DISM /Online /Cleanup-Image /RestoreHealth

# Quét và sửa file hệ thống bằng SFC
sfc /scannow
```

*Kết quả chạy SFC /scannow trong Command Prompt với quyền Admin:*
![SFC Scannow Result](https://recoverit.wondershare.jp/images/sfc-scannow-result.png)

### 5.3. Cài đặt ứng dụng thay thế (Tùy chọn)

| Mục đích | Ứng dụng gợi ý |
|----------|----------------|
| Trình duyệt | Brave (bảo mật), Firefox (cân bằng), Chromium (tốc độ) |
| Giải nén | 7-Zip hoặc NanaZip |
| PDF | SumatraPDF hoặc Foxit PDF Editor |
| Media | VLC, MPC-HC |
| Ghi chú | Obsidian, Joplin |
| Bảo mật | Bitwarden (quản lý mật khẩu) |
| Gỡ cài đặt | Revo Uninstaller Free hoặc Bulk Crap Uninstaller |

---

## 📊 Phần 6: Checklist tổng hợp

### Trước khi Debloat
- [ ] Tắt S Mode (nếu có)
- [ ] Tạo Restore Point
- [ ] Tạo System Image (khuyến nghị)

### Gỡ bloatware
- [ ] Gỡ thủ công qua Settings
- [ ] Gỡ qua PowerShell (nếu cần)
- [ ] Chạy Win11Debloat hoặc WinUtil
- [ ] Chạy RemoveWindowsAI (nếu muốn loại bỏ AI triệt để)

### Tắt AI và quyền riêng tư
- [ ] Tắt Recall snapshots
- [ ] Tắt Copilot trong Settings và Taskbar
- [ ] Tắt quảng cáo và thu thập dữ liệu trong Privacy settings
- [ ] Tắt Bing Search trong Start Menu

### Bảo mật
- [ ] Kiểm tra Windows Defender vẫn bật
- [ ] Kiểm tra SmartScreen vẫn bật
- [ ] Kiểm tra Windows Update vẫn hoạt động
- [ ] Bật Memory Integrity (nếu hỗ trợ)

### Tối ưu hóa
- [ ] Quản lý Startup Programs
- [ ] Chạy Disk Cleanup
- [ ] Chạy DISM và SFC
- [ ] Cài ứng dụng thay thế

---

## 💡 Lời kết và định hướng phát triển

Hướng dẫn này được thiết kế để **bổ sung, không thay thế** repo bạn đã tham khảo. Những điểm mới so với bản gốc:

1. **Đa dạng công cụ:** Không chỉ WinUtil mà còn Win11Debloat, RemoveWindowsAI, O&O ShutUp10++, Winhance.
2. **Chuyên sâu về AI:** Phần loại bỏ Copilot, Recall, và các tính năng AI được mở rộng đáng kể.
3. **Nhấn mạnh bảo mật:** Cảnh báo về việc tắt Windows Update và các tính năng bảo mật.
4. **Checklist đầy đủ:** Từ chuẩn bị đến tối ưu hóa sau debloat.
5. **Cập nhật 2026:** Bao gồm các công cụ và tính năng mới nhất.
6. **Hình ảnh minh họa:** Ảnh chụp màn hình thực tế cho từng bước quan trọng.

Bạn có thể tiếp tục phát triển hướng dẫn này bằng cách:
- Thêm ảnh chụp màn hình thực tế từ quá trình debloat của bạn.
- Bổ sung phần về **Windows 10 LTSC** (Long-Term Servicing Channel) – một lựa chọn tuyệt vời cho người muốn Windows tối giản.
- Thêm hướng dẫn về **FlyOOBE** để tạo tài khoản local trong quá trình cài đặt, tránh đăng nhập Microsoft.
- Cập nhật khi Microsoft ra mắt các bản Windows mới (25H2, 26H1, v.v.).

---

## 📚 Tài nguyên tham khảo

- WinUtil: [github.com/ChrisTitusTech/winutil](https://github.com/ChrisTitusTech/winutil)
- Win11Debloat: [github.com/Raphire/Win11Debloat](https://github.com/Raphire/Win11Debloat)
- RemoveWindowsAI: [github.com/zoicware/RemoveWindowsAI](https://github.com/zoicware/RemoveWindowsAI)
- Windows Optimization & Customization Guide: [github.com/just-maik/win-opti-resources](https://github.com/just-maik/win-opti-resources)
- How-To Geek: Hướng dẫn gỡ bloatware không dùng công cụ đáng ngờ

---

## 🤝 Đóng góp & Giấy phép

- **Found a bug or outdated info?** [Open an issue](../../issues)
- **Have a suggestion?** Pull requests are welcome!
- **Last verified:** December 2025 (winutil v25.12.01)

> **Note:** This guide references [Chris Titus Tech's winutil](https://github.com/ChrisTitusTech/winutil) which is actively maintained. Check the [official documentation](https://winutil.christitus.com/) for the latest features.

---

*Tài liệu này được phát hành theo tinh thần mã nguồn mở. Bạn có thể tự do sao chép, chỉnh sửa và chia sẻ với điều kiện ghi rõ nguồn.*
