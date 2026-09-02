---
title: "CF 104454N - Chỉ là một vấn đề về mảng khác"
description: "Chúng ta được cung cấp một dãy bắt đầu đã được sắp xếp theo thứ tự không giảm. Sau đó, trình tự không bị sửa đổi bằng cách thêm hoặc xóa, nhưng nó có thể được xoay theo chu kỳ nhiều lần, sang trái hoặc sang phải."
date: "2026-06-30T14:30:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104454
codeforces_index: "N"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2021"
rating: 0
weight: 104454
solve_time_s: 71
verified: true
draft: false
---

[CF 104454N - Chỉ là một vấn đề khác về mảng](https://codeforces.com/problemset/problem/104454/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy bắt đầu đã được sắp xếp theo thứ tự không giảm. Sau đó, trình tự không bị sửa đổi bằng cách thêm hoặc xóa, nhưng nó có thể được xoay theo chu kỳ nhiều lần, sang trái hoặc sang phải. 

Bên cạnh những vòng quay này, chúng tôi còn nhận được hai loại truy vấn. Một loại thực hiện phép quay bằng một số offset, dịch chuyển mảng theo hình tròn một cách hiệu quả. Loại còn lại hỏi liệu một giá trị có tồn tại trong mảng được xoay hiện tại hay không và nếu có, chúng tôi phải báo cáo vị trí xuất hiện đầu tiên của nó trong bố cục hiện tại, nếu không chúng tôi sẽ xuất ra -1. 

Quan sát quan trọng ẩn trong câu lệnh là phép quay không làm thay đổi tập hợp nhiều giá trị hoặc thứ tự tương đối của chúng trong mảng được sắp xếp ban đầu. Chỉ có điểm bắt đầu của việc lập chỉ mục thay đổi. Vì vậy, cấu trúc luôn là một mảng được sắp xếp được xem qua một cửa sổ tròn chuyển động. 

Các ràng buộc rất lớn, lên tới 200.000 phần tử và lên tới 300.000 truy vấn. Điều này ngay lập tức loại trừ mọi cách tiếp cận xoay mảng một cách vật lý cho mỗi ca, vì một vòng quay sẽ tốn O(N), dẫn đến O(NQ) trong trường hợp xấu nhất, vượt xa giới hạn. Ngay cả việc quét mảng cho từng truy vấn cũng sẽ quá chậm. 

Một vấn đề tế nhị phát sinh với việc lập chỉ mục sau nhiều lần quay. Nếu chúng ta mô phỏng các ca thay đổi không chính xác, chúng ta có thể dễ dàng mất dấu vị trí hiện tại của chỉ số 1, gây ra các câu trả lời sai cho các giá trị trùng lặp hoặc gói ranh giới. Một trường hợp phức tạp khác là các giá trị lặp lại, trong đó “lần xuất hiện đầu tiên” phụ thuộc vào điểm bắt đầu xoay hiện tại chứ không phải thứ tự ban đầu. 

Ví dụ, hãy xem xét`a = [1,1,2]`. Sau khi dịch phải 1, mảng trở thành`[2,1,1]`. Một truy vấn cho`1`phải trả về chỉ mục 2, không phải 3, vì số 1 đầu tiên trong chế độ xem được xoay ở vị trí 2. Bất kỳ giải pháp nào chỉ ghi nhớ vị trí ban đầu của các giá trị mà không điều chỉnh để xoay sẽ báo cáo sai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi duy trì mảng một cách rõ ràng. Mỗi thao tác thay đổi thực hiện một phép quay theo chu kỳ thực tế, bằng cách sử dụng các thao tác cắt hoặc deque. Mỗi truy vấn quét toàn bộ mảng để tìm lần xuất hiện đầu tiên của giá trị đích. 

Điều này đúng vì nó mô phỏng quá trình được mô tả theo đúng nghĩa đen. Tuy nhiên, mỗi vòng quay có chi phí O(N) và mỗi truy vấn có chi phí O(N). Với tối đa 300.000 thao tác, điều này dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Quan sát quan trọng là mảng tĩnh cho đến khi xoay. Chúng tôi không bao giờ thay đổi thứ tự tương đối, chỉ thay đổi chỉ số bắt đầu. Điều này có nghĩa là chúng ta không cần phải di chuyển các phần tử. Chúng tôi chỉ cần duy trì một khoảng bù logic cho chúng tôi biết chỉ mục nào hiện được coi là vị trí 1. Sau khi chúng tôi sửa phần bù này, mọi truy vấn sẽ trở thành một vấn đề dịch chỉ mục đơn giản. 

Để hỗ trợ các truy vấn thành viên nhanh chóng, chúng tôi khai thác rằng mảng được sắp xếp, do đó tìm kiếm nhị phân hoạt động trên mảng ban đầu. Điều phức tạp duy nhất là dịch chỉ mục tìm thấy trong mảng ban đầu sang vị trí được xoay của nó. 

Chúng tôi kết hợp hai ý tưởng: một phần bù đang chạy để xoay và tìm kiếm nhị phân để định vị các giá trị. Điều này làm giảm mỗi truy vấn xuống O(log N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng mảng) | O(NQ) | O(N) | Quá chậm | 
| Tối ưu (offset + tìm kiếm nhị phân) | O(Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mảng là cố định trong bộ nhớ và duy trì một biến`shift`biểu thị số lượng vị trí mà mảng đã được xoay sang bên phải so với cấu hình ban đầu của nó. 

1. Khởi tạo`shift = 0`. Điều này có nghĩa là căn chỉnh chỉ mục không thay đổi so với mảng ban đầu. 
2. Đối với hoạt động theo ca`s k`, chúng tôi chuẩn hóa nó theo modulo`N`và cập nhật ca làm việc. Một sự dịch chuyển sang phải bởi`k`làm tăng độ lệch, trong khi dịch chuyển trái sẽ làm giảm nó. Chúng tôi lưu trữ mọi thứ theo modulo`N`vì vậy sự thay đổi vẫn nằm trong phạm vi. Bước này tránh việc xoay mảng về mặt vật lý. 
3. Đối với một truy vấn`? x`, trước tiên chúng tôi thực hiện tìm kiếm nhị phân trên mảng được sắp xếp ban đầu để tìm chỉ mục xuất hiện đầu tiên`i`có giá trị`x`. Nếu không tìm thấy, chúng tôi xuất ngay`-1`. 
4. Khi chúng ta có chỉ mục`i`trong mảng ban đầu, chúng tôi chuyển đổi nó sang vị trí hiện tại trong mảng được xoay bằng cách sử dụng shift. Chỉ số gốc`i`tiến về phía trước bằng cách`shift`, vậy vị trí mới của nó là`(i + shift) mod N`, được chuyển đổi thành chỉ mục dựa trên 1. 
5. Chúng tôi xuất ra vị trí được tính toán này. 

Ý tưởng chính là mọi phần tử đều giữ nguyên danh tính và thứ tự tương đối, vì vậy điều duy nhất thay đổi là cách chúng ta diễn giải các chỉ số. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mảng tương đương với việc lấy mảng được sắp xếp ban đầu và cắt nó ở một vị trí nào đó`shift`, sau đó hoán đổi hai phần. Đây là một phép biến đổi tuần hoàn cứng nhắc, do đó thứ tự tương đối của tất cả các phần tử được giữ nguyên. Do đó, lần xuất hiện thứ k của bất kỳ giá trị nào trong mảng ban đầu vẫn là lần xuất hiện thứ k trong cấu trúc được xoay, chỉ có chỉ số của nó thay đổi đồng đều. Biến shift nắm bắt hoàn toàn sự chuyển đổi này, do đó, việc ánh xạ các chỉ số thông qua nó sẽ duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    shift = 0

    def first_occurrence(x):
        lo, hi = 0, n - 1
        ans = -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if a[mid] >= x:
                hi = mid - 1
            else:
                lo = mid + 1
        if lo < n and a[lo] == x:
            return lo
        return -1

    for _ in range(q):
        parts = input().split()
        if parts[0] == 's':
            k = int(parts[1])
            k %= n
            shift = (shift + k) % n
        else:
            x = int(parts[1])
            i = first_occurrence(x)
            if i == -1:
                print(-1)
            else:
                pos = (i + shift) % n + 1
                print(pos)

if __name__ == "__main__":
    main()
```Giải pháp giữ cho mảng không thay đổi và chỉ duy trì độ lệch xoay. Tìm kiếm nhị phân tìm thấy sự xuất hiện đầu tiên trong mảng được sắp xếp ban đầu, điều này hợp lệ vì việc sắp xếp được giữ nguyên khi xoay. Bước cuối cùng điều chỉnh chỉ mục bằng cách sử dụng số học mô-đun. Điều tinh tế duy nhất là đảm bảo hoạt động modulo xử lý các dịch chuyển âm một cách chính xác, đó là lý do tại sao các dịch chuyển được chuẩn hóa trước khi áp dụng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng ban đầu:`[1,2,3,4,5,6,7]`Chúng tôi theo dõi`shift`và xử lý từng thao tác. 

| Bước | Hoạt động | ca | Chỉ mục được tìm thấy (bản gốc) | Vị trí được tính toán | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ? 9 | 0 | - | - | -1 | 
| 2 | s 2 | 2 | - | - | - | 
| 3 | ? 4 | 2 | 3 | (3+2)%7+1 = 6 | 6 | 
| 4 | s -2 | 0 | - | - | - | 
| 5 | ? 3 | 0 | 2 | 2 | 3 | 
| 6 | s -5 | 2 | - | - | - | 
| 7 | ? 6 | 2 | 5 | (5+2)%7+1 = 1 | 1 | 

Dấu vết này cho thấy cách chỉ mục ban đầu giống nhau ánh xạ tới các vị trí khác nhau hoàn toàn thông qua biến dịch chuyển. 

### Mẫu 2 

Mảng ban đầu:`[1,1,2,2,3,3,4]`| Bước | Hoạt động | ca | Chỉ số xuất hiện đầu tiên | Vị trí được tính toán | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ? 9 | 0 | - | - | -1 | 
| 2 | s 2 | 2 | - | - | - | 
| 3 | ? 4 | 2 | 6 | (6+2)%7+1 = 2 | 2 | 
| 4 | s -1 | 1 | - | - | - | 
| 5 | ? 2 | 1 | 2 | (2+1)%7+1 = 4 | 4 | 
| 6 | s -5 | 3 | - | - | - | 
| 7 | ? 1 | 3 | 0 | (0+3)%7+1 = 4 | 4 | 

Ví dụ này làm nổi bật các bản sao. Tìm kiếm nhị phân luôn trả về lần xuất hiện đầu tiên trong mảng ban đầu, giá trị này vẫn hợp lệ sau khi xoay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log N) | Mỗi truy vấn sử dụng tìm kiếm nhị phân O(log N); ca là O(1) | 
| Không gian | O(N) | Chỉ lưu trữ mảng ban đầu | 

Độ phức tạp vừa vặn trong giới hạn vì tổng số phép toán nhiều nhất là vài trăm nghìn và mỗi truy vấn chỉ thực hiện phép tính logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import *
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    q = int(sys.stdin.readline())

    shift = 0

    def first_occurrence(x):
        lo, hi = 0, n - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if a[mid] >= x:
                hi = mid - 1
            else:
                lo = mid + 1
        if lo < n and a[lo] == x:
            return lo
        return -1

    out = []
    for _ in range(q):
        parts = sys.stdin.readline().split()
        if parts[0] == 's':
            k = int(parts[1]) % n
            shift = (shift + k) % n
        else:
            x = int(parts[1])
            i = first_occurrence(x)
            out.append(str(-1 if i == -1 else (i + shift) % n + 1))

    return "\n".join(out) + ("\n" if out else "")

# provided samples
assert run("""7
1 2 3 4 5 6 7
7
? 9
s 2
? 4
s -2
? 3
s -5
? 6
""") == """-1
6
3
1
"""

assert run("""7
1 1 2 2 3 3 4
7
? 9
s 2
? 4
s -1
? 2
s -5
? 1
""") == """-1
2
4
4
"""

# custom cases
assert run("""1
5
3
? 5
s 1
? 5
""") == """1
1
"""

assert run("""3
1 2 3
4
s 1
s 1
s -2
? 2
""") == """2
"""

assert run("""5
1 2 2 2 3
2
? 2
? 4
""") == """2
-1
"""

assert run("""6
1 2 3 4 5 6
1
s 5
""") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1, 1 | độ đúng dịch chuyển tại biên | 
| quay đầy đủ | 2 | tính nhất quán bao quanh | 
| giá trị còn thiếu | -1 | xử lý tìm kiếm không thành công | 
| không có truy vấn | trống | xử lý cạnh tầm thường | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng có kích thước 1. Mọi thao tác dịch chuyển sẽ không có hiệu ứng rõ ràng vì tất cả các phép quay đều giống hệt nhau. Thuật toán xử lý điều này vì sự dịch chuyển luôn được lấy theo modulo N và modulo 1 luôn bằng 0, do đó ánh xạ vị trí vẫn ổn định. 

Một trường hợp khác là giá trị lặp lại. Vì chúng tôi luôn trả về lần xuất hiện đầu tiên trong mảng ban đầu nên chúng tôi dựa vào tìm kiếm nhị phân trả về kết quả khớp ngoài cùng bên trái. Sau khi xoay, mặc dù các bản sao di chuyển thành một khối nhưng thứ tự bên trong của chúng vẫn được giữ nguyên, do đó chỉ mục được ánh xạ vẫn hợp lệ. 

Trường hợp cạnh cuối cùng là sự dịch chuyển âm lớn. Chúng được chuẩn hóa bằng cách sử dụng số học modulo trước khi được áp dụng. Điều này ngăn giá trị dịch chuyển trôi ra ngoài phạm vi mảng và đảm bảo ánh xạ nhất quán ngay cả sau nhiều thao tác trái và phải xen kẽ.
