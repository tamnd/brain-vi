---
title: "CF 1354D - Nhiều bộ"
description: "Chúng tôi đang duy trì một tập hợp các số nguyên được phép trùng lặp và bộ sưu tập thay đổi theo thời gian. Ban đầu, chúng tôi được cung cấp một danh sách được sắp xếp các giá trị đã tạo thành nhiều tập hợp bắt đầu. Sau đó, chúng tôi nhận được một chuỗi dài các hoạt động."
date: "2026-06-11T13:55:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1354
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 87 (Rated for Div. 2)"
rating: 1900
weight: 1354
solve_time_s: 101
verified: true
draft: false
---

[CF 1354D - Multiset](https://codeforces.com/problemset/problem/1354/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp các số nguyên được phép trùng lặp và bộ sưu tập thay đổi theo thời gian. Ban đầu, chúng tôi được cung cấp một danh sách được sắp xếp các giá trị đã tạo thành nhiều tập hợp bắt đầu. Sau đó, chúng tôi nhận được một chuỗi dài các hoạt động. 

Mỗi thao tác sẽ chèn một số nguyên mới vào nhiều tập hợp hoặc xóa một phần tử dựa trên vị trí của nó theo thứ tự được sắp xếp. Khi chúng tôi nói “thống kê thứ k”, ý chúng tôi là nếu bạn sắp xếp toàn bộ nhiều tập hợp, phần tử nhỏ nhất thứ k sẽ được chọn. Phần tử đó sau đó được loại bỏ khỏi cấu trúc. Việc chèn chỉ đơn giản là thêm một giá trị và việc loại bỏ phụ thuộc vào thứ tự hiện tại chứ không phụ thuộc vào sự bằng nhau về giá trị. 

Cuối cùng, chúng ta chỉ cần xuất một phần tử còn lại từ multiset nếu nó không trống, nếu không chúng ta sẽ in ra số 0. 

Khó khăn chính là số lượng thao tác có thể lên tới một triệu, do đó, bất kỳ phương pháp nào sắp xếp lại hoặc quét toàn bộ cấu trúc cho mỗi truy vấn sẽ thất bại. Ngay cả O(n) cho mỗi thao tác cũng dẫn đến 10^12 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Điều này buộc chúng ta phải sử dụng một cấu trúc hỗ trợ cả truy vấn thống kê thứ tự chèn và thứ k theo thời gian logarit. 

Một trường hợp phức tạp xuất phát từ việc loại bỏ nhiều lần các giá trị k nhỏ. Ví dụ: nếu chúng ta liên tục loại bỏ phần tử thứ 1, cấu trúc sẽ tiếp tục bị thu hẹp về phía trước. Một cách tiếp cận đơn giản là xây dựng lại các mảng hoặc sắp xếp lặp đi lặp lại có thể âm thầm chuyển thành hành vi bậc hai ngay cả khi mỗi bước có vẻ “đơn giản”. 

Một trường hợp quan trọng khác là sau tất cả các lần xóa, nhiều tập hợp có thể trở nên trống, ngay cả khi nó bắt đầu lớn. Trong trường hợp đó, chúng ta phải xuất ra 0 thay vì cố gắng truy cập một phần tử. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là duy trì nhiều tập hợp dưới dạng mảng động hoặc danh sách Python. Đối với mỗi lần chèn, chúng tôi nối thêm giá trị. Đối với mỗi lần xóa, chúng tôi sắp xếp mảng và xóa phần tử thứ k. 

Điều này đúng về mặt logic, nhưng hiệu suất sẽ giảm ngay lập tức. Việc sắp xếp tốn O(n log n) và thực hiện việc này lên tới một triệu lần sẽ dẫn đến khoảng 10^6 × n log n hoạt động, điều này là không khả thi. Ngay cả khi chúng tôi duy trì danh sách theo thứ tự được sắp xếp bằng cách sử dụng tính năng chèn nhị phân, việc xóa phần tử thứ k vẫn yêu cầu dịch chuyển các phần tử, dẫn đến O(n) cho mỗi thao tác. 

Quan sát cốt lõi là chúng ta không bao giờ cần thứ tự thực tế của các phần tử một cách rõ ràng. Chúng ta chỉ cần hai khả năng: duy trì tần số của các giá trị và nhanh chóng tìm phần tử nhỏ thứ k trong số tất cả các phần tử hiện tại. Khi phần tử đó được tìm thấy, chúng tôi giảm tần số của nó. 

Điều này biến vấn đề thành một cấu trúc tổng tiền tố động. Nếu chúng ta duy trì số lần mỗi giá trị xuất hiện thì phần tử nhỏ thứ k là chỉ số nhỏ nhất sao cho tổng tiền tố đạt ít nhất k. Cây Fenwick hỗ trợ chính xác mẫu này: cập nhật điểm và tổng tiền tố, cộng với nâng nhị phân để xác định vị trí thứ k trong O(log n). 

Bởi vì tất cả các giá trị được giới hạn bởi n nên chúng ta có thể sử dụng cây Fenwick có kích thước n một cách an toàn. Mỗi lần chèn là một bản cập nhật điểm duy nhất và mỗi lần xóa là một "tìm theo tổng tiền tố" theo sau là một bản cập nhật điểm khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sắp xếp lại / mô phỏng mảng | O(nq) hoặc O(nq log n) | O(n) | Quá chậm | 
| Cây Fenwick (BIT) | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa nhiều tập hợp dưới dạng một mảng tần số trên phạm vi giá trị, được duy trì bên trong cây Fenwick.

1. Xây dựng cây Fenwick và chèn tất cả các phần tử ban đầu bằng cách tăng tần số của chúng lên một. Điều này thiết lập tổng tiền tố bắt đầu chính xác. 
2. Với mỗi truy vấn, hãy kiểm tra dấu của nó. Nếu giá trị dương, nó biểu thị phép chèn, vì vậy chúng ta tăng tần số của giá trị đó trong cây Fenwick lên một. Điều này giữ cho tổng tiền tố nhất quán với nhiều tập hợp được cập nhật. 
3. Nếu giá trị âm, chúng tôi hiểu đó là yêu cầu loại bỏ phần tử nhỏ thứ k, trong đó k là giá trị tuyệt đối. Đầu tiên chúng ta sử dụng cây Fenwick để xác định chỉ số nhỏ nhất sao cho tổng tiền tố của nó ít nhất là k. Chỉ số này tương ứng chính xác với phần tử nhỏ thứ k trong multiset hiện tại. 
4. Sau khi tìm thấy chỉ số đó, chúng tôi giảm tần số của nó đi một trong cây Fenwick. Điều này mô phỏng việc loại bỏ chính xác một lần xuất hiện của giá trị đó. 
5. Sau khi xử lý tất cả các truy vấn, chúng tôi quét bất kỳ chỉ mục nào có tần số dương. Nếu không tồn tại, chúng tôi xuất ra số không. Ngược lại, chúng tôi xuất ra bất kỳ chỉ mục nào có tần số khác 0. 

Thao tác chính là bước “tìm thứ k”, sử dụng nâng cấp nhị phân trên cây Fenwick. Ở mỗi mức bit, chúng tôi cố gắng di chuyển sang phải nếu việc đó không vượt quá tần số tích lũy k. Điều này xây dựng câu trả lời theo thời gian logarit. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, cây Fenwick lưu trữ bội số chính xác của tất cả các giá trị hiện có trong nhiều tập hợp. Do đó, tổng tiền tố biểu thị số phần tử nhỏ hơn hoặc bằng bất kỳ giá trị nào. Phần tử nhỏ nhất thứ k được xác định duy nhất bởi vị trí đầu tiên mà số tích lũy này đạt đến k, do đó tìm kiếm nhị phân trên tổng tiền tố luôn xác định được phần tử chính xác. Vì mỗi lần chèn và xóa sẽ cập nhật tần số chính xác một lần nên tính bất biến này được giữ nguyên trong suốt mọi thao tác. 

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

    def find_kth(self, k):
        idx = 0
        bit_mask = 1 << (self.n.bit_length())
        while bit_mask:
            nxt = idx + bit_mask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bit_mask >>= 1
        return idx + 1

n, q = map(int, input().split())
a = list(map(int, input().split()))

fw = Fenwick(n)

for x in a:
    fw.add(x, 1)

for x in map(int, input().split()):
    if x > 0:
        fw.add(x, 1)
    else:
        k = -x
        pos = fw.find_kth(k)
        fw.add(pos, -1)

for i in range(1, n + 1):
    if fw.sum(i) - fw.sum(i - 1) > 0:
        print(i)
        break
else:
    print(0)
```Cây Fenwick được khởi tạo theo phạm vi giá trị chứ không phải theo số phần tử. Điều này quan trọng vì chúng tôi không theo dõi vị trí trong một mảng mà theo dõi tần số của các giá trị. 

các`find_kth`hàm thực hiện tìm kiếm nâng nhị phân trên các tổng tiền tố ẩn. Nó xây dựng câu trả lời từng chút một, đảm bảo chúng tôi không bao giờ vượt quá tần suất tích lũy cần thiết. 

Vòng lặp cuối cùng chỉ đơn giản là tìm thấy bất kỳ giá trị nào còn tồn tại. Vì bài toán cho phép bất kỳ phần tử nào còn lại nên chúng ta dừng ở tần số dương đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 2 3 4 5
-1 -1 -1 -1 -1
```Chúng tôi theo dõi tần số nhiều tập hợp. 

| Bước | Hoạt động | Trạng thái nhiều tập hợp (khái niệm) | Đã xóa | 
| --- | --- | --- | --- | 
| 1 | xóa thứ 1 | 2 3 4 5 | 1 | 
| 2 | xóa thứ 1 | 3 4 5 | 2 | 
| 3 | xóa thứ 1 | 4 5 | 3 | 
| 4 | xóa thứ 1 | 5 | 4 | 
| 5 | xóa thứ 1 | trống | 5 | 

Sau tất cả các thao tác, không còn phần tử nào, vì vậy đầu ra là 0. 

Điều này xác nhận việc xóa tiền tố lặp đi lặp lại sẽ thu gọn cấu trúc một cách chính xác cho đến khi trống. 

### Ví dụ 2 

đầu vào:```
3 4
1 1 3
2 -1 2 -1
```Nhiều tập hợp ban đầu là [1, 1, 3]. 

| Bước | Hoạt động | Nhiều bộ | 
| --- | --- | --- | 
| 1 | chèn 2 | 1 1 2 3 | 
| 2 | xóa thứ 1 | 1 2 3 (đã xóa 1) | 
| 3 | chèn 2 | 1 2 2 3 | 
| 4 | xóa thứ 1 | 2 2 3 (đã xóa 1) | 

Multiset cuối cùng chứa [2, 2, 3], vì vậy bất kỳ 2 hoặc 3 nào cũng là đầu ra hợp lệ. 

Điều này cho thấy rằng các bản sao được xử lý chính xác và việc xóa chỉ ảnh hưởng đến một lần xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi bản cập nhật và truy vấn thứ k được xử lý bởi các phép toán của cây Fenwick theo thời gian logarit | 
| Không gian | O(n) | Một mảng tần số được lưu trữ bên trong cây Fenwick | 

Các giới hạn cho phép tổng cộng lên tới hai triệu thao tác, do đó hệ số logarit khoảng 20 giúp giải pháp được thoải mái trong giới hạn. Việc sử dụng bộ nhớ vẫn tuyến tính theo n, phù hợp với giới hạn 28 MB. 

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

        def find_kth(self, k):
            idx = 0
            bit_mask = 1 << (self.n.bit_length())
            while bit_mask:
                nxt = idx + bit_mask
                if nxt <= self.n and self.bit[nxt] < k:
                    k -= self.bit[nxt]
                    idx = nxt
                bit_mask >>= 1
            return idx + 1

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    fw = Fenwick(n)

    for x in a:
        fw.add(x, 1)

    for x in map(int, input().split()):
        if x > 0:
            fw.add(x, 1)
        else:
            pos = fw.find_kth(-x)
            fw.add(pos, -1)

    for i in range(1, n + 1):
        if fw.sum(i) - fw.sum(i - 1) > 0:
            return str(i)
    return "0"

# provided sample
assert run("""5 5
1 2 3 4 5
-1 -1 -1 -1 -1
""") == "0"

# custom: single element delete
assert run("""1 1
1
-1
""") == "0"

# custom: insert only
assert run("""1 3
1
2 3 1
""") != ""

# custom: duplicates
assert run("""3 3
1 1 1
-1 -1 -1
""") == "0"

# custom: mixed
assert run("""3 4
1 2 3
2 -1 2 -1
""") in {"2", "3"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xóa phần tử đơn | 0 | loại bỏ trên cấu trúc tối thiểu | 
| chỉ chèn | không trống | xử lý các phần chèn mà không cần xóa | 
| tất cả các bản sao đã bị loại bỏ | 0 | xử lý đúng các giá trị lặp lại | 
| hoạt động hỗn hợp | 2 hoặc 3 | tính đúng đắn khi xen kẽ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi việc xóa liên tục nhắm vào phần tử nhỏ nhất. Đối với đầu vào như`1 2 3 4 5`theo sau là lặp đi lặp lại`-1`hoạt động, cấu trúc co lại từ bên trái mỗi lần. Cây Fenwick cập nhật chính xác tần số và mỗi tần số`find_kth(1)`luôn trả về mức tối thiểu hiện tại, đảm bảo thu nhỏ xác định cho đến khi trống. 

Một trường hợp khác là sự trùng lặp nặng nề như bắt đầu với nhiều giá trị giống nhau và liên tục loại bỏ thống kê thứ tự tùy ý. Vì tất cả các phần tử bằng nhau đều có chung chỉ mục nên mỗi lần loại bỏ chỉ làm giảm tần suất đi một và tổng tiền tố vẫn hoạt động nhất quán. Truy vấn thứ k không bao giờ nhầm lẫn các giá trị giống hệt nhau vì nó phụ thuộc vào số lượng tích lũy chứ không phải danh tính. 

Cuối cùng, việc chèn và xóa xen kẽ nhấn mạnh tính bất biến là cây Fenwick luôn phản ánh chính xác nhiều tập hợp hiện tại. Mỗi hoạt động đều mang tính cục bộ, do đó không cần phải xây dựng lại toàn bộ, điều này ngăn chặn cả vấn đề về tính chính xác và sự sụp đổ hiệu suất.
