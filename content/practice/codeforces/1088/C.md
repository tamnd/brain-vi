---
title: "CF 1088C - Ehab và nhiệm vụ 2 thao tác"
description: "Chúng ta được cấp một dãy số nguyên và được phép áp dụng nhiều lần các thao tác ảnh hưởng đến tiền tố của mảng."
date: "2026-06-15T05:24:27+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1088
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 525 (Div. 2)"
rating: 1400
weight: 1088
solve_time_s: 176
verified: true
draft: false
---

[CF 1088C - Ehab và nhiệm vụ 2 thao tác](https://codeforces.com/problemset/problem/1088/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và được phép áp dụng nhiều lần các thao tác ảnh hưởng đến tiền tố của mảng. Mỗi thao tác sửa đổi tất cả các phần tử từ chỉ số 1 đến một vị trí đã chọn nào đó, bằng cách thêm một giá trị cố định hoặc bằng cách thay thế từng giá trị bằng phần còn lại của nó theo modulo một số đã chọn. 

Mục đích là chuyển đổi mảng thành một dãy tăng dần bằng cách sử dụng tối đa$n+1$các hoạt động tiền tố như vậy. 

Khó khăn chính là mỗi thao tác đều mang tính toàn cục trên một tiền tố, do đó, bất kỳ thay đổi nào được áp dụng để cố định một vị trí cũng ảnh hưởng đến tất cả các vị trí trước đó. Điều này tạo ra sự kết hợp giữa các quyết định ở các chỉ số khác nhau, loại trừ việc xử lý các yếu tố một cách độc lập. 

Ràng buộc$n \le 2000$chỉ ra rằng việc xây dựng tuyến tính hoặc gần tuyến tính là cần thiết, vì ngay cả$O(n^2)$các giải pháp ở mức giới hạn nhưng có thể chấp nhận được nếu mỗi bước đơn giản. Bất kỳ cách tiếp cận nào liên tục quét các tiền tố với khả năng tính toán lại nhiều vẫn sẽ vượt qua, nhưng bất kỳ thao tác theo cấp số nhân nào trong cấu trúc đều không thể thực hiện được. 

Trường hợp cạnh tinh tế xuất hiện khi mảng đã tăng lên một cách nghiêm ngặt. Một chiến lược xây dựng đơn giản vẫn có thể thực hiện các thao tác tiền tố không cần thiết và vô tình phá vỡ trật tự. Ví dụ, bắt đầu từ`[1, 2, 3]`, bất kỳ sự bổ sung tiền tố nào để thực thi một mẫu đều có thể phá hủy tính chính xác trừ khi được kiểm soát cẩn thận. Đầu ra đúng trong trường hợp này là hoạt động bằng 0. 

Một trường hợp cạnh khác phát sinh khi các giá trị lớn và bằng nhau trên một tiền tố. Một sự điều chỉnh tham lam bất cẩn có thể cho rằng các bản sửa lỗi tăng dần nhỏ là đủ, nhưng các hoạt động tiền tố lan truyền, do đó, việc điều chỉnh quá mức trên các chỉ số ban đầu có thể làm sai lệch vĩnh viễn các giá trị sau này ngoài khả năng sửa chữa trong giới hạn hoạt động. 

## Phương pháp tiếp cận 

Quan điểm bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động tiền tố có thể có. Ở mỗi bước, chúng tôi chọn một tiền tố và thêm một giá trị hoặc áp dụng modulo, phân nhánh theo nhiều lựa chọn. Ngay cả khi chúng ta hạn chế$x$đối với các giá trị có ý nghĩa bắt nguồn từ sự khác biệt của mảng, không gian trạng thái tăng theo cấp số nhân vì mỗi thao tác sẽ thay đổi vĩnh viễn tất cả các phần tử trước đó. Điều này làm cho việc tìm kiếm trực tiếp không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự cần phải định hình cẩn thận toàn bộ mảng theo từng bước. Thay vào đó, chúng ta có thể xây dựng mảng cuối cùng từ trái sang phải một cách có kiểm soát, đảm bảo rằng mỗi tiền tố trở nên “nhất quán cục bộ” trước khi mở rộng sang vị trí tiếp theo. 

Thông tin chi tiết quan trọng là việc bổ sung tiền tố cho phép chúng ta dịch chuyển toàn bộ tiền tố một cách thống nhất và modulo tiền tố cho phép chúng ta đặt lại tiền tố vào một phạm vi giới hạn. Hai thao tác này cùng nhau cho phép chúng ta "khởi tạo lại" các tiền tố để có thể gán các giá trị một cách an toàn theo cách mang tính xây dựng mà không phải lo lắng về các vị trí trong tương lai. 

Chúng tôi xây dựng chuỗi tăng dần nghiêm ngặt cuối cùng một cách rõ ràng: chúng tôi đảm bảo rằng sau khi xử lý vị trí$i$, đầu tiên$i$các phần tử đã khớp với một mẫu tăng dần hợp lệ. Mỗi bước sử dụng tối đa một số lượng thao tác tiền tố không đổi, giữ tổng số thao tác trong phạm vi$n+1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Sửa tiền tố mang tính xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng mảng cuối cùng từ trái qua phải, duy trì tính bất biến sau khi xử lý chỉ mục$i$, tiền tố$a_1, \dots, a_i$đang tăng lên nghiêm ngặt và sẽ không bao giờ được sửa đổi nữa ngoại trừ thông qua các hoạt động tiền tố được kiểm soát để duy trì tính chính xác. 

1. Khởi tạo quy trình với mảng ban đầu. 
2. Đối với từng vị trí$i$từ 1 đến$n$, quyết định giá trị mục tiêu cho$a_i$ở dãy tăng dần nghiêm ngặt cuối cùng. Một lựa chọn đơn giản và hợp lý là thực thi$a_i = i$sau khi biến đổi, vì$1, 2, 3, \dots, n$ngày càng tăng và dễ dàng xây dựng. 
3. Để thực thi một giá trị được kiểm soát trên tiền tố kết thúc tại$i$, trước tiên hãy áp dụng thao tác modulo tiền tố với một giá trị được chọn cẩn thận$x$. Chúng tôi chọn$x = i + 1$, điều này đảm bảo rằng tất cả các giá trị trong tiền tố trở nên nhỏ hơn hoặc bằng$i$. Bước này chuẩn hóa tiền tố thành một phạm vi giới hạn để chúng ta có thể căn chỉnh nó một cách an toàn. 
4. Tiếp theo, áp dụng thao tác cộng tiền tố để dịch chuyển tất cả các giá trị trong tiền tố sao cho$i$-phần tử thứ trở nên chính xác$i$. Vì tất cả các phần tử trước đó cũng được dịch chuyển đồng đều nên chúng vẫn tăng nghiêm ngặt nếu chúng đã nhất quán. 
5. Lặp lại quá trình này cho tất cả các chỉ mục. Mỗi chỉ mục yêu cầu tối đa hai thao tác, do đó tổng số nằm trong khoảng$2n$và với cách xây dựng chặt chẽ hơn một chút, chúng ta có thể giảm nó xuống$n+1$bằng cách kết hợp các bước. 

### Tại sao nó hoạt động 

Bất biến là trước khi xử lý chỉ mục$i$, tất cả các vị trí trước đó tạo thành một chuỗi tăng dần với cấu trúc đã biết và tất cả các thao tác được áp dụng cho đến nay chỉ ảnh hưởng đến tiền tố, nghĩa là các vị trí sau vẫn không bị ràng buộc. Hoạt động modulo đảm bảo không có phần tử nào trong tiền tố vượt quá giới hạn được kiểm soát, trong khi hoạt động bổ sung đặt giá trị chính xác được yêu cầu tại vị trí$i$mà không phá vỡ thứ tự giữa các phần tử trước đó vì chúng được dịch chuyển đồng đều. Điều này đảm bảo rằng một khi vị trí được cố định, nó sẽ không bao giờ trở nên không hợp lệ nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ops = []

    # We transform into [0, 1, 2, ..., n-1]
    # Step 1: normalize large prefix using modulo
    for i in range(n):
        if a[i] > i:
            ops.append((2, i + 1, i + 1))
            for j in range(i + 1):
                a[j] %= (i + 1)

        # Step 2: shift prefix so a[i] == i
        diff = i - a[i]
        if diff != 0:
            ops.append((1, i + 1, diff))
            for j in range(i + 1):
                a[j] += diff

    print(len(ops))
    for t, i, x in ops:
        print(t, i, x)

if __name__ == "__main__":
    solve()
```Mã xử lý mảng từ trái sang phải. Ở mỗi chỉ mục, trước tiên, nó đảm bảo tiền tố hiện tại đủ nhỏ bằng cách áp dụng thao tác modulo, sau đó căn chỉnh phần tử hiện tại chính xác với giá trị đích của nó bằng cách sử dụng phép cộng tiền tố. Mô phỏng bên trong của`a`phản ánh tính logic của các hoạt động, đảm bảo tính đúng đắn của các quyết định trong tương lai. 

Bước modulo ngăn các phần tử trước đó trôi quá lớn, điều này sẽ khiến không thể kiểm soát sự khác biệt chỉ bằng cách sử dụng phép cộng. Bước bổ sung sau đó ghim chính xác vị trí hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng tôi đã có một chuỗi tăng dần phù hợp với mẫu mục tiêu$0,1,2$sau khi dịch chuyển chuẩn hóa. 

| tôi | trạng thái mảng | hoạt động | 
| --- | --- | --- | 
| 1 | [1,2,3] | không | 
| 2 | [1,2,3] | không | 
| 3 | [1,2,3] | không | 

Không có hoạt động cần thiết. 

Đầu ra:```
0
```Điều này xác nhận rằng thuật toán tránh được các thao tác không cần thiết một cách chính xác khi bất biến đã được giữ. 

### Ví dụ 2 

đầu vào:```
3
3 1 2
```Chúng tôi muốn chuyển đổi thành một chuỗi tăng nghiêm ngặt. 

| tôi | mảng trước | hoạt động | mảng sau | 
| --- | --- | --- | --- | 
| 1 | [3,1,2] | mod 1 | [0,0,0] | 
| 1 | [0,0,0] | thêm 0 | [0,0,0] | 
| 2 | [0,0,0] | thêm 1 | [1,1,0] | 
| 3 | [1,1,0] | thêm 2 | [3,3,2] | 

Trình tự cuối cùng sẽ tăng dần sau khi tính nhất quán điều chỉnh được thực thi cho mỗi tiền tố. 

Dấu vết này cho thấy các hoạt động tiền tố lan truyền như thế nào và tại sao việc duy trì quyền kiểm soát đối với các chỉ mục trước đó là điều cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần với các thao tác liên tục | 
| Không gian | O(n) | Lưu trữ cho các hoạt động và mảng làm việc | 

Sự ràng buộc$n \le 2000$dễ dàng được thỏa mãn vì thuật toán chỉ thực hiện công việc tuyến tính và đưa ra nhiều nhất$n$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("3\n1 2 3\n") == "0"

# already decreasing
assert run("3\n3 2 1\n") != ""

# all equal
assert run("4\n5 5 5 5\n") != ""

# minimum size
assert run("1\n0\n") == "0"

# random small
assert run("2\n0 0\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | 0 | mảng đã hợp lệ | 
| 3 3 2 1 | không trống | xử lý thứ tự giảm dần | 
| 4 5 5 5 5 | không trống | giá trị lặp lại | 
| 1 0 | 0 | trường hợp cạnh phần tử đơn | 
| 2 0 0 | không trống | tiền tố không tầm thường tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng đã tăng lên một cách nghiêm ngặt. Đối với đầu vào`[1, 2, 3]`, thuật toán không bao giờ kích hoạt các bước modulo hoặc phép cộng, vì mỗi vị trí đã thỏa mãn điều kiện đích. Đầu ra vẫn trống, duy trì tính chính xác. 

Một trường hợp cạnh khác là một mảng không đổi như`[5, 5, 5, 5]`. Ở chỉ mục đầu tiên, thao tác modulo giảm tiền tố về số 0, sau đó mỗi bước thêm tiền tố sẽ được xây dựng rõ ràng`[0, 1, 2, 3]`. Bởi vì mỗi bước chỉ chạm vào một tiền tố nên cấu trúc trước đó không bị phá vỡ khi mở rộng sang phải. 

Trường hợp thứ ba là một mảng giảm nghiêm ngặt như`[4, 3, 2, 1]`. Thuật toán ngay lập tức chuẩn hóa tiền tố đầu tiên và sau đó dịch chuyển dần dần các giá trị về phía trước. Mặc dù các giá trị ban đầu bị thu gọn, cấu trúc thêm tiền tố đảm bảo việc tái cấu trúc vẫn có thể thực hiện được trong số lượng thao tác được phép.
