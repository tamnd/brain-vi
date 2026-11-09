---
title: "CF 1472A - Thiệp Cho Bạn Bè"
description: "Nhiệm vụ là xác định xem liệu một tờ giấy có kích thước w × h có thể được chia thành ít nhất n tờ nhỏ hơn bằng cách sử dụng một bộ quy tắc cắt nghiêm ngặt hay không. Mỗi lần cắt chỉ được phép nếu chiều rộng hoặc chiều cao bằng nhau và nó tạo ra hai tấm có kích thước bằng một nửa dọc theo kích thước đó."
date: "2026-06-11T00:29:59+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1472
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 693 (Div. 3)"
rating: 800
weight: 1472
solve_time_s: 97
verified: true
draft: false
---

[CF 1472A - Thẻ dành cho bạn bè](https://codeforces.com/problemset/problem/1472/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xác định xem một tờ giấy có kích thước`w × h`ít nhất có thể được chia thành`n`các tấm nhỏ hơn bằng cách sử dụng một bộ quy tắc cắt nghiêm ngặt. Mỗi lần cắt chỉ được phép nếu chiều rộng hoặc chiều cao bằng nhau và nó tạo ra hai tấm có kích thước bằng một nửa dọc theo kích thước đó. Mục đích là để kiểm tra xem chúng tôi có thể đạt hoặc vượt quá số lượng tờ yêu cầu cho`n`bạn. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp cung cấp chiều rộng`w`, chiều cao`h`, và số tờ`n`cần thiết. Đầu ra là "CÓ" đơn giản nếu có thể nhận được ít nhất`n`tờ và "KHÔNG" nếu không. 

Những ràng buộc chỉ ra rằng`w`Và`h`có thể lên tới 10.000, và`n`có thể lớn tới 10^9. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng mọi lần cắt có thể, bởi vì ngay cả khi bắt đầu với kích thước khiêm tốn như 1024 × 1024 cũng có thể tạo ra hơn một triệu tờ nếu được mô phỏng rõ ràng và`n`có thể lớn hơn thế nhiều. 

Một điểm tinh tế là việc cắt chỉ nhân đôi số tờ giấy theo lũy thừa hai. Ví dụ: một tờ có kích thước 3 × 4 chỉ có thể được chia theo 4 chiều. Một mô phỏng đơn giản có thể cố gắng cắt bừa bãi và có thể bỏ lỡ số lượng trang tối đa hoặc cố gắng cắt không hợp lệ. Các trường hợp cạnh bao gồm các tờ sức mạnh hai chiều một chiều hoặc khi`n`là 1, không yêu cầu cắt giảm. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ cố gắng mô phỏng trực tiếp quá trình cắt: bắt đầu với tấm, kiểm tra liên tục xem`w`hoặc`h`chẵn, cắt và cộng vào số tờ. Điều này đúng về nguyên tắc nhưng quá chậm đối với các`n`, bởi vì số lần lặp có thể đạt tới`n`chính nó, lên tới 10^9, vượt xa khả năng khả thi trong giới hạn thời gian 1 giây. 

Quan sát quan trọng cho phép giải pháp nhanh hơn là mỗi chiều chỉ có thể được chia khi số chẵn, tính hiệu quả là số lần mỗi chiều có thể được chia cho 2. Giả sử`count_w`là số lần`w`có thể chia cho 2 và`count_h`tương tự cho`h`. Tổng số tờ chúng ta có thể đạt được là`2^(count_w + count_h)`vì mỗi lần chia sẽ nhân đôi số tờ hiện tại. Chúng ta không cần phải mô phỏng mọi lần cắt; chỉ cần phân tích tất cả lũy thừa của 2 từ`w`Và`h`và tính toán số tờ kết quả. Việc kiểm tra sau đó sẽ trở thành liệu con số này có đáp ứng hay vượt quá`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(log w + log h) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với kích thước đã cho`w`Và`h`. 
2. Khởi tạo một biến`sheets = 1`để thể hiện số lượng tờ hiện tại. 
3. Trong khi`w`chia hết cho 2 thì chia`w`với 2 và nhân`sheets`lên 2. Mỗi lần chia nhân đôi số tờ giấy dọc theo chiều rộng. 
4. Lặp lại tương tự cho`h`: trong khi`h`chia hết cho 2 thì chia`h`với 2 và nhân`sheets`bằng 2. Mỗi bộ phận nhân đôi các tấm dọc theo chiều cao. 
5. Sau khi phân tích tất cả lũy thừa của 2, hãy kiểm tra xem`sheets >= n`. Nếu đúng, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Mỗi lần cắt mà bài toán cho phép sẽ nhân đôi số tờ. Bằng cách tính tất cả lũy thừa của hai từ cả hai chiều, chúng tôi thu được số lượng trang tính tối đa có thể đạt được theo quy tắc. Không có vết cắt nào có thể tạo ra nhiều hơn gấp đôi dọc theo một chiều và tất cả các vết cắt tiềm năng đã được tính đến, do đó, bất biến`sheets = 2^(number of width divisions + number of height divisions)`đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    w, h, n = map(int, input().split())
    sheets = 1
    while w % 2 == 0:
        w //= 2
        sheets *= 2
    while h % 2 == 0:
        h //= 2
        sheets *= 2
    print("YES" if sheets >= n else "NO")
```Phần đầu tiên đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi đọc kích thước và số lượng trang mục tiêu. Các vòng lặp while tính lũy thừa của 2 từ`w`Và`h`, nhân`sheets`mỗi lần. Cuối cùng, chúng tôi so sánh`sheets`ĐẾN`n`và in kết quả. Phép chia số nguyên đảm bảo không có lỗi dấu phẩy động và thứ tự của phép nhân không quan trọng vì phép nhân có tính chất giao hoán. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2 2 3
3 3 2
```| w | h | n | tờ (sau khi phân tích w) | tờ (sau khi phân tích h) | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 3 | 2 | 4 | CÓ | 
| 3 | 3 | 2 | 1 | 1 | KHÔNG | 

Ở hàng đầu tiên, chiều rộng chia hết cho 2 một lần, chiều cao chia hết cho 2, được 2 * 2 = 4 tờ, tức là ≥ 3. Ở hàng thứ hai, cả hai chiều đều không chia hết cho 2 nên chỉ tồn tại 1 tờ, tức là < 2. 

### Đầu vào mẫu 2```
1 4 4
```| w | h | n | tờ | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | 4 | 

Chiều rộng không có hệ số 2, chiều cao có thể chia hai lần (4 → 2 → 1), được 2 * 2 = 4 tờ, chính xác bằng n. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * (log w + log h)) | Mỗi trường hợp thử nghiệm tính lũy thừa 2 của w và h, thực hiện tối đa các bước log2(w) + log2(h). | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng; không có bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào. | 

Với những hạn chế`w, h <= 10^4`Và`t <= 10^4`, giải pháp dễ dàng chạy trong vòng 1 giây vì log2(10^4) ≈ 14 và tổng số phép tính là khoảng 3 * 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        w, h, n = map(int, input().split())
        sheets = 1
        while w % 2 == 0:
            w //= 2
            sheets *= 2
        while h % 2 == 0:
            h //= 2
            sheets *= 2
        print("YES" if sheets >= n else "NO")
    return output.getvalue().strip()

# provided samples
assert run("5\n2 2 3\n3 3 2\n5 10 2\n11 13 1\n1 4 4\n") == "YES\nNO\nYES\nYES\nYES", "sample 1"

# custom cases
assert run("2\n1 1 1\n1 1 2\n") == "YES\nNO", "minimum size"
assert run("1\n1024 1024 1048576\n") == "YES", "maximum divisions"
assert run("1\n7 8 4\n") == "YES", "only one dimension divisible"
assert run("1\n6 5 4\n") == "NO", "insufficient sheets"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | CÓ | Đầu vào có kích thước tối thiểu không cần cắt | 
| 1 1 2 | KHÔNG | Đầu vào có kích thước tối thiểu không thể đạt tới n | 
| 1024 1024 1048576 | CÓ | Sự phân chia tối đa có thể trong giới hạn | 
| 7 8 4 | CÓ | Chỉ chia chiều cao, thử nghiệm cắt một chiều | 
| 6 5 4 | KHÔNG | Chỉ chia hết chiều rộng nhưng vẫn thiếu tờ | 

## Vỏ cạnh 

Khi nào`w`Và`h`đều bằng 1, thuật toán xử lý chính xác`n=1`bằng cách trả lại "CÓ" và`n>1`bằng cách trả về "KHÔNG". Đối với một tờ có sức mạnh lớn như`1024 × 1024`với`n=1048576`, các vòng lặp while sẽ nhân các trang tính một cách chính xác thông qua tất cả các phép chia có thể có. Đối với các trang chỉ chia hết cho một chiều, vòng lặp sẽ bỏ qua chiều không chia hết. Phương pháp này mạnh mẽ đối với bất kỳ sự kết hợp nào giữa lũy thừa hai và chiều lẻ, phản ánh trực tiếp các phép toán được phép của bài toán.
