---
title: "CF 105137A - Mục Tiêu Tốt"
description: "Chúng tôi đang mô phỏng một hệ thống tính điểm cricket đơn giản hóa, trong đó mỗi quả bóng đóng góp một số lượt chạy cố định. Người đánh bóng chỉ có thể ghi được 4 hoặc 6 lần chạy cho mỗi quả bóng và chúng tôi muốn đạt được ít nhất điểm mục tiêu $n$."
date: "2026-06-27T18:43:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105137
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #30 (Good-Forces)"
rating: 0
weight: 105137
solve_time_s: 71
verified: false
draft: false
---

[CF 105137A - Mục tiêu tốt](https://codeforces.com/problemset/problem/105137/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống tính điểm cricket đơn giản hóa, trong đó mỗi quả bóng đóng góp một số lượt chạy cố định. Người đánh bóng chỉ có thể ghi được 4 hoặc 6 lần chạy cho mỗi quả bóng và chúng tôi muốn đạt được ít nhất số điểm mục tiêu$n$. Mỗi quả bóng độc lập và luôn tạo ra một trong hai kết quả này. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là xác định hai đại lượng. Đầu tiên, số lượng bóng tối thiểu cần thiết để đạt được tổng số điểm ít nhất là$n$. Thứ hai, số lượng bóng tối đa cần thiết để vẫn đạt ít nhất$n$, giả sử chúng ta chọn kết quả cho điểm một cách tối ưu cho từng mục tiêu. 

Các ràng buộc là nhỏ, với$n \le 1000$và lên tới 100 trường hợp thử nghiệm. Điều này loại trừ bất cứ điều gì nặng hơn công việc tuyến tính không đổi hoặc rất nhỏ cho mỗi trường hợp thử nghiệm. Một lý luận tham lam trực tiếp hoặc tính toán dạng đóng cho mỗi trường hợp thử nghiệm là đủ. 

Trường hợp cạnh tinh tế xuất hiện khi$n$không thể chia hết cho 4 hoặc 6 tổ hợp. Vì mỗi quả bóng đóng góp ít nhất 4 lần chạy nên số lượng quả bóng luôn bị giới hạn giữa$\lceil n/6 \rceil$Và$\lceil n/4 \rceil$, nhưng chúng ta phải cẩn thận vì việc trộn lẫn 4s và 6s có thể làm thay đổi tính khả thi xung quanh các giá trị nhỏ. Ví dụ, nếu$n = 10$, hai quả bóng là đủ bằng cách sử dụng$4 + 6$, nhưng chỉ sử dụng 4s hoặc chỉ 6s sẽ làm sai lệch một công thức ngây thơ. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là thử tất cả các chuỗi quả bóng, tăng dần số lượng quả bóng.$k$và kiểm tra xem liệu sự kết hợp nào đó của số 4 và số 6 có thể đạt được ít nhất$n$. Đối với mỗi cố định$k$, chúng tôi sẽ liệt kê tất cả$2^k$bài tập 4 hoặc 6 và tính tổng. Điều này nhanh chóng trở nên không khả thi ngay cả đối với$k$, từ$k$có thể lớn tới 250 khi$n$là 1000 và tất cả điểm là 4. 

Quan sát quan trọng là chỉ có hai giá trị tồn tại và cả hai đều dương. Điều này biến vấn đề thành suy luận về các công trình cực trị. Để giảm thiểu số lần chạy trên mỗi quả bóng, chúng tôi muốn tối đa hóa số lần chạy trên mỗi quả bóng, vì vậy chúng tôi luôn chọn 6. Để tối đa hóa số lần chạy trên mỗi quả bóng, chúng tôi muốn giảm thiểu số lần chạy trên mỗi quả bóng trong khi vẫn khả thi, vì vậy, chúng tôi luôn chọn 4, ngoại trừ việc chúng tôi có thể cần điều chỉnh lần cuối vì các hạn chế vượt quá tương tác với yêu cầu "ít nhất n". 

Vì vậy, vấn đề giảm xuống còn hai phần trần: 

số lượng bóng tối thiểu đạt được bằng cách sử dụng tất cả 6 bóng và đạt được số lượng tối đa bằng cách sử dụng cả 4 bóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo trình tự | O(2^n) | O(n) | Quá chậm | 
| Lý luận cực đoan tham lam | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bóng tối thiểu 

1. Tính xem cần bao nhiêu quả bóng nếu mỗi quả bóng ghi được 6 lần chạy. 

Điều này là tối ưu vì 6 là mức đóng góp lớn nhất có thể có cho mỗi quả bóng, do đó nó giảm thiểu số lượng quả bóng cần thiết để đạt được ít nhất$n$. 
2. Vì chúng ta cần ít nhất$n$chạy, chúng tôi lấy số nguyên nhỏ nhất$k$như vậy$6k \ge n$. 

Điều này được tính như$\lceil n/6 \rceil$. 

### Số bóng tối đa 

1. Tính xem cần bao nhiêu quả bóng nếu mỗi quả bóng ghi được 4 lần chạy. 

Điều này tối đa hóa số lượng bóng vì 4 là số lượng đóng góp nhỏ nhất có thể có cho mỗi quả bóng. 
2. Ta lấy số nguyên nhỏ nhất$k$như vậy$4k \ge n$. 

Đây là$\lceil n/4 \rceil$. 

### Tại sao nó hoạt động 

Hệ thống tính điểm rất đơn điệu: mỗi quả bóng bổ sung sẽ làm tăng tổng số lần chạy lên 4 hoặc 6. Để giảm thiểu số bóng, việc thay thế 4 bóng bất kỳ bằng 6 chỉ có thể giảm hoặc bảo toàn số lượng bóng cần thiết, không bao giờ tăng nó. Ngược lại, để tối đa hóa số bóng, việc thay thế 6 bóng bất kỳ bằng 4 chỉ có thể tăng hoặc bảo toàn số lượng bóng cần thiết. Vì không có ràng buộc nào về việc phân phối 4 và 6 ngoài tổng tổng, các phép gán thống nhất cực trị đã xác định các giới hạn tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())
        
        # minimum balls: maximize per-ball score (6)
        min_balls = (n + 5) // 6
        
        # maximum balls: minimize per-ball score (4)
        max_balls = (n + 3) // 4
        
        print(min_balls, max_balls)

