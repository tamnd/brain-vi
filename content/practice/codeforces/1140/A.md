---
title: "CF 1140A - Sách trinh thám"
description: "Cuốn sách trinh thám của Ivan được cấu trúc sao cho mỗi trang giới thiệu một bí ẩn và lời giải cho bí ẩn đó sẽ được tiết lộ ở trang sau. Cụ thể, chúng ta có một danh sách các số nguyên trong đó số nguyên $i$-th $ai$ cho chúng ta biết trang giải quyết bí ẩn được giới thiệu trên trang $i$."
date: "2026-06-12T03:45:51+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1140
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 62 (Rated for Div. 2)"
rating: 1000
weight: 1140
solve_time_s: 81
verified: true
draft: false
---

[CF 1140A - Sách trinh thám](https://codeforces.com/problemset/problem/1140/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cuốn sách trinh thám của Ivan được cấu trúc sao cho mỗi trang giới thiệu một bí ẩn và lời giải cho bí ẩn đó sẽ được tiết lộ ở trang sau. Cụ thể, chúng ta có một danh sách các số nguyên trong đó$i$-số nguyên thứ$a_i$cho chúng tôi biết trang giải quyết bí ẩn được giới thiệu trên trang$i$. Ivan đọc tuần tự, bắt đầu từ trang đầu tiên chưa đọc mỗi ngày, nhưng anh không thể dừng lại giữa một “chuỗi bí ẩn”. Anh đọc cho đến khi mọi bí ẩn anh gặp phải đều được giải đáp. Nhiệm vụ là xác định xem anh ta phải mất bao nhiêu ngày để đọc xong cuốn sách. 

Đầu vào đủ nhỏ để$n$có thể đi lên$10^4$, do đó thuật toán lên đến$O(n)$hoặc$O(n \log n)$sẽ chạy thoải mái trong giới hạn thời gian 2 giây. Các thuật toán bậc hai duyệt toàn bộ cuốn sách nhiều lần có thể đạt tới$10^8$hoạt động, có nguy cơ bị hết thời gian. 

Trường hợp cạnh có thể tinh tế. Ví dụ: nếu tất cả những điều bí ẩn được giải thích ngay lập tức trên cùng một trang thì Ivan sẽ đọc một trang mỗi ngày. Vì$n=5$với$a = [1,2,3,4,5]$, mỗi ngày là một trang, tạo ra 5 ngày. Một cách tiếp cận bất cẩn cho rằng Ivan luôn đọc nhiều trang mỗi ngày sẽ tạo ra câu trả lời sai ở đây. Một trường hợp phức tạp khác là khi trang cuối cùng giải thích tất cả những bí ẩn trước đó, ví dụ:$a = [5,5,5,5,5]$; Ivan phải đọc hết năm trang trong một ngày. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng cách đọc của Ivan một cách rõ ràng. Bắt đầu từ trang 1 và tiếp tục đọc tuần tự, chỉ dừng lại khi mọi bí ẩn gặp phải đều được giải quyết. Đếm mỗi điểm dừng là một ngày mới. Phương pháp này đúng vì nó tuân thủ trực tiếp các quy tắc đọc, nhưng nó có thể không hiệu quả nếu được thực hiện một cách ngây thơ bằng cách kiểm tra nhiều lần để tìm ra những bí ẩn chưa được giải quyết. Độ phức tạp trong trường hợp xấu nhất có thể đạt tới$O(n^2)$khi mỗi trang tham chiếu đến một trang ở xa, vì mỗi ngày có thể yêu cầu quét từ đầu các trang chưa đọc để kiểm tra tất cả những bí ẩn trước đó. 

Quan sát quan trọng để có cách tiếp cận tối ưu là trong mỗi ngày, Ivan sẽ đọc từ trang đầu tiên chưa đọc cho đến trang xa nhất mà bất kỳ bí ẩn nào trong ngày đó yêu cầu. Về mặt hình thức, trong khi duyệt qua các trang theo thứ tự, hãy duy trì trang xa nhất$max_page$cần được đọc để giải đáp mọi bí ẩn gặp phải cho đến nay. Ivan dừng khi chỉ số trang hiện tại vượt quá$max_page$. Tăng số ngày, sau đó tiếp tục từ trang chưa đọc tiếp theo. Điều này có tác dụng vì điều kiện dừng chính xác là khi tất cả những điều bí ẩn trong đoạn liền kề hiện tại được giải quyết. Điều này làm giảm thuật toán thành một đường truyền tuyến tính duy nhất qua các trang, mang lại$O(n)$sự phức tạp về mặt thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(1) | Quá chậm đối với n lớn | 
| Theo dõi trang tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`days`về 0. Điều này sẽ đếm xem Ivan có bao nhiêu buổi đọc. 
2. Đặt con trỏ`i`ở mức 0, biểu thị chỉ mục trang chưa đọc hiện tại. 
3. Trong khi`i < n`: 

1. Khởi tạo`max_page`ĐẾN`a[i] - 1`bởi vì chúng tôi cần lập chỉ mục dựa trên số không. 
2. Khởi tạo con trỏ`j = i`để theo dõi các trang trong ngày hiện tại. 
3. Trong khi`j <= max_page`: 

1. Cập nhật`max_page = max(max_page, a[j] - 1)`. Điều này đảm bảo Ivan tiếp tục đọc cho đến khi mọi bí ẩn của ngày hiện tại được giải quyết. 
2. Tăng`j`bằng 1. 
4. Tăng`days`lên 1 kể từ khi Ivan đọc xong một ngày. 
5. Di chuyển`i`ĐẾN`j`, trang chưa đọc tiếp theo. 
4. In`days`. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến`max_page`luôn là trang xa nhất mà Ivan phải đọc để giải đáp mọi bí ẩn mà anh đã gặp phải cho đến nay. Bằng cách mở rộng`max_page`bất cứ khi nào trang sau yêu cầu đọc thêm, thuật toán sẽ đảm bảo không còn bí ẩn nào chưa được giải quyết vào cuối ngày. Mỗi trang được truy cập đúng một lần, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

days = 0
i = 0
while i < n:
    max_page = a[i] - 1
    j = i
    while j <= max_page:
        max_page = max(max_page, a[j] - 1)
        j += 1
    days += 1
    i = j

print(days)
```Vòng lặp bên ngoài bắt đầu ngày hôm sau, trong khi vòng lặp bên trong kéo dài đến bí ẩn xa nhất chưa được giải đáp. Những điểm tinh tế bao gồm việc điều chỉnh`a[i]`để lập chỉ mục và cập nhật dựa trên số không`max_page`trước khi tăng`j`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
9
1 3 3 6 7 6 8 8 9
```| tôi | j | max_page | Hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | đọc trang 1, max_page = 0, dừng ngày, ngày=1 | 
| 1 | 1 | 2 | đọc trang 2,3; max_page cập nhật lên 2; j=3, ngày dừng, ngày=2 | 
| 3 | 3 | 7 | đọc trang 4-8, max_page kéo dài đến 7; j=8, ngày dừng, ngày=3 | 
| 8 | 8 | 8 | đọc trang 9, max_page=8, j=9, ngày dừng, ngày=4 | 

Dấu vết cho thấy thuật toán mở rộng khả năng đọc một cách chính xác để bao gồm tất cả những bí ẩn chưa được giải quyết mỗi ngày. 

**Đầu vào vỏ cạnh:**```
5
5 5 5 5 5
```| tôi | j | max_page | Hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 4 | đọc trang 1-5, mở rộng max_page lên 4, j=5, dừng ngày, ngày=1 | 

Tất cả các trang được đọc trong một ngày vì trang cuối cùng giải quyết mọi bí ẩn. Thuật toán nắm bắt chính xác điều này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trang được xử lý chính xác một lần. Vòng lặp while bên trong chỉ tiến tới j, không bao giờ xem lại các trang. | 
| Không gian | O(n) | Lưu trữ mảng`a`. Các biến bổ sung sử dụng không gian O(1). | 

Với n tối đa 10^4, các thao tác O(n) đều nằm trong giới hạn 2 giây và mức sử dụng bộ nhớ vẫn ở mức dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    days = 0
    i = 0
    while i < n:
        max_page = a[i] - 1
        j = i
        while j <= max_page:
            max_page = max(max_page, a[j] - 1)
            j += 1
        days += 1
        i = j
    return str(days)

# Provided sample
assert run("9\n1 3 3 6 7 6 8 8 9\n") == "4", "sample 1"

# Minimum input
assert run("1\n1\n") == "1", "single page"

# All explained immediately
assert run("5\n1 2 3 4 5\n") == "5", "all pages self-explained"

# All explained at the last page
assert run("5\n5 5 5 5 5\n") == "1", "all explained on last page"

# Sequential chunks
assert run("6\n2 2 4 4 6 6\n") == "3", "two-page chains"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | sách một trang | 
| 1 2 3 4 5 | 5 | mỗi trang giải thích chính nó, một trang mỗi ngày | 
| 5 5 5 5 5 | 1 | trang cuối giải thích tất cả, một ngày nào đó | 
| 2 2 4 4 6 6 | 3 | chuỗi nhiều trang được đếm chính xác | 

## Vỏ cạnh 

Đối với trường hợp tất cả bí ẩn được giải quyết trên cùng một trang, thuật toán xác định chính xác rằng việc đọc phải tiếp tục cho đến trang đó. Với`a = [5,5,5,5,5]`,`i=0`,`j=0`, Và`max_page=4`. Vòng lặp bên trong lặp lại j từ 0 đến 4, đọc tất cả các trang trong một ngày. Bất biến`j <= max_page`đảm bảo không có ngày nào kết thúc sớm. Tương tự, đối với các trang tự giải thích, thuật toán xử lý số ngày có một trang bằng cách đặt`max_page = a[i]-1`, thế là ngày kết thúc ngay lập tức. Những dấu vết này xác nhận thuật toán xử lý cả hai thái cực một cách chính xác.
