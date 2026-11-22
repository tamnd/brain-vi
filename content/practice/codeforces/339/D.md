---
title: "CF 339D - Hoạt động Xenia và Bit"
description: "Chúng ta bắt đầu với một mảng có độ dài chính xác là $2^n$. Giá trị cuối cùng được tạo ra bằng cách kết hợp nhiều lần các phần tử liền kề. Cấp độ đầu tiên sử dụng OR theo bit, cấp độ tiếp theo sử dụng XOR theo bit, sau đó lại sử dụng OR, rồi lại XOR, xen kẽ cho đến khi chỉ còn lại một số."
date: "2026-06-06T17:03:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 339
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 197 (Div. 2)"
rating: 1700
weight: 339
solve_time_s: 135
verified: true
draft: false
---

[CF 339D - Hoạt động Xenia và Bit](https://codeforces.com/problemset/problem/339/D) 

**Đánh giá:** 1700 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng có độ dài chính xác$2^n$. 

Giá trị cuối cùng được tạo ra bằng cách kết hợp nhiều lần các phần tử liền kề. Cấp độ đầu tiên sử dụng OR theo bit, cấp độ tiếp theo sử dụng XOR theo bit, sau đó lại sử dụng OR, rồi lại XOR, xen kẽ cho đến khi chỉ còn lại một số. Số còn lại đó là câu trả lời cho mảng hiện tại. 

Sau khi mảng ban đầu được đưa ra, chúng tôi nhận được thông tin cập nhật. Mỗi bản cập nhật thay đổi một vị trí mảng và sau mỗi thay đổi, chúng ta phải xuất giá trị cuối cùng mới. 

Khó khăn chính là mảng có thể chứa tới$2^{17} = 131072$các phần tử và có thể có nhiều như$10^5$cập nhật. Việc tính toán lại toàn bộ quá trình rút gọn sau mỗi lần cập nhật sẽ yêu cầu chạm vào toàn bộ mảng mỗi lần. Một lần tính toán lại đầy đủ chi phí$O(2^n)$, trở thành đại khái$131072$hoạt động cho mỗi truy vấn. Với$10^5$truy vấn, con số đó tăng lên nhiều hơn$10^{10}$hoạt động, vượt xa giới hạn 2 giây cho phép. 

Cấu trúc giảm được cố định. Mỗi cặp phần tử được kết hợp, sau đó mỗi cặp kết quả đó được kết hợp, v.v. Điều duy nhất thay đổi là giá trị của một chiếc lá. Điều này gợi ý rõ ràng về một cấu trúc cây trong đó mỗi nút bên trong lưu trữ kết quả của các nút con của nó. 

Có một số trường hợp dễ bỏ sót. 

Khi$n = 1$, cây chỉ có một cấp độ bên trong. Câu trả lời đơn giản là:```
a1 OR a2
```Giải pháp luôn khởi động root bằng XOR sẽ không thành công. 

Ví dụ:```
1 1
1 2
1 3
```Ban đầu:```
1 OR 2 = 3
```Sau khi cập nhật:```
3 OR 2 = 3
```Đầu ra:```
3
```Một lỗi phổ biến khác là gán thao tác sai cho từng cấp độ. 

Ví dụ:```
2 0
1 2 3 4
```Cấp độ 1:```
1 OR 2 = 3
3 OR 4 = 7
```Cấp độ 2:```
3 XOR 7 = 4
```Trả lời:```
4
```Nếu mức OR và XOR được hoán đổi, kết quả sẽ khác. 

Trường hợp tinh vi thứ ba xuất hiện sau khi cập nhật. Chỉ các nút trên đường đi từ lá đã sửa đổi đến gốc mới thay đổi. Việc xây dựng lại các phần không liên quan của cây sẽ lãng phí thời gian và làm mất đi mục đích của cấu trúc dữ liệu. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng chính xác những gì câu lệnh mô tả. 

Đối với mỗi truy vấn, hãy cập nhật phần tử mảng, liên tục xây dựng cấp độ rút gọn tiếp theo, thay thế các phép toán OR và XOR, rồi tiếp tục cho đến khi còn lại một giá trị. Điều này đúng vì nó tuân theo định nghĩa của câu trả lời theo đúng nghĩa đen. 

Vấn đề là chi phí. Mảng chứa$2^n$các phần tử. Một lần giảm hoàn toàn sẽ chạm vào mọi phần tử với số lần không đổi, do đó mỗi truy vấn sẽ tốn$O(2^n)$. Trong trường hợp xấu nhất đó là:$$10^5 \times 131072$$hoạt động hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là quá trình rút gọn đã tạo thành một cây nhị phân hoàn hảo. 

Mỗi lá tương ứng với một phần tử mảng. Mỗi nút bên trong lưu trữ kết quả của việc kết hợp hai nút con của nó. Hoạt động chỉ phụ thuộc vào chiều cao của nút tính từ các lá. Các nút trực tiếp phía trên các lá sử dụng OR, cấp độ tiếp theo sử dụng XOR, sau đó lại sử dụng OR, v.v. 

Khi một phần tử mảng thay đổi, chỉ các nút trên đường dẫn gốc của nó mới có thể bị ảnh hưởng. Mọi cây con khác không thay đổi vì lá của nó không thay đổi. 

Đây chính xác là mẫu cập nhật được xử lý bởi cây phân đoạn. Chúng tôi lưu trữ giá trị giảm ở mọi nút. Cập nhật điểm sửa đổi một lá và chỉ tính toán lại$O(\log N)$tổ tiên. Phần gốc luôn chứa câu trả lời cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m·2^n) | O(2^n) | Quá chậm | 
| Cây phân đoạn tối ưu | O((2^n) + m log(2^n)) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn có các lá chứa các giá trị mảng ban đầu. 
2. Đối với mỗi nút nội bộ, hãy xác định thao tác nào sẽ được áp dụng ở cấp độ đó. 
3. Các nút trực tiếp phía trên các lá sử dụng bitwise OR vì bước rút gọn đầu tiên trong bài toán sử dụng OR. 
4. Cấp độ tiếp theo sử dụng XOR, sau đó cấp độ tiếp theo sử dụng OR, xen kẽ đến tận gốc. 
5. Trong quá trình xây dựng, xây dựng đệ quy các giá trị con trái và phải. 
6. Kết hợp các phần tử con sử dụng thao tác được gán cho cấp độ hiện tại và lưu kết quả vào nút. 
7. Để cập nhật$(p, b)$, di chuyển đến lá tương ứng và thay thế giá trị của nó bằng$b$. 
8. Trong khi trở về từ đệ quy, hãy tính toán lại mọi tổ tiên bằng cách sử dụng cùng một phép toán phụ thuộc cấp độ được sử dụng trong quá trình xây dựng. 
9. Sau khi cập nhật xong, giá trị gốc bằng với câu trả lời mới. Xuất nó ra. 

