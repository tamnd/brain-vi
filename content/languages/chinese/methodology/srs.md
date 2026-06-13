---
title: "Lặp lại cách nhau (SRS)"
description: "Hướng dẫn đầy đủ về cách lặp lại cách quãng cho tiếng Quan Thoại: Thiết lập Anki, cài đặt tối ưu, những gì cần đưa vào SRS, tích hợp Pleco, Skritter và các mẹo về thói quen hàng ngày."
tags: ["chinese", "mandarin", "srs", "anki", "spaced repetition", "vocabulary", "methodology", "pleco", "skritter"]
cascade:
  type: docs
date: 2026-05-30T00:00:00+07:00
---

Hệ thống lặp lại ngắt quãng (SRS) là thói quen học tập có ROI cao nhất đối với từ vựng tiếng Quan Thoại. 15–20 phút thực hành Anki nhất quán hàng ngày sẽ hiệu quả hơn hàng giờ đọc lại hoặc ôn tập thụ động. 

--- 

## SRS là gì và tại sao nó hoạt động 

**Đường cong quên Ebbinghaus** (1885) mô tả cách trí nhớ suy giảm theo cấp số nhân sau lần học đầu tiên. Nếu không xem xét, bạn sẽ quên ~70% tài liệu mới trong vòng 24 giờ và ~90% trong vòng một tuần. 

SRS khai thác **hiệu ứng giãn cách**: xem lại thông tin vào đúng thời điểm bạn sắp quên sẽ hiệu quả hơn đáng kể so với việc lặp lại hàng loạt ("nhồi nhét"). Mỗi lần xem xét thành công sẽ đẩy khoảng thời gian xem xét tiếp theo tiến xa hơn trong tương lai:```
Day 0: Learn new card
Day 1: Review (interval extends to 3 days)
Day 4: Review (interval extends to 8 days)
Day 12: Review (interval extends to 21 days)
Day 33: Review (interval extends to ~2 months)
...
```Đối với từ vựng tiếng Trung, điều này có nghĩa là một từ bạn học hôm nay có thể được duy trì chỉ bằng một lần xem lại 10 giây cứ sau vài tháng sau khi nó được mã hóa sâu - thay vì học lại hàng tuần. 

--- 

## Khoảng tối ưu cho từ vựng tiếng Trung 

Từ vựng tiếng Trung cần được củng cố nhiều hơn một chút so với từ vựng tiếng Châu Âu vì: 
- Phải ghi nhớ các dạng ký tự (không phải cùng nguồn gốc) 
- Âm phải được ghi nhớ cùng với từ 
- Ngữ cảnh sử dụng rất quan trọng (đo lường từ, cụm từ) 

**Cài đặt Anki được đề xuất:** 

| Cài đặt | Giá trị | Lý do | 
|----------|-------|--------| 
| Thẻ mới mỗi ngày | 15–20 | Bền vững không bùng nổ tồn đọng | 
| Đánh giá tối đa mỗi ngày | 200 | Cap ngăn chặn những ngày quá sức | 
| Khoảng thời gian tốt nghiệp | 1 ngày | Khoảng cách ban đầu ngắn cho thẻ mới | 
| Khoảng thời gian dễ dàng | 4 ngày | Ngăn chặn các thẻ dễ biến mất | 
| Bắt đầu dễ dàng | 250% | Tiêu chuẩn; giảm xuống 230% nếu tỷ lệ giữ chân thấp | 
| Công cụ sửa đổi khoảng thời gian | 100% (mặc định) | Điều chỉnh nếu tỷ lệ giữ chân luôn ở mức trên 95% | 

Tỷ lệ duy trì mục tiêu: **90–93%**. Nếu bạn luôn ở mức trên 95% thì khoảng thời gian của bạn quá ngắn (tăng công cụ sửa đổi khoảng thời gian). Dưới 85%, giảm tỷ lệ thẻ mới và để các đánh giá bắt kịp. 

--- 

## Hướng dẫn thiết lập Anki 

### Bước 1: Tải Anki 

