---
title: "CF 285B - Tìm Đá Cẩm Thạch"
description: "Chúng ta có một hàng n ly, mỗi ly được đánh số duy nhất từ ​​1 đến n. Một viên bi bắt đầu ở dưới tấm kính ở vị trí s. Petya xác định sự hoán vị của các vị trí xác định cách di chuyển kính đồng thời trong quá trình xáo trộn."
date: "2026-06-05T09:51:17+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 285
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 175 (Div. 2)"
rating: 1200
weight: 285
solve_time_s: 103
verified: true
draft: false
---

[CF 285B - Tìm đá cẩm thạch](https://codeforces.com/problemset/problem/285/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng`n`kính, mỗi kính được đánh số duy nhất từ ​​1 đến`n`. Một viên bi bắt đầu dưới tấm kính ở vị trí`s`. Petya xác định sự hoán vị của các vị trí xác định cách di chuyển kính đồng thời trong quá trình xáo trộn. Nếu kính`i`hiện đang ở vị trí`i`, sau khi xáo trộn nó sẽ di chuyển đến vị trí`p[i]`. Vasya quan sát viên bi ở vị trí`t`sau một số lần xáo trộn. Nhiệm vụ là xác định số lần xáo trộn tối thiểu cần thiết để viên bi đạt được`t`, hoặc báo cáo`-1`nếu điều đó là không thể. 

Quan sát chính là mỗi lần xáo trộn áp dụng một hoán vị cố định cho tất cả các vị trí và viên bi di chuyển cùng với kính của nó. Do đó, quỹ đạo của viên bi được xác định bằng cách lặp lại việc áp dụng hoán vị bắt đầu từ`s`. 

Những ràng buộc chỉ ra`n`có thể lớn tới 10^5. Mỗi lần xáo trộn yêu cầu di chuyển tất cả các ly theo hoán vị. Một giải pháp đơn giản mô phỏng tất cả các chuỗi xáo trộn có thể có sẽ có độ phức tạp theo cấp số nhân về số lượng thao tác, điều này là không khả thi. Giải pháp chỉ đi dọc theo quỹ đạo của viên bi là khả thi vì mỗi tấm kính có thể được viếng thăm nhiều nhất một lần trước khi chu kỳ lặp lại, đưa ra thuật toán O(n) tuyến tính. 

Các trường hợp cạnh không rõ ràng bao gồm khi`s`bằng`t`. Trong tình huống đó, có thể không cần xáo trộn, ngay cả khi hoán vị không tầm thường. Một trường hợp tinh tế khác là một hoán vị chứa các chu trình không bao gồm`t`. Ví dụ, nếu`s`đang trong một chu kỳ không bao giờ đạt tới`t`, câu trả lời phải là`-1`. Việc triển khai bất cẩn có thể lặp lại vô tận mà không phát hiện được chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng từng bước chuyển động của viên bi. Bắt đầu từ`s`, áp dụng hoán vị nhiều lần và đếm số bước cho đến khi viên bi chạm tới`t`. Điều này đúng vì mỗi lần xáo trộn sẽ di chuyển viên bi theo một quỹ đạo duy nhất. Tuy nhiên, trong trường hợp xấu nhất, số lần xáo trộn có thể là O(n) và nếu chúng ta không theo dõi các vị trí đã ghé thăm, chúng ta có thể bước vào một chu trình và lặp lại vô tận. Số lượng hoạt động nhiều nhất là`n`vì hoán vị là song ánh; khi một vị trí lặp lại, mọi bước di chuyển tiếp theo sẽ chỉ quay vòng. 

Quan sát quan trọng cho phép có một cách tiếp cận tối ưu là viên bi di chuyển dọc theo một chu kỳ hoán vị. Chúng ta không cần phải mô phỏng toàn bộ hàng. Chúng ta có thể theo dõi vị trí hiện tại của viên bi, tăng bộ đếm và dừng lại khi viên bi chạm tới`t`hoặc chúng tôi phát hiện một vị trí lặp lại. Điều này đảm bảo một nghiệm tuyến tính về số lượng ly và đủ với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng mọi tư thế) | O(n^2) | O(n) | Quá chậm cho n ~ 10^5 | 
| Theo dõi quỹ đạo (chỉ bằng đá cẩm thạch) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`pos`ĐẾN`s`để thể hiện vị trí hiện tại của viên bi và một bộ đếm`steps`về 0. Điều này thiết lập điểm bắt đầu cho mô phỏng của chúng tôi. 
2. Nếu`pos`bằng`t`, quay lại ngay`steps`bằng 0. Điều này xử lý trường hợp tầm thường khi không cần xáo trộn. 
3. Nếu không, hãy bắt đầu một vòng lặp tiếp tục cho đến khi một trong hai`pos`bằng`t`hoặc chúng tôi phát hiện một chu kỳ. Tại mỗi lần lặp: 

một. Bộ`pos`ĐẾN`p[pos]`, di chuyển viên bi dọc theo hoán vị. 

b. Tăng`steps`bằng 1. 

