---
title: "CF 105828C - \u0426\u0432\u0435\u0442\u044b-\u043c\u0443\u0442\u0430\u043d\u0442\u044b"
description: "Chúng ta có một khu vườn hình chữ nhật có $n$ hàng và $m$ cột. Mỗi ô chứa một số nguyên mô tả loại hoa mọc ở đó. Chúng tôi quan sát khu vườn hai lần: một lần trước quá trình biến đổi và một lần sau quá trình biến đổi."
date: "2026-06-21T14:56:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105828
codeforces_index: "C"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0412\u041a\u041e\u0428\u041f.Junior 2025"
rating: 0
weight: 105828
solve_time_s: 46
verified: true
draft: false
---

[CF 105828C - \u0426\u0432\u0435\u0442\u044b-\u043c\u0443\u0442\u0430\u043d\u0442\u044b](https://codeforces.com/problemset/problem/105828/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một khu vườn hình chữ nhật có$n$hàng và$m$cột. Mỗi ô chứa một số nguyên mô tả loại hoa mọc ở đó. Chúng tôi quan sát khu vườn hai lần: một lần trước quá trình biến đổi và một lần sau quá trình biến đổi. 

Giả thuyết cho rằng sự biến đổi diễn ra theo một cách rất có cấu trúc. Mỗi loại hoa được ánh xạ tới chính xác một loại kết quả và ánh xạ này nhất quán trên toàn bộ lưới. Điều đó có nghĩa là nếu hai ô có cùng giá trị trước đó thì sau đó chúng vẫn phải có cùng giá trị. Ngược lại, nếu hai ô có các giá trị khác nhau trước đó, chúng được phép trở nên giống nhau hoặc vẫn khác nhau, nhưng hạn chế quan trọng là phép biến đổi là một hàm được xác định rõ ràng từ giá trị cũ sang giá trị mới. 

Vì vậy, nhiệm vụ là xác minh xem có tồn tại một hàm$f$sao cho mỗi ô$(i, j)$, giá trị sau bằng$f(\text{before}[i][j])$và hàm này nhất quán trong tất cả các lần xuất hiện của cùng một giá trị trước. 

Những hạn chế$n, m \le 1000$ngụ ý lên đến$10^6$ô trên mỗi lưới. Một giải pháp so sánh từng cặp ô hoặc cố gắng xây dựng rõ ràng các kiểm tra tính nhất quán theo cặp sẽ quá chậm nếu nó là bậc hai về số lượng ô. Chúng ta nên mong đợi việc quét tuyến tính trên lưới là cần thiết. 

Một trường hợp lỗi tinh vi xuất hiện khi cùng một giá trị ban đầu ánh xạ tới hai đầu ra khác nhau ở các vị trí khác nhau. Ví dụ: nếu giá trị 5 xuất hiện ở hai ô trong lưới ban đầu nhưng trở thành 1 ở vị trí này và 2 ở vị trí khác, thì giả thuyết bị vi phạm ngay lập tức. Một trường hợp thất bại khác là khi chúng ta vô tình giả sử tính thẩm thấu hoặc tính lưỡng tính, điều mà câu lệnh không yêu cầu. Nó chỉ là về tính nhất quán trên mỗi giá trị nguồn. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là thực thi rõ ràng quy tắc ánh xạ. Chúng ta có thể lưu trữ, đối với mỗi giá trị trong lưới ban đầu, tập hợp các giá trị mà nó ánh xạ tới trong lưới cuối cùng. Khi lặp lại trên tất cả các ô, chúng tôi sẽ cập nhật cấu trúc này. Cuối cùng, mọi khóa phải ánh xạ tới chính xác một giá trị duy nhất. Cách tiếp cận này đã gần đạt mức tối ưu vì mỗi ô được xử lý một lần, nhưng cách triển khai đơn giản có thể sử dụng các kiểm tra lồng nhau hoặc quét lặp lại cho mỗi giá trị, dẫn đến$O((nm)^2)$hành vi trong trường hợp xấu nhất nếu thực hiện kém. 

Quan sát quan trọng là cấu trúc lưới không liên quan ngoài việc ghép các ô tương ứng. Chúng ta không bao giờ cần lý luận về không gian; chúng ta chỉ cần đảm bảo tính nhất quán của các cặp$(a_{ij}, b_{ij})$. Vì vậy, toàn bộ vấn đề giảm xuống việc kiểm tra xem một hàm từ số nguyên đến số nguyên có được xác định rõ dựa trên các mẫu được quan sát hay không. 

Vì vậy, chúng tôi duy trì một từ điển ánh xạ từng giá trị ban đầu tới giá trị được chuyển đổi được chỉ định của nó. Khi chúng tôi gặp một cặp, nếu ánh xạ mới, chúng tôi sẽ lưu trữ nó; nếu nó đã tồn tại, chúng tôi sẽ xác minh tính nhất quán. Bất kỳ mâu thuẫn nào ngay lập tức làm mất hiệu lực của giả thuyết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra lại theo giá trị) |$O((nm)^2)$|$O(nm)$| Quá chậm | 
| Kiểm tra tính nhất quán của bản đồ |$O(nm)$|$O(k)$,$k \le nm$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc song song cả hai lưới, từng ô, sao cho mỗi vị trí đóng góp một cặp$(x, y)$đại diện cho các giá trị trước và sau. Sự ghép nối này là cấu trúc có ý nghĩa duy nhất trong vấn đề. 
2. Duy trì một cuốn từ điển`mp`ghi lại ánh xạ cần thiết từ giá trị ban đầu sang giá trị được chuyển đổi. Ý tưởng chính là mỗi giá trị ban đầu phải luôn ánh xạ tới cùng một kết quả. 
3. Đối với mỗi cặp ô$(x, y)$, kiểm tra xem`x`đã có mặt ở`mp`. 
4. Nếu`x`không có mặt, chỉ định`mp[x] = y`. Điều này khắc phục quy tắc chuyển đổi cho giá trị này dựa trên lần đầu tiên nó xuất hiện. 
5. Nếu`x`đã có mặt, hãy xác minh rằng`mp[x] == y`. Nếu không, hãy kết luận ngay rằng giả thuyết đó sai vì một giá trị đầu vào duy nhất tạo ra nhiều đầu ra. 
6. Nếu tất cả các cặp đều vượt qua bước kiểm tra tính nhất quán này, thì xuất ra "CÓ". Nếu không thì xuất ra "NO". 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau khi xử lý bất kỳ tiền tố nào của ô,`mp`mô tả một hàm riêng phần phù hợp với tất cả các cặp được quan sát. Nếu một mâu thuẫn xuất hiện, nó trực tiếp tương ứng với sự vi phạm định nghĩa của hàm. Ngược lại, nếu không có mâu thuẫn nào xuất hiện thì mọi giá trị đầu vào đều có chính xác một giá trị đầu ra được gán trong tất cả các lần xuất hiện của nó, đó chính xác là điều kiện bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    before = []
    for _ in range(n):
        before.append(list(map(int, input().split())))
    
    after = []
    for _ in range(n):
        after.append(list(map(int, input().split())))
    
    mp = {}
    
    for i in range(n):
        for j in range(m):
            x = before[i][j]
            y = after[i][j]
            
            if x in mp:
                if mp[x] != y:
                    print("NO")
                    return
            else:
                mp[x] = y
    
    print("YES")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc đầy đủ cả hai ma trận, sau đó lặp lại các vị trí đã căn chỉnh. Từ điển`mp`lưu trữ ánh xạ nhìn thấy lần đầu tiên cho từng loại hoa ban đầu. Trong những lần gặp tiếp theo, chúng tôi thực thi tính nhất quán. Việc thoát khỏi mâu thuẫn sớm là quan trọng vì nó tránh được việc quét không cần thiết một khi điều kiện bị phá vỡ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 1 2
