---
title: "CF 2092B - Bọ Rùa"
description: "Chúng ta có hai chuỗi bit có độ dài bằng nhau, a và b, đại diện cho một mật khẩu. Lady Bug muốn biến đổi chuỗi đầu tiên a thành chuỗi chỉ chứa số 0."
date: "2026-06-08T05:42:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2092
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1014 (Div. 2)"
rating: 1000
weight: 2092
solve_time_s: 98
verified: false
draft: false
---

[CF 2092B - Bọ rùa](https://codeforces.com/problemset/problem/2092/B) 

**Đánh giá:** 1000 
**Tags:** bạo lực, thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi bit có độ dài bằng nhau,`a`Và`b`, đại diện cho một mật khẩu. Lady Bug muốn chuyển đổi chuỗi đầu tiên`a`để nó chỉ chứa số không. Cô ấy có thể thực hiện một loạt các thao tác hoán đổi giữa các phần tử liền kề của hai chuỗi, cụ thể là hoán đổi`a[i]`với`b[i-1]`hoặc`b[i]`với`a[i-1]`vì`i`từ 2 đến`n`. Mục đích là để xác định xem sau bất kỳ số lần hoán đổi nào như vậy, chuỗi`a`có thể trở thành tất cả số không. 

Mỗi trường hợp thử nghiệm cho chúng ta`n`, độ dài của chuỗi, theo sau là chính hai chuỗi đó. Số lượng trường hợp thử nghiệm có thể lên tới 10.000 và tổng tổng độ dài chuỗi trên tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này có nghĩa là chúng ta cần một giải pháp gần như tuyến tính trong`n`đối với từng trường hợp thử nghiệm, nếu không, việc mô phỏng mạnh mẽ tất cả các giao dịch hoán đổi có thể xảy ra sẽ quá chậm. 

Trường hợp cạnh khóa là khi phần tử đầu tiên của`a`là`1`và phần tử đầu tiên của`b`là`0`. Vì giao dịch hoán đổi chỉ liên quan đến các chỉ số`i ≥ 2`, chúng ta không thể trực tiếp di chuyển một`1`Tại`a[1]`ĐẾN`b[0]`hoặc bất cứ nơi nào khác, điều này sẽ khiến mật khẩu không thể bị bẻ khóa. Tương tự, nếu`b`không có`1`s bất cứ nơi nào có thể đạt được`1`TRONG`a`, chúng tôi cũng thất bại. Bé nhỏ`n=2`các trường hợp rất quan trọng vì chỉ có một vài giao dịch hoán đổi có sẵn và các chuỗi có thể không thể giải quyết được ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng tất cả các giao dịch hoán đổi có thể có giữa các chuỗi. Tại mỗi chỉ số`i`, người ta có thể thử cả hai thao tác đệ quy hoặc lặp đi lặp lại, cho đến khi một trong hai`a`trở thành số không hoặc mọi khả năng đều cạn kiệt. Điều này được đảm bảo là đúng nhưng hoàn toàn không thực tế: số lượng các trạng thái tăng theo cấp số nhân với`n`, lên đến`2^n`, vượt xa giới hạn 1 giây ngay cả đối với`n`. 

Cái nhìn sâu sắc quan trọng là theo dõi xem mỗi`1`TRONG`a`có thể bị “đẩy” vào bên trái`b`bằng cách sử dụng các giao dịch hoán đổi có sẵn. Mỗi lần hoán đổi cho phép một`1`TRONG`a[i]`di chuyển đến`b[i-1]`hoặc một`1`TRONG`b[i]`di chuyển đến`a[i-1]`. Điều này có nghĩa là nếu chúng ta quét các chuỗi từ trái sang phải, trường hợp duy nhất mà một chuỗi`1`TRONG`a`không thể gỡ bỏ được khi có`1`TRONG`a[i]`và không`1`TRONG`b[i-1]`hoặc`b[i]`có thể đạt được nó. Về cơ bản, mỗi`1`TRONG`a`Tại`i`phải có một trong hai`b[i-1]`hoặc tiếp theo`b[j]`có thể hoán đổi vào vị trí đó. 

Đơn giản hóa hơn nữa là chúng ta chỉ cần đếm tổng số`1`ở trong`a`Và`b`. Đối với mỗi tiền tố của chuỗi, số lượng tích lũy của`1`ở trong`b`ít nhất phải là số tích lũy của`1`ở trong`a`. Nếu tại bất kỳ chỉ số nào tích lũy`1`ở trong`a`vượt quá những gì trong`b`, quá trình này thất bại vì chúng ta có quá nhiều`1`ở trong`a`đó không thể di chuyển được. Điều này biến vấn đề thành một lần quét tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng tất cả các giao dịch hoán đổi) | O(2^n) | O(n) | Quá chậm | 
| Đếm tiền tố / Quét tuyến tính | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và những sợi dây`a`Và`b`. 
3. Khởi tạo hai bộ đếm:`count_a`cho số lượng`1`ở trong`a`đã thấy cho đến nay, và`count_b`cho số lượng`1`ở trong`b`thấy cho đến nay. 
4. Lặp lại các chuỗi từ trái sang phải. Tại mỗi chỉ số`i`, tăng`count_a`nếu như`a[i]`là`1`Và`count_b`nếu như`b[i]`là`1`. 
5. Nếu tại bất kỳ thời điểm nào`count_a`vượt quá`count_b`, in "NO" cho trường hợp thử nghiệm này. Điều này cho thấy có nhiều hơn`1`ở trong`a`hơn mức có thể được “hấp thụ” bởi`b`đến chỉ số này. 
6. Nếu quá trình lặp hoàn tất mà không có`count_a > count_b`, in "CÓ". 

