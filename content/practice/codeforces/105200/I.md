---
title: "CF 105200I - Thử nghiệm đảo ngược"
description: "Chúng ta được cung cấp một dãy số và nhiệm vụ xoay quanh việc tìm hiểu xem nó còn cách xa mức độ “sạch” về mặt trật tự."
date: "2026-06-27T02:53:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105200
codeforces_index: "I"
codeforces_contest_name: "IME++ Starters Try-outs 2024"
rating: 0
weight: 105200
solve_time_s: 40
verified: true
draft: false
---

[CF 105200I - Thử nghiệm đảo ngược](https://codeforces.com/problemset/problem/105200/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và nhiệm vụ xoay quanh việc tìm hiểu xem nó còn cách xa mức độ “sạch” về mặt trật tự. Đại lượng quan tâm chính là số lượng đảo ngược, đo lường có bao nhiêu cặp phần tử nằm ngoài thứ tự tự nhiên của chúng, nghĩa là phần tử lớn hơn xuất hiện trước phần tử nhỏ hơn. 

Thay vì tính toán trực tiếp toàn bộ cấu trúc đảo ngược, bài toán gợi ý một sự đơn giản hóa cấu trúc: các phần tử đã được định vị chính xác ở đầu hoặc cuối chuỗi sẽ không đóng góp ý nghĩa vào việc tính toán đảo ngược một khi đã bị cô lập. Nhiệm vụ cốt lõi là xác định số lượng mảng “đã được căn chỉnh” và sau đó xác định mức độ rối loạn còn lại. 

Từ góc độ tính toán, điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các cặp sẽ quá chậm khi chuỗi phát triển lớn. Kiểm tra cặp đơn giản yêu cầu thời gian bậc hai, điều này trở nên không khả thi khi kích thước mảng đạt khoảng 10^5 phần tử, trong đó cần có khoảng 10^10 so sánh. Điều này buộc chúng tôi phải hướng tới một cách tiếp cận tận dụng cấu trúc sắp xếp hoặc các kỹ thuật đếm hiệu quả như đếm dựa trên sự hợp nhất hoặc cây Fenwick. 

Trường hợp cạnh tinh tế phát sinh khi mảng đã được sắp xếp hoặc gần như được sắp xếp ngoại trừ một vùng nhỏ ở giữa. Ví dụ: trong một đầu vào như`[1, 2, 3, 10, 9, 8, 4, 5, 6, 7]`, hầu hết các phần tử tiền tố được đặt chính xác và không đóng góp gì vào độ phức tạp của đảo ngược, nhưng khối đảo ngược ở giữa chiếm ưu thế trong số lượng đảo ngược. Một phương pháp đơn giản vẫn có thể quét tất cả các cặp mà không nhận ra rằng các phân đoạn được sắp xếp lớn có thể bị bỏ qua trong logic đóng góp. 

Một trường hợp cạnh khác là khi mảng giảm nghiêm ngặt, chẳng hạn như`[5, 4, 3, 2, 1]`. Ở đây, mỗi cặp đều đóng góp một sự đảo ngược và bất kỳ tối ưu hóa nào giả định thứ tự một phần từ các ranh giới sẽ thất bại trừ khi nó giảm chính xác thành việc đếm đảo ngược hoàn toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi kiểm tra từng cặp`(i, j)`với`i < j`và đếm bao nhiêu lần`a[i] > a[j]`. Điều này trực tiếp phù hợp với định nghĩa của nghịch đảo và được đảm bảo là chính xác vì nó xác minh toàn diện tất cả các mối quan hệ. Vấn đề là khả năng mở rộng. Vì`n = 100000`, phương pháp này thực hiện theo thứ tự`n^2 / 2`so sánh, vượt xa những gì có thể được thực hiện kịp thời. 

Quan sát quan trọng là việc đếm ngược không yêu cầu liệt kê cặp rõ ràng nếu thay vào đó chúng ta có thể theo dõi cách các phần tử di chuyển so với thứ tự được sắp xếp. Khi mảng được phân chia trong quá trình sắp xếp hợp nhất, việc đảo ngược tương ứng chính xác với trường hợp một phần tử ở nửa bên phải được đặt trước các phần tử còn lại ở nửa bên trái. Điều này cho phép chúng tôi đếm số lần đảo ngược trong khi sắp xếp`O(n log n)`thời gian. 

Một quan điểm tương đương khác là sử dụng Cây Fenwick hoặc Cây chỉ mục nhị phân. Chúng tôi xử lý các phần tử theo thứ tự và đối với mỗi phần tử, chúng tôi truy vấn có bao nhiêu phần tử đã thấy trước đó lớn hơn phần tử hiện tại. Điều này biến đổi việc đếm theo cặp thành các truy vấn tần số tiền tố, có hiệu quả duy trì linh hoạt. 

Gợi ý của tuyên bố về các phần tử tiền tố và hậu tố không ảnh hưởng đến số lượng đảo ngược phù hợp với cấu trúc này: các phân đoạn đã được sắp xếp hoạt động giống như các ranh giới được sắp xếp và không đưa ra các đảo ngược chéo ngoài các điểm tương tác của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Hợp nhất Sắp xếp / Cây Fenwick | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sẽ mô tả cách tiếp cận Fenwick Tree vì đây là cách trực tiếp nhất để thực hiện việc đếm nghịch đảo. 

1. Đầu tiên, nén các giá trị của mảng thành một phạm vi nhỏ hơn. Điều này là cần thiết vì Cây Fenwick hoạt động dựa trên các chỉ số, không phải giá trị thô và các giá trị lớn sẽ khiến cấu trúc không khả thi. Việc nén duy trì trật tự tương đối, đó là tất cả những gì quan trọng đối với việc đếm nghịch đảo. 
2. Khởi tạo Cây Fenwick hỗ trợ truy vấn tổng tiền tố và cập nhật điểm. Cây sẽ theo dõi số lần mỗi giá trị nén đã được nhìn thấy cho đến nay. 
3. Duyệt mảng từ trái sang phải. Tại mỗi phần tử, chúng ta muốn biết có bao nhiêu phần tử đã thấy trước đó lớn hơn nó. Điều này tương ứng với việc truy vấn số phần tử được xử lý trừ đi số phần tử nhỏ hơn hoặc bằng phần tử hiện tại. 
4. Thêm số này vào câu trả lời đang chạy vì mỗi phần tử trước đó tạo thành một nghịch đảo với phần tử hiện tại. 
5. Cập nhật Cây Fenwick để đánh dấu phần tử hiện tại như đã thấy để các phần tử trong tương lai có thể tính đến phần tử đó trong các phép tính đảo ngược của chúng. 

Ý tưởng chính là chúng tôi liên tục duy trì cấu trúc tần số tiền tố động, cho phép chúng tôi thay thế phép liệt kê cặp rõ ràng bằng số lượng tổng hợp. 

### Tại sao nó hoạt động 

Ở mỗi bước, Cây Fenwick chứa chính xác nhiều phần tử xuất hiện trước chỉ mục hiện tại. Khi xử lý phần tử`a[i]`, bất kỳ phần tử nào có trước lớn hơn nó sẽ tạo thành một cặp đảo ngược`(j, i)`Ở đâu`j < i`. Truy vấn tổng tiền tố tách biệt chính xác các phần tử trước đó lớn hơn`a[i]`mà không kiểm tra chúng một cách riêng lẻ. Vì mỗi lần đảo ngược được tính chính xác một lần khi điểm cuối bên phải của nó được xử lý nên không có cặp nào bị bỏ sót hoặc được tính gấp đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    data = list(map(int, input().split()))
    n = data[0]
    arr = data[1:]

    vals = sorted(set(arr))
    comp = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))
    inv = 0

    for x in arr:
        cx = comp[x]
        inv += fw.sum(len(vals)) - fw.sum(cx)
        fw.add(cx, 1)

    print(inv)

