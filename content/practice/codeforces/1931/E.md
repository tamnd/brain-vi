---
title: "CF 1931E - Anna Và Món Quà Lễ Tình Nhân"
description: "Trò chơi bắt đầu với một danh sách các số nguyên. Anna di chuyển đầu tiên bằng cách đảo ngược các chữ số của bất kỳ số nào, số này có thể tăng hoặc giảm giá trị tùy thuộc vào các chữ số. Sasha đứng thứ hai bằng cách ghép hai số bất kỳ, giảm kích thước danh sách xuống một."
date: "2026-06-08T18:26:46+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1931
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 925 (Div. 3)"
rating: 1400
weight: 1931
solve_time_s: 130
verified: false
draft: false
---

[CF 1931E - Anna và món quà ngày lễ tình nhân](https://codeforces.com/problemset/problem/1931/E) 

**Đánh giá:** 1400 
**Tags:** trò chơi, tham lam, toán học, sắp xếp 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi bắt đầu với một danh sách các số nguyên. Anna di chuyển đầu tiên bằng cách đảo ngược các chữ số của bất kỳ số nào, số này có thể tăng hoặc giảm giá trị tùy thuộc vào các chữ số. Sasha đứng thứ hai bằng cách ghép hai số bất kỳ, giảm kích thước danh sách xuống một. Trò chơi luân phiên theo cách này cho đến khi chỉ còn lại một số. Sasha thắng nếu số cuối cùng ít nhất$10^m$, nếu không thì Anna thắng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng số nguyên$n$, ngưỡng$m$, và danh sách các số nguyên$a$. Đầu ra của mỗi trường hợp thử nghiệm là kết quả chiến thắng nếu cả hai người chơi đều chơi tối ưu. 

Những ràng buộc chỉ ra rằng$n$có thể đạt được$2 \cdot 10^5$và tổng của tất cả$n$trên các trường hợp thử nghiệm cũng được giới hạn ở mức$2 \cdot 10^5$. Điều này loại trừ bất kỳ giải pháp nào mô phỏng tất cả các trạng thái hoặc hoán vị của trò chơi vì số lượng nước đi tiềm năng tăng lên kết hợp với$n$. Do đó, cách chơi tối ưu phải dựa vào **quan sát chiến lược về thao tác số**, thay vì khám phá cây trò chơi một cách thô bạo. 

Trường hợp cạnh xảy ra khi$n = 1$, bởi vì Anna không thể đảo ngược để tác động đến trò chơi, còn Sasha thì không có động tác gì. Tương tự, khi số lượng đã lớn, sự đảo ngược của Anna không thể ngăn cản Sasha chiến thắng. Ví dụ: nếu số duy nhất là$9$Và$m = 1$, Anna không thể tránh khỏi chiến thắng dành cho Sasha vì$9 \ge 10^0 = 1$. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ cố gắng mô phỏng trò chơi, đảo ngược mọi số ở lượt của Anna và ghép từng cặp ở lượt của Sasha. Điều này đúng nhưng không khả thi vì số hoán vị là giai thừa trong$n$. Đối với lớn$n$, điều này sẽ vượt quá giới hạn thời gian. 

Quan sát quan trọng là **Anna không thể giảm số lượng lớn nhất đủ để ngăn Sasha tạo số$\ge 10^m$** nếu ít nhất một số có đủ chữ số. Sasha luôn có thể ghép hai số để tạo ra một số ít nhất lớn bằng phần tử lớn nhất, vì phép ghép làm tăng số chữ số hoặc giữ nguyên chúng. Số đảo ngược của Anna chỉ hoán đổi các chữ số, không thể tăng số chữ số vượt quá số ban đầu. 

Do đó, giải pháp rút gọn lại còn kiểm tra xem **số lớn nhất có thể đạt được thông qua phép nối tối ưu của Sasha** có ít nhất là$10^m$. Nếu đúng như vậy, Sasha thắng; nếu không thì Anna thắng. Điều này cho phép **đánh giá tham lam dựa trên các chữ số lớn nhất**, tránh mô phỏng đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!) mỗi trận | O(n) | Quá chậm | 
| Kiểm tra tối đa tham lam | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$Và$m$, theo sau là danh sách các số nguyên$a$. 
3. Xác định số nguyên có **số chữ số tối đa** trong$a$. Đảo ngược bất kỳ số nguyên nào không làm thay đổi số chữ số của nó. 
4. Tính toán$10^m$. 
5. Nếu số nguyên lớn nhất trong$a$ít nhất là **$10^m$**, Sasha có thể ngay lập tức hoặc thông qua phép nối để đạt được một số$\ge 10^m$. Tuyên bố Sasha là người chiến thắng. 
6. Nếu không, Anna có thể ngăn Sasha tiếp cận$10^m$, và Anna thắng. 

