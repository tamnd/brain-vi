---
title: "CF 265A - Đá đầy màu sắc (Phiên bản đơn giản)"
description: "Bài toán mô tả một hàng đá, mỗi viên có màu đỏ, xanh lá cây hoặc xanh lam, được biểu thị bằng một chuỗi ký tự 'R', 'G' và 'B'. Squirrel Liss bắt đầu trên viên đá đầu tiên và thực hiện một chuỗi các hướng dẫn dựa trên màu sắc, được đưa ra bởi chuỗi thứ hai t."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 265
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 162 (Div. 2)"
rating: 800
weight: 265
solve_time_s: 77
verified: false
draft: false
---

[CF 265A - Đá đầy màu sắc (Phiên bản đơn giản)](https://codeforces.com/problemset/problem/265/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một hàng đá, mỗi viên đá có màu đỏ, xanh lá cây hoặc xanh lam, được biểu thị bằng một chuỗi`s`của các ký tự 'R', 'G' và 'B'. Squirrel Liss bắt đầu trên viên đá đầu tiên và thực hiện một chuỗi các hướng dẫn dựa trên màu sắc, được đưa ra bởi chuỗi thứ hai`t`. Mỗi hướng dẫn tương ứng với một màu: nếu viên đá Liss hiện đang ở phù hợp với màu của hướng dẫn, cô ấy sẽ di chuyển về phía trước bằng một viên đá. Nếu không thì cô ấy sẽ ở lại. Chúng ta được yêu cầu tính toán vị trí cuối cùng của Liss sau khi thực hiện tất cả các hướng dẫn, với các vị trí được đánh số bắt đầu từ một. 

Các chuỗi ngắn, mỗi chuỗi tối đa 50 ký tự, cho phép mô phỏng trực tiếp các hướng dẫn mà không phải lo lắng về giới hạn thời gian. Mỗi lệnh chỉ yêu cầu một so sánh duy nhất và nếu có thể, một mức tăng. Với 50 hướng dẫn và 50 viên đá, điều này dẫn đến tối đa 50 thao tác, không đáng kể trong giới hạn thời gian 2 giây. 

Trường hợp khó phát hiện khi nhiều hướng dẫn không khớp với màu đá hiện tại. Ví dụ, nếu`s = "RGB"`Và`t = "GGG"`, Liss sẽ không bao giờ di chuyển khỏi đá 1. Một cạm bẫy tiềm ẩn khác là xử lý đúng cách việc lập chỉ mục dựa trên 1; Việc xử lý mảng dựa trên số 0 ngây thơ có thể dẫn đến từng lỗi một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng trực tiếp chuyển động của Liss. Chúng ta bắt đầu với một biến`pos = 0`cho viên đá đầu tiên (dựa trên số 0 để tính toán nội bộ). Đối với mỗi hướng dẫn, chúng tôi kiểm tra xem`s[pos]`phù hợp với hướng dẫn. Nếu có, chúng tôi tăng`pos`. Điều này mô hình hóa chính xác các quy tắc và dễ thực hiện. Số lượng so sánh bằng độ dài của`t`, vì vậy trong trường hợp xấu nhất, chúng tôi thực hiện 50 thao tác, điều này có thể chấp nhận được. 

Không cần thuật toán phức tạp hơn vì kích thước đầu vào cực kỳ nhỏ. Cái nhìn sâu sắc quan trọng là đây là một vấn đề mô phỏng thuần túy; không tính toán trước, cấu trúc dữ liệu hoặc chiến lược tham lam cải thiện hiệu suất. Giải pháp tối ưu giống như mô phỏng brute-force nhưng phải xử lý cẩn thận đầu ra dựa trên 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu / Mô phỏng | O( | t | ) | 
| Tối ưu hóa | O( | t | ) | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`pos = 0`đại diện cho chỉ số đá hiện tại của Liss (dựa trên số 0). Lựa chọn này đơn giản hóa việc lập chỉ mục chuỗi trong Python. 
2. Lặp lại từng ký tự`instruction`trong chuỗi`t`. Mỗi ký tự đại diện cho một hướng dẫn màu sắc. 
3. Đối với mỗi`instruction`, kiểm tra xem`s[pos]`phù hợp với nó. Điều này mô hình hóa quy tắc Liss chỉ di chuyển nếu hướng dẫn phù hợp với màu đá. 
4. Nếu trùng khớp thì tăng dần`pos`bởi một. Nếu không, không làm gì cả và chuyển sang hướng dẫn tiếp theo. 
5. Sau khi xử lý tất cả các lệnh, xuất ra`pos + 1`để chuyển đổi từ vị trí dựa trên 0 sang vị trí dựa trên một. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`pos`luôn đại diện cho chỉ số đá hiện tại của Liss. Mỗi lệnh được áp dụng theo thứ tự và chuyển động chỉ xảy ra khi được phép. Bởi vì chúng tôi lặp lại các hướng dẫn một cách tuần tự nên không có chuyển động nào bị bỏ qua hoặc lặp lại không chính xác. Đầu ra cuối cùng chuyển chính xác từ việc lập chỉ mục dựa trên số 0 bên trong sang đánh số dựa trên một của vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
t = input().strip()

pos = 0  # zero-based index

for instruction in t:
    if s[pos] == instruction:
        pos += 1

print(pos + 1)
```Giải pháp trước tiên sẽ đọc và loại bỏ cả hai chuỗi để loại bỏ bất kỳ dòng mới nào ở cuối. các`pos`biến theo dõi vị trí hiện tại, bắt đầu từ 0. Vòng lặp for lặp qua từng lệnh, thực hiện một kiểm tra đẳng thức duy nhất và mức tăng tùy chọn. Cuối cùng, kết quả được in dưới dạng chỉ mục dựa trên 1. Cần chú ý chỉ tăng theo các hướng dẫn khớp để tránh các lỗi sai lệch. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
s = "RGB"
t = "RRR"
```| Bước | tư thế | hướng dẫn | s[pos] == hướng dẫn? | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | R | R == R | pos += 1 → pos = 1 | 
| 2 | 1 | R | G == R | không di chuyển | 
| 3 | 1 | R | G == R | không di chuyển | 

Đầu ra cuối cùng: dựa trên 1`pos + 1 = 2`. 

Dấu vết này cho thấy các hướng dẫn lặp đi lặp lại không khớp với màu đá hiện tại sẽ bị bỏ qua một cách chính xác. 

### Mẫu 2 

đầu vào:```
s = "RGBB"
t = "RGBB"
```| Bước | tư thế | hướng dẫn | s[pos] == hướng dẫn? | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | R | R == R | vị trí = 1 | 
| 2 | 1 | G | G == G | vị trí = 2 | 
| 3 | 2 | B | B == B | vị trí = 3 | 
| 4 | 3 | B | B == B | vị trí = 4 | 

Đầu ra cuối cùng: dựa trên 1`pos + 1 = 5`, điều đó đúng. Điều này xác nhận sự gia tăng thích hợp ở mỗi trận đấu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | t | 
| Không gian | O(1) | Chỉ có một biến chỉ số duy nhất`pos`là cần thiết; chuỗi chỉ là đầu vào. | 

Với các ràng buộc (tối đa 50 viên đá và 50 hướng dẫn), thuật toán này chạy trong micro giây, nằm trong giới hạn 2 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    t = input().strip()
    pos = 0
    for instruction in t:
        if s[pos] == instruction:
            pos += 1
    return str(pos + 1)

# Provided sample
assert run("RGB\nRRR\n") == "2", "sample 1"

# Custom cases
assert run("R\nR\n") =
```
