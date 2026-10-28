---
title: "CF 106160I - Intermill Logistics"
description: "Chúng tôi có một bộ sưu tập các nhà máy bột mì. Mỗi nhà máy có hai đặc tính: nó có thể xử lý bao nhiêu lúa mì mỗi giờ và mất bao nhiêu giờ. Vận chuyển lúa mì đến nhà máy và mang bột về mất gấp đôi thời gian di chuyển nhất định."
date: "2026-06-25T11:13:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "I"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 27
verified: true
draft: false
---

[CF 106160I - Hậu cần Intermill](https://codeforces.com/problemset/problem/106160/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các nhà máy bột mì. Mỗi nhà máy có hai đặc tính: nó có thể xử lý bao nhiêu lúa mì mỗi giờ và mất bao nhiêu giờ. Vận chuyển lúa mì đến nhà máy và mang bột về mất gấp đôi thời gian di chuyển nhất định. Nếu một nhà máy nhận được`x`kg, phần mài mất`x / p`giờ, ở đâu`p`là tốc độ xử lý của nó. 

Nhiệm vụ là chia tổng lượng lúa mì giữa các nhà máy sao cho thời điểm chuyến hàng cuối cùng về càng sớm càng tốt. Đầu ra là thời gian hoàn thiện tối thiểu đó, với độ chính xác của dấu phẩy động. 

Kích thước đầu vào lớn. Có thể có tới`100000`các nhà máy, vì vậy việc kiểm tra mọi phân phối có thể là không thể. Lượng lúa mì có thể đạt tới`10^9`, tốc độ và thời gian di chuyển cũng có thể rất lớn, vì vậy lời giải cần tìm kiếm logarit trên đáp án thay vì mô phỏng quá trình. Một giải pháp gần`O(n)`hoặc`O(n log C)`cần thiết, ở đâu`C`là phạm vi của các câu trả lời có thể. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Một máy nghiền có tốc độ xử lý rất cao có thể sớm vẫn vô dụng vì thời gian vận chuyển. Ví dụ:```
2 100

1000 100
1 1
```Nhà máy thứ hai có thể xử lý đủ nhanh, nhưng riêng lô hàng đầu tiên đã mất 200 giờ trước khi bắt đầu nghiền. Một giải pháp bất cẩn chỉ so sánh tốc độ xử lý có thể chọn sai nhà máy. 

Một trường hợp khác là khi một số nhà máy hoàn thành cùng một lúc. Ví dụ:```
3 7

1 1
1 1
1 1
```Câu trả lời là không`7`, vì lúa mì có thể được chia thành ba phần bằng nhau. Mỗi tay cầm máy nghiền`7/3`kg, yêu cầu`7/3`giờ nghiền cộng thêm`2`số giờ di chuyển, đưa ra:```
13/3 = 4.333333333333
```Giải pháp giao toàn bộ lúa mì cho một nhà máy sẽ bỏ lỡ quá trình xử lý song song. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là quyết định mỗi nhà máy nhận được bao nhiêu lúa mì. Đối với số tiền đã chọn`x_i`, cối xay`i`kết thúc sau:```
2 * t_i + x_i / p_i
```giờ. Mục tiêu là giảm thiểu tối đa các giá trị này. Một cách tiếp cận mạnh mẽ sẽ thử cách phân phối lúa mì khác nhau giữa các nhà máy và giữ được thời gian hoàn thiện tốt nhất. Ngay cả việc chỉ biểu diễn các phần chia số nguyên kilogam cũng đã mang lại vô số khả năng. Nếu có`n`nhà máy và`w`kg, số lượng phân bố có thể có là theo thứ tự kết hợp của`w + n`, điều này hoàn toàn không khả thi khi cả hai giá trị đều lớn. 

Quan sát hữu ích là đảo ngược câu hỏi. Thay vì hỏi "lịch trình nhanh nhất là gì?", hãy hỏi "liệu chúng ta có`T`số giờ có sẵn, chúng ta có thể ăn hết bao nhiêu lúa mì?" 

Trong thời gian hoàn thành cố định`T`, máy nghiền chỉ có thể được sử dụng sau khi đã hết thời gian vận chuyển khứ hồi. Nếu như`T <= 2*t_i`, nhà máy đó không thể hoàn thành được một hạt lúa mì nào. Nếu không thì thời gian còn lại là:```
T - 2*t_i
```và nhà máy có thể xử lý:```
p_i * (T - 2*t_i)
```kilôgam. 

Việc cộng số này trên tất cả các nhà máy sẽ cho ra tổng lượng lúa mì có thể hoàn thành theo thời gian`T`. Giá trị này chỉ tăng khi`T`tăng lên, vì vậy chúng ta có thể tìm kiếm nhị phân nhỏ nhất`T`có thể xử lý tất cả`w`kilôgam. 

Lực lượng vũ phu thất bại vì nó cố gắng tự xây dựng sự phân phối. Quan sát trên loại bỏ sự cần thiết phải biết phân phối. Chúng tôi chỉ cần kiểm tra xem có đủ năng lực trước một thời hạn nhất định hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng phân phối có thể có | O(1) | Quá chậm | 
| Tối ưu | O(n log C) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các nhà máy và lưu trữ tốc độ xử lý cũng như thời gian di chuyển của chúng. Thông tin duy nhất cần thiết trong quá trình tìm kiếm là mỗi nhà máy đóng góp bao nhiêu công suất cho thời gian hoàn thiện của ứng viên. 
2. Tìm giới hạn dưới và giới hạn trên cho câu trả lời. Giới hạn dưới có thể bằng 0. Đối với giới hạn trên, liên tục nhân đôi giá trị cho đến khi giá trị đó đủ lớn để mọi nhà máy cùng nhau có thể xử lý tất cả lúa mì vào thời điểm đó. Điều này tránh phụ thuộc vào giá trị tối đa chưa biết. 
3. Tìm kiếm nhị phân vào thời điểm hoàn thiện`T`. Đối với điểm giữa, hãy tính tổng lượng lúa mì có thể được chế biến. 
4. Đối với mỗi nhà máy, chỉ cộng phần đóng góp của họ khi`T > 2*t_i`. Đóng góp của nó là`p_i * (T - 2*t_i)`. Nếu công suất tích lũy đạt`w`, lần này là khả thi. 
5. Nếu thời gian hiện tại khả thi, hãy tìm kiếm nửa nhỏ hơn vì thời gian hoàn thành sớm hơn cũng có thể phù hợp. Nếu không, hãy tìm kiếm nửa lớn hơn. 
6. Sau khi lặp đủ số lần, xuất ra giới hạn dưới. Tìm kiếm nhị phân dấu phẩy động không cần điểm dừng chính xác vì lỗi yêu cầu chỉ`10^-6`. 

Tại sao nó hoạt động: Vào bất kỳ thời gian cố định nào`T`, mỗi nhà máy đều có số lượng lúa mì tối đa có thể hoàn thành trước thời hạn đó. Bất kỳ lịch trình hợp lệ nào kết thúc trước`T`không thể vượt quá công suất này và bất kỳ số lượng nào trong khả năng này đều có thể được phân bổ độc lập giữa các nhà máy. Vì thế,`T`có thể thực hiện được chính xác khi tổng của những công suất này ít nhất bằng tổng lượng lúa mì. Vì tính khả thi là đơn điệu nên tìm kiếm nhị phân tìm ra thời gian hoàn thành nhỏ nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, w = map(int, input().split())
    mills = []

    for _ in range(n):
        p, t = map(int, input().split())
        mills.append((p, t))

    def can(x):
        total = 0.0
        for p, t in mills:
            remain = x - 2.0 * t
            if remain > 0:
                total += p * remain
                if total >= w:
                    return True
        return False

    lo = 0.0
    hi = 1.0

    while not can(hi):
        hi *= 2.0

    for _ in range(100):
        mid = (lo + hi) / 2.0
        if can(mid):
            hi = mid
        else:
            lo = mid

    print("{:.12f}".format(hi))

if __name__ == "__main__":
    solve()
```các`can`chức năng là cốt lõi của giải pháp. Nó trả lời liệu một thời hạn cụ thể có đủ hay không. Đối với mỗi nhà máy, thời gian vận chuyển tiêu tốn`2*t`giờ trước nên thời gian xay chỉ là phần còn lại. Việc tính toán dung lượng sử dụng các giá trị dấu phẩy động vì câu trả lời không nhất thiết phải là số nguyên. 

Giới hạn trên được xây dựng linh hoạt. Nhân đôi tránh việc đoán câu trả lời tối đa và đảm bảo rằng tìm kiếm nhị phân bắt đầu với một điểm cuối không thể và một điểm cuối có thể. 

Tìm kiếm nhị phân sử dụng một số lần lặp cố định thay vì so sánh với một khoảng thời gian nhỏ. Khoảng 100 lần lặp là vượt xa mức cần thiết để đạt được độ chính xác gấp đôi và giúp việc triển khai trở nên đơn giản. 

Dung lượng có thể rất lớn, nhưng số nguyên và giá trị dấu phẩy động của Python xử lý phạm vi được yêu cầu một cách an toàn. Sự trở lại sớm trong`can`tránh được những công việc không cần thiết sau khi đã đạt được số lượng lúa mì mục tiêu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 1000

80 3
120 5
160 4
```Tìm kiếm nhị phân kiểm tra thời hạn. Tại`T = 11`, công suất là: 

| Nhà máy | Tỷ lệ | Thời gian di chuyển | Thời gian mài có sẵn | Công suất | 
| --- | --- | --- | --- | --- | 
| 1 | 80 | 3 | 5 | 400 | 
| 2 | 120 | 5 | 1 | 120 | 
| 3 | 160 | 4 | 3 | 480 | 

Tổng công suất là`1000`, vậy 11 giờ là khả thi. Bất kỳ giá trị nhỏ hơn nào đều tạo ra tổng công suất dưới 1000 kg. 

Đối với mẫu thứ ba:```
3 7

1 1
1 1
1 1
```| Đã kiểm tra thời gian | Công suất máy nghiền mỗi | Tổng công suất | Kết quả | 
| --- | --- | --- | --- | 
| 4 | 2 | 6 | Quá nhỏ | 
| 4.333333 | 2.333333 | 7 | Khả thi | 

Các phương pháp tìm kiếm nhị phân`4.333333333333`, phù hợp với độ chính xác cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log C) | Mỗi lần lặp tìm kiếm nhị phân sẽ quét tất cả các nhà máy và khoảng 100 lần lặp được sử dụng | 
| Không gian | O(n) | Danh sách các nhà máy được lưu trữ | 

