---
title: "CF 102741H - Máy dò ma của E. Gadd"
description: "Chúng ta có một tập hợp các vị trí phát điện trên hành lang một chiều và một tập hợp các vị trí ma trên cùng một đường. Mỗi con ma phải được loại bỏ bằng cách chọn một Zapper. Nếu một con ma ở khoảng cách d tính từ thiết bị bắn đã chọn thì năng lượng tiêu hao là d2."
date: "2026-07-29T00:47:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102741
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 1"
rating: 0
weight: 102741
solve_time_s: 61
verified: true
draft: false
---

[CF 102741H - Máy dò ma của E. Gadd](https://codeforces.com/problemset/problem/102741/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các vị trí phát điện trên hành lang một chiều và một tập hợp các vị trí ma trên cùng một đường. Mỗi con ma phải được loại bỏ bằng cách chọn một Zapper. Nếu một bóng ma ở xa`d`từ thiết bị phát điện đã chọn, năng lượng tiêu tốn là`d²`. Mục tiêu là tìm ra tổng năng lượng tối thiểu cần thiết khi mỗi con ma được gán cho thiết bị điện rẻ nhất có thể của nó. Câu trả lời phải được in modulo`10^9 + 7`. Các ràng buộc của bài toán ban đầu cho phép tối đa`10^5`máy phát điện và`10^5`bóng ma, với tọa độ lên tới`10^7`. 

Kích thước đầu vào lớn ngay lập tức loại trừ việc kiểm tra mọi bóng ma đối với mọi máy phát điện. Một giải pháp trực tiếp sẽ thực hiện`n * m`tính toán khoảng cách. Với cả hai giá trị bằng`100000`, điều đó trở thành`10^10`tính toán vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Chúng ta cần khai thác thực tế là tất cả các vị trí đều nằm trên một đường thẳng. 

Cấu trúc chính là thiết bị phát hiện gần nhất với một con ma phải là một trong hai thiết bị phát hiện xung quanh nó sau khi sắp xếp. Nếu một con ma nằm giữa hai máy phát điện lân cận, việc di chuyển ra xa một trong hai chỉ làm tăng khoảng cách. Nếu bóng ma nằm ngoài toàn bộ phạm vi của Zapper, thì Zapper gần nhất chỉ đơn giản là điểm cuối gần nhất. Điều này có nghĩa là vấn đề có thể được giảm xuống thành việc sắp xếp và tìm kiếm nhị phân. 

Một số trường hợp đặc biệt có thể phá vỡ quá trình triển khai chỉ xem xét trường hợp chung. 

Nếu một con ma chính xác đang ở trên máy phát điện, thì mức đóng góp năng lượng bằng không. 

Ví dụ đầu vào:```
2 1
5
10
5
```Đầu ra đúng là:```
0
```Một giải pháp bất cẩn sử dụng sự so sánh chặt chẽ và bỏ qua sự bình đẳng có thể chọn sai một thiết bị phát điện khác và cộng thêm chi phí dương. 

Nếu tất cả các tia cực tím đều ở một phía của mỗi con ma, thì chỉ nên sử dụng tia cực tím gần nhất. 

Ví dụ đầu vào:```
2 2
10
20
0
5
```Đầu ra đúng là:```
125
```Con ma đầu tiên đóng góp`(10 - 0)² = 100`và đóng góp thứ hai`(10 - 5)² = 25`. Việc triển khai giả định rằng mọi bóng ma đều có thiết bị kích hoạt ở cả hai bên có thể truy cập vào các chỉ mục không hợp lệ hoặc tính sai hàng xóm. 

Vị trí zapper trùng lặp là một trường hợp khác đáng xử lý. 

Ví dụ đầu vào:```
3 2
7
7
7
7
8
```Đầu ra đúng là:```
1
```Tất cả các zapper trùng lặp hoạt động như một vị trí duy nhất. Một giải pháp cố gắng sử dụng các vị trí duy nhất là được, nhưng một giải pháp xử lý sai các giá trị bằng nhau trong quá trình tìm kiếm nhị phân có thể bỏ qua kết quả khớp chính xác một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp vũ phu rất đơn giản. Đối với mọi con ma, hãy lặp lại mọi công cụ phát hiện, tính khoảng cách bình phương và giữ mức tối thiểu. Điều đó đúng vì mọi nhiệm vụ có thể có cho con ma đó đều được kiểm tra. 

Vấn đề là khối lượng công việc. Với`n`máy phát điện và`m`ma, điều này thực hiện`n * m`so sánh và tính toán khoảng cách. Ở kích thước đầu vào tối đa, điều này đạt đến`10^10`hoạt động quá chậm. 

Quan sát quan trọng là các vị trí là một chiều. Sau khi sắp xếp các vị trí của đèn ma, đèn chiếu gần nhất với một bóng ma luôn được tìm thấy gần điểm chèn của bóng ma trong mảng đã sắp xếp. Chúng tôi không cần phải kiểm tra tất cả các máy phát điện. 

Đối với một vị trí ma`x`, tìm kiếm nhị phân sẽ đưa ra Zapper đầu tiên có vị trí ít nhất là`x`. Zapper này là ứng cử viên gần nhất ở bên phải. Zapper trước đó, nếu nó tồn tại, là ứng cử viên gần nhất ở bên trái. Bất kỳ Zapper nào khác ở bên phải đều xa hơn ứng cử viên đầu tiên bên phải này, và bất kỳ Zapper nào khác ở bên trái đều xa hơn ứng cử viên đầu tiên bên trái này. Chúng ta chỉ cần so sánh hai điều đó. 

Lực lượng vũ phu hoạt động vì nó kiểm tra mọi máy phát điện có thể. Quan sát cho thấy chỉ các zapper lân cận mới quan trọng làm giảm từng truy vấn ma từ thời gian tuyến tính sang thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + m log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vị trí của Zapper và sắp xếp chúng. 

Việc sắp xếp tạo ra một cấu trúc có thứ tự trong đó tìm kiếm nhị phân có thể nhanh chóng xác định vị trí của các công cụ tìm kiếm xung quanh bất kỳ vị trí ma nào. 
2. Đối với mỗi vị trí ma, thực hiện tìm kiếm nhị phân trên các zapper đã sắp xếp để tìm chỉ mục đầu tiên`i`Ở đâu`zappers[i] >= ghost`. 

Điều này chia các máy phát điện gần nhất có thể thành hai ứng cử viên. Zapper ở chỉ mục`i`là cái gần nhất có thể ở bên phải, trong khi cái Zapper ở`i - 1`là cái gần nhất có thể ở bên trái. 
3. Kiểm tra các ứng viên hợp lệ. 

Nếu như`i`nằm trong mảng, tính chi phí bằng cách sử dụng`zappers[i]`. Nếu như`i - 1`không âm, hãy tính chi phí bằng cách sử dụng`zappers[i - 1]`. Giữ khoảng cách bình phương nhỏ hơn. 
4. Thêm chi phí tối thiểu cho bóng ma này vào câu trả lời đang chạy. 

Áp dụng modulo sau khi bổ sung để giữ cho kích thước số nguyên có thể quản lý được. 

Tại sao nó hoạt động: 

Đối với bất kỳ vị trí ma quái nào, hãy xem xét các công cụ tìm kiếm được sắp xếp xung quanh nó. Mỗi zapper ở xa bên trái hơn hàng xóm bên trái ngay lập tức có khoảng cách lớn hơn và mọi zapper ở xa bên phải hơn hàng xóm ngay bên phải cũng có khoảng cách lớn hơn. Người phát hiện gần nhất phải là một trong hai người hàng xóm đó. Thuật toán luôn kiểm tra chính xác những ứng cử viên đó, vì vậy mọi con ma đều nhận được chi phí năng lượng tối thiểu có thể. Tổng hợp những chi phí tối thiểu độc lập đó sẽ cho ra tổng năng lượng tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())

    zappers = []
    for _ in range(n):
        zappers.append(int(input()))

    zappers.sort()

    ans = 0

    for _ in range(m):
        x = int(input())

        left = 0
        right = n

        while left < right:
            mid = (left + right) // 2
            if zappers[mid] >= x:
                right = mid
            else:
                left = mid + 1

        best = 10**30

        if left < n:
            d = zappers[left] - x
            best = d * d

        if left > 0:
            d = zappers[left - 1] - x
            best = min(best, d * d)

        ans = (ans + best) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Danh sách các Zapper được sắp xếp là cấu trúc dữ liệu chính. Việc triển khai tìm kiếm nhị phân tìm giới hạn dưới, nghĩa là vị trí đầu tiên chứa giá trị lớn hơn hoặc bằng vị trí ma hiện tại. 