### Tại sao nó hoạt động 

Mỗi nút bên trong biểu thị chính xác giá trị sẽ xuất hiện trong quá trình rút gọn trung gian tương ứng của quy trình ban đầu. Cấp độ nội bộ thấp nhất lưu trữ các kết quả OR theo cặp, khớp với lần lặp đầu tiên. Cấp độ tiếp theo lưu trữ kết quả XOR của các giá trị đó, khớp với lần lặp thứ hai, v.v. 

Điều bất biến là mỗi nút luôn lưu trữ giá trị rút gọn chính xác cho phân đoạn của nó theo quy tắc OR/XOR xen kẽ. Việc xây dựng thiết lập tính bất biến cho tất cả các nút. Cập nhật điểm chỉ thay đổi một lá, do đó chỉ các nút có phân đoạn chứa lá đó mới có thể trở nên không hợp lệ. Việc tính toán lại đường dẫn tới thư mục gốc sẽ khôi phục tính bất biến. Vì gốc đại diện cho toàn bộ mảng nên giá trị của nó luôn là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    size = 1 << n
    tree = [0] * (4 * size)

    def build(node, left, right, use_or):
        if left == right:
            tree[node] = arr[left]
            return

        mid = (left + right) // 2

        build(node * 2, left, mid, not use_or)
        build(node * 2 + 1, mid + 1, right, not use_or)

        if use_or:
            tree[node] = tree[node * 2] | tree[node * 2 + 1]
        else:
            tree[node] = tree[node * 2] ^ tree[node * 2 + 1]

    def update(node, left, right, idx, value, use_or):
        if left == right:
            tree[node] = value
            return

        mid = (left + right) // 2

        if idx <= mid:
            update(node * 2, left, mid, idx, value, not use_or)
        else:
            update(node * 2 + 1, mid + 1, right, idx, value, not use_or)

        if use_or:
            tree[node] = tree[node * 2] | tree[node * 2 + 1]
        else:
            tree[node] = tree[node * 2] ^ tree[node * 2 + 1]

    build(1, 0, size - 1, n % 2 == 1)

    answers = []

    for _ in range(m):
        p, b = map(int, input().split())
        update(1, 0, size - 1, p - 1, b, n % 2 == 1)
        answers.append(str(tree[1]))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    main()
