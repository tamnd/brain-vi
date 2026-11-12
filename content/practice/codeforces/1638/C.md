---
title: "CF 1638C - Đồ thị nghịch đảo"
description: "Chúng ta được cấp một hoán vị có kích thước $n$, là một mảng chứa tất cả các số nguyên từ 1 đến $n$ đúng một lần theo một thứ tự nào đó. Từ hoán vị này, chúng ta xây dựng một đồ thị vô hướng trên các đỉnh $n$, trong đó mỗi đỉnh tương ứng với một vị trí trong hoán vị."
date: "2026-06-10T04:29:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 1638
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 771 (Div. 2)"
rating: 1300
weight: 1638
solve_time_s: 71
verified: true
draft: false
---

[CF 1638C - Đồ thị nghịch đảo](https://codeforces.com/problemset/problem/1638/C) 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, dsu, đồ thị, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, là một mảng chứa tất cả các số nguyên từ 1 đến$n$đúng một lần theo thứ tự nào đó. Từ hoán vị này, chúng ta xây dựng một đồ thị vô hướng trên$n$các đỉnh, trong đó mỗi đỉnh tương ứng với một vị trí trong hoán vị. Một cạnh tồn tại giữa hai đỉnh$i < j$khi và chỉ khi giá trị ở vị trí trước lớn hơn giá trị ở vị trí sau,$p_i > p_j$. Về cơ bản, mỗi phép đảo ngược trong hoán vị sẽ xác định một cạnh trong biểu đồ. 

Nhiệm vụ là xác định số lượng thành phần được kết nối trong biểu đồ này cho nhiều trường hợp thử nghiệm. Thành phần liên thông là tập con tối đa của các đỉnh sao cho mỗi cặp được kết nối trực tiếp hoặc gián tiếp bởi các cạnh. 

Những ràng buộc cho phép$n$lên tới$10^5$cho mỗi trường hợp thử nghiệm và tổng của$n$trên tất cả các trường hợp thử nghiệm lên đến$2 \cdot 10^5$. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp tuyến tính hoặc gần tuyến tính kém hơn sẽ quá chậm. Một cách tiếp cận đơn giản để kiểm tra mọi cặp đỉnh có thể có sẽ đạt tới$O(n^2)$hoạt động cho mỗi trường hợp thử nghiệm, điều này là không khả thi. 

Các trường hợp cạnh tinh tế bao gồm các hoán vị tăng nghiêm ngặt, trong đó không có cạnh nào tồn tại, do đó mỗi đỉnh tạo thành thành phần riêng của nó và giảm nghiêm ngặt các hoán vị, trong đó tất cả các đỉnh được kết nối trong một thành phần duy nhất. Các hoán vị xen kẽ giữa giá trị cao và thấp có thể tạo ra nhiều thành phần đan xen, đòi hỏi phải xử lý cẩn thận. Ví dụ, đối với$[2,1,4,3,5]$, các thành phần không rõ ràng ngay lập tức nếu không theo dõi mức tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là xây dựng biểu đồ đầy đủ một cách rõ ràng bằng cách lặp lại từng cặp$(i,j)$với$i<j$và kiểm tra xem$p_i>p_j$. Sau đó, chúng ta có thể chạy BFS hoặc DFS từ mỗi đỉnh chưa được thăm dò để đếm các thành phần được kết nối. Mặc dù đúng nhưng điều này sẽ yêu cầu$O(n^2)$hoạt động để xây dựng các cạnh cộng$O(n + e)$để đi qua đồ thị, có thể lên tới$O(n^2)$BẰNG$e$cũng có thể là$O(n^2)$. Điều này thất bại đối với$n=10^5$. 

Quan sát quan trọng là các kết nối trong biểu đồ chỉ được xác định bằng phép đảo ngược và chúng có thứ tự từ trái sang phải. Nói cách khác, bất kỳ đỉnh nào cũng được kết nối với tất cả các đỉnh ở bên phải của nó có giá trị nhỏ hơn và việc đóng bắc cầu hàm ý rằng các đoạn liên tiếp có các cực đại chồng chéo tạo thành các thành phần đơn lẻ. Chúng ta không cần lưu trữ các cạnh một cách rõ ràng. Thay vào đó, chúng ta có thể quét hoán vị từ trái sang phải trong khi theo dõi giá trị lớn nhất được thấy cho đến nay. Bất cứ khi nào chỉ số hiện tại bằng giá trị lớn nhất nhìn thấy cho đến nay, chúng ta có thể kết thúc một thành phần vì tất cả các đỉnh trong phân đoạn này được kết nối với nhau bằng phép đảo ngược, trực tiếp hoặc gián tiếp. 

Quan sát này làm giảm vấn đề thành một lần quét tuyến tính duy nhất của hoán vị, duy trì mức tối đa đang chạy và đếm các điểm mà mức tối đa bằng chỉ số hiện tại. Đây là$O(n)$cho mỗi trường hợp thử nghiệm, đủ hiệu quả cho các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng biểu đồ + BFS/DFS) | O(n^2) | O(n^2) | Quá chậm | 
| Quét tuyến tính với khả năng theo dõi tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho các thành phần,`components = 0`và một biến để theo dõi mức chạy tối đa,`current_max = 0`. 
2. Lặp lại hoán vị bằng chỉ mục`i`từ 1 đến`n`(Dựa trên 1 để rõ ràng hoặc dựa trên 0 trong mã có điều chỉnh). 
3. Cập nhật từng vị trí`current_max`là tối đa của`current_max`Và`p[i]`. Điều này theo dõi giá trị lớn nhất được thấy trong phân khúc hiện tại. 
4. Nếu chỉ số hiện tại`i`bằng`current_max`, chúng ta đã đến phần cuối của một thành phần được kết nối. Tăng`components`bằng 1. Lý do là tất cả các chỉ số trước đó trong đoạn này được kết nối thông qua chuỗi đảo ngược, do đó đỉnh hiện tại sẽ đóng đoạn đó. 
5. Sau khi quét toàn bộ hoán vị,`components`chứa số lượng thành phần được kết nối cho trường hợp thử nghiệm này. 
6. Lặp lại các bước trên cho từng trường hợp kiểm thử và xuất kết quả. 

