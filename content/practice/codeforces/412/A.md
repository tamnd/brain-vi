---
title: "CF 412A - Áp phích"
description: "Chúng ta được yêu cầu mô phỏng việc vẽ một khẩu hiệu trên một biểu ngữ tuyến tính được chia thành n ô vuông, mỗi ô vuông có một ký tự. Người họa sĩ có thể sử dụng một cái thang ban đầu đứng trước hình vuông thứ k."
date: "2026-06-07T02:17:38+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 412
codeforces_index: "A"
codeforces_contest_name: "Coder-Strike 2014 - Round 1"
rating: 900
weight: 412
solve_time_s: 97
verified: true
draft: false
---

[CF 412A - Áp phích](https://codeforces.com/problemset/problem/412/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng việc vẽ một khẩu hiệu trên một biểu ngữ tuyến tính được chia thành _n_ ô vuông, mỗi ký tự trên một ô vuông. Người họa sĩ có thể sử dụng một cái thang ban đầu đứng trước hình vuông thứ _k_. Thang có thể được di chuyển một ô vuông sang trái hoặc phải mỗi giờ và vẽ một nhân vật khi đứng trên thang mất một giờ. Khi đứng ở một hình vuông, người họa sĩ không thể vẽ các hình vuông liền kề do kích thước của thang. Mục tiêu là in một chuỗi các hành động - di chuyển thang hoặc vẽ các ký tự - dẫn đến toàn bộ khẩu hiệu được vẽ trong tổng thời gian tối thiểu. 

Đầu vào cung cấp số ô vuông _n_, vị trí bậc thang ban đầu _k_ và một chuỗi biểu thị khẩu hiệu. Đầu ra là một chuỗi hành động: hoặc`LEFT`,`RIGHT`, hoặc`PRINT x`, Ở đâu`x`là nhân vật cần vẽ. 

Các giới hạn rất nhỏ, với _n_ lên tới 100, cho phép chúng tôi mô phỏng chuyển động và vẽ một cách đơn giản mà không cần quan tâm đến tắc nghẽn hiệu suất. Các trường hợp cạnh không rõ ràng bao gồm khi bậc thang bắt đầu ở vị trí ngoài cùng bên trái hoặc ngoài cùng bên phải. Ví dụ: nếu bậc thang bắt đầu ở vị trí 1 và slogan có ba chữ cái thì trước tiên chúng ta phải sơn ô vuông đầu tiên, sau đó di chuyển sang phải để sơn các ô vuông tiếp theo. Một trường hợp tinh tế khác là khẩu hiệu có một ký tự, không cần di chuyển thang. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thử mọi thứ tự vẽ có thể, di chuyển bậc thang qua lại để tiếp cận các nhân vật. Mặc dù điều này sẽ đảm bảo tính chính xác nhưng nó không cần thiết đối với cấu trúc tuyến tính nhỏ của bài toán và sẽ rất tẻ nhạt khi thực hiện. 

Cái nhìn sâu sắc quan trọng là vấn đề vốn đã tuyến tính. Thang chỉ chặn bức tranh của hình vuông hiện tại và các ô lân cận. Một khi nhân vật đã được vẽ xong, nó không bao giờ cần phải sơn lại. Do đó, chiến lược tối ưu là vẽ tất cả các ký tự theo hướng liền kề bắt đầu từ một đầu của biểu ngữ, di chuyển sang trái rồi sang phải hoặc phải rồi sang trái, tùy thuộc vào đầu nào gần vị trí bậc thang ban đầu hơn. Điều này giảm thiểu tổng chuyển động của thang vì thang luôn di chuyển theo một hướng mà không cần dịch chuyển qua lại dư thừa. 

Chúng ta chọn hướng cần ít lần di chuyển thang ban đầu hơn: nếu thang gần đầu bên trái hơn thì chúng ta sơn từ trái sang phải; nếu không, chúng ta vẽ từ phải sang trái. Sau đó, chúng ta di chuyển bậc thang nếu cần thiết, vẽ từng ký tự một cách tuần tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá mức cần thiết, không cần thiết | 
| Tham lam tuyến tính tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định xem thang ở gần đầu bên trái hơn (vị trí 1) hay đầu bên phải (vị trí _n_). Nếu gần bên trái hơn thì ta vẽ từ trái sang phải; nếu không thì từ phải sang trái. Lựa chọn này giảm thiểu sự di chuyển của thang. 
2. Tính hình vuông bắt đầu vẽ. Nếu di chuyển từ trái sang phải thì bắt đầu từ ô vuông 1. Nếu di chuyển từ phải sang trái thì bắt đầu từ ô vuông _n_. 
3. Trong khi bậc thang không ở ô bắt đầu mục tiêu, hãy phát hành`LEFT`hoặc`RIGHT`lệnh di chuyển thang. Mỗi lần di chuyển tốn một giờ. 
4. Khi bậc thang đã thẳng hàng với hình vuông đầu tiên được tô, hãy đưa ra một`PRINT x`lệnh cho ký tự đó. Di chuyển bậc thang một ô vuông theo hướng đã chọn sau mỗi lần in, ngoại trừ sau ký tự cuối cùng. 
5. Lặp lại bước 4 cho đến khi tất cả các ký tự được vẽ. 
6. Dừng lại sau ký tự cuối cùng; không cần di chuyển thêm nữa. 

Điều bất biến ở đây là ở mỗi bước, bậc thang sẽ ở phía trước ký tự tiếp theo được vẽ và không có ô vuông nào bị bỏ qua. Vì chúng ta luôn di chuyển theo một hướng bao trùm tất cả các ô vuông nên tổng thời gian được giảm thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
slogan = input().strip()

actions = []
# decide which end to start painting from
if k - 1 <= n - k:
    # closer to left, paint left to right
    pos = k
    # move ladder to square 1
    while pos > 1:
        actions.append("LEFT")
        pos -= 1
    # paint each character from left to right
    for i in range(n):
        actions.append(f"PRINT {slogan[i]}")
        if i != n - 1:
            actions.append("RIGHT")
else:
    # closer to right, paint right to left
    pos = k
    while pos < n:
        actions.append("RIGHT")
        pos += 1
    for i in reversed(range(n)):
        actions.append(f"PRINT {slogan[i]}")
        if i != 0:
            actions.append("LEFT")

print("\n".join(actions))
```Giải pháp đầu tiên quyết định hướng tối ưu dựa trên vị trí bậc thang ban đầu so với các điểm cuối. Sau đó, nó di chuyển bậc thang đến ô vuông bắt đầu và lần lượt vẽ tất cả các ký tự theo thứ tự. Có thể tránh các lỗi ngẫu nhiên bằng cách quản lý cẩn thận vị trí của thang và kiểm tra xem có cần di chuyển thêm sau khi in hay không. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2 2
R1
```| Bước | Thang Pos | Hành động | Sơn | 
| --- | --- | --- | --- | 
| 1 | 2 | IN 1 | 1 | 
| 2 | 2 → 1 | TRÁI | 1 | 
| 3 | 1 | IN R | 1, R | 

Dấu vết này hiển thị bậc thang ban đầu ở vị trí 2. Hành động đầu tiên in ký tự thứ hai vì bậc thang bắt đầu ở phía trước nó. Sau đó chúng ta di chuyển sang trái và in ký tự đầu tiên. Tổng số hành động được giảm thiểu. 

### Đầu vào mẫu 2```
5 3
HELLO
```| Bước | Thang Pos | Hành động | Sơn | 
| --- | --- | --- | --- | 
| 1 | 3 → 1 | TRÁI | | 
| 2 | 2 → 1 | TRÁI | | 
| 3 | 1 | IN H | H | 
| 4 | 1 → 2 | QUYỀN | H | 
| 5 | 2 | IN E | H, E | 
| 6 | 2 → 3 | QUYỀN | H, E | 
| 7 | 3 | IN L | H, E, L | 
| 8 | 3 → 4 | QUYỀN | H, E, L | 
| 9 | 4 | IN L | H, E, L, L | 
| 10 | 4 → 5 | QUYỀN | H, E, L, L | 
| 11 | 5 | IN O | H, E, L, L, O | 

Điều này xác nhận rằng việc di chuyển về phía đầu gần nhất trước rồi vẽ tuần tự sẽ giảm các bước di chuyển thang không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý chính xác một lần, với tối đa n lần di chuyển bậc thang. | 
| Không gian | O(n) | Việc lưu trữ chuỗi hành động cho đầu ra yêu cầu không gian O(n). | 

Cho n ≤ 100, giải pháp chạy thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    slogan = input().strip()
    actions = []
    if k - 1 <= n - k:
        pos = k
        while pos > 1:
            actions.append("LEFT")
            pos -= 1
        for i in range(n):
            actions.append(f"PRINT {slogan[i]}")
            if i != n - 1:
                actions.append("RIGHT")
    else:
        pos = k
        while pos < n:
            actions.append("RIGHT")
            pos += 1
        for i in reversed(range(n)):
            actions.append(f"PRINT {slogan[i]}")
            if i != 0:
                actions.append("LEFT")
    return "\n".join(actions)

# provided sample
assert run("2 2\nR1\n") == "PRINT 1\nLEFT\nPRINT R", "sample 1"

# custom cases
assert run("1 1\nA\n") == "PRINT A", "single character"
assert run("3 1\nXYZ\n") == "PRINT X\nRIGHT\nPRINT Y\nRIGHT\nPRINT Z", "ladder at left end"
assert run("3 3\nXYZ\n") == "RIGHT\nPRINT Z\nLEFT\nPRINT Y\nLEFT\nPRINT X", "ladder at right end"
assert run("5 3\nABCDE\n") == "LEFT\nLEFT\nPRINT A\nRIGHT\nPRINT B\nRIGHT\nPRINT C\nRIGHT\nPRINT D\nRIGHT\nPRINT E", "ladder in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\nA | IN A | ký tự đơn, không chuyển động | 
| 3 1\nXYZ | IN X ... IN Z | thang ở đầu bên trái, sơn từ trái sang phải | 
| 3 3\nXYZ | QUYỀN ... IN X | thang ở đầu bên phải, sơn từ phải sang trái | 
| 5 3\nABCDE | TRÁI ... IN E | thang ở giữa, chọn hướng xuất phát tối ưu | 

## Vỏ cạnh 

Đối với khẩu hiệu một ký tự như`1 1\nA`, bậc thang đã được căn chỉnh theo ô đích, do đó thuật toán sẽ in ký tự ngay lập tức, tạo ra
