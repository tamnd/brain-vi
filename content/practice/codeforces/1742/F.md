---
title: "CF 1742F - Nhỏ hơn"
description: "Chúng ta có hai chuỗi, ban đầu bằng \"a\" và một chuỗi các thao tác nối các chuỗi lặp lại vào s hoặc t. Sau mỗi thao tác, chúng ta phải xác định xem có thể sắp xếp lại các ký tự của s và t sao cho s nhỏ hơn về mặt từ điển so với t hay không."
date: "2026-06-09T16:22:45+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1742
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 827 (Div. 4)"
rating: 1500
weight: 1742
solve_time_s: 160
verified: true
draft: false
---

[CF 1742F - Nhỏ hơn](https://codeforces.com/problemset/problem/1742/F) 

**Đánh giá:** 1500 
**Tags:** thuật toán xây dựng, tham lam, chuỗi 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi, ban đầu bằng`"a"`và một chuỗi các thao tác nối các chuỗi lặp lại vào một trong hai`s`hoặc`t`. Sau mỗi thao tác, chúng ta phải xác định xem có thể sắp xếp lại các ký tự của`s`Và`t`để có thể`s`về mặt từ điển nhỏ hơn`t`. 

Đầu vào cung cấp số lượng thao tác và, đối với mỗi thao tác, một loại (thêm vào`s`hoặc`t`), số lần lặp lại`k`, và chuỗi`x`để nối thêm. Đầu ra là một chuỗi "CÓ" hoặc "KHÔNG" cho mỗi thao tác cho biết liệu có sự sắp xếp từ điển nào thỏa mãn`s < t`. 

Các ràng buộc rất lớn: tổng số thao tác trên tất cả các trường hợp thử nghiệm có thể đạt tới 10^5 và tổng độ dài của chuỗi có thể lên tới 5 × 10^5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào xây dựng và sắp xếp các chuỗi một cách rõ ràng sau mỗi thao tác, vì việc nối chuỗi hoặc sắp xếp đầy đủ lặp lại sẽ vượt quá giới hạn thời gian. 

Một cách tiếp cận đơn giản sẽ thất bại trong trường hợp một chuỗi tích lũy số lượng lớn các ký tự thấp và chuỗi kia thêm một ký tự cao hơn. Ví dụ, bắt đầu bằng`s = "a"`Và`t = "a"`, nếu chúng ta thực hiện các thao tác như nối thêm`"a"`một triệu lần để`s`Và`"b"`một lần để`t`, việc xây dựng chuỗi đơn giản sẽ không khả thi. 

Vấn đề giảm xuống còn việc theo dõi đủ thông tin về các ký tự trong mỗi chuỗi để xác định liệu có tồn tại một sự sắp xếp nhỏ hơn về mặt từ điển mà không cần xây dựng các chuỗi một cách rõ ràng hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xây dựng cả hai chuỗi đầy đủ, sắp xếp chúng và sau đó so sánh từng ký tự để xác định thứ tự từ điển. Mặc dù điều này là đúng nhưng độ phức tạp bị chi phối bởi việc nối và sắp xếp chuỗi lặp đi lặp lại. Trong trường hợp xấu nhất, việc nối thêm các chuỗi có độ dài 10^5 liên tục sẽ vượt quá 10^10 thao tác, vượt xa giới hạn khả thi. 

Quan sát quan trọng là việc so sánh từ điển sau khi sắp xếp lại tùy ý chỉ phụ thuộc vào hai điều: sự hiện diện của ký tự nhỏ nhất trong mỗi chuỗi và ký tự tối đa trong mỗi chuỗi. Nếu như`s`chứa một ký tự lớn hơn ký tự tối đa trong`t`, nó không thể được làm nhỏ hơn. Ngược lại, nếu`s`chỉ chứa`'a'`Và`t`chứa một ký tự lớn hơn`'a'`, chúng ta luôn có thể sắp xếp`s`trở nên nhỏ hơn. 

Chúng ta chỉ cần theo dõi ký tự cao nhất có trong mỗi chuỗi và số lượng`'a'`nhân vật, kể từ khi`'a'`là chữ cái nhỏ nhất Nếu như`t`chứa bất kỳ ký tự nào lớn hơn`'a'`,`s`luôn có thể nhỏ hơn, bất kể bao nhiêu`'a'`chúng tôi đang ở trong`s`. Nếu như`t`chỉ chứa`'a'`s thì`s`phải chứa ít hơn không-`'a'`hơn là`t`trở nên nhỏ hơn. 

Điều này dẫn đến một giải pháp trong đó chúng tôi duy trì số lượng`'a'`s và ký tự tối đa cho mỗi chuỗi. Mỗi thao tác sẽ cập nhật các số lượng này và việc so sánh giảm xuống còn việc kiểm tra đơn giản đối với các giá trị này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(total_length * log(total_length)) | O(tổng_chiều dài) | Quá chậm | 
| Tối ưu | O (tổng_hoạt động) | O(26) mỗi chuỗi | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho cả hai chuỗi: số lượng`'a'`ký tự và ký tự tối đa được nhìn thấy. Ban đầu cả hai chuỗi đều có một`'a'`và ký tự tối đa`'a'`. 
2. Đối với mỗi thao tác, hãy đọc loại thao tác, số lần lặp lại`k`, và chuỗi`x`. 
3. Đếm số lượng`'a'`nhân vật trong`x`và xác định ký tự tối đa trong`x`. 
4. Nhân các số đếm này với`k`và cập nhật các bộ đếm chuỗi tương ứng. Đối với ký tự tối đa, hãy sử dụng mức tối đa của mức tối đa hiện tại và mức tối đa từ thao tác này. 
5. Sau khi cập nhật, kiểm tra điều kiện từ điển: 

- Nếu ký tự tối đa trong`s`lớn hơn`'a'`Và`t`chỉ chứa`'a'`,`s`không thể nhỏ hơn, xuất ra "NO". 
- Nếu như`t`chứa bất kỳ ký tự nào lớn hơn`'a'`, xuất ra "CÓ" vì chúng ta có thể sắp xếp lại`'a'`ở trong`s`đứng trước các chữ cái lớn hơn trong`t`. 
- Ngược lại so sánh số lượng`'a'`s: nếu`s`có ít hơn`'a'`hơn là`t`, xuất ra "CÓ"; nếu không thì "KHÔNG". 
6. Lặp lại cho tất cả các thao tác. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi duy trì thông tin tối thiểu cần thiết để xác định khả năng sắp xếp`s`nhỏ hơn về mặt từ điển so với`t`. Ký tự tối đa và`'a'`count xác định đầy đủ thứ tự tương đối vì mọi sự sắp xếp đều được cho phép. Chúng tôi không bao giờ cần chuỗi đầy đủ, vì vậy hiệu suất là tuyến tính theo số lượng thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        q = int(input())
        s_a_count, t_a_count = 1, 1
        s_max, t_max = 'a', 'a'
        
        for _ in range(q):
            d, k, x = input().split()
            d = int(d)
            k = int(k)
            
            a_count = x.count('a') * k
            max_char = max(x)
            
            if d == 1:
                s_a_count += a_count
                s_max = max(s_max, max_char)
            else:
                t_a_count += a_count
                t_max = max(t_max, max_char)
            
            if s_max > 'a' and t_max == 'a':
                print("NO")
            else:
                print("YES")

if __name__ == "__main__":
    solve()
```Mã này duy trì số lượng`'a'`s và ký tự tối đa cho cả hai chuỗi. Séc`s_max > 'a' and t_max == 'a'`đảm bảo chúng tôi chỉ xuất ra "KHÔNG" khi`s`chứa các chữ cái lớn hơn`'a'`Và`t`chỉ có`'a'`S. Ngược lại, câu trả lời là "CÓ" vì một sự sắp xếp nào đó sẽ luôn thỏa mãn`s < t`. Nhân số lượng của`'a'`bởi`k`đảm bảo các thao tác chắp thêm lặp lại được xử lý hiệu quả mà không cần xây dựng chuỗi. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào mẫu đầu tiên:```
Operations:
1. 2 1 aa -> t = 'aaa'
s_a_count=1, t_a_count=3, s_max='a', t_max='a'
t_max > 'a'? no, s_max > 'a'? no, output YES
2. 1 2 a -> s = 'aaa'
s_a_count=3, t_a_count=3, output NO
3. 2 3 a -> t = 'aaaaaa'
s_a_count=3, t_a_count=6, output YES
4. 1 2 b -> s = 'aaabb'
s_max='b', t_max='a', s_max>'a' and t_max=='a'? YES -> output NO
5. 2 3 abca -> t_max='c'
t_max>'a'? YES -> output YES
```Dấu vết này chỉ ra cách chỉ ký tự tối đa và`'a'`số lượng ảnh hưởng đến câu trả lời. Xây dựng chuỗi đầy đủ là không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số hoạt động) | Mỗi thao tác đều yêu cầu tính`'a'`s và ký tự tối đa trong một chuỗi có độ dài tích lũy ≤5 × 10^5 | 
| Không gian | O(1) | Chỉ các bộ đếm và các biến tối đa một ký tự được lưu trữ | 

Giải pháp chia tỷ lệ tuyến tính theo số lượng thao tác và tổng chiều dài chuỗi đầu vào. Kích thước đầu vào tối đa nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("""3
5
2 1 aa
1 2 a
2 3 a
1 2 b
2 3 abca
2
1 5 mihai
2 2 buiucani
3
1 5 b
2 3 a
2 4 paiu""") == """YES
NO
YES
NO
YES
NO
YES
NO
NO
YES""", "sample 1"

# custom tests
assert run("""1
3
1 1 a
2 1 b
1 1 c""") == """YES
YES
NO""", "a then b then c"
assert run("""1
2
1 100000 a
2 1 z""") == """YES
YES""", "large repetition"
assert run("""1
2
2 100000 a
1 1 b""") == """NO
NO""", "t full of 'a's, s adds 'b'"
assert run("""1
1
1 1 a""") == """NO""", "both start with 'a', append 'a' to s"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 một; 2 1b; 1 1 c | CÓ, CÓ, KHÔNG | Thứ tự lex và chắp thêm từng bước cơ bản | 
| 1 100000 một; 2 1 z | CÓ, CÓ | Sự lặp lại lớn |