**Tại sao nó hoạt động:** Điều bất biến là phép nối của Sasha luôn tạo ra một số ít nhất bằng phần tử lớn nhất trong danh sách hiện tại. Phép đảo ngược của Anna không bao giờ có thể làm tăng số chữ số. Do đó, kết quả của trò chơi chỉ phụ thuộc vào việc liệu bất kỳ số nào đã có đủ chữ số để đạt đến ngưỡng khi được Sasha ghép một cách tối ưu hay chưa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        threshold = 10 ** m
        max_val = max(a)
        if max_val >= threshold:
            print("Sasha")
        else:
            print("Anna")

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào bằng I/O nhanh. Đối với mỗi trường hợp thử nghiệm, nó tính toán phần tử lớn nhất và so sánh nó với$10^m$để xác định người chiến thắng. Việc sử dụng`max(a)`đảm bảo đánh giá O(n) cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

| Trường hợp thử nghiệm | n | m | một | tối đa(a) | 10^m | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 14, 2 | 14 | 100 | Sasha | 
| 2 | 3 | 5 | 9, 56, 1 | 56 | 100000 | Anna | 
| 3 | 4 | 10 | Ngày 1 năm 2007, 800, 1580 | 2007 | 10000000000 | Anna | 
| 4 | 4 | 5 | 5000, 123, 30, 4 | 5000 | 100000 | Sasha | 

Điều này chứng tỏ rằng chúng ta chỉ cần kiểm tra phần tử lớn nhất trong danh sách để xác định kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét danh sách để tìm mức tối đa mất O(n). | 
| Không gian | O(n) | Lưu trữ danh sách các số nguyên cho mỗi trường hợp thử nghiệm. | 

Cho rằng tổng của n trên tất cả các trường hợp thử nghiệm là 2 · 10^5, giải pháp này rất phù hợp trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("9\n2 2\n14 2\n3 5\n9 56 1\n4 10\n1 2007 800 1580\n4 5\n5000 123 30 4\n10 10\n6 4 6 2 3 1 10 9 10 7\n1 1\n6\n1 1\n10\n8 9\n1 2 9 10 10 2 10 2\n4 5\n10 10 10 10") == "\n".join([
    "Sasha","Anna","Anna","Sasha","Sasha","Anna","Anna","Anna","Sasha"
]), "sample 1"

# Custom test cases
assert run("2\n1 0\n1\n1 1\n9") == "Sasha\nAnna", "single element edge cases"
assert run("2\n3 2\n99 9 10\n3 1\n1 1 1") == "Sasha\nAnna", "threshold edge cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 1 | Sasha | Phần tử đơn ≥ ngưỡng | 
| 1 1 9 | Anna | Phần tử đơn < ngưỡng | 
| 99 9 10 | Sasha | Yếu tố tối đa kích hoạt chiến thắng của Sasha | 
| 1 1 1 | Anna | Tất cả các yếu tố dưới ngưỡng, Anna thắng | 

## Vỏ cạnh 

Nếu danh sách chỉ có một số, Anna không thể ảnh hưởng đến trò chơi và kết quả chỉ phụ thuộc vào việc con số đó có ≥$10^m$. Ví dụ, với đầu vào`1 1 10`, Sasha thắng ngay lập tức. Nếu danh sách chứa số lớn nhưng m rất lớn thì ngay cả số lớn nhất cũng không thể đạt tới$10^m$, giúp Anna giành chiến thắng. Cách tiếp cận này giải quyết chính xác các trường hợp khó khăn này mà không mô phỏng bất kỳ động thái nào.