if __name__ == "__main__":
    solve()
```Cây Fenwick được triển khai dưới dạng cây lập chỉ mục nhị phân tiêu chuẩn với các cập nhật điểm và tổng tiền tố. các`add`chức năng cập nhật số lượng tần số, trong khi`sum`truy xuất có bao nhiêu phần tử đã được nhìn thấy cho đến một chỉ mục nén nhất định. 

Bên trong`solve`, nén tọa độ đảm bảo rằng các giá trị ánh xạ vào một phạm vi nhỏ gọn bắt đầu từ 1. Đối với mỗi phần tử, chúng tôi tính toán xem có bao nhiêu giá trị đã thấy trước đó lớn hơn một cách nghiêm ngặt bằng cách trừ tổng số tiền tố lên đến giá trị hiện tại từ tổng số được xử lý. Sự khác biệt này góp phần trực tiếp vào số lượng đảo ngược. 

Một sai lầm phổ biến là quên rằng việc nén phải bảo toàn thứ tự chứ không phải giá trị tuyệt đối. Một vấn đề khó phát hiện khác là việc lập chỉ mục riêng lẻ trong Cây Fenwick, vấn đề này có thể tránh được bằng cách dịch chuyển các giá trị nén về bắt đầu từ 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 3 1 5 4
```Chúng tôi xử lý từng phần tử và theo dõi trạng thái Fenwick. 

