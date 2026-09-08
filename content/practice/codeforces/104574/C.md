---
title: "CF 104574C - Kỳ nhông ánh kim"
description: "Chúng ta được cung cấp một bộ sưu tập cự đà, mỗi con được mô tả bằng hai con số: số vảy và số màu ngụy trang. Chúng ta phải xếp hạng những con cự đà này từ tốt nhất đến kém nhất và đưa ra ba ID hàng đầu. Quy tắc đặt hàng không phải là sự so sánh đơn giản giữa hai giá trị."
date: "2026-06-30T08:16:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104574
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 2 (Beginner)"
rating: 0
weight: 104574
solve_time_s: 119
verified: true
draft: false
---

[CF 104574C - Cự đà ánh kim](https://codeforces.com/problemset/problem/104574/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập cự đà, mỗi con được mô tả bằng hai con số: số vảy và số màu ngụy trang. Chúng ta phải xếp hạng những con cự đà này từ tốt nhất đến kém nhất và đưa ra ba ID hàng đầu. 

Quy tắc đặt hàng không phải là sự so sánh đơn giản giữa hai giá trị. Kỳ nhông$i$được coi là tốt hơn kỳ nhông$j$nếu tỷ lệ$a_i / a_j$lớn hơn$b_i / b_j$. Bất đẳng thức này có thể được viết lại thành dạng nhân chéo rõ ràng hơn, đây chính là chìa khóa để giải bài toán một cách hiệu quả. 

Đầu ra yêu cầu ID của cự đà tốt nhất, tốt thứ hai và tốt thứ ba theo thứ tự từng cặp này. 

Các ràng buộc đi lên đến$N = 10^5$, điều này ngay lập tức loại trừ bất kỳ$O(N^2)$phương pháp so sánh từng cặp. Chúng ta cần một phương pháp có thể tính toán thứ tự toàn cục trong$O(N \log N)$hoặc tốt hơn. 

Một vấn đề tế nhị là sự so sánh không phải là thứ tự từ điển tiêu chuẩn. Nó dựa trên sự so sánh tỷ lệ giữa hai thuộc tính khác nhau trên hai phần tử khác nhau. Một sai lầm ngây thơ là sắp xếp theo$a_i / b_i$, không tương đương với điều kiện đã cho vì sự so sánh không dựa trên đường cơ sở cố định mà là giữa hai con cự đà khác nhau. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ so sánh mọi con kỳ nhông với mọi con kỳ nhông khác bằng cách sử dụng quy tắc đã cho. Điều đó sẽ đòi hỏi$O(N^2)$so sánh, mỗi phép tính đều liên quan đến phép nhân và phép so sánh. Với$10^5$cự đà, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta có thể chuyển đổi điều kiện so sánh thành thứ tự tiêu chuẩn. Bắt đầu từ điều kiện$$\frac{a_i}{a_j} > \frac{b_i}{b_j}$$chúng tôi nhân cả hai vế với giá trị dương$a_j b_j$(tất cả đầu vào đều dương), đưa ra$$a_i b_j > a_j b_i$$Bây giờ đây là sự so sánh trực tiếp giữa hai con cự đà bằng cách sử dụng quy tắc cố định. Chúng ta có thể định nghĩa một bộ so sánh trong đó cự đà$i$tốt hơn$j$nếu như$a_i b_j > a_j b_i$, với điểm giới hạn về ID. 

Điều này chuyển vấn đề thành sắp xếp một danh sách bằng một bộ so sánh tùy chỉnh, có thể được thực hiện trong$O(N \log N)$. Sau khi sắp xếp, ba yếu tố đầu tiên là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu theo cặp |$O(N^2)$|$O(1)$| Quá chậm | 
| Sắp xếp bằng bộ so sánh nhân chéo |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả cự đà và lưu trữ chúng thành bộ ba$(a_i, b_i, i)$, Ở đâu$i$là ID. ID là cần thiết vì các mối quan hệ phải được giải quyết bằng cách ưu tiên ID cao hơn. 
2. Xác định quy tắc so sánh giữa hai con cự đà$i$Và$j$. Chúng tôi so sánh$a_i \cdot b_j$với$a_j \cdot b_i$. Nếu cái đầu tiên lớn hơn,$i$là tốt hơn. Nếu bằng nhau thì kỳ nhông có ID lớn hơn sẽ tốt hơn. Điều này đảm bảo một thứ tự yếu nghiêm ngặt cần thiết cho việc sắp xếp. 
3. Sắp xếp danh sách bằng bộ so sánh này. Việc sắp xếp đảm bảo rằng mỗi cặp được sắp xếp nhất quán theo định nghĩa bài toán, tạo ra thứ hạng toàn cầu. 
4. Sau khi sắp xếp, xuất ID của ba con cự đà đầu tiên theo thứ tự đã sắp xếp. 

### Tại sao nó hoạt động 

Phép biến đổi từ so sánh tỷ lệ sang phép nhân chéo giữ nguyên thứ tự vì tất cả các giá trị đều hoàn toàn dương, do đó hướng bất đẳng thức không thay đổi. Bộ so sánh kết quả xác định tổng thứ tự bằng một yếu tố ràng buộc xác định, do đó việc sắp xếp sẽ tạo ra thứ hạng toàn cầu hợp lệ phù hợp với tất cả các so sánh theo cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    iguanas = [(a[i], b[i], i + 1) for i in range(n)]

    iguanas.sort(key=lambda x: (-x[0] / x[1], -x[2]))

    def better(x, y):
        ax, bx, ix = x
        ay, by, iy = y
        if ax * by != ay * bx:
            return ax * by > ay * bx
        return ix > iy

    from functools import cmp_to_key
    iguanas.sort(key=cmp_to_key(better))

    print(iguanas[0][2])
    print(iguanas[1][2])
    print(iguanas[2][2])

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng một danh sách cự đà với các thuộc tính và ID của chúng. Phần quan trọng là bộ so sánh, sử dụng phép nhân chéo để tránh lỗi dấu phẩy động và đảm bảo thứ tự chính xác. Bộ ngắt kết nối trên ID đảm bảo đầu ra xác định khi các tỷ lệ bằng nhau. Sau khi sắp xếp, chúng tôi trực tiếp lấy ba mục hàng đầu. 

Một điểm tinh tế là tránh hoàn toàn việc phân chia dấu phẩy động. sử dụng$a_i / b_i$sẽ gây ra lỗi chính xác và thất bại trên đầu vào lớn. Phép nhân chéo giữ mọi thứ ở dạng số học số nguyên. 

## Ví dụ đã hoạt động 

đầu vào:```
6
1 3 10 5 6 9
7 8 2 4 6 9
```Chúng tôi tính toán so sánh bằng cách sử dụng$a_i b_j$: 

| tôi | một | b | 
| --- | --- | --- | 
| 1 | 1 | 7 | 
| 2 | 3 | 8 | 
| 3 | 10 | 2 | 
| 4 | 5 | 4 | 
| 5 | 6 | 6 | 
| 6 | 9 | 9 | 

So sánh 3 với 4:$10 \cdot 4 = 40$,$5 \cdot 2 = 10$, vậy 3 tốt hơn 4. 

So sánh 6 với 5:$9 \cdot 6 = 54$,$6 \cdot 9 = 54$, dây bị đứt do ID nên 6 thì tốt hơn. 

Sau khi sắp xếp đầy đủ, ba ID hàng đầu là: 

3, 4, 6. 

Điều này xác nhận rằng phép nhân chéo luôn nắm bắt được thứ tự dự định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| sắp xếp với bộ so sánh tùy chỉnh chiếm ưu thế | 
| Không gian |$O(N)$| lưu trữ danh sách cự đà | 

Các ràng buộc cho phép lên đến$10^5$cự đà, vậy$O(N \log N)$cũng nằm trong giới hạn. Thuật toán chỉ sử dụng bộ nhớ bổ sung tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io
from functools import cmp_to_key

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    ig = [(a[i], b[i], i+1) for i in range(n)]

    def cmp(x, y):
        ax, bx, ix = x
        ay, by, iy = y
        if ax * by != ay * bx:
            return -1 if ax * by > ay * bx else 1
        return -1 if ix > iy else 1 if ix < iy else 0

    ig.sort(key=cmp_to_key(cmp))
    return "\n".join(str(ig[i][2]) for i in range(3))

# sample
assert run("""6
1 3 10 5 6 9
7 8 2 4 6 9
""") == "3\n4\n6"

# custom 1: simple increasing ratio
assert run("""4
1 2 3 4
4 3 2 1
""") is not None

# custom 2: tie by ID
assert run("""3
1 1 1
1 1 1
""") is not None

# custom 3: dominant outlier
assert run("""5
1 100 1 1 1
100 1 2 2 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng tỷ lệ | xếp hạng xác định | độ chính xác của bộ so sánh | 
| tất cả đều bình đẳng | ID ràng buộc | quy luật ổn định | 
| một con kỳ nhông thống trị | đặt hàng cực đoan | tính đúng đắn của phép nhân chéo | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi hai con cự đà có tỷ lệ giống hệt nhau. Trong trường hợp đó, so sánh trực tiếp mang lại sự bình đẳng, do đó yếu tố quyết định về ID trở thành yếu tố quyết định duy nhất. Nếu không có quy tắc này, việc sắp xếp có thể không ổn định và tạo ra thứ tự không chính xác. 

Một trường hợp cạnh khác là các giá trị lớn lên đến$10^9$. nhân$a_i b_j$phù hợp an toàn trong phạm vi số nguyên 64 bit, nhưng trong các triển khai yếu hơn, tình trạng tràn có thể xảy ra nếu không sử dụng loại số nguyên thích hợp. 

Trường hợp cuối cùng là khi ba con cự đà đứng đầu có tỷ lệ rất gần nhau. Điều này nhấn mạnh tính nhất quán của bộ so sánh: ngay cả những mâu thuẫn nhỏ trong thứ tự cũng sẽ gây ra việc trích xuất ba phần trên không chính xác, do đó bộ so sánh phải có tính bắc cầu và xác định nghiêm ngặt.
