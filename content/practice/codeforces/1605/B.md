---
title: "CF 1605B - ​​Sắp xếp ngược"
description: "Chúng ta được cấp một chuỗi nhị phân và muốn chuyển đổi nó thành thứ tự được sắp xếp, điều này đối với chuỗi nhị phân có nghĩa là tất cả các số 0 đều xuất hiện trước tất cả các số 1. Một thao tác cho phép chúng ta chọn một dãy con có giá trị không tăng."
date: "2026-06-10T08:09:22+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1605
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 754 (Div. 2)"
rating: 1000
weight: 1605
solve_time_s: 793
verified: false
draft: false
---

[CF 1605B - Sắp xếp ngược](https://codeforces.com/problemset/problem/1605/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 13m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân và muốn chuyển đổi nó thành thứ tự được sắp xếp, điều này đối với chuỗi nhị phân có nghĩa là tất cả`0`s xuất hiện trước tất cả`1`S. 

Một thao tác cho phép chúng ta chọn một dãy con có giá trị không tăng. Vì chuỗi chỉ chứa`0`Và`1`, dãy con như vậy chỉ có thể trông giống một số nào đó`1`s theo sau là một số`0`S. Sau đó chúng tôi đảo ngược chuỗi đã chọn tại chỗ. 

Nhiệm vụ không chỉ là xác định số lượng thao tác tối thiểu mà còn đưa ra một chuỗi thao tác hợp lệ để đạt được mức tối thiểu đó. 

Những hạn chế là rất nhỏ. Tổng độ dài của tất cả các chuỗi nhiều nhất là 1000, do đó, ngay cả phép tính bậc hai cho mỗi trường hợp kiểm thử cũng có thể được chấp nhận. Thách thức thực sự là khám phá cấu trúc của hoạt động. 

Một điểm tinh tế là chúng ta đang làm việc với các chuỗi con chứ không phải chuỗi con. Các vị trí được chọn không cần phải liên tiếp. 

Hãy xem xét chuỗi`0011111`. Nó đã được sắp xếp rồi. Bất kỳ giải pháp nào luôn thực hiện một thao tác sẽ không chính xác vì số lượng thao tác tối thiểu bằng không. 

Coi như`10100`. Các ký tự đặt sai vị trí là ngoài cùng bên trái`1`điều đó sẽ di chuyển sang phải và ngoài cùng bên phải`0`s nên di chuyển sang trái. Một chiến lược đơn giản là hoán đổi các ký tự liền kề sẽ hoạt động nhưng sẽ không tôn trọng thao tác được xác định trong bài toán. 

Coi như`111000`. Chuỗi chưa được sắp xếp, nhưng chỉ cần một thao tác được chọn cẩn thận là đủ. Thiếu quan sát này dẫn đến các giải pháp sử dụng nhiều thao tác không cần thiết. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ đến việc liên tục sửa chữa các nghịch đảo. Sự đảo ngược là một cặp trong đó một`1`xuất hiện trước một`0`. Người ta có thể cố gắng di chuyển các ký tự xung quanh cho đến khi chuỗi được sắp xếp. Những cách tiếp cận như vậy là không cần thiết và không khai thác được cấu trúc đặc biệt của hoạt động. 

Quan sát chính xuất phát từ việc kiểm tra xem một dãy con hợp lệ trông như thế nào trong một chuỗi nhị phân. Vì dãy con phải không tăng nên nó có thể chứa bất kỳ tập hợp các`1`s và bất kỳ bộ sưu tập nào của`0`s xuất hiện sau đó. Khi chuỗi con đó bị đảo ngược, mọi lựa chọn`1`di chuyển về phía bên phải và mọi lựa chọn`0`di chuyển về phía bên trái. 

Bây giờ so sánh chuỗi với phiên bản được sắp xếp của nó. Mỗi vị trí trong chuỗi hiện tại chứa`1`nhưng chuỗi được sắp xếp chứa`0`là một sự thất lạc`1`. Tương tự, mọi vị trí trong chuỗi hiện tại chứa`0`nhưng chuỗi được sắp xếp chứa`1`là một sự thất lạc`0`. 

Tất cả đặt nhầm chỗ`1`s xảy ra trước khi tất cả bị đặt sai chỗ`0`S. Nếu chúng ta tập hợp chính xác những vị trí đó thành một dãy con thì dãy con đó bao gồm một số`1`theo sau là một số`0`s, thỏa mãn yêu cầu vận hành. Đảo ngược nó sẽ đặt mọi ký tự đã chọn vào đúng vị trí ngay lập tức. 

Điều này có nghĩa là mọi chuỗi không được sắp xếp đều có thể được sửa bằng chính xác một thao tác. Nếu chuỗi đã được sắp xếp, chúng ta không cần thực hiện thao tác nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng sửa lỗi đảo ngược | O(n²) | O(n) | Không cần thiết | 
| So sánh với chuỗi đã sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

Vì bảng chữ cái chỉ`{0,1}`, ngay cả bước sắp xếp cũng có thể được thay thế bằng cách đếm số 0, nhưng việc triển khai tiêu chuẩn với tính năng sắp xếp đã quá đủ nhanh. 

## Hướng dẫn thuật toán 

1. Đối với chuỗi hiện tại, hãy tạo phiên bản được sắp xếp của nó. 
2. Tìm mọi chỉ mục trong đó chuỗi gốc khác với chuỗi đã sắp xếp. 
3. Lưu trữ tất cả các chỉ số đó. 
4. Nếu không có chỉ số nào khác nhau thì chuỗi đã được sắp xếp. đầu ra`0`. 
5. Ngược lại, xuất ra`1`bởi vì một thao tác là đủ. 
6. Xuất các chỉ số đã thu thập dưới dạng dãy con đã chọn. 

Lý do bước 6 có hiệu quả là vì mọi vị trí không khớp ở phía bên trái đều đóng góp một`1`phải di chuyển sang phải, trong khi mọi vị trí không khớp ở phía bên phải đều góp phần`0`đó phải di chuyển sang trái. Do đó, dãy con được thu thập là một dãy gồm`1`theo sau là`0`s, hợp lệ cho hoạt động. 

### Tại sao nó hoạt động 

hãy để`t`là phiên bản được sắp xếp của chuỗi. 

Bất cứ khi nào`s[i] != t[i]`, ký tự ở vị trí`i`phải tham gia vào bất kỳ sự chuyển đổi thành công nào. Vì số lượng`0`cát`1`s không thay đổi, các vị trí không khớp tự nhiên được chia thành hai nhóm: các vị trí chứa thêm`1`s và các vị trí chứa thêm`0`S. 

Tất cả thêm`1`đó là lời nói dối trước tất cả`0`S. Do đó, các ký tự ở vị trí không khớp sẽ tạo thành một dãy con không tăng. Đảo ngược chuỗi con này sẽ hoán đổi hai nhóm và đặt mọi vị trí không khớp vào giá trị chính xác của nó. Sau khi đảo ngược, toàn bộ chuỗi trở thành bằng`t`. 

Vì vậy, một thao tác là đủ bất cứ khi nào chuỗi chưa được sắp xếp và không cần thao tác nào khi chuỗi đã được sắp xếp. Điều này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        target = ''.join(sorted(s))

        pos = []
        for i in range(n):
            if s[i] != target[i]:
                pos.append(i + 1)

        if not pos:
            print(0)
        else:
            print(1)
            print(len(pos), *pos)

solve()
```Giải pháp đầu tiên xây dựng chuỗi mục tiêu được sắp xếp. Mọi vị trí mà chuỗi gốc khác với chuỗi đích đều phải được sửa. Những vị trí đó chính xác là các chỉ số được sử dụng trong một thao tác. 

Các chỉ số được in theo thứ tự tăng dần, điều này là bắt buộc vì chúng tạo thành một dãy con. Sự cố sử dụng chỉ mục dựa trên một, do đó mỗi vị trí được báo cáo là`i + 1`. 

Lỗi triển khai phổ biến nhất là quên chuyển đổi một lần. Một lỗi phổ biến khác là cố gắng xuất ra nhiều thao tác mặc dù một thao tác luôn đủ cho mọi chuỗi nhị phân không được sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`10100`Phiên bản sắp xếp:`00011`| Vị trí | Bản gốc | Đã sắp xếp | Khác biệt? | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | Có | 
| 2 | 0 | 0 | Không | 
| 3 | 1 | 0 | Có | 
| 4 | 0 | 1 | Có | 
| 5 | 0 | 1 | Có | 

Các chỉ số được chọn:`[1, 3, 4, 5]`Đầu ra:`1`

`4 1 3 4 5`Các ký tự được chọn là`1 1 0 0`, tức là không tăng. Đảo ngược chúng tạo ra`0 0 1 1`, fixing all mismatches at once.

 ### Ví dụ 2 

Chuỗi đầu vào:`0011111`Phiên bản sắp xếp:`0011111`| Vị trí | Bản gốc | Đã sắp xếp | Khác biệt? | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | Không | 
| 2 | 0 | 0 | Không | 
| 3 | 1 | 1 | Không | 
| 4 | 1 | 1 | Không | 
| 5 | 1 | 1 | Không | 
| 6 | 1 | 1 | Không | 
| 7 | 1 | 1 | Không | 

Không có sự không phù hợp. 

Đầu ra:`0`Điều này thể hiện trường hợp hoạt động tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chuỗi nhị phân | 
| Không gian | O(n) | Chuỗi mục tiêu và chỉ số không khớp | 

Tổng kích thước đầu vào tối đa là 1000 ký tự trong tất cả các trường hợp thử nghiệm. Ngay cả giải pháp O(n²) cũng có thể phù hợp một cách thoải mái, do đó O(n log n) dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    data = io.StringIO(inp)

    def input():
        return data.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        target = ''.join(sorted(s))
        pos = [i + 1 for i in range(n) if s[i] != target[i]]

        if not pos:
            out.append("0")
        else:
            out.append("1")
            out.append(str(len(pos)) + " " + " ".join(map(str, pos)))

    return "\n".join(out)

# sample-style checks
assert run("1\n7\n0011111\n") == "0"

# already sorted
assert run("1\n1\n0\n") == "0"

# all ones
assert run("1\n5\n11111\n") == "0"

# reverse-sorted binary string
assert run("1\n6\n111000\n").splitlines()[0] == "1"

# alternating pattern
assert run("1\n4\n1010\n").splitlines()[0] == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0 operations`| Kích thước tối thiểu | 
|`11111`|`0 operations`| Tất cả các ký tự bằng nhau | 
|`111000`|`1 operation`| Nồng độ không phù hợp tối đa | 
|`1010`|`1 operation`| Mô hình xen kẽ | 
|`0011111`|`0 operations`| Đã sắp xếp chuỗi | 

## Vỏ cạnh 

Xem xét đầu vào`1`theo sau là chuỗi`0`. Phiên bản được sắp xếp giống hệt nhau nên tập hợp không khớp sẽ trống. Thuật toán đưa ra các hoạt động bằng 0 ngay lập tức. 

Hãy xem xét chuỗi`111000`. Phiên bản được sắp xếp là`000111`. Mỗi vị trí đều khác nhau nên tất cả sáu chỉ số đều được chọn. Dãy con được chọn là`1 1 1 0 0 0`, tức là không tăng. Đảo ngược nó trực tiếp tạo ra chuỗi được sắp xếp. 

Hãy xem xét chuỗi`001011`. Phiên bản được sắp xếp là`000111`. Sự không phù hợp chỉ xảy ra ở các vị trí ở giữa. Việc chọn chính xác các vị trí đó sẽ tránh chạm vào các ký tự đã đúng và một lần đảo ngược sẽ sửa hoàn toàn chuỗi. 

Những trường hợp này minh họa tính bất biến trung tâm: các vị trí không khớp luôn tạo thành một chuỗi con không tăng hợp lệ và việc đảo ngược chuỗi con đó sẽ giải quyết đồng thời mọi chuỗi không khớp.
