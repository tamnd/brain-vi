---
title: "CF 102501D - Mèo Gnalcat"
description: "Gen là một chương trình ngắn giúp sửa đổi phần đầu của một chuỗi axit amin cực kỳ dài. Đầu vào chứa hai chương trình như vậy và nhiệm vụ là quyết định xem chúng có luôn hoạt động giống hệt nhau trên mọi chuỗi axit amin đơn giản đủ dài hay không."
date: "2026-08-06T04:59:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 1027
verified: true
draft: false
---

[CF 102501D - Gnalcats](https://codeforces.com/problemset/problem/102501/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 17 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Gen là một chương trình ngắn giúp sửa đổi phần đầu của một chuỗi axit amin cực kỳ dài. Đầu vào chứa hai chương trình như vậy và nhiệm vụ là quyết định xem chúng có luôn hoạt động giống hệt nhau trên mọi chuỗi axit amin đơn giản đủ dài hay không. 

Chuỗi rất lớn nên chúng tôi không thể xây dựng đầu vào thực sự. Phần duy nhất mà gen có thể chạm tới là tiền tố hữu hạn của chuỗi. Mỗi thao tác thao tác với một hoặc hai axit amin đầu tiên bằng cách loại bỏ, sao chép, hoán đổi, kết hợp hoặc tách chúng. Axit amin phức hợp là cây nhị phân có con là các axit amin khác. 

Tổng chiều dài của cả hai gen nhiều nhất là 10000. Điều này loại trừ việc thử nhiều protein đầu vào có thể hoặc liên tục mở rộng cây hoàn chỉnh. Một giải pháp cần xử lý từng thao tác trong thời gian gần như không đổi. Mô phỏng sao chép toàn bộ cấu trúc axit amin sau mỗi thao tác sẽ trở nên quá chậm vì việc sao chép lặp đi lặp lại có thể làm cho đối tượng được biểu diễn lớn theo cấp số nhân. 

Các trường hợp khó khăn chính đến từ việc coi axit amin là giá trị thay vì danh tính và từ việc bỏ qua các lỗi. Ví dụ:```
L
R
```có câu trả lời`True`. Cả hai thao tác đều thất bại ở mọi đầu vào có thể vì axit amin đầu tiên luôn đơn giản. Việc triển khai bất cẩn chỉ so sánh các chuyển đổi thành công có thể báo cáo không chính xác rằng chúng khác nhau. 

Một trường hợp quan trọng khác là sự bình đẳng về cấu trúc của các axit amin phức tạp.```
PU
SS
```có câu trả lời`True`. Hai gen giữ nguyên chuỗi ban đầu không thay đổi. Các cấu trúc trung gian có thể khác nhau nhưng cây nhị phân cuối cùng phải được so sánh theo cấu trúc. 

Cái bẫy cuối cùng là các tài liệu tham khảo trùng lặp.```
C
P
```có câu trả lời`False`. Gen đầu tiên biến đổi`a-b-c-...`vào trong`a-a-b-c-...`, trong khi chất thứ hai tạo ra axit amin phức tạp`<a,b>`. Chỉ so sánh tập hợp các lá sẽ coi chúng như nhau một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra chuỗi đầu vào mang tính biểu tượng, chạy cả hai gen và so sánh các chuỗi kết quả. Điều này đúng vì đầu vào luôn được tạo từ các axit amin đơn giản, do đó tiền tố ký hiệu hữu hạn có thể đại diện cho mọi trường hợp có thể xảy ra. Vấn đề là chọn kích thước tiền tố và lưu trữ cấu trúc một cách hiệu quả. Nếu chúng ta sao chép cây mỗi khi một thao tác tạo ra một axit amin phức tạp mới, thì một chuỗi gồm nhiều`C`Và`P`các hoạt động có thể lặp lại nhiều lần các biểu thức lớn. 

Quan sát hữu ích là mọi thao tác chỉ thay đổi tham chiếu đến axit amin. Một axit amin phức tạp có thể được lưu trữ dưới dạng một nút chứa hai tham chiếu con. Tạo`<a,b>`chỉ tạo một nút mới và các cặp bằng nhau có thể chia sẻ cùng một nút thông qua thực tập. Toàn bộ protein trở thành một tập hợp các mã định danh nút. 

Độ dài gen cũng đưa ra giới hạn về tiền tố ban đầu được yêu cầu. Trong 10000 thao tác, chương trình có thể loại bỏ tối đa 10000 axit amin cấp cao nhất. Bắt đầu với nhiều hơn một chút có nghĩa là quy tắc lỗi đặc biệt gây ra bằng cách giảm độ dài chuỗi xuống còn ba hoặc ít hơn sẽ không bao giờ xuất hiện trong quá trình mô phỏng. Những thất bại duy nhất còn lại là do áp dụng`L`,`R`, hoặc`U`thành một axit amin đơn giản mà biểu diễn ngăn xếp phát hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L * kích thước của cây mở rộng) | O(kích thước cây mở rộng) | Quá chậm | 
| Tối ưu | O(L) | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một nút duy nhất cho mỗi axit amin đơn giản ban đầu trong chuỗi ký hiệu đủ dài. Chuỗi chỉ cần dài hơn số lần loại bỏ tối đa mà một gen có thể thực hiện, bởi vì tất cả các phần tử hậu tố chưa được chạm tới vẫn giống hệt nhau. 
2. Xử lý một gen từ trái sang phải trong khi lưu trữ protein hiện tại dưới dạng một tập hợp các mã định danh nút. Đỉnh của ngăn xếp đại diện cho axit amin đầu tiên trong chuỗi. 
3. Đối với`C`, nhân đôi phần tử ngăn xếp trên cùng. Vì`D`, loại bỏ nó. Vì`S`, hoán đổi hai phần tử trên cùng. Các thao tác này chỉ sắp xếp lại các tài liệu tham khảo nên mất nhiều thời gian. 
4. Đối với`P`, thay thế hai phần tử ngăn xếp đầu tiên bằng sự kết hợp phức tạp của chúng. Cặp định danh con được tra cứu trong bảng sao cho cùng một axit amin phức tạp luôn nhận được cùng một định danh. 
5. Đối với`L`,`R`, Và`U`, kiểm tra xem nút trên cùng có phức tạp không. Nếu nó đơn giản thì gen sẽ thất bại. Nếu không thì thay thế phần tử trên cùng bằng phần tử con hoặc các phần tử con được yêu cầu. 
6. Chạy cùng một mô phỏng cho cả hai gen bằng cách sử dụng cùng một chuỗi ký hiệu ban đầu. Nếu một mô phỏng thất bại và mô phỏng kia thành công thì các gen sẽ khác nhau. Nếu cả hai đều thất bại, chúng tương đương nhau. 
7. Nếu cả hai đều thành công, hãy so sánh từng phần tử trong ngăn xếp cuối cùng. Bởi vì tất cả các nút phức tạp đều nằm bên trong nên các số nhận dạng bằng nhau có nghĩa là cấu trúc axit amin bằng nhau. 

