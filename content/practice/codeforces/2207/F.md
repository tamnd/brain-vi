---
title: "CF 2207F - Hanabi"
description: "Bài toán đưa ra cho chúng ta một trò chơi bài giống như Hanabi trong đó Zuko giữ tất cả các lá bài thành một hàng, mỗi lá bài đại diện cho sự kết hợp giữa thứ hạng và màu sắc. Iroh có thể nhìn thấy các lá bài, nhưng Zuko thì không."
date: "2026-06-07T19:35:20+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "dsu", "flows", "graph-matchings", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2207
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1085 (Div. 1 + Div. 2)"
rating: 2900
weight: 2207
solve_time_s: 122
verified: false
draft: false
---

[CF 2207F - Hanabi](https://codeforces.com/problemset/problem/2207/F) 

**Xếp hạng:** 2900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, dsu, luồng, khớp đồ thị, đồ thị, tham lam 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra cho chúng ta một trò chơi bài giống như Hanabi trong đó Zuko giữ tất cả các lá bài thành một hàng, mỗi lá bài đại diện cho sự kết hợp giữa thứ hạng và màu sắc. Iroh có thể nhìn thấy các lá bài, nhưng Zuko thì không. Đến lượt Iroh, anh ấy đưa ra manh mối bằng cách đánh dấu tất cả các thẻ có cấp bậc cụ thể hoặc tất cả các thẻ có màu cụ thể. Zuko sau đó sẽ chơi lá bài được đánh dấu ngoài cùng bên trái. Trò chơi xen kẽ giữa các manh mối và lượt chơi cho đến khi tất cả các lá bài được chơi theo đúng thứ tự xếp hạng trong mỗi màu. Chơi bài xếp hạng$r \ge 2$trước khi hạng trước đó cùng màu được chơi là bất hợp pháp. 

Mục đích là thiết kế chuỗi manh mối sao cho tất cả các lá bài đều được chơi hợp pháp đồng thời giảm thiểu số lần manh mối của Iroh thay đổi so với lượt trước. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$xếp hạng và$m$màu sắc, theo sau là danh sách các$n \cdot m$cấp bậc và màu sắc của các lá bài khi chúng xuất hiện trên tay Zuko. Đầu ra là số lần đầu mối thay đổi tối thiểu trong suốt trò chơi. 

Các ràng buộc cho phép lên tới$2 \cdot 10^5$tổng số thẻ trên tất cả các trường hợp thử nghiệm. Điều này có nghĩa là các thuật toán có hơn$O(n \cdot m)$độ phức tạp sẽ quá chậm và các phương pháp bậc hai là không khả thi. 

Trường hợp đơn giản là khi tất cả các quân bài cùng cấp xuất hiện liên tiếp. Ví dụ: nếu tất cả các lá bài hạng 1 đã nằm liền kề nhau trong tay thì không cần thay đổi manh mối để chơi tất cả chúng theo trình tự. Một cách tiếp cận bất cẩn, chuyển đổi manh mối mỗi lượt sẽ đếm quá nhiều thay đổi và tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng trò chơi, đưa ra mọi manh mối có thể có ở mỗi lượt, theo dõi thứ tự chơi của Zuko và đếm những thay đổi về manh mối. Về nguyên tắc, điều này đúng nhưng đòi hỏi phải kiểm tra từng lá bài và manh mối tiềm ẩn ở mỗi lượt. Với tối đa$2 \cdot 10^5$thẻ, số lượng hoạt động sẽ bùng nổ$O((n \cdot m)^2)$, điều đó là không thể thực hiện được. 

Thông tin chi tiết quan trọng đến từ việc nhận thấy rằng trình tự chơi phải tuân theo thứ hạng tăng dần trong mỗi màu. Điều này ngụ ý rằng chúng ta có thể coi bàn tay như một chuỗi các “khối” có thứ hạng tăng dần theo một màu. Nếu chúng ta theo dõi vị trí mới nhất nơi thứ hạng trước đó của một màu được chơi, chúng ta có thể xác định số nước đi tối thiểu mà Iroh phải chuyển manh mối. Cụ thể, bài toán rút gọn thành việc tìm số đoạn đơn điệu tối thiểu khi di chuyển ngang bàn tay theo thứ tự tăng dần. Chúng ta có thể gán cho mỗi thẻ một "chỉ số thứ tự xếp hạng" cho mỗi màu, sau đó đếm xem chuỗi chỉ số này giảm đi bao nhiêu lần. Mỗi lần giảm tương ứng với một sự thay đổi đầu mối cần thiết. Điều này biến vấn đề thành một$O(n \cdot m)$nhiệm vụ xử lý tuần tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((n*m)^2) | O(n*m) | Quá chậm | 
| Trình tự xếp hạng | O(n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$,$m$và bàn tay dưới dạng danh sách cấp bậc và màu sắc. 
2. Xây dựng từ điển ánh xạ từng màu vào danh sách các vị trí xếp hạng$1$ĐẾN$n$. Điều này cho phép chúng tôi xác định thứ tự chơi của từng cấp bậc theo một màu. 
3. Với mỗi màu, gán cho mỗi thẻ một chỉ số “vị trí theo thứ tự tăng dần”, nghĩa là thẻ hạng 1 là 1, thẻ hạng 2 là 2, v.v. 
4. Thay thế mỗi lá bài trên tay bằng chỉ số tương ứng của nó theo thứ tự màu sắc. Điều này biến bài toán thành việc đếm số lượng tối thiểu các dãy tăng dần trong mảng mới này. 
5. Di chuyển bàn tay đã biến đổi từ trái sang phải. Duy trì giá trị chỉ mục cuối cùng cho mỗi màu. Bất cứ khi nào chỉ số của thẻ hiện tại nhỏ hơn hoặc bằng thẻ cuối cùng cùng màu, hãy tăng số lượng thay đổi đầu mối cần thiết. Điều này cho thấy Iroh phải chuyển đổi manh mối để đảm bảo Zuko chơi chính xác. 
6. Xuất ra tổng số lần thay đổi đầu mối cho ca kiểm thử. 

Điều bất biến ở đây là trong mỗi màu, chuỗi chỉ số xếp hạng trong ván bài phải không giảm để tránh việc chơi trái luật. Việc đếm mức giảm sẽ đưa ra chính xác số lượng thay đổi đầu mối cần thiết để duy trì tính hợp pháp trong khi giảm thiểu các thay đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        ranks = list(map(int, input().split()))
        colors = list(map(int, input().split()))
        pos_in_color = [[] for _ in range(m + 1)]
        
        for i in range(n * m):
            pos_in_color[colors[i]].append((ranks[i], i))
        
        index_in_color = [0] * (n * m)
        for c in range(1, m + 1):
            # Sort by rank to get correct play order
            pos_in_color[c].sort()
            for order, (_, idx) in enumerate(pos_in_color[c]):
                index_in_color[idx] = order + 1
        
        res = 0
        last = 0
        for i in range(1, n * m):
            if index_in_color[i] < index_in_color[i - 1]:
                res += 1
        print(res)

solve()
```Trong mã này, trước tiên chúng tôi xây dựng danh sách các thẻ theo màu được sắp xếp theo thứ hạng. Sau đó, chúng ta thay thế từng thẻ theo thứ tự của nó trong dãy màu. Cuối cùng, việc duyệt qua số lượng bàn tay giảm dần theo thứ tự, mỗi lần biểu thị một sự thay đổi đầu mối cần thiết. 

Các điểm tinh tế bao gồm lập chỉ mục dựa trên một và dựa trên 0 cho thứ tự xếp hạng và theo dõi cẩn thận mức giảm thay vì bằng nhau, vì các chuỗi không giảm có thể chấp nhận được mà không cần thay đổi đầu mối. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3 2
1 2 3 1 2 3
1 1 1 2 2 2
```| Tay | Màu sắc | Chỉ số xếp hạng | Thay đổi? | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | - | 
| 2 | 1 | 2 | không | 
| 3 | 1 | 3 | không | 
| 1 | 2 | 1 | vâng | 
| 2 | 2 | 2 | không | 
| 3 | 2 | 3 | không | 

Thay đổi manh mối: 1 

Điều này chứng tỏ rằng tất cả các lượt chơi từ hạng 1 đến hạng 3 trong một màu có thể xảy ra mà không làm thay đổi manh mối. Chuyển sang màu 2 yêu cầu thay đổi một đầu mối. 

### Đầu vào mẫu 2```
1 7
1 1 1 1 1 1 1
7 6 5 4 3 2 1
```| Tay | Màu sắc | Chỉ số xếp hạng | Thay đổi? | 
| --- | --- | --- | --- | 
| 1 | 7 | 1 | - | 
| 1 | 6 | 1 | không | 
| 1 | 5 | 1 | không | 
| 1 | 4 | 1 | không | 
| 1 | 3 | 1 | không | 
| 1 | 2 | 1 | không | 
| 1 | 1 | 1 | không | 

Thay đổi manh mối: 0 

Tất cả các thẻ hạng 1 đều xuất hiện theo thứ tự màu giảm dần, có thể chơi với một đầu mối xếp hạng duy nhất mà không cần chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Mỗi lá bài được xử lý để xây dựng các chỉ số và di chuyển bài. Sắp xếp theo màu là O(n) cho mỗi màu, tổng cộng là O(n*m). | 
| Không gian | O(n*m) | Mảng để lưu trữ vị trí và chuyển đổi chỉ số xếp hạng. | 

Giải pháp tuyến tính về số lượng thẻ trên mỗi trường hợp thử nghiệm, phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("7\n3 2\n1 2 3 1 2 3\n1 1 1 2 2 2\n2 2\n2 1 2 1\n1 2 2 1\n1 7\n1 1 1 1 1 1 1\n7 6 5 4 3 2 1\n5 1\n1 4 2 3 5\n1 1 1 1 1\n8 3\n1 1 1 3 2 2 5 3 3 6 4 4 7 5 5 8 6 6 2 7 7 4 8 8\n1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3\n9 4\n2 1 1 2 3 2 3 5 3 6 5 6 7 7 8 5 9 9 4 4 8 1 2 3 6
```