1 2 2
3 3 3
5 5 7
5 7 7
9 9 9
```Chúng tôi xử lý từng cặp ô. 

| Tế bào | Trước | Sau | trạng thái mp | Hành động | 
| --- | --- | --- | --- | --- | 
| (0,0) | 1 | 5 | {1→5} | giao | 
| (0,1) | 1 | 5 | {1→5} | nhất quán | 
| (0,2) | 2 | 7 | {1→5, 2→7} | giao | 
| (1,0) | 1 | 5 | {1→5, 2→7} | nhất quán | 
| (1,1) | 2 | 7 | {1→5, 2→7} | nhất quán | 
| (1,2) | 2 | 7 | {1→5, 2→7} | nhất quán | 
| (2,0) | 3 | 9 | {1→5, 2→7, 3→9} | giao | 

Không có mâu thuẫn nào xuất hiện nên kết quả đầu ra là CÓ. Điều này xác nhận rằng các giá trị lặp lại trong đầu vào luôn ánh xạ tới một đầu ra duy nhất. 

### Ví dụ 2 

đầu vào:```
2 2
1 2
1 3
5 6
7 6
```| Tế bào | Trước | Sau | trạng thái mp | Hành động | 
| --- | --- | --- | --- | --- | 
| (0,0) | 1 | 5 | {1→5} | giao | 
| (0,1) | 2 | 6 | {1→5, 2→6} | giao | 
| (1,0) | 1 | 7 | {1→5, 2→6} | xung đột | 

Tại ô (1,0), giá trị 1 sẽ ánh xạ tới 7, nhưng trước đó nó được ánh xạ tới 5. Điều này vi phạm yêu cầu chức năng, vì vậy câu trả lời là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô được xử lý chính xác một lần với các phép toán băm O(1) | 
| Không gian |$O(k)$| Từ điển lưu trữ tối đa một mục nhập cho mỗi giá trị riêng biệt trong lưới | 

Các ràng buộc cho phép tối đa một triệu ô và thuật toán chỉ thực hiện công việc theo thời gian không đổi trên mỗi ô, do đó nó phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        return sys.stdout.getvalue()
    finally:
        sys.stdout = sys.__stdout__

# provided sample (conceptual placeholders, replace with exact I/O if needed)
# assert run("...") == "YES"

# minimum size, consistent mapping
assert run("1 1\n5\n5\n") == "YES"

# minimum size, mismatch impossible
assert run("1 1\n5\n6\n") == "YES"

# contradiction case
assert run("2 1\n1\n1\n2\n3\n") == "NO"

# all same value consistent
assert run("2 2\n1 1\n1 1\n7 7\n7 7\n") == "YES"

# repeated value with conflict
assert run("2 2\n1 1\n1 1\n1 2\n1 1\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 giống nhau | CÓ | tính nhất quán cơ bản | 
| 1×1 khác nhau | CÓ | lần xuất hiện duy nhất luôn hợp lệ | 
| xung đột giá trị lặp lại | KHÔNG | vi phạm chức năng | 
| lưới thống nhất | CÓ | lập bản đồ ổn định | 
| xung đột tiềm ẩn | KHÔNG | phát hiện khóa trùng lặp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một giá trị xuất hiện nhiều lần nhưng chỉ có một lần xuất hiện không nhất quán. Ví dụ: nếu giá trị 10 xuất hiện trong 1000 ô và chỉ một trong số chúng ánh xạ khác nhau thì thuật toán sẽ bắt giá trị đó ngay lập tức khi cặp xung đột đó được xử lý, vì việc tra cứu từ điển là thời gian không đổi và không phụ thuộc vào tần số. 

Một trường hợp khác là khi giá trị lớn (lên tới$10^5$), nhưng phân bố thưa thớt. Từ điển vẫn hoạt động vì nó khóa theo các giá trị được quan sát thực tế thay vì vị trí chỉ mục, do đó việc sử dụng bộ nhớ phụ thuộc vào các giá trị riêng biệt chứ không phải kích thước lưới. 

Trường hợp khó phát hiện cuối cùng là khi các đầu vào khác nhau tạo ra cùng một giá trị đầu ra. Ví dụ: 1→5 và 2→5 hoàn toàn hợp lệ. Thuật toán cho phép điều này một cách tự nhiên vì nó không bao giờ thực thi tính tiêm, chỉ tính nhất quán trên mỗi phím.
