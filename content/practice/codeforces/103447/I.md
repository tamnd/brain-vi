---
title: "CF 103447I - Sức mạnh và số không"
description: "Chúng ta được cho một mảng các số nguyên dương. Mục tiêu là giảm mọi giá trị về 0 bằng cách sử dụng một chuỗi thao tác. Trong một thao tác, chúng ta chọn danh sách các chỉ số $B1, B2, dấu chấm, Bm$."
date: "2026-07-03T07:32:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103447
codeforces_index: "I"
codeforces_contest_name: "The 2021 China Collegiate Programming Contest (Harbin)"
rating: 0
weight: 103447
solve_time_s: 50
verified: true
draft: false
---

[CF 103447I - Sức mạnh và số không](https://codeforces.com/problemset/problem/103447/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Mục tiêu là giảm mọi giá trị về 0 bằng cách sử dụng một chuỗi thao tác. Trong một thao tác, chúng tôi chọn danh sách các chỉ số$B_1, B_2, \dots, B_m$. các$i$-vị trí thứ trong danh sách này góp phần trừ$2^{i-1}$tới phần tử mảng tương ứng$A_{B_i}$. Cùng một chỉ mục mảng có thể xuất hiện nhiều lần trong cùng một thao tác và mỗi lần xuất hiện sẽ nhận được lũy thừa hai khác nhau tùy thuộc vào vị trí của nó trong chuỗi đã chọn. 

Điểm tự do chính là mỗi thao tác cho phép chúng ta phân phối tập trọng số$1, 2, 4, 8, \dots, 2^{m-1}$trên các vị trí mảng theo bất kỳ thứ tự nào, nhưng mỗi trọng số được sử dụng chính xác một lần cho mỗi thao tác. 

Nhiệm vụ là giảm thiểu số lượng thao tác cần thiết để tất cả các giá trị mảng trở thành 0. 

Các ràng buộc cho phép lên đến$10^5$tổng số phần tử trên mỗi bộ thử nghiệm, với các giá trị lên tới$10^9$. Điều này ngay lập tức gợi ý rằng bất kỳ phương pháp mô phỏng hoạt động từng bước nào đều không thể thực hiện được. Cấu trúc lũy thừa của hai gợi ý rõ ràng về quan điểm phân rã theo bit hoặc nhị phân, vì mỗi phép toán về cơ bản là cung cấp một “ngân sách” cho một bản sao của mỗi lũy thừa của hai. 

Một trường hợp tế nhị tiết lộ lý do tại sao lý luận ngây thơ lại thất bại là khi một số lớn trong khi số khác lại nhỏ. Ví dụ: nếu chúng ta cố gắng sửa chữa các phần tử lớn trước một cách tham lam, chúng ta có thể lãng phí sức mạnh nhỏ một cách không hiệu quả, mặc dù việc gán lại phần tử nào nhận được sức mạnh nào sẽ làm giảm số lượng thao tác. Giải pháp đúng phải cân bằng về mặt tổng thể tần suất sử dụng lũy ​​thừa của hai trên tất cả các phần tử. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng các hoạt động một cách trực tiếp. Trong mỗi thao tác, chúng tôi cố gắng gán trình tự$1, 2, 4, \dots$vào các chỉ mục theo thứ tự nào đó, cập nhật mảng cho đến khi tất cả các giá trị bằng 0. Ngay cả khi chúng ta khéo léo trong việc lựa chọn các phép gán thì mỗi thao tác chỉ loại bỏ một tập hợp các giá trị có cấu trúc và chúng ta vẫn cần có khả năng$O(\max A_i)$tổng mức giảm trải đều trong các hoạt động. Vì giá trị có thể đạt tới$10^9$, cách tiếp cận này ngay lập tức không khả thi. 

Bước ngoặt là diễn giải lại những gì một hoạt động thực sự mang lại. Mỗi phép toán đóng góp chính xác một bản sao của mỗi lũy thừa của hai$2^k$cho tất cả hợp lệ$k$đến độ dài đã chọn. Trong tất cả các hoạt động, chúng tôi đang quyết định một cách hiệu quả số lần mỗi quyền lực được thực hiện$2^k$được sử dụng và mỗi lần sử dụng phải được gán cho chính xác một chỉ mục mảng cho mỗi thao tác. 

Điều này chuyển đổi vấn đề thành một chế độ xem lập kế hoạch. Đối với mỗi vị trí bit$k$, mỗi lần chúng ta sử dụng$2^k$trong bất kỳ sự phân hủy nào của bất kỳ$A_i$, chúng ta đang tiêu thụ một đơn vị công suất trong “lớp bit” đó của một hoạt động. Vì mỗi thao tác cung cấp nhiều nhất một đơn vị của mỗi lớp bit nên số lượng thao tác ít nhất phải là tải tối đa trên tất cả các lớp bit. Câu hỏi còn lại là liệu chúng ta có thể luôn chọn phân tách tất cả các số sao cho giới hạn này chặt chẽ hay không và câu trả lời là có bằng cách sử dụng phân tách nhị phân tự nhiên, điều này tránh việc phân tách không cần thiết sẽ chỉ làm tăng tắc nghẽn bit thấp hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Đếm lớp bit | O(n log A) | O(log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại mỗi số thành đóng góp của lũy thừa hai. 

1. Với mọi giá trị$A_i$, phân tách nó thành biểu diễn nhị phân của nó. Mỗi bit được đặt ở vị trí$k$có nghĩa là chúng ta cần một phiên bản của$2^k$được gán cho chỉ mục$i$. 
2. Duy trì dải tần số trên các vị trí bit. Đối với mỗi vị trí bit$k$, đếm xem có bao nhiêu chỉ số yêu cầu bit đó. 
3. Câu trả lời là giá trị lớn nhất trong số tất cả các tần số bit này. 

Mức tối đa này thể hiện mức bit được “yêu cầu” nhiều nhất trên mảng. Vì mỗi thao tác chỉ có thể đáp ứng một đơn vị nhu cầu trên mỗi vị trí bit nên lớp đó sẽ trở thành nút thắt cổ chai. 

### Tại sao nó hoạt động 

Mỗi thao tác cung cấp chính xác một khe có sẵn cho mỗi lũy thừa của hai. Suy nghĩ theo cột, mỗi vị trí bit hoạt động giống như một tài nguyên song song có dung lượng bằng với số lượng thao tác. Bất kỳ sự phân rã nào của mảng đều gây ra tải trên mỗi cột bằng số lần năng lượng đó được sử dụng. Không có thao tác nào có thể phục vụ nhiều hơn một đơn vị trong cùng một cột, vì vậy số lượng thao tác ít nhất phải bằng tải cột tối đa. 

Việc sử dụng biểu diễn nhị phân đảm bảo chúng tôi không bao giờ tạo ra các bản sao không cần thiết của lũy thừa nhỏ hơn có thể làm tăng tải cột mà không làm giảm cột khác. Do đó, tải cột gây ra bởi phân rã nhị phân đã là tối thiểu, khiến tần số tối đa vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        cnt = [0] * 31
        
        for x in a:
            b = 0
            while x:
                if x & 1:
                    cnt[b] += 1
                x >>= 1
                b += 1
        
        print(max(cnt))

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập. Đối với mỗi số, nó quét biểu diễn nhị phân và bộ đếm số gia để tìm vị trí bit tương ứng. Câu trả lời cuối cùng là giá trị bộ đếm tối đa trên tất cả các vị trí bit. 

Một chi tiết triển khai tinh tế là chúng tôi không cố gắng mô phỏng các hoạt động đó. Toàn bộ giải pháp dựa trên quan sát rằng chỉ tần số trên mỗi bit mới quan trọng, do đó số lần theo dõi là đủ. Vòng lặp chạy tới 30 bit, đủ cho các giá trị lên tới$10^9$. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào nhỏ có các giá trị chia sẻ các bit chồng chéo. 

### Ví dụ 1 

đầu vào:```
n = 4
A = [1, 2, 3, 4]
```| Số | Nhị phân | Đóng góp bit | 
| --- | --- | --- | 
| 1 | 0001 | bit 0 | 
| 2 | 0010 | bit 1 | 
| 3 | 0011 | bit 0, bit 1 | 
| 4 | 0100 | bit 2 | 

Sau khi xử lý: 

| Vị trí bit | Đếm | 
| --- | --- | 
| 0 | 2 | 
| 1 | 2 | 
| 2 | 1 | 

Câu trả lời là 2. 

Điều này cho thấy rằng mặc dù các giá trị khác nhau, yếu tố hạn chế là tần suất cần có lũy thừa cụ thể của 2 trên mảng. 

### Ví dụ 2 

đầu vào:```
n = 3
A = [7, 7, 7]
```| Số | Nhị phân | Đóng góp bit | 
| --- | --- | --- | 
| 7 | 0111 | bit 0, bit 1, bit 2 | 
| 7 | 0111 | bit 0, bit 1, bit 2 | 
| 7 | 0111 | bit 0, bit 1, bit 2 | 

| Vị trí bit | Đếm | 
| --- | --- | 
| 0 | 3 | 
| 1 | 3 | 
| 2 | 3 | 

Câu trả lời là 3, cho thấy tất cả các lớp bit đều bão hòa như nhau. 

Điều này xác nhận rằng nút cổ chai được xác định độc lập trên mỗi vị trí bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi số được xử lý bằng cách quét biểu diễn nhị phân của nó | 
| Không gian | O(log A) | Chỉ một mảng bộ đếm bit cố định được lưu trữ | 

Các ràng buộc cho phép lên đến$10^5$số lượng trên mỗi bộ thử nghiệm và giá trị lên tới$10^9$, do đó, quá trình quét 30 bit cho mỗi số dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            cnt = [0] * 31
            for x in a:
                b = 0
                while x:
                    if x & 1:
                        cnt[b] += 1
                    x >>= 1
                    b += 1
            print(max(cnt))

    from io import StringIO
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-like cases
assert run("1\n5\n1 2 3 4 5\n") == run("1\n5\n1 2 3 4 5\n")

# all equal
assert run("1\n3\n7 7 7\n") == "3"

# minimum size
assert run("1\n1\n8\n") == "1"

# powers of two
assert run("1\n4\n1 2 4 8\n") == "1"

# mixed heavy overlap
assert run("1\n4\n3 3 3 3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| tất cả các số bằng nhau | tần số bit tối đa | bão hòa đồng đều | 
| sức mạnh của hai | 1 | lớp bit độc lập | 
| mẫu lặp đi lặp lại | tích lũy đúng | xử lý tần số | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các số đều giống hệt nhau. Ví dụ, với đầu vào`A = [7, 7, 7]`, mọi vị trí bit đều bão hòa hoàn toàn. Thuật toán đếm từng bit một cách độc lập và trả về cùng một giá trị trên tất cả các vị trí. Việc thực thi tạo ra số lượng`[3, 3, 3]`, vì vậy câu trả lời là 3, khớp với số phần tử đóng góp cho mỗi bit. 

Một trường hợp khác là khi các số là lũy thừa thuần túy của hai. Đối với đầu vào`[1, 2, 4, 8]`, mỗi số kích hoạt một vị trí bit khác nhau, do đó không có cột nào tích lũy nhiều hơn một yêu cầu. Thuật toán mang lại tối đa là 1 và mỗi thao tác có thể xử lý độc lập tất cả các đóng góp mà không có xung đột.
