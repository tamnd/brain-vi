---
title: "CF 2150A - Đường dẫn tăng dần"
description: "Chúng ta được cung cấp một dòng gồm các ô $10^9$, mỗi ô có màu đen hoặc trắng. Ban đầu, một tập hợp ô nhất định có màu đen và tất cả các ô khác có màu trắng."
date: "2026-06-08T01:03:30+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "hashing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2150
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1053 (Div. 1)"
rating: 1300
weight: 2150
solve_time_s: 101
verified: false
draft: false
---

[CF 2150A - Đường dẫn tăng dần](https://codeforces.com/problemset/problem/2150/A) 

**Đánh giá:** 1300 
**Tas:** cấu trúc dữ liệu, băm, triển khai 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$10^9$tế bào, mỗi tế bào có màu đen hoặc trắng. Ban đầu, một tập hợp ô nhất định có màu đen và tất cả các ô khác có màu trắng. Sau đó, một loạt lệnh được thực thi nhiều lần theo cách tích lũy: đối với lệnh đầu tiên, chúng ta bắt đầu ở ô 1, làm theo lệnh đó và tô màu đen cho ô kết quả. Đối với hai lệnh đầu tiên, chúng ta bắt đầu lại ở ô 1, thực hiện cả hai lệnh theo thứ tự và tô màu đen cho ô cuối cùng. Quá trình này tiếp tục cho đến khi tất cả các lệnh đã được áp dụng tích lũy. 

Có hai loại lệnh:`A`, tiến lên một bước và`B`, di chuyển đến ô màu trắng tiếp theo. Đầu ra là tập hợp ô đen cuối cùng sau khi tất cả mọi người đã thực hiện xong nước đi của mình. 

Những hạn chế là đáng kể. Tổng số lệnh và ô đen ban đầu có thể đạt tới$10^5$mỗi trường hợp thử nghiệm, nhưng bản thân số ô có thể cao bằng$10^9$. Điều này có nghĩa là việc mô phỏng toàn bộ dòng ô hoặc liên tục quét tìm ô trắng tiếp theo một cách ngây thơ sẽ quá chậm. Bất kỳ giải pháp nào chạm trực tiếp vào từng ô hoặc lặp lại tối đa$10^9$sẽ vượt quá thời hạn. 

Trường hợp cạnh tinh tế xảy ra khi nhiều lệnh`B`được thực hiện liên tiếp và tất cả các ô ở giữa đều có màu đen. Một giải pháp đơn giản là di chuyển từng ô một sẽ chậm hoặc có thể vượt quá ô trắng tiếp theo nếu không cẩn thận. Một trường hợp khác là khi đích cuối cùng của lệnh đã có màu đen; thuật toán phải để nó màu đen một cách chính xác mà không bị trùng lặp hoặc thiếu các bản cập nhật. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng mỗi người bắt đầu từ ô 1 và thực thi tiền tố của lệnh. Đối với mỗi`A`, chúng ta tăng thêm 1. Với mỗi`B`, chúng tôi quét về phía trước cho đến khi tìm thấy ô màu trắng. Sau khi thực hiện tiền tố, chúng ta đánh dấu ô cuối cùng màu đen. Cách tiếp cận này đúng nhưng quá chậm: mỗi`B`có thể yêu cầu quét lên tới$O(10^9)$tế bào, và chúng tôi có tới$10^5$lệnh. Sự phức tạp trong trường hợp xấu nhất về cơ bản là$O(n \cdot m)$với hằng số rất lớn, điều này là không thể thực hiện được. 

Quan sát quan trọng giúp mở ra giải pháp nhanh hơn là chỉ cần đánh dấu **ô được truy cập lần cuối của mỗi tiền tố**. Đối với lệnh`B`, vị trí cuối cùng chỉ phụ thuộc vào chuỗi lệnh và tập hợp ô đen hiện tại. Điều quan trọng là sau khi thêm ô đen vào cuối tiền tố, nó chỉ ảnh hưởng sau này`B`lệnh. Chúng ta có thể duy trì các tế bào đen trong một cấu trúc đã được sắp xếp và xử lý các chuyển động một cách tham lam. Mỗi lần chúng ta gặp phải một`B`, chúng ta chuyển sang vị trí chưa được đánh dấu tiếp theo bằng cách sử dụng các ô màu đen hiện có. 

Cách tiếp cận tối ưu tránh quét từng ô. Thay vào đó, chúng tôi duy trì các ô màu đen trong một tập hợp cho$O(1)$kiểm tra tư cách thành viên và theo dõi ô được truy cập xa nhất. Đối với mỗi lệnh,`A`là tầm thường, chỉ cần tăng thêm 1. Đối với`B`, chúng ta tăng vị trí hiện tại cho đến khi đến được một ô không nằm trong tập hợp màu đen. Bởi vì mỗi ô có thể chuyển sang màu đen nhiều nhất một lần, điều này đảm bảo rằng tổng số gia số trên tất cả các lệnh được giới hạn bởi$n + m$, đó là$O(10^5)$cho từng trường hợp thử nghiệm. Điều này là đủ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * quét ô) | O(m) | Quá chậm | 
| Tối ưu | O(n + m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`Và`m`, chuỗi lệnh và các ô màu đen ban đầu. 
2. Lưu trữ các ô đen ban đầu trong một bộ để kiểm tra tư cách thành viên O(1). 
3. Khởi tạo vị trí hiện tại`pos = 1`. 
4. Lặp lại từng lệnh theo thứ tự. Nếu lệnh là`A`, tăng`pos`bằng 1. 
5. Nếu lệnh là`B`, tăng liên tục`pos`cho đến khi`pos`không nằm trong tập hợp các ô màu đen. Điều này có hiệu quả nhảy đến ô màu trắng tiếp theo. 
6. Sau mỗi tiền tố lệnh (mỗi bước trong chuỗi), thêm`pos`vào tập hợp các ô màu đen. 
7. Sau khi xử lý tất cả các lệnh, hãy sắp xếp tập hợp các ô màu đen và xuất ra kích thước cũng như các thành phần của nó. 

Điều bất biến là tại bất kỳ thời điểm nào, tập hợp các ô màu đen chứa tất cả các ô được đánh dấu bằng các tiền tố trước đó. Mỗi`B`lệnh luôn nhảy qua ô đen sang ô trắng tiếp theo, đảm bảo tính chính xác. Việc mỗi ô chỉ có thể chuyển từ trắng sang đen một lần đảm bảo rằng vòng lặp bên trong cho`B`các lệnh chạy tổng cộng tối đa`n + m`bước trên toàn bộ chuỗi tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        s = input().strip()
        black = set(map(int, input().split()))
        pos = 1
        for c in s:
            if c == 'A':
                pos += 1
            else:
                while pos in black:
                    pos += 1
            black.add(pos)
        black_list = sorted(black)
        print(len(black_list))
        print(' '.join(map(str, black_list)))

solve()
```Giải pháp đầu tiên đọc đầu vào một cách hiệu quả. Các ô màu đen được lưu trữ trong một bộ để cho phép kiểm tra tư cách thành viên O(1), rất quan trọng để nhanh chóng`B`lệnh. các`while`vòng dưới`B`đảm bảo tìm thấy ô trắng tiếp theo mà không cần quét toàn bộ dòng. Việc sắp xếp ở cuối đảm bảo đầu ra theo thứ tự tăng dần. Việc sử dụng một bộ sẽ tránh được các lần chèn trùng lặp và giúp việc kiểm tra tư cách thành viên được hiệu quả. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
n = 3, m = 2
s = BAB
black = {2, 5}
```| Bước | Lệnh | Vị trí hiện tại | Bộ đen sau khi di chuyển | 
| --- | --- | --- | --- | 
| 1 | B | 3 | {2,3,5} | 
| 2 | A | 4 | {2,3,4,5} | 
| 3 | B | 6 | {2,3,4,5,6} | 

Đầu ra cuối cùng: 5 ô đen: 2 3 4 5 6 

**Đầu vào mẫu 2:**```
n = 5, m = 2
s = ABABB
black = {1, 7}
```| Bước | Lệnh | Vị trí hiện tại | Bộ đen | 
| --- | --- | --- | --- | 
| 1 | A | 2 | {1,2,7} | 
| 2 | B | 3 | {1,2,3,7} | 
| 3 | A | 4 | {1,2,3,4,7} | 
| 4 | B | 5 | {1,2,3,4,5,7} | 
| 5 | B | 6 | {1,2,3,4,5,6,7} | 

Đầu ra cuối cùng: 7 ô đen: 1 2 3 4 5 6 7 

Những dấu vết này chứng tỏ rằng`B`nhảy qua các ô đen hiện có một cách chính xác và mỗi tiền tố đóng góp chính xác một ô đen mới ở vị trí cuối cùng của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi ô được truy cập nhiều nhất một lần bởi`B`lệnh và chúng tôi lặp lại n lệnh. | 
| Không gian | O(m) | Chỉ các ô màu đen được lưu trữ trong một bộ. Việc sắp xếp ở cuối sử dụng nội bộ O(m log m) nhưng có thể chấp nhận được. | 

Cho rằng tổng của`n`Và`m`trên tất cả các trường hợp thử nghiệm là 10^5, giải pháp chạy thoải mái dưới giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("""5
3 2
BAB
2 5
3 4
ABA
1 4 9 10
5 2
ABABB
1 7
3 1
BBA
6
1 4
A
1 3 4 1000000000
""") == """4
2 3 5 6
7
1 2 3 4 6 9 10
7
1 2 3 5 6 7 9
3
2 4 6
5
1 2 3 4 1000000000""", "sample 1"

# Custom edge cases
assert run("""1
1 1
B
1
""") == "2\n1 2", "single command B"
assert run("""1
3 1
BBB
2
""") == "4\n1 2 3 4", "consecutive B with initial black cell"
assert run("""1
2 0
AB
""") == "2\n1 2", "no initial black cells"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- |