if __name__ == "__main__":
    solve()
```Việc thực hiện chuyển trực tiếp hai phép chia trần thành số học số nguyên. biểu hiện$(n + 5) // 6$tính toán$\lceil n/6 \rceil$, Và$(n + 3) // 4$tính toán$\lceil n/4 \rceil$. Sử dụng phép toán số nguyên sẽ tránh được lỗi dấu phẩy động và giữ cho thời gian giải không đổi cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 10$Bóng tối thiểu: 

| Bước | Giá trị | 
| --- | --- | 
| Tính trần(10/6) | 2 | 
| Giải thích | 6 + 4 đạt 10 | 

Số bóng tối đa: 

| Bước | Giá trị | 
| --- | --- | 
| Tính trần(10/4) | 3 | 
| Giải thích | 4 + 4 + 4 đạt ít nhất 10 | 

Điều này cho thấy việc trộn 4 và 6 không làm thay đổi giới hạn cực trị; giới hạn chỉ phụ thuộc vào các điểm cực trị của mỗi quả bóng. 

### Ví dụ 2:$n = 21$Bóng tối thiểu: 

| Bước | Giá trị | 
| --- | --- | 
| trần(21/6) | 4 | 
| Xây dựng | 6 + 6 + 6 + 6 = 24 | 

Số bóng tối đa: 

| Bước | Giá trị | 
| --- | --- | 
| trần(21/4) | 6 | 
| Xây dựng | 4 + 4 + 4 + 4 + 4 + 4 = 24 | 

Điều này khẳng định cả hai giới hạn vẫn đảm bảo đạt được ít nhất$n$, ngay cả khi chúng vượt quá giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một phép tính thời gian không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ | 

Lời giải dễ dàng nằm trong giới hạn vì$t \le 100$và mỗi trường hợp chỉ yêu cầu hai phép tính số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input().strip())
        min_balls = (n + 5) // 6
        max_balls = (n + 3) // 4
        out.append(f"{min_balls} {max_balls}")
    return "\n".join(out)

# provided sample
assert run("4\n10\n20\n30\n40\n") == "2 3\n4 5\n5 8\n7 10"

# custom cases
assert run("1\n1\n") == "1 1", "minimum edge"
assert run("1\n4\n") == "1 1", "exact 4 boundary"
assert run("1\n6\n") == "1 2", "mix boundary"
assert run("1\n1000\n") == "167 250", "large boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 1 | điểm nhỏ nhất có thể | 
| 4 | 1 1 | quả bóng đơn chạy 4 lần chính xác | 
| 6 | 1 2 | chuyển đổi giữa chế độ 4 và 6 | 
| 1000 | 167 250 | độ đúng giới hạn trên | 

## Vỏ cạnh 

cho$n = 1, 2, 3$, công thức vẫn hoạt động chính xác mặc dù không có một quả bóng nào có thể khớp chính xác với mục tiêu. Vì$n = 3$, quả bóng tối thiểu là$\lceil 3/6 \rceil = 1$và tối đa là$\lceil 3/4 \rceil = 1$, phản ánh chính xác rằng một quả bóng đã vượt quá mục tiêu. 

Vì$n = 4$, cả hai giới hạn đều giảm xuống 1, vì một quả bóng chạy 4 lần là đủ và cũng cần thiết cho việc xây dựng tối đa. 

Đối với lớn$n = 1000$, thuật toán tạo ra$167$số bóng tối thiểu sử dụng tất cả 6s và$250$bóng tối đa sử dụng tất cả 4s. Cả hai công trình đều vượt quá hoặc đáp ứng mục tiêu theo yêu cầu và không có hỗn hợp nào có thể phá vỡ các giới hạn cực đoan này vì việc thay thế bất kỳ số 6 nào bằng số 4 sẽ làm tăng nghiêm ngặt số lượng bóng cần thiết và thay thế bất kỳ số 4 nào bằng số 6 sẽ làm giảm nghiêm ngặt số bóng đó.
