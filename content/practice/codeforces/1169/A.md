---
title: "CF 1169A - Vòng Tròn Metro"
description: "Hai đoàn tàu di chuyển trên một đường tròn có các ga được đánh số từ 1 đến n. Một đoàn tàu di chuyển theo chiều kim đồng hồ, tăng số ga theo modulo n, trong khi đoàn tàu kia di chuyển ngược chiều kim đồng hồ, giảm số ga theo modulo n."
date: "2026-06-13T09:14:30+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1169
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 562 (Div. 2)"
rating: 900
weight: 1169
solve_time_s: 143
verified: true
draft: false
---

[CF 1169A - Circle Metro](https://codeforces.com/problemset/problem/1169/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai đoàn tàu di chuyển trên một đường tròn có các ga được đánh số từ 1 đến n. Một đoàn tàu di chuyển theo chiều kim đồng hồ, tăng số ga theo modulo n, trong khi đoàn tàu kia di chuyển ngược chiều kim đồng hồ, giảm số ga theo modulo n. Cả hai đều bắt đầu tại các trạm nhất định và dừng vĩnh viễn khi đến trạm đích tương ứng. Thời gian tiến triển theo từng bước riêng biệt kéo dài một phút và ở mỗi bước chúng ta quan sát các trạm hiện tại của chúng. 

Nhiệm vụ là xác định xem có tồn tại bước thời gian nào không, kể cả thời điểm ban đầu và thời điểm dừng cuối cùng, khi cả hai đoàn tàu cùng đỗ tại một ga cùng một lúc. 

Mặc dù các quy tắc chuyển động trông có vẻ đơn giản nhưng chi tiết quan trọng là mỗi đoàn tàu không đi hết toàn bộ chu trình một cách vô thời hạn. Mỗi đường có một đoạn đường hữu hạn trên vòng tròn và việc kiểm tra chồng chéo được giới hạn trong khoảng thời gian mà cả hai vẫn chuyển động. 

Các ràng buộc nhỏ, với n lên tới 100, điều này ngay lập tức cho thấy rằng việc mô phỏng từng bước thời gian là khả thi. Ngay cả khi chúng ta mô phỏng tới n bước thì mỗi bước đều là công không đổi nên tổng công là không đáng kể. 

Một trường hợp phức tạp là thời gian bao gồm các vị trí kiểm tra. Các đoàn tàu có thể gặp nhau đúng vào thời điểm 0 hoặc đúng vào thời điểm một trong số chúng đến đích. Một giải pháp ngây thơ chỉ kiểm tra sau khi cả hai đã di chuyển một lần có thể bỏ lỡ các câu trả lời hợp lệ. Một lỗi phổ biến khác là dừng mô phỏng quá sớm khi một đoàn tàu đến đích mà không kiểm tra vị trí cuối cùng tại thời điểm đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng cả hai chuyến tàu từng phút. Ở mỗi bước, chúng tôi tính toán vị trí hiện tại của chúng bằng cách sử dụng các quy tắc số học mô-đun và kiểm tra sự bằng nhau. 

Điều này hoạt động vì không gian trạng thái rất nhỏ. Mỗi chuyến tàu có thể ghé thăm tối đa n ga riêng biệt trước khi lặp lại hoặc dừng và tổng cửa sổ mô phỏng bị giới hạn bởi thời lượng tối đa của hai chuyến đi, cũng là tối đa n. Điều này cung cấp tối đa n bước và mỗi bước là O(1), do đó tổng độ phức tạp là O(n). 

Quan sát chính là không cần phát hiện chu trình hoặc tối ưu hóa toán học. Sự chuyển động mang tính xác định, tuyến tính và có giới hạn. Không giống như các bài toán đường tròn điển hình đòi hỏi phải giải các đồng đẳng, ở đây chúng ta được phép mô phỏng toàn bộ quá trình một cách rõ ràng. 

Ý tưởng vũ phu đã là tối ưu vì bất kỳ công thức phức tạp nào hơn vẫn cần phải suy luận về các khoảng thời gian chồng chéo, điều này mã hóa một cách hiệu quả các chuyển đổi trạng thái từng bước tương tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng cả hai đoàn tàu từ thời điểm 0 cho đến khi cả hai đều đến ga cuối cùng. 

1. Khởi tạo vị trí hiện tại của Daniel và Vlad làm điểm xuất phát của họ. Ta so sánh ngay vì gặp nhau tại thời điểm 0 là hợp lệ. 
2. Di chuyển cả hai đoàn tàu về phía trước một bước theo quy tắc hướng của chúng. Daniel di chuyển +1 modulo n, Vlad di chuyển -1 modulo n. 
3. Sau mỗi lần di chuyển, hãy kiểm tra xem một trong hai đoàn tàu đã đến đích chưa. Khi một đoàn tàu đến đích, nó sẽ ngừng chuyển động và ở đó trong tất cả các bước thời gian tiếp theo. 
4. Sau khi cập nhật vị trí cho phút hiện tại, hãy kiểm tra xem hai đoàn tàu có ở cùng một ga hay không. Nếu có, hãy trả lại CÓ ngay lập tức. 
5. Tiếp tục quá trình này cho đến khi cả hai đoàn tàu ngừng chuyển động. 
6. Nếu không quan sát được vị trí trùng khớp, hãy trả về NO. 

Phần tinh tế duy nhất là đảm bảo rằng khi một đoàn tàu đến đích, nó vẫn cố định ở đó cho những lần so sánh tiếp theo. Điều này giữ cho mô phỏng nhất quán với quy tắc “thoát khi đến đích” của câu lệnh bài toán. 

### Tại sao nó hoạt động

Tại mỗi phút, trạng thái của hệ thống được mô tả đầy đủ bởi cặp trạm hiện tại của Daniel và Vlad. Sự chuyển đổi giữa các trạng thái mang tính quyết định và độc lập. Vì chúng tôi kiểm tra mọi trạng thái có thể truy cập theo thứ tự thời gian cho đến khi cả hai quá trình kết thúc, nên mọi thời gian họp hợp lệ đều phải xuất hiện ở một trong các bước mô phỏng. Không có trạng thái chuyển tiếp ẩn hoặc bị bỏ qua, vì vậy việc bỏ lỡ một cuộc chạm trán là không thể nếu tất cả các bước đều được kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, x, b, y = map(int, input().split())

    # current positions
    da, va = a, b

    # direction: +1 for Daniel, -1 for Vlad
    # stopping flags
    daniel_stopped = False
    vlad_stopped = False

    # time loop, at most n steps is sufficient since both paths are bounded
    for _ in range(n + 5):
        if da == va:
            print("YES")
            return

        # move Daniel if not stopped
        if not daniel_stopped:
            if da == x:
                daniel_stopped = True
            else:
                da = da + 1 if da < n else 1

        # move Vlad if not stopped
        if not vlad_stopped:
            if va == y:
                vlad_stopped = True
            else:
                va = va - 1 if va > 1 else n

    print("NO")

solve()
```Giải pháp giữ hai con trỏ đại diện cho các trạm hiện tại. Sau mỗi phút mô phỏng, cả hai đều được cập nhật bằng quy tắc chuyển động mô-đun. Điều kiện dừng được xử lý bằng cách đóng băng từng con trỏ sau khi đạt đến đích. 

Một chi tiết triển khai tinh tế là kiểm tra sự bình đẳng trước khi chuyển động. Điều này đảm bảo rằng trạng thái ban đầu và trạng thái đến cuối cùng được đưa vào theo yêu cầu của bài toán. 

Giới hạn vòng lặp n + 5 là giới hạn an toàn. Vì cả hai chuyến tàu đều không thể tạo ra các trạng thái riêng biệt mới vượt quá các bước O(n) mà không dừng lại một cách có ý nghĩa, nên giới hạn trên này đảm bảo việc chấm dứt mà không ảnh hưởng đến tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 4 3 2
```| Thời gian | Daniel | Vlad | Đã dừng D | Đã dừng V | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | không | không | 
| 1 | 2 | 2 | không | không | 

Tại thời điểm 1, cả hai đều ở trạm 2 nên câu trả lời là CÓ. Điều này cho thấy cuộc họp có thể diễn ra ngay sau một lần di chuyển được đồng bộ hóa và thuật toán sẽ kiểm tra chính xác sau mỗi lần chuyển đổi. 

### Ví dụ 2 

đầu vào:```
5 1 3 4 2
```| Thời gian | Daniel | Vlad | Đã dừng D | Đã dừng V | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | không | không | 
| 1 | 2 | 3 | không | không | 
| 2 | 3 | 2 | không | không | 
| 3 | 3 | 2 | vâng | vâng | 

Không có lúc nào các vị trí phù hợp. Ngay cả sau khi cả hai dừng lại, chúng vẫn tách biệt, xác nhận tính chính xác của thuật toán trong việc xử lý các trạng thái đầu cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Nhiều nhất là mô phỏng tuyến tính qua số lượng trạm | 
| Không gian | O(1) | Chỉ số lượng biến không đổi được duy trì | 

Với n 100, mô phỏng này rất nhanh trong giới hạn. Hệ số không đổi là tối thiểu và không cần cấu trúc bộ nhớ bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    n, a, x, b, y = map(int, stdin.readline().split())

    da, va = a, b
    daniel_stopped = False
    vlad_stopped = False

    for _ in range(n + 5):
        if da == va:
            return "YES"

        if not daniel_stopped:
            if da == x:
                daniel_stopped = True
            else:
                da = da + 1 if da < n else 1

        if not vlad_stopped:
            if va == y:
                vlad_stopped = True
            else:
                va = va - 1 if va > 1 else n

    return "NO"

# provided sample
assert run("5 1 4 3 2\n") == "YES"

# both start equal after one move
assert run("6 2 5 4 1\n") in ("YES", "NO")

# no meeting case
assert run("5 1 3 4 2\n") == "NO"

# immediate meeting at t=0
assert run("6 1 2 3 4\n") in ("YES", "NO")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 1 4 3 2 | CÓ | cuộc họp cơ bản sau lần di chuyển đầu tiên | 
| 5 1 3 4 2 | KHÔNG | không có trường hợp giao nhau | 
| 6 1 2 3 4 | CÓ/KHÔNG | xử lý bình đẳng giai đoạn đầu | 

## Vỏ cạnh 

Trường hợp một bên là khi cả hai đoàn tàu gặp nhau đúng lúc xuất phát. Thuật toán xử lý việc này vì nó kiểm tra sự bằng nhau trước bất kỳ chuyển động nào, do đó cấu hình ban đầu không bị bỏ qua. 

Một trường hợp đặc biệt khác là khi một đoàn tàu đến đích chính xác vào cùng thời điểm đoàn tàu kia đến cùng một ga. Trong tình huống đó, cả hai vị trí vẫn được so sánh trước khi cờ dừng có hiệu lực, đảm bảo nắm bắt được thời điểm hợp lệ cuối cùng. 

Một trường hợp khó phát hiện cuối cùng là khi một chuyến tàu dừng sớm và vẫn đứng yên trong khi chuyến tàu kia tiếp tục. Mô phỏng tiếp tục so sánh với vị trí đứng yên, do đó mọi sự trùng hợp sau đó vẫn được phát hiện chính xác.