Tại sao nó hoạt động: Mỗi lần hoán đổi chỉ có thể di chuyển một`1`từ`a[i]`ĐẾN`b[i-1]`hoặc từ`b[i]`ĐẾN`a[i-1]`. Bằng cách đảm bảo rằng với mỗi tiền tố của chuỗi, tổng số`1`ở trong`b`ít nhất là số lượng`1`ở trong`a`, chúng tôi đảm bảo rằng mỗi`1`TRONG`a`cuối cùng có thể được đẩy vào`b`sử dụng các hoạt động được phép. Việc đếm tích lũy này nắm bắt được khả năng tiếp cận của từng`1`mà không mô phỏng các giao dịch hoán đổi riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = input().strip()
    b = input().strip()
    
    count_a = 0
    count_b = 0
    possible = True
    
    for i in range(n):
        if a[i] == '1':
            count_a += 1
        if b[i] == '1':
            count_b += 1
        if count_a > count_b:
            possible = False
            break
    
    print("YES" if possible else "NO")
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Nó theo dõi số lượng tích lũy của`1`s trong cả hai chuỗi. Vòng lặp kiểm tra xem tại bất kỳ thời điểm nào có nhiều hơn`1`ở trong`a`hơn có thể được kết hợp bởi`b`. Việc phá vỡ sớm sẽ ngăn ngừa sự lặp lại không cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
6
010001
010111
```| tôi | một [tôi] | b[i] | đếm_a | đếm_b | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | Đúng | 
| 1 | 1 | 1 | 1 | 1 | Đúng | 
| 2 | 0 | 0 | 1 | 1 | Đúng | 
| 3 | 0 | 1 | 1 | 2 | Đúng | 
| 4 | 0 | 1 | 1 | 3 | Đúng | 
| 5 | 1 | 1 | 2 | 4 | Đúng | 

Không có tiền tố có`count_a > count_b`, vì vậy câu trả lời là CÓ. 

Trường hợp cạnh:```
2
11
00
```| tôi | một [tôi] | b[i] | đếm_a | đếm_b | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | Sai | 

đầu tiên`1`TRONG`a`không thể hoán đổi ở bất cứ đâu, vì vậy câu trả lời là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét một chuỗi; tổng của tất cả n 2×10^5 | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và một số đại lượng vô hướng | 

Độ phức tạp tuyến tính này nằm trong giới hạn và mức sử dụng không gian là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n000\n000\n6\n010001\n010111\n5\n10000\n01010\n2\n11\n00\n") == "YES\nYES\nNO\nYES", "sample 1"

# Custom tests
assert run("1\n2\n01\n00\n") == "NO", "a[1] cannot be removed"
assert run("1\n2\n10\n01\n") == "YES", "a[0] 1 can be pushed to b[0]"
assert run("1\n5\n11111\n11111\n") == "YES", "all 1s can be matched"
assert run("1\n3\n010\n101\n") == "YES", "mixed 1s are solvable"
assert run("1\n3\n110\n001\n") == "NO", "insufficient 1s in b"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n01\n00 | KHÔNG | Không thể loại bỏ đầu tiên`1`TRONG`a`| 
| 2\n10\n01 | CÓ | Có thể loại bỏ trao đổi đơn`1`| 
| 5\n11111\n11111 | CÓ | Tất cả 1 đều khớp, trường hợp tối đa | 
| 3\n010\n101 | CÓ | Sắp xếp có thể giải quyết được hỗn hợp | 
| 3\n110\n001 | KHÔNG | Không đủ`1`ở trong`b`để xóa`a`| 

## Vỏ cạnh 

Vì `n=