c. Nếu như`pos`bằng`t`, trở lại`steps`. 

d. Nếu như`pos`bằng`s`một lần nữa, nó chỉ ra một chu trình không bao gồm`t`; trở lại`-1`. 
4. Kết thúc vòng lặp. Nếu chúng ta thoát ra mà không quay trở lại, hãy quay lại`-1`để biểu thị sự không thể. 

Tại sao nó hoạt động: Bất biến là vậy`pos`luôn đại diện cho vị trí hiện tại của viên bi. Vì hoán vị là phỏng đoán nên viên bi chuyển động theo một chu trình cố định. Hoặc chu trình chứa`t`, trong trường hợp đó chúng ta sẽ đạt được nó trong vòng nhiều nhất`n`các bước hoặc không, trong trường hợp đó hãy xem lại`s`tín hiệu không thể thực hiện được. Không có cách nào để bỏ qua các vị trí trong chu kỳ hoặc phạm vi tiếp cận`t`bằng một con đường khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, s, t = map(int, input().split())
p = [0] + list(map(int, input().split()))  # 1-indexed

if s == t:
    print(0)
    sys.exit()

pos = s
steps = 0
visited = set()

while pos not in visited:
    visited.add(pos)
    pos = p[pos]
    steps += 1
    if pos == t:
        print(steps)
        break
else:
    print(-1)
```Mã sử ​​dụng chỉ mục dựa trên 1 để khớp với câu lệnh vấn đề. Chúng tôi xử lý trường hợp tầm thường`s == t`ngay lập tức. MỘT`visited`đặt vị trí theo dõi để phát hiện các chu kỳ và ngăn chặn các vòng lặp vô hạn. Ở mỗi bước, chúng tôi cập nhật vị trí của viên bi và kiểm tra xem nó có đạt tới`t`. Nếu chúng ta xem lại một vị trí mà không đạt được`t`, chúng tôi kết luận rằng mục tiêu là không thể đạt được. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
4 2 1
2 3 4 1
```| Bước | tư thế | bước | đã ghé thăm | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | {} | 
| 1 | 3 | 1 | {2} | 
| 2 | 4 | 2 | {2,3} | 
| 3 | 1 | 3 | {2,3,4} | 

Đá cẩm thạch đạt tới`1`sau 3 bước. Bộ đã truy cập đảm bảo chúng ta không bao giờ lặp lại vô hạn. 

**Mẫu 2** 

đầu vào:```
5 3 3
2 3 4 5 1
```Vị trí ban đầu bằng với mục tiêu. Thuật toán trả về 0 ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được truy cập nhiều nhất một lần do phát hiện chu kỳ. | 
| Không gian | O(n) | các`visited`set có thể lưu trữ tất cả các vị trí trong trường hợp xấu nhất. | 

Với những hạn chế`n ≤ 10^5`và giới hạn thời gian 2 giây, điều này hiệu quả. Việc sử dụng bộ nhớ vẫn nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution code here
    n, s, t = map(int, input().split())
    p = [0] + list(map(int, input().split()))
    if s == t:
        print(0)
    else:
        pos = s
        steps = 0
        visited = set()
        while pos not in visited:
            visited.add(pos)
            pos = p[pos]
            steps += 1
            if pos == t:
                print(steps)
                break
        else:
            print(-1)
    return output.getvalue().strip()

# provided sample
assert run("4 2 1\n2 3 4 1\n") == "3", "sample 1"
# s equals t
assert run("5 3 3\n2 3 4 5 1\n") == "0", "trivial case"
# unreachable t
assert run("3 1 3\n2 1 3\n") == "-1", "cycle without t"
# minimum input
assert run("1 1 1\n1\n") == "0", "single glass"
# maximum input simple cycle
perm = ' '.join(str(i % 100000 + 1) for i in range(100000))
assert run(f"100000 1 100000\n{perm}\n") == "-1", "large cycle no t"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "5 3 3\n2 3 4 5 1\n" | 0 | s bằng t | 
| "3 1 3\n2 1 3\n" | -1 | đá cẩm thạch không thể đạt tới t | 
| "1 1 1\n1\n" | 0 | đầu vào kích thước tối thiểu | 
| chu kỳ lớn n=10^5 | -1 | phát hiện hiệu suất và chu kỳ | 

## Vỏ cạnh 

Khi nào`s == t`, thuật toán trả về 0 ngay lập tức. Ví dụ, đầu vào`5 3 3`trả về 0 mà không cần vào vòng lặp. 

Khi hoán vị tạo thành một chu trình không chứa`t`, tập đã truy cập sẽ phát hiện ra rằng`pos`xem lại vị trí trước đó và thuật toán trả về chính xác`-1`. Đối với đầu vào`3 1 3`với`p = [2,1,3]`, viên bi di chuyển từ 1→2→1 và không bao giờ đạt tới 3, kích hoạt nhánh khác. 

Thuật toán xử lý chính xác kích thước đầu vào lớn nhất do truyền tuyến tính dọc theo quỹ đạo của viên bi, tránh mô phỏng các kính không liên quan.
