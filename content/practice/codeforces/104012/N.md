---
title: "CF 104012N - Thời Gian Mới"
description: "Chúng ta có hai khoảnh khắc trong một ngày được ghi trên đồng hồ kỹ thuật số 24 giờ. Đầu tiên là thời gian hiển thị hiện tại và thứ hai là thời gian chính xác của mục tiêu."
date: "2026-07-02T05:10:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104012
codeforces_index: "N"
codeforces_contest_name: "2022-2023 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104012
solve_time_s: 42
verified: true
draft: false
---

[CF 104012N - Thời gian mới](https://codeforces.com/problemset/problem/104012/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai khoảnh khắc trong một ngày được ghi trên đồng hồ kỹ thuật số 24 giờ. Đầu tiên là thời gian hiển thị hiện tại và thứ hai là thời gian chính xác của mục tiêu. Đồng hồ chỉ có thể được thay đổi về phía trước bằng hai thao tác: một nút tăng thời gian đúng một phút và nút kia tăng thời gian đúng một giờ. Cả hai hoạt động đều diễn ra trong ngày 24 giờ, do đó, chuyển tiếp qua 23:59 sẽ trở về 00:00. 

Nhiệm vụ là xác định số lần nhấn nút tối thiểu cần thiết để chuyển thời gian hiển thị thành thời gian mục tiêu. 

Một cách hữu ích để hình dung về không gian trạng thái là đồng hồ có 24 × 60 = 1440 trạng thái riêng biệt và cả hai phép toán đều tiến lên dọc theo cấu trúc hình tròn này. Mỗi phút nhấn tiến lên 1 trạng thái, mỗi giờ nhấn tiến thêm 60 trạng thái theo modulo 1440. 

Các ràng buộc đủ nhỏ để bất kỳ phương pháp nào chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm đều đủ. Ngay cả việc tìm kiếm đường đi ngắn nhất bằng vũ lực trên tất cả 1440 trạng thái cũng không đáng kể, nhưng cấu trúc cho phép một giải pháp dạng đóng thậm chí còn đơn giản hơn. 

Trường hợp cạnh tinh tế phát sinh từ hành vi bao quanh. Ví dụ: chuyển đổi 23:59 thành 00:00 chỉ là một lần nhấn một phút chứ không phải là một sự điều chỉnh lùi lớn. Một trường hợp góc khác là khi thời gian mục tiêu trong ngày sớm hơn thời gian hiện tại theo thứ tự thông thường, vì chuyển động về phía trước phải vượt qua nửa đêm. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô hình hóa vấn đề này như một bài toán đường đi ngắn nhất trên biểu đồ hình tròn gồm 1440 nút, trong đó mỗi nút tương ứng với một thời gian trong ngày. Từ mỗi nút, có hai cạnh đi ra: một đến phút tiếp theo và một đến giờ tiếp theo. Chúng tôi có thể chạy BFS từ thời điểm bắt đầu cho đến khi đạt được thời gian mục tiêu. Điều này đúng vì mọi thao tác đều có chi phí như nhau và BFS đảm bảo đường đi ngắn nhất trong biểu đồ không có trọng số. 

Tuy nhiên, BFS không cần thiết ở đây vì cấu trúc đồ thị cực kỳ đều đặn. Bất kỳ chuỗi thao tác nào cũng tương ứng với việc chọn tổng số bước giờ và bước phút có hiệu ứng kết hợp là độ dịch chuyển về phía trước modulo 1440. Nếu chúng ta biểu thị sự khác biệt về số phút giữa hai thời gian là D, thì vấn đề sẽ giảm xuống biểu thị D dưới dạng kết hợp của các bước tiến 60 phút và 1 phút với số lượng thao tác tối thiểu. 

Quan sát quan trọng là di chuyển theo giờ luôn hiệu quả hơn di chuyển 60 phút, vì máy ép một giờ thay thế máy ép 60 phút bằng một thao tác duy nhất. Do đó, đối với khoảng cách chuyển tiếp cố định D, chiến lược tối ưu là sử dụng càng nhiều lượt di chuyển trong giờ càng tốt, sau đó là các bước di chuyển theo phút trong thời gian còn lại. 

Vì vậy, bài toán rút gọn về việc tính khoảng cách chuyển tiếp theo phút modulo 1440, sau đó giảm thiểu: 

tầng(D/60) + (D mod 60). 

Điều này giúp loại bỏ mọi nhu cầu về tìm kiếm hoặc lập trình động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS trên 1440 tiểu bang | O(1440) | O(1440) | Được chấp nhận nhưng không cần thiết | 
| Phân rã số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi cả thời gian đầu vào thành tổng số phút từ 00:00. Điều này đưa ra một biểu diễn số nguyên duy nhất cho mỗi lần, loại bỏ nhu cầu xử lý giờ và phút riêng biệt. Sự biến đổi là`hh * 60 + mm`. 
2. Tính chênh lệch kỳ hạn`D`từ thời điểm hiện tại đến thời gian mục tiêu trên đồng hồ tròn 1440 phút. Nếu thời gian mục tiêu chậm hơn thời gian hiện tại, chúng tôi sẽ kết thúc bằng cách cộng 1440 trước khi trừ đi. Điều này đảm bảo chúng tôi luôn chỉ đo chuyển động về phía trước. 
3. Khi đã biết D, hãy tính số lần nhảy cả giờ phù hợp với nó bằng cách sử dụng phép chia số nguyên`D // 60`. Mỗi lần nhảy như vậy tương ứng với một lần nhấn nút B. 
4. Tính số phút còn lại sau khi sử dụng tính năng nhảy giờ bằng cách sử dụng`D % 60`. Mỗi phút còn lại cần nhấn nút A một lần. 
5. Cộng hai số này để có tổng số lần nhấn nút. 
6. Xuất kết quả. 

Lý do điều này hoạt động là vì cả hai thao tác chỉ tiến về phía trước, do đó, bất kỳ chuỗi thao tác hợp lệ nào đều tương ứng chính xác với việc phân tách D thành các đoạn 60 phút và các đoạn 1 phút. Không có hiệu ứng tương tác giữa các hoạt động, vì vậy việc tối đa hóa số bước giờ một cách tham lam không thể làm giải pháp trở nên tồi tệ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse(t):
    h = int(t[:2])
    m = int(t[3:])
    return h * 60 + m

cur = parse(input().strip())
tar = parse(input().strip())

diff = (tar - cur) % (24 * 60)

ans = diff // 60 + diff % 60

print(ans)
```Giải pháp bắt đầu bằng cách phân tích định dạng HH:MM thành phút. Điều này tránh logic nổi và giữ mọi thứ dựa trên số nguyên. Hoạt động modulo đảm bảo hành vi bao trùm trong chu kỳ 24 giờ, xử lý các trường hợp mục tiêu sớm hơn thời gian hiện tại. 

Bước phân tách rất đơn giản: phép chia số nguyên sẽ trích ra số lần nhấn cả giờ là hữu ích và phần còn lại tính cho số phút còn sót lại. Không có vấn đề đặt hàng nào phát sinh vì cả hai hoạt động đều được cộng dồn nghiêm ngặt trong thời gian chuyển tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1: 11:57 → 12:00 

Chúng tôi chuyển đổi thời gian thành phút: 

| Bước | Hiện tại | Mục tiêu | Sự khác biệt | 
| --- | --- | --- | --- | 
| Chuyển đổi | 717 | 720 | - | 
| Khác biệt thô | - | - | 3 | 
| Bản 1440 | - | - | 3 | 

Bây giờ chúng tôi phân hủy 3 phút: 

| Số giờ sử dụng | Số phút còn lại | Tổng số máy ép | 
| --- | --- | --- | 
| 0 | 3 | 3 | 

Điều này phù hợp với việc nhấn nút phút ba lần. 

Dấu vết cho thấy rằng khi khoảng cách nhỏ hơn 60, không sử dụng chuyển động theo giờ. 

### Ví dụ 2: 19:44 → 08:50 

Chuyển đổi sang phút: 

19:44 = 1184 

08:50 = 530 

Chúng tôi tính toán sự khác biệt về phía trước: 

| Bước | Giá trị | 
| --- | --- | 
| Cur | 1184 | 
| Tar | 530 | 
| Khác biệt (bọc) | 530 - 1184 + 1440 = 786 | 

Bây giờ phân hủy 786: 

| Số giờ sử dụng | Số phút còn lại | Tổng số máy ép | 
| --- | --- | --- | 
| 13 | 6 | 19 | 

Vậy đáp án là 13 + 6 = 19. 

Điều này thể hiện trường hợp bao quanh trong đó đường đi chính xác vượt qua nửa đêm và bước modulo đảm bảo tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ sử dụng các biến số nguyên | 

Về nguyên tắc, các ràng buộc của vấn đề cho phép đầu vào rất lớn, nhưng giải pháp là thời gian không đổi cho mỗi trường hợp, do đó, nó phù hợp thoải mái trong giới hạn ngay cả đối với nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def parse(t):
        h = int(t[:2])
        m = int(t[3:])
        return h * 60 + m

    cur = parse(sys.stdin.readline().strip())
    tar = parse(sys.stdin.readline().strip())

    diff = (tar - cur) % (24 * 60)
    return str(diff // 60 + diff % 60)

# provided samples
assert run("11:57\n12:00\n") == "3"
assert run("09:09\n21:21\n") == str(( (21*60+21) - (9*60+9) ) % 1440 // 60 + ( (21*60+21) - (9*60+9) ) % 1440 % 60)

# custom cases
assert run("00:00\n00:00\n") == "0"
assert run("23:59\n00:00\n") == "1"
assert run("01:00\n02:00\n") == "1"
assert run("10:10\n09:09\n") == str(( (9*60+9)-(10*60+10) ) % 1440 // 60 + ( (9*60+9)-(10*60+10) ) % 1440 % 60)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 00:00 → 00:00 | 0 | trường hợp chuyển động bằng không | 
| 23:59 → 00:00 | 1 | phút quấn nửa đêm | 
| 01:00 → 02:00 | 1 | tối ưu di chuyển giờ thuần túy | 
| 10:10 → 09:09 | tính toán | đảo ngược bọc đúng cách | 

## Vỏ cạnh 

Trường hợp quan trọng là khi thời gian giống hệt nhau. Trong trường hợp này, chênh lệch bằng 0 và đóng góp của cả giờ và phút đều bằng 0, do đó kết quả đầu ra chính xác bằng 0. Việc tính toán modulo tự nhiên bảo toàn kết quả này mà không cần viết hoa đặc biệt. 

Một trường hợp khác là quá nửa đêm. Ví dụ: 23:59 đến 00:00 trở thành chênh lệch 1 phút sau khi áp dụng modulo 1440. Thuật toán xử lý điều này vì phép trừ sẽ âm và hiệu chỉnh modulo sẽ chuyển nó thành khoảng cách thuận chính xác. 

Trường hợp cuối cùng là khi mục tiêu đi trước đúng một giờ nhưng không căn chỉnh theo ranh giới giờ, chẳng hạn như 10:10 đến 11:09. Sự khác biệt là 59 phút, do đó không sử dụng máy ép giờ và giải pháp chính xác là ưu tiên máy ép 59 phút hơn máy ép một giờ sẽ vượt quá mức.
