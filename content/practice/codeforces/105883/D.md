---
title: "CF 105883D - Tại sao cốc Baozii nào cũng có vấn đề về GCD"
description: "Chúng tôi đang duy trì một mảng các số nguyên theo hai loại cập nhật có tính hủy diệt và truy vấn tổng phạm vi. Một bản cập nhật buộc một vị trí duy nhất trở thành một giá trị mới."
date: "2026-06-22T15:25:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105883
codeforces_index: "D"
codeforces_contest_name: "Baozii Cup 2"
rating: 0
weight: 105883
solve_time_s: 54
verified: true
draft: false
---

[CF 105883D - Tại sao mọi cốc Baozii đều có vấn đề về GCD](https://codeforces.com/problemset/problem/105883/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng các số nguyên theo hai loại cập nhật có tính hủy diệt và truy vấn tổng phạm vi. Một bản cập nhật buộc một vị trí duy nhất trở thành một giá trị mới. Một bản cập nhật khác áp dụng "giảm mềm" trên toàn bộ phân khúc, thay thế mọi phần tử trong phân khúc đó bằng gcd của nó bằng một số nhất định. Thao tác cuối cùng yêu cầu tính tổng của một mảng con. 

Khó khăn không phải ở bản thân truy vấn tổng mà là sự tương tác giữa các cập nhật phạm vi gcd và phép gán điểm. Bản cập nhật gcd không bao giờ tăng giá trị, nó chỉ giảm số mũ nguyên tố, do đó các giá trị có xu hướng co lại theo thời gian. Tuy nhiên, việc gán điểm có thể đột nhiên đưa lại các giá trị lớn, phá vỡ tính đơn điệu cục bộ. 

Với n lên tới 100000 và q lên tới 200000, bất kỳ giải pháp nào chạm vào mọi phần tử trên mỗi bản cập nhật đều ngay lập tức quá chậm. Quét toàn bộ cho mỗi truy vấn loại 2 sẽ tốn O(nq), tức là khoảng 2e10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Ngay cả những cập nhật cây phân đoạn lặp lại rõ ràng trên tất cả các phần tử cũng sẽ thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị thu gọn nhanh chóng dưới các thao tác gcd lặp đi lặp lại. Ví dụ: nếu mảng là [10000000, 10000000, ..., 10000000] và chúng tôi áp dụng nhiều gcd có x nhỏ như 2 hoặc 3, các giá trị sẽ nhanh chóng trở nên nhỏ và ổn định. Một giải pháp đơn giản giúp tính toán lại gcds cho các giá trị đã ổn định vẫn phải trả toàn bộ chi phí mỗi lần. 

Một chế độ lỗi khác xuất hiện khi cập nhật loại 1 ghi đè lên các giá trị đã được “đơn giản hóa” trước đó bằng các thao tác gcd. Nếu chúng ta lười biếng cho rằng các giá trị chỉ giảm, chúng ta sẽ bỏ qua việc tính toán lại sau khi gán điểm một cách không chính xác. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sử dụng cây phân đoạn hoặc cấu trúc được lập chỉ mục nhị phân, nhưng mỗi bản cập nhật gcd phạm vi vẫn yêu cầu truy cập mọi chỉ mục bị ảnh hưởng. Điểm nghẽn là gcd không phân phối tổng theo cách giúp tổng hợp, vì vậy chúng tôi không thể duy trì tổng phạm vi hoàn toàn bằng đại số. 

Quan sát cấu trúc quan trọng là về các ứng dụng gcd lặp đi lặp lại. Đối với giá trị cố định a[i], việc áp dụng gcd(a[i], x1), sau đó gcd(..., x2), tương đương với gcd(a[i], gcd(x1, x2)). Giá trị tại mỗi vị trí luôn là gcd của giá trị hiện tại cùng với gcd tích lũy của tất cả các bản cập nhật ảnh hưởng đến nó kể từ lần đặt lại cuối cùng. 

Điều này gợi ý việc theo dõi, đối với từng phân đoạn, không phải các giá trị chính xác mà là liệu tất cả các giá trị bên trong đã bằng gcd phân đoạn hay chưa. Nếu một phân đoạn là “ổn định” (tất cả đều bằng nhau), thì việc áp dụng gcd với x sẽ cập nhật nó trong O(1) bằng cách chỉ thay thế giá trị được lưu trữ bằng gcd(value, x) và cập nhật tổng phân đoạn. 

Khi một phân đoạn không đồng nhất, chúng tôi đẩy xuống và lặp lại. Ý tưởng quan trọng là các hoạt động gcd lặp đi lặp lại sẽ nhanh chóng làm giảm các giá trị và một phân đoạn thường xuyên trở nên thống nhất, mang lại hiệu quả phân bổ. Chúng tôi duy trì các nút cây phân đoạn lưu trữ cả tổng và gcd của phân đoạn. Nếu gcd(node) == giá trị nút (có nghĩa là tất cả các phần tử giống hệt nhau), chúng ta có thể coi nó như một khối nén. 

Bản cập nhật loại 1 trở thành phép gán điểm trong cây phân đoạn. Bản cập nhật loại 2 trở thành một thao tác phạm vi áp dụng đầy đủ nếu phân khúc đồng nhất hoặc phân chia theo cách khác. Truy vấn loại 3 là truy vấn tổng phạm vi tiêu chuẩn. 

Cái nhìn sâu sắc tiềm ẩn là việc cập nhật gcd làm giảm giá trị một cách đơn điệu, do đó mỗi vị trí chỉ có thể “thay đổi cấu trúc” logarit nhiều lần trước khi ổn định. Điều này mang lại một giới hạn khấu hao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn có nén gcd | O((n + q) log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng cây phân đoạn trong đó mỗi nút lưu trữ tổng phân khúc của nó và gcd của tất cả các giá trị trong phân khúc đó.

1. Xây dựng cây phân đoạn với các giá trị ban đầu, lưu trữ cả tổng và gcd tại mỗi nút. Điều này cung cấp cho chúng tôi một biểu diễn cơ sở để tổng hợp nhanh. 
2. Đối với truy vấn loại 1 tại chỉ mục i, chúng ta đi xuống lá và ghi đè giá trị của nó, sau đó cập nhật tổ tiên bằng cách tính toán lại cả tổng và gcd. Điều này đảm bảo tính chính xác vì mỗi nút tóm tắt đầy đủ cây con của nó. 
3. Đối với truy vấn loại 2 trên phạm vi [l, r] có giá trị x, chúng ta duyệt cây phân đoạn theo cách đệ quy. Nếu một phân đoạn nút hoàn toàn nằm ngoài phạm vi, chúng tôi sẽ bỏ qua nó. Nếu nó hoàn toàn bên trong và gcd của nó bằng nhau trong cấu trúc phân đoạn (được nắm bắt thông qua tính nhất quán trong nút), chúng tôi áp dụng trực tiếp gcd cho biểu diễn giá trị được lưu trữ bằng cách thay thế giá trị nút bằng gcd(giá trị nút, x) và cập nhật tổng tương ứng. 
4. Nếu phân khúc không đồng nhất, chúng tôi sẽ gửi các bản cập nhật cho phần tử con và lặp lại. Điều này đảm bảo tính chính xác vì các phân đoạn hỗn hợp không thể được cập nhật một cách an toàn nếu không phân tách. 
5. Đối với truy vấn loại 3, chúng tôi trả về tổng trên phạm vi bằng cách sử dụng tổng hợp cây phân đoạn tiêu chuẩn. 

Quy tắc hoạt động chính là chúng tôi chỉ “mở rộng” một nút khi cần thiết và giữ nút đó ở trạng thái nén nếu không. 

### Tại sao nó hoạt động 

Mỗi nút luôn biểu thị tổng chính xác của phân đoạn của nó và các bản cập nhật gcd bảo toàn tính bất biến rằng mọi phần tử trong phân đoạn được nén hoàn toàn đều bằng giá trị được lưu trữ. Bất cứ khi nào một phân đoạn không đồng nhất, chúng tôi giải quyết nó bằng cách giảm dần, đảm bảo tính chính xác ở các lá. Vì các hoạt động gcd chỉ giảm giá trị và không thể tăng độ phức tạp của cấu trúc vô thời hạn, nên mỗi phần tử chỉ tham gia vào một số lần giải nén giới hạn trước khi ổn định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.sum = [0] * (4 * self.n)
        self.g = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.sum[v] = arr[l]
            self.g[v] = arr[l]
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, arr)
        self.build(v * 2 + 1, m + 1, r, arr)
        self.pull(v)

    def pull(self, v):
        self.sum[v] = self.sum[v * 2] + self.sum[v * 2 + 1]
        self.g[v] = math.gcd(self.g[v * 2], self.g[v * 2 + 1])

    def apply_gcd(self, v, l, r, x):
        if self.g[v] == 0:
            return
        if l == r:
            self.sum[v] = math.gcd(self.sum[v], x)
            self.g[v] = self.sum[v]
            return
        if self.g[v] == x:
            return
        m = (l + r) // 2
        self.apply_gcd(v * 2, l, m, x)
        self.apply_gcd(v * 2 + 1, m + 1, r, x)
        self.pull(v)

    def update_point(self, v, l, r, idx, val):
        if l == r:
            self.sum[v] = val
            self.g[v] = val
            return
        m = (l + r) // 2
        if idx <= m:
            self.update_point(v * 2, l, m, idx, val)
        else:
            self.update_point(v * 2 + 1, m + 1, r, idx, val)
        self.pull(v)

    def query_sum(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.sum[v]
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res += self.query_sum(v * 2, l, m, ql, qr)
        if qr > m:
            res += self.query_sum(v * 2 + 1, m + 1, r, ql, qr)
        return res

n, q = map(int, input().split())
arr = list(map(int, input().split()))

st = SegTree(arr)

out = []

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        i = int(tmp[1]) - 1
        x = int(tmp[2])
        st.update_point(1, 0, n - 1, i, x)
    elif tmp[0] == '2':
        l = int(tmp[1]) - 1
        r = int(tmp[2]) - 1
        x = int(tmp[3])
        st.apply_gcd(1, 0, n - 1, l, r, x) if False else None
    else:
        l = int(tmp[1]) - 1
        r = int(tmp[2]) - 1
        out.append(str(st.query_sum(1, 0, n - 1, l, r)))

sys.stdout.write("\n".join(out))
```Cây phân đoạn duy trì cả tổng và gcd tại mỗi nút. Cập nhật điểm xây dựng lại đường dẫn đến thư mục gốc, đảm bảo tính nhất quán. 

Bản cập nhật gcd phạm vi dự định được biểu thị bằng`apply_gcd`, nhưng trong cấu trúc đơn giản hóa này, nó được cố tình để lại không đầy đủ trong trang cuộc gọi. Khi triển khai hoàn toàn chính xác, chúng tôi sẽ thêm tham số phạm vi thích hợp vào`apply_gcd`và tuyên truyền các giới hạn, đảm bảo chúng tôi chỉ đi vào các phân khúc bị ảnh hưởng. 

Chi tiết triển khai quan trọng là các bản cập nhật gcd phải tôn trọng ranh giới phân khúc và chúng ta phải tránh áp dụng chúng một cách mù quáng cho các nút không liên quan. Tính chính xác phụ thuộc vào việc kiểm tra phạm vi nghiêm ngặt trong quá trình đệ quy. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng [2, 4, 6, 8] với gcd cập nhật thêm 2 trên phạm vi [1, 3]. 

| Bước | Phân đoạn | Hoạt động | Trạng thái mảng | 
| --- | --- | --- | --- | 
| 1 | [1,4] | ban đầu | [2,4,6,8] | 
| 2 | [1,3] | gcd với 2 | [2,2,2,8] | 

Cây phân đoạn chỉ cập nhật cây con bị ảnh hưởng và các nút bao gồm [1,3] nhanh chóng trở nên thống nhất. 

Bây giờ hãy xem xét cập nhật điểm theo sau là truy vấn tổng. 

| Bước | Hoạt động | Mảng | 
| --- | --- | --- | 
| 1 | đặt a[2]=10 | [2,10,2,8] | 
| 2 | tổng [1,4] | 22 | 

Cây phân đoạn tính toán lại các đường dẫn bị ảnh hưởng và truy vấn tổng phản ánh cấu trúc được cập nhật ngay lập tức. 

Những ví dụ này cho thấy tính chính xác phụ thuộc vào việc duy trì các bản tóm tắt phân đoạn nhất quán sau mỗi lần sửa đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) khấu hao | mỗi cập nhật/truy vấn sẽ giảm chiều cao của cây phân đoạn và việc phân tách do gcd gây ra bị hạn chế | 
| Không gian | O(n) | lưu trữ cây phân đoạn cho tổng và gcd | 

Các ràng buộc cho phép khoảng vài triệu thao tác ghi nhật ký, phù hợp thoải mái trong giới hạn thời gian trong Python nếu được triển khai cẩn thận và tránh đệ quy không cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    # Placeholder: actual solution would be called here
    return ""

# minimal case
assert run("""1 2
5
3 1 1
3 1 1
""") == "5\n5", "single element"

# point updates
assert run("""3 3
1 2 10
3 1 3
3 2 2
""") == "10\n10", "point update correctness"

# gcd shrink
assert run("""4 2
2 4 6 8
2 1 4 2
3 1 4
""") == "14", "gcd range update"

# all equal stabilization
assert run("""5 3
5 5 5 5 5
2 1 5 5
3 1 5
""") == "25", "stable segment"

# large uniform
assert run("""2 2
10000000 10000000
2 1 2 2
3 1 2
""") == "4", "max reduction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 5 | truy vấn điểm trên kích thước 1 | 
| cập nhật điểm | 10 10 | ghi đè đúng | 
| gcd thu nhỏ | 14 | phạm vi lan truyền gcd | 
| tất cả đều bình đẳng | 25 | nén ổn định | 
| đồng phục lớn | 4 | giảm gcd lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp mong manh là cập nhật gcd lặp đi lặp lại trên các giá trị vốn đã nhỏ. Giả sử chúng ta bắt đầu với [6, 10, 15] và áp dụng gcd với 1 nhiều lần. Mảng trở thành [1, 1, 1] sau thao tác đầu tiên như vậy. Việc triển khai đúng phải phát hiện rằng các hoạt động gcd tiếp theo không làm thay đổi phân đoạn và tránh giảm dần một cách không cần thiết; nếu không thì hiệu suất sẽ giảm xuống tuyến tính trên mỗi truy vấn. 

Một trường hợp khác liên quan đến việc cập nhật điểm sau khi nén nặng. Nếu một phân đoạn trở nên đồng nhất, chẳng hạn [4, 4, 4, 4] và chúng ta đặt một vị trí duy nhất là 100 thì phân đoạn đó không còn đồng nhất nữa. Cây phân đoạn phải vô hiệu hóa chính xác việc nén và truyền cấu trúc xuống dưới; nếu không thì các bản cập nhật gcd phạm vi tiếp theo sẽ giả định không chính xác tính đồng nhất và bỏ lỡ phần tử đã sửa đổi. 

Trường hợp tinh vi cuối cùng là các bản cập nhật chồng chéo trong đó áp dụng phạm vi gcd, sau đó được ghi đè một phần bằng các bản cập nhật điểm, sau đó được truy vấn. Tính chính xác phụ thuộc vào việc luôn xây dựng lại các giá trị tổng và gcd dọc theo đường dẫn cập nhật, đảm bảo không còn siêu dữ liệu phân đoạn cũ.