```Cây lưu trữ chính xác các giá trị rút gọn được mô tả bởi bài toán. tham số`use_or`cho một nút biết hoạt động nào thuộc về cấp độ của nó. 

Hoạt động của rễ phụ thuộc vào chiều cao của cây. Nếu như`n`là số lẻ, gốc sử dụng OR. Nếu như`n`chẵn, gốc sử dụng XOR. Điều này diễn ra trực tiếp từ các hoạt động xen kẽ bắt đầu bằng OR ở cấp độ dưới cùng. 

Trong quá trình xây dựng, mỗi lệnh gọi đệ quy sẽ chuyển hoạt động sang cấp độ tiếp theo bằng cách sử dụng`not use_or`. Vào thời điểm đệ quy đạt đến mức phía trên các lá, phép toán luôn là OR. 

Các bản cập nhật tuân theo logic tương tự. Chỉ có một đường dẫn từ gốc đến lá được truy cập, tạo ra độ phức tạp logarit. Sau khi sửa đổi lá, mỗi tổ tiên được tính toán lại bằng cách sử dụng chính xác thao tác tương tự mà nó đã sử dụng trong quá trình xây dựng. 

Không tồn tại mối lo ngại tràn vì tất cả các thao tác đều theo bit OR và XOR trên các giá trị nhỏ hơn$2^{30}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
1 6 3 5
1 4
3 4
1 2
1 2
```Cây ban đầu: 

| Phân đoạn | Giá trị | 
| --- | --- | 
| [1] | 1 | 
| [2] | 6 | 
| [3] | 3 | 
| [4] | 5 | 
| [1,2] = HOẶC | 7 | 
| [3,4] = HOẶC | 7 | 
| [1,4] = XOR | 0 | 

Sau lần cập nhật đầu tiên`(1,4)`: 

| Phân đoạn | Giá trị | 
| --- | --- | 
| [1] | 4 | 
| [2] | 6 | 
| [1,2] | 6 | 
| [3,4] | 7 | 
| Gốc | 1 | 

Đầu ra:```
1
```Tiếp tục tương tự tạo ra:```
1
3
3
3
```Dấu vết này cho thấy chỉ các nút trên đường dẫn của lá được cập nhật mới thay đổi. Phần lớn của cây vẫn còn nguyên. 

### Ví dụ 2 

đầu vào:```
1 2
1 2
2 0
1 7
```Cây ban đầu: 

| Cấp độ | Giá trị | 
| --- | --- | 
| Lá | 1, 2 | 
| Gốc (HOẶC) | 3 | 

Sau khi cập nhật`(2,0)`: 

| Cấp độ | Giá trị | 
| --- | --- | 
| Lá | 1, 0 | 
| Gốc | 1 | 

Sau khi cập nhật`(1,7)`: 

| Cấp độ | Giá trị | 
| --- | --- | 
| Lá | 7, 0 | 
| Gốc | 7 | 

Đầu ra:```
1
7
```Ví dụ này thực hiện cây nhỏ nhất có thể và xác nhận rằng thao tác duy nhất được sử dụng là OR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n + m·n) | Xây dựng một lần, mỗi bản cập nhật chạm vào một đường dẫn từ gốc đến lá | 
| Không gian | O(2^n) | Lưu trữ cây phân đoạn | 

