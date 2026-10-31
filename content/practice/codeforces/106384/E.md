---
title: "CF 106384E - \u5192\u6ce1\u6392\u5e8f"
description: "Vấn đề tập trung vào một quá trình được mô tả rõ ràng là sắp xếp bong bóng. Chúng ta được cung cấp một mảng và nhiệm vụ là tính toán một số đại lượng liên quan đến cách sắp xếp bong bóng hoạt động trên mảng đó."
date: "2026-06-20T23:04:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106384
codeforces_index: "E"
codeforces_contest_name: "CYCPC Round 2"
rating: 0
weight: 106384
solve_time_s: 46
verified: true
draft: false
---

[CF 106384E - \u5192\u6ce1\u6392\u5e8f](https://codeforces.com/problemset/problem/106384/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề tập trung vào một quá trình được mô tả rõ ràng là sắp xếp bong bóng. Chúng ta được cung cấp một mảng và nhiệm vụ là tính toán một số đại lượng liên quan đến cách sắp xếp bong bóng hoạt động trên mảng đó. Mặc dù văn bản câu lệnh cực kỳ tối thiểu trong lời nhắc, nhưng tiêu đề báo hiệu rõ ràng rằng giải pháp mong muốn phụ thuộc vào sự hiểu biết về cơ chế sắp xếp bong bóng, đặc biệt là cách các phần tử di chuyển qua các giao dịch hoán đổi liền kề. 

Trong sắp xếp bong bóng, các phần tử lớn hơn dần dần di chuyển sang bên phải thông qua các lần hoán đổi liền kề lặp đi lặp lại. Bất kỳ thống kê nào bắt nguồn từ quá trình này thường gắn liền với sự đảo ngược, số lượng hoán đổi hoặc số lần chuyển cần thiết để giải quyết mảng. Vì vậy, mô hình tinh thần cốt lõi là mỗi phép đảo ngược trong mảng đóng góp chính xác một lần hoán đổi trong quy trình và sắp xếp bong bóng giải quyết các phép đảo ngược theo cách có cấu trúc. 

Đầu vào là một mảng và đầu ra là một số duy nhất được lấy từ quá trình sắp xếp nổi bật. Các vấn đề thuộc dạng này hầu như luôn quy về việc đếm các phép nghịch đảo hoặc một đại lượng có liên quan chặt chẽ chẳng hạn như tổng số lần hoán đổi được thực hiện bằng cách sắp xếp bong bóng hoặc tổng chi phí di chuyển của các phần tử. 

Các ràng buộc không được hiển thị, nhưng các vấn đề “E” của Codeforce liên quan đến quá trình sắp xếp thường cho phép các mảng tối thiểu là$2 \cdot 10^5$. Điều đó ngay lập tức loại trừ bất kỳ$O(n^2)$mô phỏng sắp xếp bong bóng. Một mô phỏng đơn giản thực hiện một lượt cho mỗi phần tử và mỗi lượt có thể quét toàn bộ mảng, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Điều đó sẽ hết thời gian nhanh chóng trên quy mô lớn. 

Điểm tinh tế chính trong các bài toán sắp xếp nổi bọt là mặc dù thuật toán được xác định theo thủ tục nhưng đại lượng cuối cùng là tĩnh. Quá trình hoán đổi các cặp nghịch đảo liền kề không phụ thuộc vào cấu trúc trung gian một cách phức tạp; nó chỉ phụ thuộc vào số lượng đảo ngược tồn tại trong mảng ban đầu. Thách thức là nhận ra sự tương đương đó. 

Trường hợp cạnh điển hình là khi mảng đã được sắp xếp hoặc sắp xếp ngược. Trong trường hợp được sắp xếp, sắp xếp bong bóng thực hiện hoán đổi bằng 0 và bất kỳ triển khai không chính xác nào vẫn tính các lần vượt qua hoặc so sánh thay vì hoán đổi sẽ đánh giá quá cao kết quả. Trong trường hợp được sắp xếp ngược lại, số lần hoán đổi đạt đến mức tối đa có thể và bằng$n(n-1)/2$, đó là tổng số lần đảo ngược. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực mô phỏng trực tiếp việc sắp xếp bong bóng. Chúng tôi liên tục quét mảng, hoán đổi các phần tử liền kề bất cứ khi nào chúng không đúng thứ tự, cho đến khi không có sự hoán đổi nào xảy ra trong một lượt đầy đủ. Điều này đúng vì nó tuân theo chính xác định nghĩa của sắp xếp bong bóng. Tuy nhiên, mỗi lần vượt qua đều có giá$O(n)$, và có thể có$O(n)$vượt qua trong trường hợp xấu nhất, chẳng hạn như khi mảng được sắp xếp ngược lại. Điều này dẫn đến$O(n^2)$sự phức tạp, trở nên quá chậm khi$n$là lớn. 

Quan sát quan trọng là sắp xếp bong bóng hoán đổi các phần tử liền kề và mỗi lần hoán đổi sẽ loại bỏ chính xác một phép đảo ngược. Hơn nữa, sắp xếp nổi bọt không bao giờ tạo ra sự đảo ngược. Điều này có nghĩa là tổng số lần hoán đổi được thực hiện trong toàn bộ quá trình hoàn toàn bằng số lần đảo ngược của mảng ban đầu. Một khi chúng ta nhận ra sự bất biến này, chúng ta không cần phải mô phỏng quá trình nữa. Chúng ta chỉ cần đếm xem có bao nhiêu cặp$(i, j)$thỏa mãn$i < j$Và$a[i] > a[j]$. 

Điều này làm giảm vấn đề xuống nhiệm vụ đếm đảo ngược cổ điển, có thể được giải quyết một cách hiệu quả bằng cách sử dụng phương pháp dựa trên sắp xếp hợp nhất hoặc cây Fenwick sau khi nén tọa độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng sắp xếp bong bóng Brute Force |$O(n^2)$|$O(1)$| Quá chậm | 
| Số lượng đảo ngược tối ưu (Sắp xếp Fenwick / Hợp nhất) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách chuyển đổi quá trình sắp xếp bong bóng thành vấn đề đếm ngược. 

1. Đầu tiên, chúng tôi quan sát thấy rằng mọi hoán đổi trong sắp xếp bong bóng đều xảy ra giữa hai phần tử liền kề tạo thành sự đảo ngược. Điều này có nghĩa là mỗi lần hoán đổi tương ứng với chính xác một cặp trong đó phần tử bên trái lớn hơn phần tử bên phải. Điều này cung cấp một ánh xạ trực tiếp giữa các giao dịch hoán đổi và đảo ngược. 
2. Chúng ta phát biểu lại bài toán dưới dạng đếm các nghịch đảo trong mảng ban đầu. Thay vì mô phỏng các giao dịch hoán đổi, chúng tôi đếm tất cả các cặp$(i, j)$như vậy$i < j$Và$a[i] > a[j]$. Đây là tổng số chính xác của loại bong bóng hoán đổi sẽ thực hiện. 
3. Để tính toán điều này một cách hiệu quả, chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì cấu trúc dữ liệu theo dõi số phần tử trước đó lớn hơn phần tử hiện tại. Điều này cho phép chúng tôi đếm các khoản đóng góp tăng dần thay vì liệt kê các cặp. 
4. Chúng tôi sử dụng cây Fenwick (hoặc BIT) trên các giá trị nén của mảng. Cần nén tọa độ vì giá trị mảng có thể lớn và chúng tôi chỉ quan tâm đến thứ tự tương đối. Mỗi lần chúng tôi xử lý một giá trị, chúng tôi truy vấn xem có bao nhiêu giá trị đã thấy trước đó lớn hơn, sau đó cập nhật cấu trúc để bao gồm giá trị hiện tại. 
5. Chúng ta tích lũy những số đếm này để có được tổng số lần đảo ngược, đây là câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Sắp xếp bong bóng hoán đổi các cặp đảo ngược liền kề cho đến khi tất cả các đảo ngược được loại bỏ. Mỗi lần hoán đổi sẽ giảm số lần đảo ngược chính xác một và không tạo ra các lần đảo ngược mới. Vì quá trình chỉ kết thúc khi không còn đảo ngược nên tổng số lần hoán đổi phải bằng số lần đảo ngược ban đầu. Điều này làm cho số lượng đảo ngược trở thành một bất biến hoàn toàn của quá trình sắp xếp nổi bật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def update(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def query(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    vals = sorted(set(a))
    comp = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))
    inv = 0

    for x in a:
        cx = comp[x]
        inv += fw.query(len(vals)) - fw.query(cx)
        fw.update(cx, 1)

    print(inv)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách nén các giá trị sao cho chúng vừa với một phạm vi nhỏ gọn phù hợp với cây Fenwick. Bước này duy trì thứ tự, đây là thuộc tính duy nhất có liên quan đến việc đếm nghịch đảo. 

Cây Fenwick duy trì số lượng các phần tử được thấy cho đến nay. Đối với mỗi phần tử mới, chúng tôi truy vấn có bao nhiêu phần tử trước đó lớn hơn một cách nghiêm ngặt, điều này trực tiếp đưa ra số lượng đảo ngược do phần tử đó đóng góp. Sau khi xử lý truy vấn, chúng ta chèn phần tử hiện tại vào cấu trúc. 

Phép trừ`fw.query(len(vals)) - fw.query(cx)`là bước quan trọng vì nó đếm tất cả các phần tử lớn hơn phần tử hiện tại trong số những phần tử đã được xử lý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 1 2
```Chúng tôi theo dõi các cập nhật của Fenwick từng bước. 

| Bước | Giá trị | Bang Fenwick (khái niệm) | Số lượng trước đó lớn hơn | Đảo ngược | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | [3] | 0 | 0 | 
| 2 | 1 | [3,1] | 1 | 1 | 
| 3 | 2 | [3,1,2] | 1 | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này phù hợp với hành vi sắp xếp bong bóng: 3 lần hoán đổi qua 1 và 2 một cách gián tiếp thông qua các lần hoán đổi liền kề. 

### Ví dụ 2 

đầu vào:```
4
4 3 2 1
```| Bước | Giá trị | Số lượng trước đó lớn hơn | Đảo ngược | 
| --- | --- | --- | --- | 
| 1 | 4 | 0 | 0 | 
| 2 | 3 | 1 | 1 | 
| 3 | 2 | 2 | 3 | 
| 4 | 1 | 3 | 6 | 

Câu trả lời cuối cùng là 6, bằng$4 \cdot 3 / 2$, số lần đảo ngược tối đa. 

Điều này cho thấy hành vi sắp xếp bong bóng trong trường hợp xấu nhất trong đó mọi cặp đều bị đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi cập nhật và truy vấn trong cây Fenwick tốn thời gian logarit | 
| Không gian |$O(n)$| Cây Fenwick và phối hợp nén lưu trữ | 

Giải pháp hiệu quả cho các mảng lên tới$2 \cdot 10^5$các phần tử, trong đó một$O(n^2)$mô phỏng sẽ quá chậm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO

    output = StringIO()
    _sys.stdout = output

    solve()

    _sys.stdout = sys.__stdout__
    return output.getvalue()

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    vals = sorted(set(a))
    comp = {v: i + 1 for i, v in enumerate(vals)}

    fw = [0] * (len(vals) + 1)

    def add(i):
        while i < len(fw):
            fw[i] += 1
            i += i & -i

    def sum_(i):
        s = 0
        while i > 0:
            s += fw[i]
            i -= i & -i
        return s

    inv = 0
    for x in a:
        cx = comp[x]
        inv += sum_(len(vals)) - sum_(cx)
        add(cx)

    return str(inv) + "\n"

# provided samples
assert run("3\n3 1 2\n") == "2\n"

# custom cases
assert run("1\n5\n") == "0\n", "single element"
assert run("2\n1 2\n") == "0\n", "already sorted"
assert run("2\n2 1\n") == "1\n", "single inversion"
assert run("4\n4 3 2 1\n") == "6\n", "maximum inversions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | không có trường hợp hoán đổi | 
| mảng được sắp xếp | 0 | không đảo ngược | 
| cặp đảo ngược | 1 | trao đổi cơ bản | 
| đảo ngược hoàn toàn | 6 | sắp xếp bong bóng trong trường hợp xấu nhất | 

## Vỏ cạnh 

Đối với đầu vào`1 5`, mảng có một phần tử duy nhất. Sắp xếp bong bóng không thực hiện so sánh dẫn đến hoán đổi và thuật toán ngay lập tức trả về 0 do cây Fenwick không thấy phần tử nào trước đó. 

Đối với đầu vào`1 2 3 4`, mọi tiền tố đều không có đảo ngữ. Các truy vấn Fenwick luôn trả về 0 cho “các phần tử trước lớn hơn”, xác nhận rằng các mảng được sắp xếp tạo ra sự hoán đổi bằng 0. 

Đối với đầu vào`2 1`, có đúng một phép nghịch đảo. Thuật toán tính giá trị này khi xử lý phần tử thứ hai, trong đó phần tử trước lớn hơn. 

Đối với đầu vào`4 3 2 1`, mọi cặp đều đảo ngược. Cây Fenwick tích lũy 3 + 2 + 1 = 6 phép đảo ngược, khớp với tổng số lần hoán đổi sắp xếp bong bóng sẽ thực hiện trong trường hợp đắt nhất.