Giới hạn đầu vào của`100000`các nhà máy thực hiện quét tuyến tính trên mỗi lần lặp lại cần thiết. 100 lần lặp tìm kiếm nhị phân cố định cung cấp khoảng mười triệu thao tác đơn giản, vừa vặn thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    n, w = map(int, input().split())
    mills = [tuple(map(int, input().split())) for _ in range(n)]

    def can(x):
        total = 0.0
        for p, t in mills:
            rem = x - 2 * t
            if rem > 0:
                total += p * rem
        return total >= w

    lo, hi = 0.0, 1.0
    while not can(hi):
        hi *= 2

    for _ in range(100):
        mid = (lo + hi) / 2
        if can(mid):
            hi = mid
        else:
            lo = mid

    sys.stdin = old
    return hi

# sample 1
assert abs(solve_data("""3 1000
80 3
120 5
160 4
""") - 11) < 1e-6

# sample 2
assert abs(solve_data("""2 100
100 1
500 2
""") - 3) < 1e-6

# minimum size
assert abs(solve_data("""1 1
1 1
""") - 3) < 1e-6

# all equal values
assert abs(solve_data("""3 7
1 1
1 1
1 1
""") - 4.333333333333) < 1e-6

# very fast mill with large shipping delay
assert abs(solve_data("""2 100
1000 100
100 1
""") - 3) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nhà máy đơn | 3 | Vận chuyển cơ bản cộng với tính toán xử lý | 
| Ba nhà máy giống hệt nhau | 4.333333 | Phân chia công việc song song | 
| Thời gian di chuyển lớn | 3 | Phải bao gồm cả sự chậm trễ vận chuyển | 
| Mẫu gốc | 11 và 3 | Tính đúng đắn chung | 

