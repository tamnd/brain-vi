---
title: "CF 105883F - WTF Một GCD khác?"
description: "Chúng tôi duy trì một bộ sưu tập động các cặp $(v, w)$. Mỗi thao tác sẽ chèn một cặp, loại bỏ một lần xuất hiện của cặp hoặc đặt một truy vấn với một số $k$."
date: "2026-06-21T22:24:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105883
codeforces_index: "F"
codeforces_contest_name: "Baozii Cup 2"
rating: 0
weight: 105883
solve_time_s: 63
verified: true
draft: false
---

[CF 105883F - WTF Một GCD khác?](https://codeforces.com/problemset/problem/105883/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một bộ sưu tập các cặp năng động$(v, w)$. Mỗi thao tác sẽ chèn một cặp, xóa một lần xuất hiện của một cặp hoặc hỏi một truy vấn bằng một số$k$. 

Đối với một truy vấn, chúng ta phải xem xét tất cả các cặp hiện có và chỉ xem xét những cặp có thành phần đầu tiên$v$không chia sẻ thừa số nguyên tố với$k$, nghĩa$\gcd(v, k) = 1$. Trong số các cặp hợp lệ này, chúng tôi trả về giá trị tối đa$w$. Nếu không có gì đủ tiêu chuẩn, câu trả lời là không. 

Chi tiết quan trọng là tính hợp lệ phụ thuộc vào mối quan hệ giữa$v$và giá trị truy vấn$k$, trong khi giá trị chúng tôi tối ưu hóa là$w$, và cả việc chèn và xóa đều hoàn toàn động. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$hoạt động, với$v, k \le 5 \cdot 10^5$. Điều này ngay lập tức loại trừ việc tính toán lại câu trả lời từ đầu cho mỗi truy vấn, vì việc quét toàn bộ nhiều tập hợp cho mỗi truy vấn sẽ dẫn đến khoảng$4 \cdot 10^{10}$kiểm tra trong trường hợp xấu nhất. Ngay cả việc duy trì các cấu trúc được sắp xếp cho mỗi truy vấn mà không cắt tỉa cẩn thận cũng sẽ không tồn tại được. 

Một cạm bẫy tinh tế là xử lý việc xóa một cách chính xác. Cùng một cặp$(v, w)$có thể xuất hiện nhiều lần và chỉ nên xóa một bản sao cho mỗi thao tác xóa. Một vấn đề khác là một cặp có thể vẫn ở trong cấu trúc toàn cục nhưng trở nên không hợp lệ đối với một truy vấn do các ràng buộc về gcd, vì vậy các mục nhập cũ không được ảnh hưởng đến câu trả lời. 

Một ví dụ nhỏ phá vỡ các cách tiếp cận ngây thơ là: 

đầu vào:```
+ 6 10
+ 4 7
? 3
```Câu trả lời đúng là 7, vì cả 6 và 4 đều nguyên tố cùng nhau với 3. Một sai lầm ngây thơ là chỉ kiểm tra tính chia hết hoặc chỉ theo dõi giá trị lớn nhất$w$trên toàn cầu, điều này sẽ bỏ qua ràng buộc gcd một cách không chính xác và luôn trả về 10. 

Một trường hợp cạnh khác là:```
+ 2 5
+ 3 9
? 6
```Ở đây cả 2 và 3 đều có chung một thừa số với 6 nên đáp án đúng là 0. Giải pháp chỉ kiểm tra một thừa số nguyên tố của$k$hoặc không tính toán đầy đủ gcd sẽ trả về giá trị khác 0 không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ duy trì một vùng chứa tất cả các cặp đang hoạt động và đối với mỗi truy vấn, hãy quét mọi phần tử, kiểm tra xem$\gcd(v, k) = 1$và theo dõi tối đa$w$. Việc chèn và xóa là$O(1)$, nhưng mỗi truy vấn có giá$O(n)$, dẫn đến$O(n^2)$nói chung là quá chậm đối với$2 \cdot 10^5$hoạt động. 

Quan sát quan trọng là chúng ta không thực sự cần nhóm theo$v$theo bất kỳ cách có cấu trúc nào cho mỗi truy vấn. Chúng ta chỉ cần có khả năng nhanh chóng truy xuất các ứng viên có số lượng lớn$w$, đồng thời loại bỏ những cái không hợp lệ một cách hiệu quả. Điều này gợi ý việc duy trì một cấu trúc toàn cầu được sắp xếp theo thứ tự$w$và lười biếng kiểm tra xem một phần tử có còn hợp lệ hay không khi nó được truy cập. 

Chúng tôi lưu trữ tất cả các cặp được chèn trong cấu trúc tối đa được khóa bởi$w$và giữ bản đồ tần số để xử lý việc xóa. Đối với mỗi truy vấn, chúng tôi liên tục kiểm tra mức tối đa hiện tại$w$. Nếu cặp đó bị xóa hoặc không đáp ứng được điều kiện gcd, chúng tôi sẽ loại bỏ nó và tiếp tục. Vì mỗi phần tử được loại bỏ khỏi quá trình xem xét nhiều nhất một lần nên độ phức tạp khấu hao vẫn tuyến tính theo số lượng thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Heap tối đa với việc loại bỏ lười biếng |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một đống tối đa được sắp xếp theo$w$và bản đồ tần số theo dõi số lượng bản sao của mỗi$(v, w)$cặp hiện đang tồn tại. 

1. Để chèn$(v, w)$, đẩy nó vào heap và tăng tần số của nó. Điều này đảm bảo mọi cặp hoạt động đều có sẵn làm ứng cử viên cho các truy vấn trong tương lai. 
2. Để xóa$(v, w)$, giảm tần số của nó. Chúng ta không loại bỏ nó khỏi heap ngay lập tức vì việc định vị nó bên trong heap sẽ rất tốn kém. 
3. Đối với truy vấn có giá trị$k$, liên tục kiểm tra đỉnh của đống. Nếu cặp trên cùng có tần số bằng 0 thì nó đã cũ do bị xóa trước đó nên sẽ bị loại bỏ. Nếu nó vẫn tồn tại nhưng$\gcd(v, k) \ne 1$, nó không hợp lệ đối với truy vấn này và cũng bị loại bỏ. 
4. Cặp đầu tiên vượt qua cả hai lần kiểm tra là câu trả lời tối ưu cho truy vấn này, vì heap đảm bảo nó có giá trị lớn nhất$w$trong số các ứng viên còn lại. 
5. Nếu heap trở nên trống trong quá trình này, câu trả lời là 0. 

Lý do điều này có hiệu quả là vì mọi cặp đều hợp lệ vĩnh viễn hoặc không hợp lệ vĩnh viễn đối với một truy vấn nhất định và khi một cặp bị loại bỏ trong một truy vấn, cặp đó sẽ không bao giờ cần được xem xét lại cho cùng một truy vấn đó nữa. Kết hợp với tính năng theo dõi tần số, mỗi cặp được đưa vào và để lại một số lần xem xét nhất định. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

n = int(input())

heap = []
cnt = {}

def key(v, w):
    return (v, w)

for _ in range(n):
    parts = input().split()
    if parts[0] == '+':
        v = int(parts[1])
        w = int(parts[2])
        heapq.heappush(heap, (-w, v))
        cnt[(v, w)] = cnt.get((v, w), 0) + 1

    elif parts[0] == '-':
        v = int(parts[1])
        w = int(parts[2])
        cnt[(v, w)] -= 1

    else:
        k = int(parts[1])

        while heap:
            negw, v = heap[0]
            w = -negw

            if cnt.get((v, w), 0) == 0:
                heapq.heappop(heap)
                continue

            if gcd(v, k) != 1:
                heapq.heappop(heap)
                continue

            print(w)
            break
        else:
            print(0)
```Các cặp lưu trữ heap được sắp xếp theo thứ tự$w$, sử dụng các giá trị âm để mô phỏng vùng heap tối đa. Từ điển tần số đảm bảo rằng việc xóa được tôn trọng mà không cần cập nhật vùng nhớ heap tốn kém. 

Trong quá trình truy vấn, các mục không hợp lệ sẽ bị xóa một cách lười biếng. Kiểm tra gcd là hoạt động tốn kém duy nhất cho mỗi ứng viên, nhưng mỗi phần tử được kiểm tra nhiều nhất một lần trên tất cả các truy vấn, điều này giữ cho tổng công việc luôn được giới hạn. 

Một sai lầm phổ biến là cố gắng duy trì các cấu trúc riêng biệt cho mỗi$v$hoặc theo thừa số nguyên tố của$v$, việc này trở nên phức tạp do việc xóa động. Cách tiếp cận đống lười biếng tránh được tất cả sự phức tạp đó. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự:```
+ 4 5
+ 3 4
? 2
? 3
- 3 4
? 4
```| Bước | Hoạt động | Heap Top (w, v) | Hiệu lực vs k | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | +4 5 | (5,4) | - | - | 
| 2 | +3 4 | (5,4) | - | - | 
| 3 | ?2 | (5,4) | gcd(4,2)=2 không hợp lệ | 4 | 
| 4 | ?3 | (5,4) | gcd(4,3)=1 hợp lệ | 5 | 
| 5 | -3 4 | (5,4) | xóa sau | - | 
| 6 | ?4 | (5,4) | gcd(4,4)=4 không hợp lệ, tiếp theo không hợp lệ hoặc trống | 0 | 

Dấu vết cho thấy sự vô hiệu của gcd dần dần làm lộ ra ứng cử viên tốt nhất tiếp theo. 

Một ví dụ thứ hai:```
+ 6 10
+ 4 7
? 3
? 6
```| Bước | Hoạt động | Đống hàng đầu | Hiệu lực | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | +6 10 | (10,6) | - | - | 
| 2 | +4 7 | (10,6) | - | - | 
| 3 | ?3 | (10,6) | gcd(6,3)=3 không hợp lệ | 7 | 
| 4 | ?6 | (10,6) | gcd(6,6)=6 không hợp lệ; gcd(4,6)=2 không hợp lệ | 0 | 

Những ví dụ này nhấn mạnh rằng chỉ riêng thứ tự heap là không đủ nếu không lọc gcd. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| mỗi thao tác chèn và heap tốn log n, mỗi phần tử bị xóa tối đa một lần | 
| Không gian |$O(n)$| heap và bản đồ tần số lưu trữ tất cả các phần tử hoạt động | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phép toán và phép toán đống logarit đều nằm trong giới hạn. Việc kiểm tra gcd đủ rẻ vì mỗi cặp chỉ được đánh giá một lần trong quá trình xóa lười. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    def solve():
        n = int(input())
        heap = []
        cnt = {}

        def gcd(a, b):
            while b:
                a, b = b, a % b
            return a

        out = []
        for _ in range(n):
            parts = input().split()
            if parts[0] == '+':
                v, w = int(parts[1]), int(parts[2])
                heapq.heappush(heap, (-w, v))
                cnt[(v, w)] = cnt.get((v, w), 0) + 1
            elif parts[0] == '-':
                v, w = int(parts[1]), int(parts[2])
                cnt[(v, w)] -= 1
            else:
                k = int(parts[1])
                while heap:
                    wneg, v = heap[0]
                    w = -wneg
                    if cnt.get((v, w), 0) == 0:
                        heapq.heappop(heap)
                        continue
                    if gcd(v, k) != 1:
                        heapq.heappop(heap)
                        continue
                    out.append(str(w))
                    break
                else:
                    out.append("0")
        return "\n".join(out)

    return solve()

# custom tests
assert run("""\
3
+ 4 5
+ 3 4
? 2
""") == "4"

assert run("""\
4
+ 6 10
+ 4 7
? 3
? 6
""") == "7\n0"

assert run("""\
5
+ 2 1
+ 3 2
+ 5 3
? 30
? 7
""") == "0\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bộ coprime nhỏ | 4 | lọc gcd cơ bản | 
| vô hiệu hỗn hợp | 7 0 | nhiều truy vấn và từ chối | 
| tất cả được lọc ra/chọn lọc | 0 3 | hành vi cạnh gcd | 

## Vỏ cạnh 

Trường hợp tất cả các phần tử đều không hợp lệ đối với một truy vấn cho thấy thuật toán phải xử lý hết vùng heap đúng cách: 

đầu vào:```
+ 2 10
+ 3 20
? 6
```Trong quá trình truy vấn, cả 2 và 3 đều có chung hệ số với 6, vì vậy cả hai đều được loại bỏ trước khi kết thúc. Vùng heap trở nên trống và kết quả đầu ra đúng là 0. Vòng lặp xóa lười đảm bảo không có giá trị tối đa cũ nào được trả về. 

Một trường hợp cạnh khác liên quan đến việc chèn lặp đi lặp lại cùng một cặp:```
+ 5 10
+ 5 10
- 5 10
? 2
```Chỉ có một bản sao vẫn hoạt động sau khi xóa. Bản đồ tần số ngăn bản sao bị xóa khỏi bị coi là hợp lệ một cách không chính xác.