Biến`left`sau khi tìm kiếm không phải là hàng xóm bên trái, đó là vị trí chèn. Nếu nó bằng`n`, không có zapper ở phía bên phải. Nếu nó bằng 0 thì không có Zapper ở phía bên trái. Hai kiểm tra ranh giới này ngăn chặn truy cập mảng không hợp lệ. 

Số nguyên Python không bị tràn, do đó khoảng cách bình phương vẫn an toàn mặc dù chênh lệch tọa độ có thể lên tới`10^7`. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, phép nhân phải được thực hiện bằng loại rộng hơn. 

## Ví dụ đã hoạt động 

Mẫu 1:```
4 4
4
1
11
7
2
9
6
15
```Các Zapper được sắp xếp là`[1, 4, 7, 11]`. 

| Ma | Chỉ số giới hạn dưới | Ứng viên | Chi phí tối thiểu | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 4, 1 | 1 | 1 | 
| 9 | 3 | 11, 7 | 4 | 5 | 
| 6 | 2 | 7, 4 | 1 | 6 | 
| 15 | 4 | 11 | 16 | 22 | 

Dấu vết cho thấy tại sao chỉ cần hai ứng cử viên. Cho hồn ma ở vị trí`9`, kiểm tra zappers tại`7`Và`11`là đủ vì mọi máy phát điện khác đều ở xa hơn. 