## Vỏ cạnh 

Đối với trường hợp thời gian di chuyển cao:```
2 100

1000 100
100 1
```Nhà máy đầu tiên không thể đóng góp cho đến khi 200 giờ trôi qua. Nhà máy thứ hai có thể xử lý`100 * (T - 2)`kg sau hai giờ. Thời gian tối thiểu có thể thực hiện được là`3`, nơi có đúng một giờ để xử lý lúa mì. 

Đối với trường hợp máy nghiền bằng nhau:```
3 7

1 1
1 1
1 1
```Tại thời điểm ứng cử viên`4.333333333333`, mỗi nhà máy có`2.333333333333`giờ nghiền. Mỗi người đóng góp nhiều lúa mì như vậy và cộng lại họ đạt được 7 kg. Thuật toán phát hiện ra điều này một cách tự nhiên vì nó chỉ kiểm tra tổng công suất chứ không kiểm tra một nhiệm vụ cụ thể nào. 

Đối với một nhà máy duy nhất:```
1 1

1 1
```Nhà máy cần hai giờ để vận chuyển và một giờ để nghiền. Việc kiểm tra năng lực trở thành sự thật chính xác vào thời điểm đó`3`, do đó tìm kiếm nhị phân trả về giá trị đúng mà không yêu cầu bất kỳ trường hợp đặc biệt nào.