Từ$n \le 17$, mỗi bản cập nhật thực hiện tối đa 17 cấp độ đệ quy. Ngay cả với$10^5$cập nhật, tổng công việc chỉ có khoảng 1,7 triệu bản cập nhật nút, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline

        n, m = map(int, input().split())
        arr = list(map(int, input().split()))

        size = 1 << n
        tree = [0] * (4 * size)

        def build(node, l, r, use_or):
            if l == r:
                tree[node] = arr[l]
                return

            mid = (l + r) // 2
            build(node * 2, l, mid, not use_or)
            build(node * 2 + 1, mid + 1, r, not use_or)

            if use_or:
                tree[node] = tree[node * 2] | tree[node * 2 + 1]
            else:
                tree[node] = tree[node * 2] ^ tree[node * 2 + 1]

        def update(node, l, r, idx, val, use_or):
            if l == r:
                tree[node] = val
                return

            mid = (l + r) // 2

            if idx <= mid:
                update(node * 2, l, mid, idx, val, not use_or)
            else:
                update(node * 2 + 1, mid + 1, r, idx, val, not use_or)

            if use_or:
                tree[node] = tree[node * 2] | tree[node * 2 + 1]
            else:
                tree[node] = tree[node * 2] ^ tree[node * 2 + 1]

        build(1, 0, size - 1, n % 2 == 1)

        out = []
        for _ in range(m):
            p, b = map(int, input().split())
            update(1, 0, size - 1, p - 1, b, n % 2 == 1)
            out.append(str(tree[1]))

        return "\n".join(out) + ("\n" if out else "")

    return solve()

# provided sample
assert run(
"""2 4
1 6 3 5
1 4
3 4
1 2
1 2
"""
) == """1
3
3
3
"""

# minimum size
assert run(
"""1 1
1 2
1 3
"""
) == """3
"""

# all zeros
assert run(
"""2 1
0 0 0 0
2 5
"""
) == """5
"""

# repeated update of same position
assert run(
"""1 3
7 1
1 7
1 0
1 7
"""
) == """7
1
7
"""

# off-by-one check on last index
assert run(
"""2 1
1 2 4 8
4 0
"""
) == """7
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`với một bản cập nhật |`3`| Cây hợp lệ nhỏ nhất | 
| Tất cả số không thì cập nhật một lá |`5`| Tuyên truyền đúng hướng lên | 
| Cập nhật lặp đi lặp lại trên cùng một chỉ mục |`7,1,7`| Không còn giá trị cũ | 
| Cập nhật vị trí cuối cùng |`7`| Lập chỉ mục ranh giới bên phải | 

## Vỏ cạnh 

Xét cây nhỏ nhất:```
1 1
1 2
1 3
```Chỉ có một nút nội bộ. Cây tính toán:```
3 OR 2 = 3
```Thuật toán xây dựng một gốc với`use_or=True`bởi vì`n`thật kỳ quặc. Câu trả lời được báo cáo chính xác là`3`. 

Hãy xem xét trường hợp hoạt động gốc là XOR:```
2 0
1 2 3 4
```Các cấp độ của cây là:```
1 OR 2 = 3
3 OR 4 = 7
3 XOR 7 = 4
```Thuật toán bắt đầu gốc với`use_or=False`bởi vì`n`là chẵn. Luân phiên đệ quy tạo ra mức OR thấp hơn, khớp chính xác với trình tự được yêu cầu. 

Xem xét cập nhật vị trí mảng cuối cùng:```
2 1
1 2 4 8
4 0
```Trước khi cập nhật:```
(1|2)=3
(4|8)=12
3^12=15
```Sau khi cập nhật:```
(1|2)=3
(4|0)=4
3^4=7
```Chỉ các nút bao phủ lá thứ tư và tổ tiên của chúng được tính toán lại. Thuật toán cập nhật chính xác đường dẫn và kết quả đầu ra đó`7`, xác nhận việc xử lý chính xác các chỉ số biên.