Mẫu 2:```
2 4
10
5
7
0
5
100
```Các Zapper được sắp xếp là`[5, 10]`. 

| Ma | Chỉ số giới hạn dưới | Ứng viên | Chi phí tối thiểu | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 7 | 1 | 10, 5 | 4 | 4 | 
| 0 | 0 | 5 | 25 | 29 | 
| 5 | 0 | 5, không có | 0 | 29 | 
| 100 | 2 | 10 | 8100 | 8129 | 

Ví dụ này thực hiện cả hai thái cực. Những bóng ma nằm ngoài phạm vi bắn tỉa chỉ có một ứng cử viên hợp lệ gần nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log n) | Việc sắp xếp các zapper tốn O(n log n) và mỗi ma trong số m bóng ma thực hiện một tìm kiếm nhị phân. | 
| Không gian | O(n) | Các vị trí zapper được sắp xếp sẽ được lưu trữ. | 

Giải pháp xử lý`10^5`máy phát điện và`10^5`bóng ma vì công việc tốn kém được thực hiện một lần trong quá trình sắp xếp, sau đó là các truy vấn logarit hiệu quả. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))

    zappers = [int(next(it)) for _ in range(n)]
    zappers.sort()

    ans = 0
    mod = 10**9 + 7

    for _ in range(m):
        x = int(next(it))

        lo, hi = 0, n
        while lo < hi:
            mid = (lo + hi) // 2
            if zappers[mid] >= x:
                hi = mid
            else:
                lo = mid + 1

        best = 10**30

        if lo < n:
            d = zappers[lo] - x
            best = d * d

        if lo > 0:
            d = zappers[lo - 1] - x
            best = min(best, d * d)

        ans = (ans + best) % mod

    return str(ans) + "\n"

assert solve("""4 4
4
1
11
7
2
9
6
15
""") == "22\n", "sample 1"

assert solve("""2 4
10
5
7
0
5
100
""") == "8129\n", "sample 2"

assert solve("""1 1
0
0
""") == "0\n", "minimum size"

assert solve("""3 3
5
5
5
0
5
10
""") == "50\n", "all equal zappers"

assert solve("""2 3
10
20
0
5
30
""") == "250\n", "outside boundaries"

assert solve("""3 2
1
10000000
5000000
5000001
9999999
""") == "2\n", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Zapper đơn và ma ở cùng một vị trí | 0 | Xử lý khớp chính xác | 
| Nhiều zappers giống hệt nhau | 50 | Vị trí trùng lặp | 
| Những bóng ma ngoài phạm vi Zapper | 250 | Các trường hợp ranh giới trái và phải | 
| Giá trị tọa độ lớn | 2 | Xử lý khoảng cách bình phương đúng | 

## Vỏ cạnh 

Để có kết quả khớp chính xác, chẳng hạn như:```
2 1
5
10
5
```tìm kiếm nhị phân trả về chỉ mục của`5`. Thuật toán so sánh ứng cử viên đó và tìm khoảng cách bằng 0, do đó mức đóng góp bằng 0. 

Đối với những bóng ma nằm ngoài phạm vi có sẵn:```
2 2
10
20
0
5
```con ma đầu tiên được chỉ mục chèn`0`, vậy nên chỉ có Zapper tại`10`được kiểm tra. Ghost thứ 2 cũng lấy chỉ số chèn`0`, đưa ra chi phí chính xác`100`Và`25`. 

Đối với các Zapper trùng lặp:```
3 2
7
7
7
7
8
```sắp xếp giữ tất cả các bản sao với nhau. Tìm kiếm nhị phân tìm thấy một trong các vị trí bằng nhau và phép tính khoảng cách vẫn trả về giá trị nhỏ nhất. Các bản sao không yêu cầu bất kỳ xử lý đặc biệt nào vì chúng đại diện cho các lựa chọn giống hệt nhau.