Tại sao nó hoạt động: mọi hoạt động trong gen đều có tác động lên biểu hiện ngăn xếp giống hệt như tác động lên chuỗi protein thực. Sự khác biệt duy nhất là các hậu tố vô hạn không thay đổi được biểu diễn bằng một tập hợp hữu hạn các axit amin đơn giản tượng trưng. Vì một gen chỉ có thể truy cập vào một tiền tố bị chặn nên biểu diễn hữu hạn này chứa mọi thứ mà gen có thể quan sát được. Việc thực tập duy trì sự bình đẳng về cấu trúc, do đó so sánh cuối cùng phù hợp với định nghĩa về sự tương đương của gen. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

pairs = {}
left = []
right = []
simple_count = 25050

nodes = [None]

for i in range(simple_count):
    nodes.append((0, i))

def get_complex(a, b):
    key = (a, b)
    if key not in pairs:
        pairs[key] = len(nodes)
        nodes.append((1, a, b))
    return pairs[key]

def run_gene(gene):
    stack = list(range(simple_count, 0, -1))

    for ch in gene:
        if ch == 'C':
            stack.append(stack[-1])
        elif ch == 'D':
            stack.pop()
        elif ch == 'S':
            stack[-1], stack[-2] = stack[-2], stack[-1]
        elif ch == 'P':
            a = stack.pop()
            b = stack.pop()
            stack.append(get_complex(a, b))
        elif ch == 'L':
            a = stack[-1]
            if nodes[a][0] == 0:
                return None
            stack[-1] = nodes[a][1]
        elif ch == 'R':
            a = stack[-1]
            if nodes[a][0] == 0:
                return None
            stack[-1] = nodes[a][2]
        elif ch == 'U':
            a = stack.pop()
            if nodes[a][0] == 0:
                return None
            stack.append(nodes[a][2])
            stack.append(nodes[a][1])

    return stack

def solve():
    a = input().strip()
    b = input().strip()

    x = run_gene(a)
    y = run_gene(b)

    if x is None or y is None:
        print("True" if x is None and y is None else "False")
    else:
        print("True" if x == y else "False")

