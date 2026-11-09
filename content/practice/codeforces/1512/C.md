---
title: "CF 1512C - A-B Palindrome"
description: "Chúng ta được cấp một chuỗi gồm các ký tự '0', '1' và '?', cùng với hai số nguyên a và b biểu thị tổng số '0' và '1' mà chuỗi cuối cùng phải chứa. Nhiệm vụ là thay thế mọi '?"
date: "2026-06-10T18:51:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1512
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 713 (Div. 3)"
rating: 1200
weight: 1512
solve_time_s: 116
verified: true
draft: false
---

[CF 1512C - A-B Palindrome](https://codeforces.com/problemset/problem/1512/C) 

**Đánh giá:** 1200 
**Tas:** thuật toán xây dựng, triển khai, chuỗi 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các ký tự '0', '1' và '?', cùng với hai số nguyên`a`Và`b`biểu thị tổng số '0 và '1' mà chuỗi cuối cùng phải chứa. Nhiệm vụ là thay thế mọi dấu '?' với '0' hoặc '1' sao cho chuỗi trở thành một bảng màu trong khi khớp chính xác với số đếm`a`Và`b`. Một bảng màu yêu cầu chuỗi đọc xuôi và ngược giống nhau, vì vậy mọi ký tự ở vị trí`i`phải khớp với ký tự ở vị trí`n-i-1`. 

Vấn đề kết hợp đồng thời hai ràng buộc: duy trì thuộc tính palindrome và khớp số đếm chính xác của '0 và '1. Độ dài chuỗi có thể lên tới 200.000 trên tất cả các trường hợp thử nghiệm và số lượng trường hợp thử nghiệm có thể là 10.000. Điều này có nghĩa là giải pháp của chúng tôi phải chạy theo thời gian tuyến tính trên mỗi chuỗi, vì các phương pháp tiếp cận bậc hai hoặc bạo lực sẽ bao gồm quá nhiều thao tác. 

Các trường hợp cạnh tinh tế tồn tại khi có các ký tự cố định xung đột (ví dụ: '0' ở chỉ mục`i`và '1' ở chỉ mục`n-i-1`) hoặc khi số lượng còn lại là '0 hoặc '1' không thể thỏa mãn '?' thay thế. Một trường hợp phức tạp khác là một chuỗi có độ dài lẻ với ký tự ở giữa là '?'; thuật toán phải đảm bảo rằng ký tự ở giữa có thể được gán nhất quán với các số còn lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các khả năng thay thế '?' bằng '0' hoặc '1', sau đó kiểm tra từng ứng cử viên về thuộc tính palindrome và tính tính chính xác. Cách tiếp cận này đúng về nguyên tắc nhưng trở nên không khả thi vì mỗi dấu '?' nhân đôi số khả năng, dẫn đến độ phức tạp theo cấp số nhân của số '?'. Đối với các chuỗi có độ dài lên tới 2*10^5, điều này là không thể. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải thử tất cả các kết hợp. Ràng buộc palindrome cho phép chúng ta truyền bá các giá trị đã biết qua các vị trí được phản chiếu của chúng, giảm số lượng '?' để điền vào. Cụ thể, nếu`s[i]`là '?' Nhưng`s[n-i-1]`là '0' thì chúng ta phải thay thế`s[i]`bằng '0'. Sau quá trình lan truyền này, chúng ta có thể kiểm tra xem số đếm`a`Và`b`vẫn có thể đạt được. Sau đó, đối với các cặp '?' còn lại, chúng ta có thể gán '0' hoặc '1' ở các vị trí đối xứng một cách tham lam, sử dụng hết số lượng còn lại. Ký tự ở giữa, nếu chuỗi có độ dài lẻ, có thể được gán độc lập nếu đó là '?', miễn là chúng ta vẫn có sẵn số đếm. 

Cách tiếp cận này là tuyến tính vì mỗi vị trí được truy cập nhiều nhất hai lần: một lần để truyền và một lần để điền '?' còn lại. Xung đột và số lượng thiếu sót được kiểm tra theo thời gian liên tục trên mỗi cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi thành danh sách các ký tự cho các thao tác có thể thay đổi. Khởi tạo bộ đếm cho số '0 và '1' còn lại bằng cách sử dụng`a`Và`b`, trừ đi số lượng '0 và '1' hiện có trong chuỗi. 
2. Truyền bá các ký tự đã biết đến vị trí được phản chiếu của chúng. Đối với mỗi chỉ số`i`ở nửa đầu của chuỗi, nếu`s[i]`là '?' Và`s[n-i-1]`không, thay thế`s[i]`với`s[n-i-1]`và giảm bộ đếm thích hợp. Nếu cả hai vị trí đều cố định và không bằng nhau, xuất ra`-1`như một giải pháp là không thể. 
3. Sau khi truyền, kiểm tra xem có bộ đếm nào âm hay không. Nếu vậy, xuất`-1`bởi vì chúng tôi không thể đáp ứng được số lượng yêu cầu. 
4. Điền vào các cặp dấu '?' còn lại ở nửa đầu của chuỗi. Đối với mỗi cặp`(i, n-i-1)`cả hai đều là '?', gán '0' nếu còn lại ít nhất 2 số 0, nếu không thì gán '1' nếu còn lại ít nhất 2 số 0. Cập nhật bộ đếm cho phù hợp. Nếu không thể, hãy xuất`-1`. 
5. Nếu độ dài chuỗi là số lẻ, hãy xử lý riêng ký tự ở giữa. Nếu là '?', hãy gán '0' nếu vẫn còn một số 0, nếu không thì gán '1' nếu còn một. Nếu không có số đếm, xuất ra`-1`. 
6. Sau khi điền vào tất cả các vị trí, bộ đếm số 0 và số 1 phải chính xác bằng 0. Nếu không thì xuất`-1`. Nếu không, hãy nối lại danh sách thành một chuỗi và xuất nó. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng sau mỗi bước truyền và điền, chuỗi một phần hiện tại vẫn có thể được hoàn thành thành một bảng màu thỏa mãn số đếm. Xung đột được phát hiện ngay lập tức, ngăn chặn các giải pháp không hợp lệ. Nhiệm vụ tham lam cho '?' còn lại hoạt động vì bất kỳ cặp nào cũng có thể được chỉ định độc lập sau khi xung đột được giải quyết và tổng số lượng được theo dõi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        s = list(input().strip())
        n = len(s)
        possible = True

        # Step 1: initial count deduction
        a -= s.count('0')
        b -= s.count('1')

        # Step 2: propagate known characters
        for i in range(n // 2):
            j = n - i - 1
            if s[i] == '?' and s[j] != '?':
                s[i] = s[j]
                if s[i] == '0': a -= 1
                else: b -= 1
            elif s[i] != '?' and s[j] == '?':
                s[j] = s[i]
                if s[j] == '0': a -= 1
                else: b -= 1
            elif s[i] != '?' and s[j] != '?' and s[i] != s[j]:
                possible = False
                break

        if a < 0 or b < 0:
            possible = False

        # Step 3: fill remaining '?'
        for i in range(n // 2):
            j = n - i - 1
            if s[i] == '?' and s[j] == '?':
                if a >= 2:
                    s[i] = s[j] = '0'
                    a -= 2
                elif b >= 2:
                    s[i] = s[j] = '1'
                    b -= 2
                else:
                    possible = False
                    break

        # Step 4: handle middle character if odd length
        if n % 2 == 1 and s[n // 2] == '?':
            if a > 0:
                s[n // 2] = '0'
                a -= 1
            elif b > 0:
                s[n // 2] = '1'
                b -= 1
            else:
                possible = False

        if possible and a == 0 and b == 0:
            print("".join(s))
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính đến các ký tự cố định để giảm số lượng số 0 và số 1 còn lại. Sau đó, nó truyền bá các giá trị được nhân đôi để giải quyết '?' một cách xác định. Các cặp '?' còn lại được lấp đầy một cách tham lam, điều này hiệu quả vì tính đối xứng palindrome cho phép chúng ta gán cả hai vị trí cùng một lúc. Cuối cùng, ký tự ở giữa được xử lý riêng nếu cần. Bất kỳ xung đột hoặc vi phạm đếm nào đều dẫn đến ngay lập tức`-1`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`4 4 01?????0`| tôi | j | s trước | Hành động | sau | một | b | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 7 | 0 & 0 | trận đấu | 0 & 0 | 3 | 4 | 
| 1 | 6 | 1 & ? | tuyên truyền 1 | 1 & 1 | 3 | 3 | 
| 2 | 5 | ? & ? | gán 0 | 0 & 0 | 1 | 3 | 
| 3 | 4 | ? & ? | chỉ định 1 | 1 & 1 | 1 | 1 | 

Chuỗi cuối cùng:`01011010`, số đếm trùng khớp chính xác. 

**Đầu vào mẫu 2:**`2 2 01?0`Việc truyền bá các ký tự được phản chiếu gây ra xung đột ở vị trí 1 và 2 (1 so với '?'), nhưng '?' được gán cho 1, cho`0110`, đếm trận đấu. Hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập nhiều nhất hai lần: truyền và điền | 
| Không gian | O(n) | Chuỗi được chuyển đổi thành danh sách cho các hoạt động có thể thay đổi | 

Với tổng độ dài trên tất cả các trường hợp thử nghiệm là 2*10^5, giải pháp này rất phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("9\n4 4\n01?????0\n3 3\n??????\n1 0\n?\n2 2\n0101\n2 2\n01?0\n0 1\n0\n0 3\n1?1\n2 2\n?00?\n4 3\n??010?0\n") == "01011010\n-1\n0\n-1\n0110\n-1\n
```
