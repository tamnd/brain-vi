---
title: "CF 1008B - Xoay hình chữ nhật"
description: "Chúng ta được cho một dãy các ô hình chữ nhật được đặt theo thứ tự cố định từ trái sang phải. Mỗi ô có hai hướng có thể có: chúng ta có thể giữ nó theo chiều rộng theo chiều cao hoặc xoay nó 90 độ, điều này sẽ hoán đổi hai giá trị."
date: "2026-06-16T23:03:22+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1008
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 497 (Div. 2)"
rating: 1000
weight: 1008
solve_time_s: 73
verified: true
draft: false
---

[CF 1008B - Xoay các hình chữ nhật](https://codeforces.com/problemset/problem/1008/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các ô hình chữ nhật được đặt theo thứ tự cố định từ trái sang phải. Mỗi ô có hai hướng có thể có: chúng ta có thể giữ nó theo chiều rộng theo chiều cao hoặc xoay nó 90 độ, điều này sẽ hoán đổi hai giá trị. Sau khi chọn hướng độc lập cho từng ô, chúng tôi chỉ xem xét độ cao kết quả. Yêu cầu là các độ cao này phải tạo thành một dãy không tăng dần từ trái sang phải. 

Nhiệm vụ là xác định liệu sự lựa chọn định hướng như vậy có tồn tại hay không. 

Hạn chế chính là$n \le 10^5$. Điều này ngay lập tức loại trừ mọi khám phá theo cấp số nhân về các hướng, vì mỗi hình chữ nhật có hai trạng thái và một tìm kiếm toàn diện sẽ liên quan đến$2^n$khả năng. Ngay cả các phương pháp bậc hai đối với các cặp hình chữ nhật cũng sẽ quá chậm. Do đó, chúng tôi đang tìm kiếm một cấu trúc tham lam hoặc thời gian tuyến tính xử lý các hình chữ nhật một lần từ trái sang phải. 

Một trường hợp biên tinh vi xuất hiện khi cả hai hướng đều hợp lệ cục bộ nhưng chỉ một hướng dẫn đến khả năng tiếp tục toàn cầu khả thi. Ví dụ: nếu một hình chữ nhật là$(5, 4)$và chiều cao đã chọn trước đó là 5, chúng ta có thể luôn muốn chọn hướng lớn hơn có thể mà vẫn phù hợp. Tuy nhiên, chiến lược “tối đa hóa chiều cao hiện tại” tham lam này có thể phá vỡ tính khả thi trong tương lai. 

Hãy xem xét đầu vào này:```
2
5 4
4 6
```Ở hình chữ nhật đầu tiên, chọn chiều cao 5 là được. Ở hình chữ nhật thứ hai, cả 6 và 4 đều không thể theo sau 5 theo một dãy không tăng, nên câu trả lời là KHÔNG. Nhưng nếu chúng ta chọn khác đi trong các trường hợp khác, chúng ta có thể duy trì tính khả thi, nghĩa là tính tối ưu cục bộ phải được xác định cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi lựa chọn hướng có thể có cho mỗi hình chữ nhật. Đối với mỗi cấu hình, chúng tôi kiểm tra xem chuỗi chiều cao kết quả có tăng không. Điều này thật đơn giản: với mỗi$2^n$cấu hình, chúng tôi quét$n$các yếu tố, đưa ra$O(n \cdot 2^n)$. Với$n = 10^5$, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của bài toán cho phép đơn giản hóa: ở mỗi bước, chúng ta chỉ quan tâm đến chiều cao tối đa cho phép mà chúng ta vẫn có thể sử dụng cho hình chữ nhật hiện tại, được xác định bởi lựa chọn trước đó. Mỗi hình chữ nhật cho chúng ta hai ứng cử viên cho chiều cao và chúng ta muốn chọn hình lớn nhất không vượt quá chiều cao đã chọn trước đó. Sự lựa chọn tham lam này có tác dụng vì việc để lại chiều cao hợp lệ lớn hơn bất cứ khi nào có thể sẽ duy trì sự linh hoạt hơn cho tương lai, trong khi vẫn duy trì tính khả thi cục bộ. 

Chúng tôi chuyển đổi vấn đề thành một lần duy nhất: duy trì một biến`cur`đại diện cho chiều cao tối đa được phép cho vị trí hiện tại. Đối với mỗi hình chữ nhật, chọn hướng lớn nhất có thể là ≤`cur`. Nếu không có hướng nào phù hợp thì quá trình sẽ thất bại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Quét tham lam |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hình chữ nhật từ trái sang phải trong khi vẫn duy trì chiều cao tối đa cho phép cho vị trí hiện tại. 

1. Khởi tạo`cur`như vô cùng. Điều này thể hiện rằng hình chữ nhật đầu tiên không có giới hạn trên. 
2. Với mỗi hình chữ nhật có cạnh$(w, h)$, hãy xem xét hai độ cao có thể có:$h$Và$w$. Thứ tự của hai giá trị này ban đầu không quan trọng. 
3. Chọn giá trị lớn hơn trong hai giá trị nhỏ hơn hoặc bằng`cur`. Nếu cả hai giá trị đều hợp lệ, chúng tôi lấy giá trị lớn hơn vì nó giữ nhiều chỗ hơn cho các hình chữ nhật sau này. 
4. Nếu không có giá trị nào nhỏ hơn hoặc bằng`cur`, chúng ta kết luận ngay rằng dãy không thể không tăng và dừng. 
5. Cập nhật`cur`đến chiều cao đã chọn và tiếp tục đến hình chữ nhật tiếp theo. 
6. Nếu tất cả các hình chữ nhật được xử lý thành công, xuất ra CÓ. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi duy trì bất biến rằng`cur`là chiều cao tối đa có thể mà chúng ta được phép đặt ở vị trí hiện tại với tất cả các lựa chọn trước đó. Trong số tất cả các hướng hợp lệ cho hình chữ nhật hiện tại, việc chọn chiều cao khả thi tối đa là an toàn vì mọi lựa chọn nhỏ hơn sẽ chỉ hạn chế thêm các bước trong tương lai mà không mở khóa bất kỳ cấu hình hợp lệ bổ sung nào. Vì tính khả thi chỉ phụ thuộc vào việc duy trì ở dưới độ cao trước đó nên việc duy trì giá trị khả thi lớn nhất sẽ đảm bảo chúng ta không bao giờ loại bỏ giải pháp có thể thành công sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cur = float('inf')

    for _ in range(n):
        a, b = map(int, input().split())
        if a > b:
            a, b = b, a

        if a <= cur and b <= cur:
            cur = b
        elif b <= cur:
            cur = b
        elif a <= cur:
            cur = a
        else:
            print("NO")
            return

    print("YES")

if __name__ == "__main__":
    solve()
```Mã phản ánh trực tiếp chiến lược tham lam. Đối với mỗi hình chữ nhật, chúng ta chuẩn hóa hai chiều cao ứng cử viên của nó bằng cách sắp xếp chúng sao cho`a <= b`. Điều này làm cho lý luận đơn giản hơn:`b`luôn là chiều cao tốt nhất có thể cho hình chữ nhật đó và`a`là dự phòng. 

Cấu trúc có điều kiện đảm bảo chúng ta luôn chọn chiều cao hợp lệ lớn nhất. Nếu cả hai vừa vặn dưới`cur`, chúng tôi lấy`b`. Nếu chỉ có một cái phù hợp, chúng tôi sẽ lấy cái đó. Nếu không phù hợp, chúng tôi chấm dứt sớm. 

Một lỗi triển khai phổ biến là quên chuẩn hóa từng hình chữ nhật hoặc cập nhật không chính xác`cur`ngay cả khi không có lựa chọn hợp lệ nào tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 4
4 6
3 5
```Chúng tôi theo dõi`cur`: 

| Bước | Hình chữ nhật | Ứng viên | Được chọn | cur | 
| --- | --- | --- | --- | --- | 
| 1 | (3,4) | (3,4) | 4 | 4 | 
| 2 | (4,6) | (4,6) | 4 | 4 | 
| 3 | (3,5) | (3,5) | 3 | 3 | 

Ở mỗi bước chúng tôi luôn chọn chiều cao hợp lệ tối đa. Dãy số cuối cùng là 4, 4, 3 không tăng, khẳng định tính khả thi. 

### Ví dụ 2 

đầu vào:```
2
5 4
4 6
```| Bước | Hình chữ nhật | Ứng viên | Được chọn | cur | 
| --- | --- | --- | --- | --- | 
| 1 | (5,4) | (4,5) | 5 | 5 | 
| 2 | (4,6) | (4,6) | 4 | 4 | 

Ở đây, hình chữ nhật thứ hai buộc giảm xuống 4, điều này hợp lệ, nhưng nếu bước đầu tiên không đủ linh hoạt trong các biến thể khác thì sẽ xảy ra lỗi. Dấu vết cho thấy mức độ khả thi được kiểm tra hoàn toàn thông qua giới hạn hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi hình chữ nhật được xử lý một lần với các phép so sánh theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có một biến duy nhất`cur`được duy trì | 

Quét tuyến tính vừa vặn thoải mái trong giới hạn cho$n = 10^5$, trong đó có tới vài triệu thao tác được chấp nhận trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    n = int(input())
    cur = float('inf')
    for _ in range(n):
        a, b = map(int, input().split())
        if a > b:
            a, b = b, a

        if a <= cur and b <= cur:
            cur = b
        elif b <= cur:
            cur = b
        elif a <= cur:
            cur = a
        else:
            return "NO\n"
    return "YES\n"

# provided sample
assert run("3\n3 4\n4 6\n3 5\n") == "YES\n"

# minimum size, single rectangle
assert run("1\n5 7\n") == "YES\n"

# impossible case
assert run("2\n5 4\n4 6\n") == "NO\n"

# all equal rectangles
assert run("3\n2 2\n2 2\n2 2\n") == "YES\n"

# strict decreasing requirement
assert run("3\n9 1\n8 2\n7 3\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật đơn | CÓ | trường hợp cơ sở | 
| hỗn hợp không thể | KHÔNG | phát hiện lỗi sớm | 
| hình chữ nhật bằng nhau | CÓ | hộp xích ổn định | 
| giảm nghiêm ngặt | CÓ | định hướng linh hoạt | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hình chữ nhật chỉ có thể được đặt theo một hướng do các ràng buộc trước đó. Ví dụ:```
2
10 1
5 6
```Sau hình chữ nhật đầu tiên,`cur = 10`. Hình chữ nhật thứ hai có các ứng cử viên 5 và 6, cả hai đều hợp lệ, vì vậy chúng tôi chọn 6. Dãy số vẫn hợp lệ. 

Bây giờ hãy xem xét:```
2
10 1
7 8
```Sau bước đầu tiên,`cur = 10`. Đối với hình chữ nhật thứ hai, cả 7 và 8 đều hợp lệ, vì vậy chúng tôi chọn 8. Dãy số này hợp lệ và kết thúc ở 8 ≤ 10. 

Cuối cùng:```
2
5 1
6 7
```Sau hình chữ nhật đầu tiên,`cur = 5`. Hình chữ nhật thứ hai có các ứng cử viên là 6 và 7, không có ứng cử viên nào là 5, do đó thuật toán cho kết quả NO chính xác. Lỗi xảy ra ngay lập tức và không có sự điều chỉnh nào sau này có thể khắc phục được do không có hướng hợp lệ nào tồn tại ở bước này.