solve()
```Mảng nút lưu trữ biểu tượng đầy đủ của các axit amin. Một axit amin đơn giản được thể hiện bằng một loại nút và một chỉ số duy nhất. Một axit amin phức tạp lưu trữ các tham chiếu đến hai đứa con của nó. 

các`get_complex`chức năng là chi tiết triển khai chính. Nếu không thực tập, hai cây phức tạp giống hệt nhau được tạo thông qua các chuỗi thao tác khác nhau sẽ cần so sánh đệ quy. Thực tập chuyển đổi đẳng thức cấu trúc thành đẳng thức số nguyên. 

Ngăn xếp được khởi tạo theo thứ tự ngược lại vì phần cuối của danh sách là axit amin đầu tiên hiện tại. Điều này làm cho mọi thao tác trên mặt trước của protein trở thành thao tác trên`stack[-1]`. 

Việc xử lý lỗi được giới hạn ở`L`,`R`, Và`U`. Tình trạng lỗi độ dài chuỗi không thể xảy ra do chuỗi mô phỏng bắt đầu dài hơn bất kỳ gen nào có thể tiêu thụ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
PU
SS
```| gen | Hoạt động | Hiệu ứng ngăn xếp | 
| --- | --- | --- | 
| PU | P | Kết hợp hai axit amin đầu tiên thành`<a,b>`| 
| PU | Bạn | Tách ra`<a,b>`quay lại`a,b`| 
| SS | S | Hoán đổi hai axit amin đầu tiên | 
| SS | S | Hoán đổi chúng lại | 

Cả hai đều hoàn thành với ngăn xếp ban đầu, vì vậy câu trả lời là`True`. 

Đối với mẫu thứ hai:```
L
R
```| gen | Hoạt động | Kết quả | 
| --- | --- | --- | 
| L | Kiểm tra axit amin đầu tiên | Thật đơn giản, thất bại | 
| R | Kiểm tra axit amin đầu tiên | Thật đơn giản, thất bại | 

Cả hai phép biến đổi đều thất bại trên mọi đầu vào hợp lệ, vì vậy chúng tương đương nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cơ sở được xử lý một lần và mọi thao tác ngăn xếp đều có thời gian không đổi. | 
| Không gian | O(n) | Nhiều nhất là một số lượng tuyến tính các mục ngăn xếp và các nút phức tạp được tạo ra. | 

Tổng số thao tác được giới hạn bởi độ dài gen kết hợp là 10000, do đó mô phỏng tuyến tính dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("PU\nSS\n") == "True\n", "sample 1"
assert run("L\nR\n") == "True\n", "sample 2"
assert run("U\nC\n") == "False\n", "sample 3"
assert run("PL\nPR\n") == "False\n", "sample 4"

assert run("C\nC\n") == "True\n", "same duplication"
assert run("D\nS\n") == "False\n", "different stack changes"
assert run("LLLL\nRRRR\n") == "True\n", "both always fail"
assert run("P\nP\n") == "True\n", "same complex creation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`C`vs`C`| Đúng | Chuyển đổi ngăn xếp giống hệt nhau | 
|`D`vs`S`| Sai | Các hiệu ứng khác nhau trên tiền tố | 
|`LLLL`vs`RRRR`| Đúng | Thất bại tương đương | 
|`P`vs`P`| Đúng | Tạo và so sánh nút phức tạp | 

## Vỏ cạnh 

Trường hợp cả hai gen luôn bị lỗi sẽ được xử lý trước khi so sánh các ngăn xếp cuối cùng. Vì`L`Và`R`, ký hiệu đầu tiên của chuỗi luôn là một axit amin đơn giản, do đó trình mô phỏng sẽ trả về lỗi ngay lập tức. Việc so sánh xử lý chính xác hai lỗi tương đương. 

Trường hợp các cấu trúc bằng nhau được tạo thông qua các đường dẫn khác nhau sẽ được xử lý bằng bản đồ nội bộ. Một axit amin phức tạp không được so sánh bởi lịch sử tạo ra nó. Nó chỉ được đại diện bởi hai mã định danh con của nó, vì vậy hai cấu trúc giống hệt nhau luôn có chung mã định danh. 

Trường hợp sao chép làm tăng kích thước ngăn xếp được xử lý bằng cách sử dụng tham chiếu thay vì sao chép. Một chuỗi dài các`C`các hoạt động chỉ tạo ra nhiều tham chiếu hơn đến cùng một nút, do đó việc sử dụng bộ nhớ vẫn ở mức tuyến tính.