- **Máy tính để bàn** (Windows/Mac/Linux): Miễn phí tại [apps.ankiweb.net](https://apps.ankiweb.net/) 
- **Android**: AnkiDroid — Miễn phí trên Google Play 
- **iOS**: AnkiMobile — $24,99 mua một lần (tài trợ cho sự phát triển của Anki) 

Đồng bộ hóa với tài khoản AnkiWeb miễn phí để giữ cho tất cả các thiết bị được đồng bộ hóa. 

### Bước 2: Lấy bộ bài tốt nhất 

1. Mở Anki → **Được chia sẻ** (hoặc truy cập [ankiweb.net/shared/decks](https://ankiweb.net/shared/decks/)) 
2. Tìm kiếm: **"HSK 3.0"** — chọn bộ bài có âm thanh, ký tự và bính âm 
3. Cách khác: tìm kiếm **"Spoonfed Chinese"** để tìm bộ bài ở cấp độ câu (tốt hơn cho người học ở trình độ trung cấp) 
4. Nhập khẩu`.apkg`tập tin 

**Bộ bài được đề xuất:** 
- HSK 3.0 Complete (trình độ từ vựng, dành cho người mới bắt đầu) 
- Tiếng Trung Spoonfed (trình độ câu, tiếp thu ngữ pháp tốt hơn) 
- Tiếng Trung Hanping (bao gồm âm thanh từ Pleco) 

### Bước 3: Cài đặt thẻ 

Điều hướng đến bộ bài → **Tùy chọn**:```
Daily Limits:
  New cards/day: 15
  Maximum reviews/day: 200

New Cards:
  Learning steps: 1m 10m
  Graduating interval: 1
  Easy interval: 4
  Starting ease: 250%

Reviews:
  Maximum interval: 36500 (100 years — no artificial cap)
  Easy bonus: 130%
  Interval modifier: 100%
```### Bước 4: Định dạng thẻ 

Dạng thẻ từ vựng tiếng Trung lý tưởng: 

**Mặt trước:**```
[Simplified Chinese character(s)]
```**Mặt sau:**```
[Pinyin with tone marks]
[English translation]
[Example sentence in Chinese]
[Example sentence translation]
[Audio pronunciation]
```Ví dụ: 
- Mặt trước: 图书馆 
- Mặt sau: túshūguǎn/ thư viện/ 我在图书馆学习。/ Tôi học ở thư viện. 

Nhìn thấy ký tự đầu tiên (không có bính âm) sẽ buộc bạn phải chủ động nhớ lại — hướng nhớ lại hiệu quả nhất khi đọc tiếng Trung. 

--- 

## Nên đặt gì vào SRS so với không 

### Đưa vào SRS 

| Nội dung | Tại sao | 
|----------|------| 
| Từ vựng mới (chữ + bính âm + nghĩa) | Chức năng cốt lõi của SRS | 
| Các mẫu ngữ pháp làm ví dụ về câu | Nội hóa cấu trúc thông qua sự lặp lại | 
| Đo cặp từ + danh từ (一本书, 一张桌子) | Những cụm từ này phải được ghi nhớ | 
| Chengyu định nghĩa và câu ví dụ | Tỷ lệ quên cao mà không đánh giá cách nhau | 
| Các ký tự thường bị nhầm lẫn (买/卖, 已/己) | Phân biệt đối xử trực quan có mục tiêu | 

### KHÔNG đưa vào SRS 

| Nội dung | Tại sao không | 
|----------|----------| 
| Thứ tự đột quỵ | Thay vào đó hãy sử dụng Skritter - nó kiểm tra bản vẽ thực tế | 
| Giải thích quy tắc ngữ pháp | Các quy tắc là tài liệu tham khảo, không phải mục tiêu thu hồi | 
| Quy tắc phát âm | Học một lần; họ áp dụng một cách có hệ thống | 
| Những từ cực đơn giản bạn đã biết | Lãng phí thời gian xem xét | 
| Toàn bộ đoạn văn | Quá dài để có thẻ thu hồi hiệu quả | 

--- 

## Khai thác câu 

Khai thác câu có nghĩa là thêm thẻ từ vựng từ tài liệu bạn đang đọc hoặc nghe. Điều này hiệu quả hơn so với các bộ bài làm sẵn ở trình độ trung cấp+ vì: 
- Các từ xuất hiện trong ngữ cảnh mà bạn quan tâm 
- Câu ví dụ là câu bạn đã gặp rồi 
- Mật độ từ vựng phù hợp với trình độ hiện tại của bạn 

**Quy trình khai thác với Du Chinese:** 
1. Đọc một bài viết bằng tiếng Du Trung Quốc; nhấn vào những từ chưa biết 
2. Du Chinese hiện định nghĩa + câu ví dụ 
3. Xuất các từ được gắn cờ sang CSV 
4. Nhập CSV vào Anki dưới dạng thẻ mới 

**Quy trình khai thác với Pleco:** 
1. Gặp từ lạ trong bất kỳ văn bản nào 
2. Sao chép từ → Tra cứu Pleco → nhấn dấu sao để thêm vào danh sách flashcard 
3. Xuất danh sách flashcard Pleco → nhập vào Anki 
4. (Hoặc sử dụng trực tiếp hệ thống flashcard tích hợp của Pleco) 

--- 

## Tích hợp Pleco 

[Pleco](https://www.pleco.com/) là ứng dụng từ điển tiếng Trung thiết yếu. Nó có hệ thống flashcard SRS riêng có thể đóng vai trò thay thế hoặc bổ sung cho Anki. 

**Pleco → Xuất khẩu Anki:** 
1. Trong Pleco: **Flashcards** → **My Cards** → chọn thẻ để xuất 
2. Xuất dưới dạng tệp văn bản (.txt) sang thiết bị của bạn 
3. Trong Anki: **Nhập tệp** → ánh xạ các trường lên Mặt trước/Sau 
4. Kết quả: tất cả các từ có gắn dấu sao Pleco đều trở thành thẻ Anki có định nghĩa 

**Sử dụng hệ thống flashcard tích hợp của Pleco:** 
- Thiết lập đơn giản hơn Anki; ít tùy chọn cấu hình hơn 
- Tốt cho những người học muốn có ma sát tối thiểu 
- Thiếu hệ sinh thái Anki (plugin, loại thẻ tùy chỉnh, độ sâu thống kê) 

--- 

## Người trượt ván 

[Skritter](https://www.skritter.com/) là SRS dành riêng cho từng nhân vật với phản hồi về hành trình. Không giống như Anki, nó yêu cầu bạn viết ký tự bằng cách vẽ các nét theo đúng thứ tự - hệ thống sẽ phát hiện lỗi. 

**Khi nào nên sử dụng Skritter:** 
- Nếu bạn cần viết tay tiếng Trung (du học Trung Quốc, thư pháp, viết trang trọng) 
- Để phân biệt các ký tự giống nhau về mặt hình ảnh thông qua bộ nhớ động cơ 
- Là phần bổ sung cho Anki, không thay thế 

**Skritter không cần thiết nếu:** 
- Bạn chỉ cần đọc và gõ tiếng Trung (nhập bàn phím không cần nhớ nét) 
- Ngân sách bị giới hạn ($14,99/tháng đăng ký) 

--- 

## Phục hồi sau khi bị tụt lại phía sau 

Việc bỏ qua thậm chí 3 ngày sẽ gây ra tình trạng tồn đọng đánh giá đáng kể. Thiếu một tuần với 20 thẻ mới/ngày sẽ tạo ra 140 thẻ quá hạn cộng với thẻ đánh giá tích lũy - dễ dàng có hơn 300 đánh giá. 

**Giao thức phục hồi:** 
1. **Dừng tất cả các thẻ mới ngay lập tức** — đặt thẻ mới/ngày thành 0 
2. **Xóa hồ sơ tồn đọng trước** — xử lý các đánh giá quá hạn trong vài ngày 
3. **Tiếp tục thẻ mới từ từ** — khởi động lại ở mức 10/ngày, không phải mức trước đó của bạn 
4. **Không bao giờ xóa thẻ** — thà chôn tạm còn hơn xóa; thẻ bị chôn vùi tự động trở lại 

**Phòng ngừa:** Đặt báo thức ôn tập hàng ngày vào cùng một thời điểm mỗi ngày (buổi sáng là tối ưu — quá trình củng cố trí nhớ diễn ra qua đêm). Thậm chí 10 phút còn hơn không. 

--- 

## Thói quen SRS hàng ngày

**15–20 phút mỗi sáng, trước khi học bài khác.** 

Vị trí này quan trọng: 
- Đánh giá được thực hiện dựa trên ngày hôm trước; làm chúng vào buổi sáng sẽ giảm thiểu thời gian quá hạn 
- Ý chí cao nhất vào buổi sáng 
- Hoàn thành SRS trước tiên sẽ loại bỏ cảm giác tội lỗi và dành thời gian học còn lại để nhập liệu 

**Tập hợp thói quen:** Gắn Anki vào thói quen buổi sáng hiện có. Cà phê + Anki. Đi lại + Anki (di động). Ăn sáng + Anki. Yếu tố kích hoạt khiến thói quen trở nên tự động. 

**Đừng để hàng đợi tăng lên.** Một đơn hàng tồn đọng 300 thẻ sẽ mất ~40 phút để giải quyết và tiêu diệt động lực. Cái giá của một ngày bỏ qua là rất cao. Tính nhất quán quan trọng hơn số lượng mỗi phiên. 

--- 

## Xem thêm 

- [Roadmap](roadmap/) — Khi nào nên thêm SRS vào lịch học của bạn 
- [Những sai lầm thường gặp](common-mistakes/) — Quá dựa dẫm vào SRS như một phương pháp nghiên cứu duy nhất 
- [Từ vựng](../vocabulary/) — Danh sách từ vựng HSK 
- [Tài nguyên](../resources/) — Liên kết Pleco, Skritter và Anki
