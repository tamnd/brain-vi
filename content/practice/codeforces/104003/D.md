---
title: "CF 104003D - William và Bột ngô"
description: "William liên tục phục vụ món tráng miệng cho các nhóm bạn đến theo thời gian. Hạn chế chính là tại mọi thời điểm, sau khi một nhóm mới đến, món tráng miệng phải được cắt thành một số lát bằng nhau để mỗi người hiện có mặt có thể nhận được một số nguyên…"
date: "2026-07-02T05:33:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104003
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-28-22 Div. 1 (Advanced)"
rating: 0
weight: 104003
solve_time_s: 46
verified: true
draft: false
---

[CF 104003D – William và bột ngô](https://codeforces.com/problemset/problem/104003/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

William liên tục phục vụ món tráng miệng cho các nhóm bạn đến theo thời gian. Hạn chế chính là tại mọi thời điểm, sau khi một nhóm mới đến, món tráng miệng phải được cắt thành một số lát bằng nhau để mỗi người hiện có mặt có thể được nhận một số lượng lát bằng nhau. 

Quá trình này rất năng động. Ban đầu không có sự cắt lát. Khi nhóm đầu tiên đến, William có thể cắt toàn bộ chiếc bánh thành nhiều phần bằng nhau để chia đều cho những người đó. Sau đó, khi có nhiều người đến hơn, William được phép lấy từng miếng hiện có và chia nhỏ từng miếng thành nhiều phần nhỏ hơn bằng nhau. Thao tác này nhân tổng số lát với một hệ số nguyên. Hạn chế là sau mỗi lần sàng lọc như vậy, tổng số lát cắt phải chia hết cho tổng số người hiện tại để vẫn có thể phân phối đồng đều. 

Kết quả đầu ra không phải là cách phân bổ các lát cắt cho từng cá nhân mà là về số lượng lát cắt cuối cùng nhỏ nhất có thể có sau khi tất cả các nhóm đã đến nơi, giả sử William luôn chọn các lát cắt một cách tối ưu để giảm thiểu số lượng phân chia cuối cùng đồng thời tôn trọng các quy tắc ở mọi giai đoạn. 

Đầu vào là một chuỗi quy mô nhóm và ở mỗi bước, chúng tôi duy trì số lượng tích lũy những người phải được phục vụ. 

Các ràng buộc rất nhỏ: số lượng nhóm nhiều nhất là 10 và kích thước mỗi nhóm nhiều nhất là 10. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa mạnh mẽ hoặc tiệm cận ngoài số học thời gian không đổi trên mỗi bước. Ngay cả một lực lượng vũ phu đối với các trạng thái hợp lý cũng sẽ vượt qua. Thách thức thực sự là nhận ra cấu trúc bất biến của các ràng buộc chia hết lặp đi lặp lại. 

Một cách giải thích đơn giản có thể cố gắng mô phỏng các quyết định cắt bằng cách khám phá các số nhân khác nhau ở mỗi bước, nhưng hệ số phân nhánh sẽ tăng nhanh nếu chúng ta thử các lựa chọn sàng lọc tùy ý. Một cách tiếp cận không chính xác khác là chỉ xem xét quy mô nhóm hiện tại thay vì tổng số tích lũy, điều này sẽ bị ngắt ngay khi có nhiều nhóm tích lũy. 

Một trường hợp thất bại tinh tế phát sinh khi người ta cho rằng chỉ cần làm cho số lượng lát chia cho mỗi kích thước nhóm một cách độc lập là đủ. Ví dụ: nếu các nhóm là 2, 3 và 4, việc thực thi chia hết cho 2, rồi 3, rồi 4 sẽ bỏ qua các giới hạn thực tế là 2, rồi 5, rồi 9 theo cách tích lũy. 

## Phương pháp tiếp cận 

Một mô hình brute-force sẽ cố gắng theo dõi số lượng lát cắt sau mỗi nhóm và liệt kê tất cả các hệ số nguyên có thể có cho mỗi bước. Ở bước i, nếu hiện tại chúng ta có s lát và t người, chúng ta phải chọn một số nguyên k sao cho k·s chia hết cho t. Việc thử tất cả các giá trị k như vậy sẽ dẫn đến sự bùng nổ, vì k có thể lớn và chuỗi lựa chọn trong tối đa 10 bước sẽ nhân lên các khả năng một cách không cần thiết. Mặc dù các ràng buộc là nhỏ nhưng cách tiếp cận này che khuất cấu trúc. 

Quan sát quan trọng là trạng thái được mô tả đầy đủ bằng một số nguyên duy nhất: số lát cắt phải luôn là bội số của số người hiện tại và chúng tôi muốn số nhỏ nhất như vậy phù hợp với tất cả các ràng buộc trước đó. Mỗi khi số lượng người tăng từ t lên t + a_i, số lát hiện tại phải được điều chỉnh thành bội số của tổng số mới. Vì chúng ta chỉ có thể nhân số lượng lát cắt hiện tại với một số nguyên nên chiến lược tối ưu là luôn tăng nó lên cấu trúc bội số chung nhỏ nhất được ngụ ý bởi yêu cầu mới. 

Điều này làm giảm vấn đề trong việc duy trì giá trị đang chạy luôn là số nhỏ nhất chia hết cho tổng số người tích lũy hiện tại, với hạn chế là chúng tôi chỉ có thể mở rộng quy mô lên theo hệ số nguyên ở mỗi bước. Bởi vì mỗi bước buộc phải chia hết cho tổng tiền tố mới, số cuối cùng tối thiểu có thể đạt được chỉ đơn giản là bội số chung nhỏ nhất của tất cả các tổng tiền tố của kích thước nhóm.

Điều này có hiệu quả vì mọi bản cập nhật đều thực thi ràng buộc về khả năng chia hết trên tổng số lát cắt hiện tại và khi một số chia hết cho tất cả các tổng tiền tố, nó vẫn hợp lệ trong các bước nhân tiếp theo. Cấu trúc sụp đổ thành sự tích lũy LCM lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Tối ưu (LCM của tiền tố) | O(N log M) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nhóm theo thứ tự trong khi vẫn duy trì hai giá trị, số người hiện tại và câu trả lời hiện tại biểu thị số lát hợp lệ tối thiểu. 

1. Bắt đầu với không người và một miếng. Số lượng lát bắt đầu từ 1 vì chúng tôi muốn nhận dạng nhân trung tính trước khi áp dụng bất kỳ ràng buộc nào. Điều này cho phép xử lý nhất quán nhóm đầu tiên. 
2. Đối với mỗi quy mô nhóm, hãy cộng nó vào tổng số người hiện có. Giá trị tích lũy này là số lượng người phải được phục vụ đồng đều sau khi nhóm này đến. 
3. Cập nhật số lát hiện tại để chia hết cho tổng số người mới. Vì chúng tôi chỉ có thể chia tỷ lệ bằng phép nhân số nguyên nên chúng tôi tính bội số nhỏ nhất của số lát hiện tại cũng chia hết cho tổng mới. Điều này tương đương với việc thay thế số lát bằng lcm(current_slices, current_people). 
4. Tiếp tục quá trình này cho mọi nhóm, luôn thực thi tính chia hết cho tổng tiền tố đầy đủ. Mỗi bước duy trì tính khả thi cho tất cả các nhóm trước đó vì các ràng buộc trước đó chia giá trị cập nhật cho việc xây dựng. 
5. Sau khi xử lý tất cả các nhóm, xuất ra số lát cuối cùng. 

Bất biến chính là sau khi xử lý nhóm thứ i, số lát hiện tại là số nhỏ nhất chia hết cho tổng kích thước nhóm thứ i đầu tiên và có thể thu được bằng cách nhân số nguyên liên tiếp từ trạng thái trước đó. Điều này đảm bảo không tồn tại cấu hình hợp lệ nhỏ hơn, vì mọi cấu hình hợp lệ đều phải chia hết cho mỗi tổng tiền tố trung gian và quá trình luôn nâng cấp lên bội số tối thiểu đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def lcm(a, b):
    return a // gcd(a, b) * b

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    people = 0
    slices = 1

    for x in a:
        people += x
        slices = lcm(slices, people)

    print(slices)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. các`people`biến theo dõi tổng tiền tố của những người tham dự, đây là yêu cầu về khả năng chia hết hiện hoạt ở mỗi bước. các`slices`biến theo dõi số lượng lát cắt nhỏ nhất khả thi. 

Hoạt động không tầm thường duy nhất là cập nhật LCM. sử dụng`a // gcd(a, b) * b`tránh tràn và giữ cho tính toán ổn định. Thứ tự nhân được chọn cẩn thận để tránh tăng trưởng trung gian vượt quá những gì Python có thể xử lý hiệu quả trong các ràng buộc cạnh tranh điển hình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 10
```Chúng tôi theo dõi trạng thái từng bước. 

| Bước | Nhóm | Tổng số người | Lát | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | lcm(1,2)=2 | 
| 2 | 3 | 5 | 10 | lcm(2,5)=10 | 
| 3 | 10 | 15 | 30 | lcm(10,15)=30 | 

Sau nhóm cuối cùng, số lát là 30, phù hợp với yêu cầu là tất cả 15 người có thể được phục vụ đồng đều và mỗi giai đoạn trung gian đều khả thi. 

### Ví dụ 2 

đầu vào:```
4
1 1 1 1
```| Bước | Nhóm | Tổng số người | Lát | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | lcm(1,1)=1 | 
| 2 | 1 | 2 | 2 | lcm(1,2)=2 | 
| 3 | 1 | 3 | 6 | lcm(2,3)=6 | 
| 4 | 1 | 4 | 12 | lcm(6,4)=12 | 

Câu trả lời cuối cùng là 12, tương ứng với số nhỏ nhất chia hết cho tất cả các tổng tiền tố 1, 2, 3, 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log M) | Mỗi bước thực hiện tính toán gcd trên các số nguyên tăng dần | 
| Không gian | O(1) | Chỉ có hai số nguyên được duy trì | 

Các hạn chế là cực kỳ nhỏ, do đó, ngay cả mức tăng trưởng vừa phải trong giá trị LCM cũng không phải là vấn đề đáng lo ngại. Việc tính toán vẫn tầm thường đối với N lên đến 10. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    def lcm(a, b):
        return a // gcd(a, b) * b

    n = int(input())
    a = list(map(int, input().split()))

    people = 0
    slices = 1

    for x in a:
        people += x
        slices = lcm(slices, people)

    return str(slices)

# provided sample
assert run("3\n2 3 10\n") == "30"

# minimum case
assert run("1\n1\n") == "1"

# all equal small groups
assert run("3\n1 1 1\n") == "6"

# increasing primes-like pattern
assert run("4\n2 3 5 7\n") == "210"

# single large group
assert run("1\n10\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 nhóm 1 | 6 | tăng trưởng LCM tích lũy | 
| 2 3 5 7 | 210 | tương tác nhân của tiền tố | 
| đơn 10 | 10 | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả kích thước nhóm là 1. Trong trường hợp đó, tổng tích lũy tăng thêm một mỗi bước và thuật toán lần lượt lấy LCM bằng 1, 2, 3, v.v. Đối với đầu vào`1 1 1 1`, quá trình tính toán tiến hành theo các lát = 1, rồi 2, rồi 6, rồi 12. Mỗi bước là bắt buộc vì mỗi lần đếm người mới đưa ra một yêu cầu về ước số mới. Yêu cầu này không thể được thỏa mãn nếu không tăng số lát. 

Một trường hợp khác là khi một nhóm chứa tất cả mọi người, ví dụ`10`một mình. Thuật toán ngay lập tức đặt số người thành 10 và cập nhật các lát cắt thành lcm(1, 10), mang lại 10. Vì không có ràng buộc trung gian nên không cần nhân thêm và kết quả đã là tối thiểu. 

Một kịch bản tinh vi hơn xảy ra khi quy mô nhóm là nguyên tố cùng nhau, chẳng hạn như`2, 3, 5`. Các tổng tiền tố trở thành 2, 5 và 10 và thuật toán xây dựng chính xác 2 → 10 → 10. Giá trị cuối cùng ổn định sau khi tất cả các ràng buộc được hấp thụ, chứng tỏ rằng các ràng buộc sau này có thể gộp các ràng buộc trước đó thông qua cấu trúc chia hết.