Tại sao nó hoạt động: Theo định nghĩa, các cạnh tồn tại bất cứ khi nào giá trị lớn hơn đứng trước giá trị nhỏ hơn. Bất kỳ chuỗi chỉ số nào có giá trị lớn nhất chính xác là chỉ số ngoài cùng bên phải của chuỗi sẽ tạo thành một đoạn trong đó mọi đỉnh được kết nối trực tiếp hoặc gián tiếp. Mức tối đa đang chạy đảm bảo rằng chúng tôi nắm bắt được tất cả các kết nối gián tiếp gây ra bởi các đảo ngược chồng chéo. Mỗi khi mức tối đa đang chạy khớp với chỉ mục hiện tại, không có đỉnh tiếp theo nào có thể kết nối trở lại phân đoạn này, do đó nó tạo thành một thành phần riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    components = 0
    current_max = 0
    for i, val in enumerate(p, start=1):
        current_max = max(current_max, val)
        if current_max == i:
            components += 1
    print(components)
```Mã đọc số lượng trường hợp kiểm thử và sau đó lặp qua từng trường hợp kiểm thử. Đối với mỗi hoán vị, nó theo dõi mức chạy tối đa. Bất cứ khi nào vị trí hiện tại khớp với mức tối đa đang chạy, một thành phần sẽ kết thúc và bộ đếm tăng lên. Enumerate được sử dụng với`start=1`để căn chỉnh các chỉ số mảng với các giá trị hoán vị dựa trên 1 của bài toán. Điều này ngăn ngừa các lỗi xảy ra khi so sánh các chỉ số với các giá trị. 

## Ví dụ đã hoạt động 

**Ví dụ 1: [1,2,3]** 

| tôi | giá trị | hiện tại_max | current_max == tôi? | thành phần | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | vâng | 1 | 
| 2 | 2 | 2 | vâng | 2 | 
| 3 | 3 | 3 | vâng | 3 | 

Hoán vị tăng nghiêm ngặt tạo ra 3 thành phần, mỗi đỉnh một mình. 

**Ví dụ 2: [6,1,4,2,5,3]** 

| tôi | giá trị | hiện tại_max | current_max == tôi? | thành phần | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 6 | không | 0 | 
| 2 | 1 | 6 | không | 0 | 
| 3 | 4 | 6 | không | 0 | 
| 4 | 2 | 6 | không | 0 | 
| 5 | 5 | 6 | không | 0 | 
| 6 | 3 | 6 | vâng | 1 | 

Tất cả các đỉnh được kết nối, tạo thành một thành phần duy nhất. Điều này xác nhận rằng phân đoạn dựa trên mức tối đa xác định chính xác tất cả các kết nối gián tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trên mỗi trường hợp thử nghiệm, O(tổng n) tổng thể | Mỗi phần tử được truy cập chính xác một lần, cập nhật max và kiểm tra điều kiện | 
| Không gian | O(1) thêm | Chỉ cần một bộ đếm và một biến max; hoán vị được đọc tại chỗ | 

Với ràng buộc rằng tổng số tiền`n`trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, giải pháp này chạy theo thời gian tuyến tính và phù hợp thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        components = 0
        current_max = 0
        for i, val in enumerate(p, start=1):
            current_max = max(current_max, val)
            if current_max == i:
                components += 1
        print(components)
    return out.getvalue().strip()

# provided samples
assert run("6\n3\n1 2 3\n5\n2 1 4 3 5\n6\n6 1 4 2 5 3\n1\n1\n6\n3 2 1 6 5 4\n5\n3 1 5 2 4") == "3\n3\n1\n1\n2\n1"

# custom cases
assert run("2\n1\n1\n2\n2 1") == "1\n1", "minimum size and two-element reverse"
assert run("1\n5\n5 4 3 2 1") == "1", "strictly decreasing"
assert run("1\n5\n1 3 2 5 4") == "3", "interleaving"
assert run("1\n4\n1 4 2 3") == "2", "mixed order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | đỉnh đơn tạo thành một thành phần | 
| 2 yếu tố đảo ngược | 1 | hai đỉnh tạo thành một thành phần | 
| 5 yếu tố giảm dần | 1 | thành phần đơn đầy đủ | 
| 5 yếu tố đan xen | 3 | nhiều thành phần đan xen | 
| 4 yếu tố trộn lẫn | 2 | phân đoạn một phần được xử lý | 

## Vỏ cạnh 

Để tăng hoán vị nghiêm ngặt như`[1,2,3,4]`, thuật toán xác định chính xác mỗi đỉnh là thành phần riêng của nó. Tại mỗi chỉ số`i`,`current_max`bằng`i`, Vì thế`components`tăng mỗi bước, cho 4 thành phần. 

Đối với các hoán vị giảm nghiêm ngặt như `[5,4,