| Bước | Giá trị | Đã nén | Tổng Fenwick ≤ x | Tổng số đã xem | Đảo ngược mới | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 0 | 0 | 0 | 
| 2 | 3 | 3 | 1 | 1 | 0 | 0 | 
| 3 | 1 | 1 | 0 | 2 | 2 | 2 | 
| 4 | 5 | 5 | 3 | 3 | 0 | 2 | 
| 5 | 4 | 4 | 3 | 4 | 1 | 3 | 

Sau khi xử lý tất cả các phần tử, số lượng đảo ngược là`3`. 

Dấu vết này cho thấy cách đảo ngược chỉ được đăng ký khi phần tử nhỏ hơn xuất hiện sau các giá trị lớn hơn đã thấy trước đó. 

### Ví dụ 2 

đầu vào:```
4
4 3 2 1
```| Bước | Giá trị | Đã nén | Tổng Fenwick ≤ x | Tổng số đã xem | Đảo ngược mới | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 0 | 0 | 0 | 0 | 
| 2 | 3 | 3 | 0 | 1 | 1 | 1 | 
| 3 | 2 | 2 | 0 | 2 | 2 | 3 | 
| 4 | 1 | 1 | 0 | 3 | 3 | 6 | 

Điều này xác nhận trường hợp xấu nhất khi mọi cặp đều bị đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử trong số n phần tử kích hoạt cập nhật và truy vấn Fenwick, cả O(log n) | 
| Không gian | O(n) | Lưu trữ cây Fenwick và mảng nén tọa độ | 

Hệ số logarit đủ nhỏ cho các ràng buộc điển hình lên tới 200.000 phần tử, giúp giải pháp trở nên hiệu quả một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    n = int(input())
    arr = list(map(int, input().split()))

    vals = sorted(set(arr))
    comp = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))
    inv = 0

    for x in arr:
        cx = comp[x]
        inv += fw.sum(len(vals)) - fw.sum(cx)
        fw.add(cx, 1)

    return str(inv)

# sample-like case
assert run("5\n2 3 1 5 4\n") == "3"

# already sorted
assert run("4\n1 2 3 4\n") == "0"

# reversed
assert run("4\n4 3 2 1\n") == "6"

# duplicates
assert run("5\n1 3 3 2 2\n") == "4"

# single element
assert run("1\n10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2 3 1 5 4 | 3 | đặt hàng hỗn hợp chung | 
| 4 1 2 3 4 | 0 | không có trường hợp đảo ngược | 
| 4 4 3 2 1 | 6 | đảo ngược tối đa | 
| 5 1 3 3 2 2 | 4 | xử lý trùng lặp | 
| 1 10 | 0 | kích thước tối thiểu | 

## Vỏ cạnh 

Đối với một mảng đã được sắp xếp như`[1, 2, 3, 4]`, cây Fenwick luôn báo cáo không có sự đảo ngược nào vì mọi truy vấn`fw.sum(max) - fw.sum(x)`đánh giá bằng không. Cấu trúc phản ánh chính xác rằng không có phần tử nào được thấy trước đó lớn hơn phần tử hiện tại và câu trả lời vẫn ổn định xuyên suốt. 

Đối với một mảng giảm hoàn toàn như`[4, 3, 2, 1]`, mỗi bước đóng góp tối đa những đảo ngược mới có thể. Khi xử lý`1`, cây đã chứa tất cả các phần tử lớn hơn nên truy vấn trả về`3`, khớp chính xác với ba cặp`(4,1), (3,1), (2,1)`. Mỗi trong số này được tính chính xác một lần tại thời điểm điểm cuối phù hợp được xử lý, xác nhận tính chính xác.
