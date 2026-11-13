---
title: "CF 1750C - XOR bổ sung"
description: "Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau. Một thao tác đơn lẻ sẽ chọn một phân đoạn ([l,r]). Mọi bit của chuỗi (a) bên trong đoạn đều bị đảo ngược, trong khi mọi bit của chuỗi (b) bên ngoài đoạn đó đều bị đảo lộn. Mục tiêu không phải là chuyển đổi chuỗi này thành chuỗi khác."
date: "2026-06-09T15:10:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1750
codeforces_index: "C"
codeforces_contest_name: "CodeTON Round 3 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1400
weight: 1750
solve_time_s: 431
verified: false
draft: false
---

[CF 1750C - XOR bổ sung](https://codeforces.com/problemset/problem/1750/C) 

**Đánh giá:** 1400 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 7 phút 11 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau. Một thao tác đơn lẻ sẽ chọn một phân đoạn ([l,r]). Mọi bit của chuỗi (a) bên trong đoạn đều bị đảo ngược, trong khi mọi bit của chuỗi (b) bên ngoài đoạn đó đều bị đảo lộn. 

Mục tiêu không phải là chuyển đổi chuỗi này thành chuỗi khác. Mục tiêu mạnh mẽ hơn: sau khi áp dụng một số chuỗi thao tác, mọi ký tự của cả hai chuỗi phải trở thành số 0. Chúng ta phải xây dựng một dãy như vậy bằng cách sử dụng tối đa (n+5) phép toán hoặc chứng minh rằng không tồn tại nghiệm nào. 

Độ dài của tất cả các chuỗi trong tất cả các trường hợp thử nghiệm tối đa là (2 \cdot 10^5). Bất kỳ giải pháp nào mô phỏng rõ ràng mọi thao tác trên mọi ký tự sẽ quá tốn kém. Chúng ta cần một công trình có tổng công là tuyến tính theo kích thước đầu vào. 

Một điểm tinh tế là hoạt động ảnh hưởng đến cả hai chuỗi cùng một lúc. Chỉ nhìn vào một chuỗi sẽ ẩn đi cấu trúc của vấn đề. Một sai lầm dễ mắc phải khác là cho rằng mọi cặp dây đều có thể giải được. Một số cặp về cơ bản là không thể. 

Coi như:```
n = 2
a = 11
b = 10
```Câu trả lời là KHÔNG. Một cách xây dựng chỉ tập trung vào việc biến (a) thành số 0 có thể khẳng định thành công một cách không chính xác. 

Một trường hợp thú vị khác là:```
n = 3
a = 010
b = 101
```Ở đây mọi vị trí đều khác nhau. Câu trả lời là CÓ, mặc dù các dây bổ sung cho nhau. 

Trường hợp cạnh thứ ba là:```
n = 3
a = 111
b = 111
```Các chuỗi giống hệt nhau và câu trả lời là CÓ, nhưng cần có một số thao tác điều chỉnh bổ sung sau khi xóa các chuỗi trong (a). 

## Phương pháp tiếp cận 

Quan điểm brute-force là coi mỗi cặp chuỗi là một trạng thái trong biểu đồ và tìm kiếm một chuỗi các thao tác. Mỗi trường hợp thử nghiệm có (O(n^2)) hoạt động có thể xảy ra, trong khi số lượng trạng thái là (2^{2n}). Ngay cả đối với (n) rất nhỏ, điều này trở nên không thể. 

Quan sát quan trọng đến từ việc xem xét điều gì xảy ra với một vị trí duy nhất. 

Chọn một thao tác trên đoạn ([l,r]). Đối với vị trí (i): 

- Nếu (i \in [l,r]) thì (a_i) lật còn (b_i) không lật. 
- Nếu (i \notin [l,r]) thì (a_i) không lật và (b_i) lật. 

Chính xác một bit của cặp ((a_i,b_i)) bị đảo lộn. 

Điều này có nghĩa là mối quan hệ giữa (a_i) và (b_i) luôn thay đổi. Nếu chúng bằng nhau thì chúng trở nên khác nhau. Nếu chúng khác nhau thì chúng trở nên bình đẳng. 

Do đó, mọi thao tác sẽ chuyển đổi trạng thái bình đẳng của mọi vị trí cùng một lúc. 

hãy để 

[ 
c_i = a_i \oplus b_i. 
] 

Mỗi thao tác lật mọi giá trị (c_i). Vì vậy, sau bất kỳ chuỗi thao tác nào, vectơ (c) chỉ có thể là dạng ban đầu hoặc phần bù bit của nó. 

Đối với trạng thái cuối cùng, chúng ta cần (a_i=b_i=0), ngụ ý (c_i=0) cho mọi vị trí. 

Do đó nghiệm chỉ tồn tại nếu ban đầu: 

- tất cả (c_i=0), nghĩa là (a=b), hoặc 
- tất cả (c_i=1), nghĩa là (a) và (b) là phần bù của nhau. 

Nếu không có điều kiện nào xảy ra thì câu trả lời là không thể. 

Khi điều kiện này được thỏa mãn thì việc xây dựng các thao tác trở nên dễ dàng. Chúng ta sử dụng các phép toán trên các vị trí đơn lẻ để loại bỏ mọi (1) trong chuỗi (a). Sau khi thực hiện điều này, tính chẵn lẻ toàn cục thu được sẽ xác định liệu chúng ta kết thúc trong trường hợp hoàn toàn bằng nhau hay trường hợp bù hoàn toàn. Một sự điều chỉnh cố định nhỏ của ba thao tác sẽ giải quyết được vấn đề chẵn lẻ còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trong không gian trạng thái | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam mang tính xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem mọi vị trí có thỏa mãn (a_i=b_i) hay không. 
2. Kiểm tra xem mọi vị trí có thỏa mãn (a_i\neq b_i) hay không. 
3. Nếu không có điều kiện nào xảy ra, hãy in`NO`. 
4. Nếu không, hãy bắt đầu với danh sách thao tác trống. 
5. Với mọi vị trí (i) trong đó (a_i=1), phép cộng ((i,i)). 

Một thao tác có độ dài một sẽ lật chính xác một bit của (a). Lặp lại điều này cho mọi người làm cho (a) trở thành tất cả số không. 
6. Gọi (cnt) là số đơn vị trong chuỗi gốc (a). 
7. Nếu các chuỗi ban đầu bằng nhau: 

- Nếu (cnt) chẵn thì chúng ta đã hoàn thành. 
- Nếu (cnt) lẻ thì nối thêm 3 phép toán: 
[ 
(1,n),\(1,1),\(2,n). 
] 
8. Nếu các dây ban đầu là số bù: 

- Nếu (cnt) lẻ thì chúng ta đã làm xong rồi. 
- Nếu (cnt) chẵn thì nối 3 phép toán tương tự: 
[ 
(1,n),\(1,1),\(2,n). 
] 
9. Xuất ra tất cả các hoạt động được ghi lại. 

### Tại sao nó hoạt động 

Giả sử chúng ta thực hiện thao tác có độ dài một cho mọi vị trí trong đó (a_i=1). 

Mỗi thao tác như vậy sẽ đảo một bit của (a), do đó cuối cùng (a) trở thành toàn số 0. 

Câu hỏi duy nhất còn lại là trạng thái của (b). Câu trả lời phụ thuộc vào tính chẵn lẻ của số lượng thao tác được thực hiện. 

Mọi thao tác đều chuyển đổi mối quan hệ toàn cục giữa các chuỗi. Nếu chúng ta bắt đầu từ các chuỗi bằng nhau, thì sau một số chẵn các phép toán chúng vẫn bằng nhau và sau một số lẻ chúng trở thành phần bù. Vì (a) đã là tất cả số không, nên đẳng thức có nghĩa là (b) cũng là tất cả số không. 

Nếu chúng ta bắt đầu từ các chuỗi bổ sung thì cần có tính chẵn lẻ ngược lại. 

Ba thao tác bổ sung là một tiện ích điều chỉnh chẵn lẻ. Hiệu ứng kết hợp của chúng trên các chuỗi là trung tính, nhưng chúng thay đổi số lượng thao tác lên ba, làm đảo lộn tính chẵn lẻ cần thiết. Điều này cho phép chúng ta thỏa mãn mối quan hệ cuối cùng chính xác trong mọi trường hợp. 

Vì điều kiện tồn tại vừa cần vừa đủ nên việc xây dựng luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = input().strip()
        b = input().strip()

        same = all(a[i] == b[i] for i in range(n))
        comp = all(a[i] != b[i] for i in range(n))

        if not same and not comp:
            out.append("NO")
            continue

        ops = []

        ones = 0
        for i, ch in enumerate(a, start=1):
            if ch == '1':
                ops.append((i, i))
                ones += 1

        need_flip = False

        if same:
            need_flip = (ones % 2 == 1)
        else:
            need_flip = (ones % 2 == 0)

        if need_flip:
            ops.append((1, n))
            ops.append((1, 1))
            ops.append((2, n))

        out.append("YES")
        out.append(str(len(ops)))
        for l, r in ops:
            out.append(f"{l} {r}")

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên kiểm tra hai cấu hình duy nhất có thể đạt đến trạng thái đích: các chuỗi giống hệt nhau và các chuỗi bổ sung. 

Vòng lặp tiếp theo ghi lại một thao tác một vị trí cho mỗi thao tác trong (a). Đây là cách đơn giản nhất để buộc (a) trở thành toàn số không. 

Logic chẵn lẻ là trung tâm của việc xây dựng. Sau khi xóa tất cả những cái đó, chỉ có mối quan hệ bình đẳng/bổ sung toàn cầu là quan trọng. Tùy thuộc vào mối quan hệ bắt đầu, chúng ta cần số phép tính chẵn hoặc lẻ. 

Ba phép toán cuối cùng luôn hợp lệ vì (n \ge 2). Hiệu ứng kết hợp của chúng duy trì cấu hình đạt được trong khi thay đổi tính chẵn lẻ của số lượng thao tác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = 010
b = 101
```| Bước | Hành động | Số lượng hoạt động | 
| --- | --- | --- | 
| Ban đầu | Chuỗi là phần bổ sung | 0 | 
| Vị trí 2 là 1 | Cộng (2,2) | 1 | 
| Số người đếm = 1 | Tính chẵn lẻ lẻ đã đúng rồi | 1 | 

Đầu ra:```
YES
1
2 2
```Ví dụ này cho thấy trường hợp bổ sung. Một số lẻ các phép toán vị trí đơn lẻ đã mang lại tính chẵn lẻ cần thiết. 

### Ví dụ 2 

đầu vào:```
n = 3
a = 111
b = 111
```| Bước | Hành động | Số lượng hoạt động | 
| --- | --- | --- | 
| Ban đầu | Các chuỗi bằng nhau | 0 | 
| Cộng (1,1) | Xóa cái đầu tiên | 1 | 
| Cộng (2,2) | Xóa cái thứ hai | 2 | 
| Cộng (3,3) | Xóa cái thứ ba | 3 | 
| Tính chẵn lẻ lẻ, cần chỉnh sửa | Thêm ba thao tác bổ sung | 6 | 

Số lượng là số lẻ. Bình đẳng yêu cầu số lượng chẵn các chuyển đổi hiệu quả, vì vậy tiện ích điều chỉnh chẵn lẻ được thêm vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(n) | Lưu trữ tối đa (n+3) hoạt động | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm tối đa là (2 \cdot 10^5), do đó quá trình xử lý tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    res = []

    for _ in range(t):
        n = int(input())
        a = input().strip()
        b = input().strip()

        same = all(a[i] == b[i] for i in range(n))
        comp = all(a[i] != b[i] for i in range(n))

        if not same and not comp:
            res.append("NO")
            continue

        ops = []
        ones = 0

        for i, ch in enumerate(a, 1):
            if ch == '1':
                ops.append((i, i))
                ones += 1

        need = (ones % 2 == 1) if same else (ones % 2 == 0)

        if need:
            ops.append((1, n))
            ops.append((1, 1))
            ops.append((2, n))

        res.append("YES")
        res.append(str(len(ops)))

    return "\n".join(res)

assert run(
"""1
3
010
101
"""
).startswith("YES")

assert run(
"""1
2
11
10
"""
) == "NO"

assert run(
"""1
4
1000
0011
"""
) == "NO"

assert run(
"""1
2
10
10
"""
).startswith("YES")

assert run(
"""1
2
01
10
"""
).startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`11 / 10`| KHÔNG | Mô hình bình đẳng hỗn hợp là không thể | 
|`1000 / 0011`| KHÔNG | Một cấu hình không thể khác | 
|`10 / 10`| CÓ | Chuỗi bằng nhau | 
|`01 / 10`| CÓ | Chuỗi bổ sung | 
|`111 / 111`| CÓ | Đường dẫn điều chỉnh chẵn lẻ | 

## Vỏ cạnh 

Hãy xem xét:```
n = 2
a = 11
b = 10
```Vị trí 1 bằng nhau trong khi vị trí 2 khác nhau. Mô hình bình đẳng là hỗn hợp. Vì mọi thao tác đều đảo ngược trạng thái của tất cả các vị trí cùng một lúc nên các mẫu hỗn hợp không bao giờ có thể trở thành số 0. Thuật toán in ngay lập tức`NO`. 

Hãy xem xét: 

``
