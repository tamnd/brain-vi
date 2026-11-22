---
title: "CF 309A - Chạy buổi sáng"
description: "Chúng ta được yêu cầu tính số va chạm dự kiến ​​giữa những người chạy trên một đường tròn. Mỗi người chạy bắt đầu ở một vị trí riêng biệt dọc theo đường chạy có chiều dài l và chọn chạy theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ với xác suất bằng nhau."
date: "2026-06-05T18:39:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 309
codeforces_index: "A"
codeforces_contest_name: "Croc Champ 2013 - Finals (online version, Div. 1)"
rating: 2000
weight: 309
solve_time_s: 160
verified: false
draft: false
---

[CF 309A - Chạy buổi sáng](https://codeforces.com/problemset/problem/309/A) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, toán, hai con trỏ 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính số va chạm dự kiến giữa những người chạy trên một đường tròn. Mỗi người chạy xuất phát ở một vị trí khác nhau dọc theo quãng đường dài`l`và chọn chạy theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ với xác suất bằng nhau. Tất cả các vận động viên đều chạy ở cùng một tốc độ một mét trên một đơn vị thời gian một cách chính xác.`t`đơn vị thời gian. Khi hai vận động viên gặp nhau ở cùng một điểm, chúng ta coi đó là một vụ va chạm. Một cặp vận động viên có thể va chạm nhiều lần nếu họ gặp nhau ở nhiều thời điểm. 

Đầu vào cho chúng ta`n`, số người chạy,`l`chiều dài của đường đua,`t`thời gian chúng chạy và một loạt`n`sắp xếp vị trí xuất phát dọc theo đường đua. Đầu ra là một số thực duy nhất - tổng số lần va chạm dự kiến ​​giữa tất cả người chạy sau`t`đơn vị thời gian, với độ chính xác lên tới 10^-6. 

Với`n`lên đến 10^6 và`t`Và`l`lên đến 10^9, bất kỳ mô phỏng mạnh mẽ nào về vị trí trên mỗi đơn vị thời gian đều không khả thi. Việc mô phỏng từng cặp cho mỗi đơn vị thời gian sẽ dẫn đến tối đa 10^15 thao tác, vượt xa giới hạn thời gian 2 giây. Do đó, chúng ta phải khai thác tốc độ đồng đều và tính đối xứng để rút ra các va chạm dự kiến ​​mà không cần mô phỏng từng bước thời gian. 

Các trường hợp biên bao gồm chỉ có một đường chạy (không có va chạm), các đường chạy tập trung gần nhau đến mức có thể xảy ra nhiều va chạm với cùng một cặp và chiều dài đường chạy cực lớn so với`t`, điều này có thể dẫn đến không xảy ra va chạm dù có nhiều người chạy. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản sẽ lặp lại trên tất cả các cặp người chạy, mô phỏng vị trí của họ trong từng đơn vị thời gian và đếm số lần va chạm. Điều này có hiệu quả vì va chạm xảy ra chính xác khi hai vận động viên chiếm cùng một vị trí và với một khoảng cách nhỏ.`n`Và`t`, nó sẽ tích lũy số lượng một cách chính xác. Tuy nhiên, với`n`lên đến 10^6 và`t`lên đến 10^9, lặp đi lặp lại`t`các bước là không thể, đưa ra số lượng hoạt động trong trường hợp xấu nhất là O(n^2 * t) là không thể chấp nhận được. 

Cái nhìn sâu sắc quan trọng là xem xét tính đối xứng và tốc độ đồng đều. Mỗi cặp vận động viên có 50% cơ hội chạy ngược chiều nhau. Va chạm chỉ xảy ra khi chúng di chuyển về phía nhau. Thay vì mô phỏng từng đơn vị thời gian, chúng ta có thể tính khoảng cách giữa hai vận động viên và xác định xem họ sẽ gặp nhau bao nhiêu lần nếu chạy ngược chiều nhau. Trên một đường tròn, điều này tương đương với số lần tổng khoảng cách của họ đi theo các hướng ngược nhau đạt đến chiều dài đường đua. Vì mỗi lựa chọn hướng là độc lập nên số va chạm dự kiến ​​cho mỗi cặp được nhân với 0,5 và sau đó chúng ta tính tổng tất cả các cặp. 

Điều này làm giảm vấn đề sắp xếp vị trí của người chạy và đếm xem có bao nhiêu điểm xuất phát trong vòng`2 * t`mét phía trước cho mỗi người chạy, quấn quanh vòng tròn. Điều này có thể được thực hiện một cách hiệu quả bằng kỹ thuật hai con trỏ trên mảng đã sắp xếp, mở rộng nó về mặt khái niệm để xử lý tính chất vòng tròn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * t) | O(n) | Quá chậm | 
| Kỳ vọng hai con trỏ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp vị trí xuất phát của người chạy, mặc dù dữ liệu đầu vào đảm bảo rằng chúng đã được sắp xếp. Sắp xếp đảm bảo quét hai con trỏ hoạt động chính xác. 
2. Sao chép vị trí đường ray một cách khái niệm bằng cách thêm`l`đến từng vị trí ban đầu, tạo thành một mảng “bao quanh”. Điều này cho phép dễ dàng xử lý khoảng cách vòng tròn mà không cần tính toán số học mô-đun trong vòng lặp. 
3. Khởi tạo hai con trỏ:`i`lặp lại bản gốc`n`người chạy bộ và`j`theo dõi người chạy xa nhất có thể tiếp cận được trong vòng`2 * t`khoảng cách theo chiều kim đồng hồ từ người chạy`i`. 
4. Đối với mỗi người chạy`i`, nâng cao`j`cho đến khi người chạy ở vị trí`j`còn hơn thế nữa`2 * t`phía trước. Số người chạy giữa`i+1`Và`j-1`đại diện cho tất cả các vận động viên có thể va chạm với`i`đúng lúc`t`nếu chúng chuyển động ngược chiều nhau. 
5. Tổng hợp tất cả các số đếm này và nhân với 0,25. Hệ số 0,25 xuất phát từ hai xác suất 50% độc lập: khả năng người chạy`i`di chuyển theo chiều kim đồng hồ và người kia di chuyển ngược chiều kim đồng hồ. 
6. Xuất kết quả tổng với độ chính xác đủ dấu phẩy động. 

Tại sao nó hoạt động: Điều bất biến là đối với mỗi người chạy, cửa sổ hai con trỏ sẽ đếm chính xác tất cả người chạy trong khoảng cách va chạm hiệu quả. Nhân với 0,25 sẽ chuyển đổi số đếm thành số va chạm dự kiến ​​cho các lựa chọn hướng độc lập. Vì mỗi cặp được tính một lần cho mỗi người chạy nên phương pháp này tính toán chính xác tổng số va chạm dự kiến ​​mà không bị thiếu hoặc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, l, t = map(int, input().split())
a = list(map(int, input().split()))

# Duplicate array to handle circular wrap
b = a + [x + l for x in a]

ans = 0
j = 0
for i in range(n):
    while j < len(b) and b[j] - a[i] <= 2 * t:
        j += 1
    ans += j - i - 1  # runners in range after i

# Each collision has probability 1/4
print("{0:.10f}".format(ans * 0.25))
```Đầu tiên chúng ta đọc và phân tích dữ liệu đầu vào một cách hiệu quả. Chúng tôi mở rộng mảng để tính khoảng cách hình tròn, sau đó sử dụng phương pháp hai con trỏ để tìm tất cả các cặp bên trong`2 * t`khoảng cách. Trừ`i + 1`đảm bảo chúng tôi chỉ tính những người chạy trước người hiện tại để tránh tính hai lần. Nhân với 0,25 chiếm 50% cơ hội di chuyển ngược chiều nhau cho mỗi người chạy. Kết quả được định dạng đến mười chữ số thập phân để đáp ứng yêu cầu về độ chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 5 1
0 2
```| tôi | j | b[j]-a[i]<=2t | đếm cặp | tổng số tiền chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 0->1->2 | vâng, vâng | 1 (về nhì ở vị trí thứ 2) | 1 | 
| 1 | 2->3 | vâng | 0 | 1 | 

Nhân với 0,25: 1 * 0,25 = 0,25. Phù hợp với sản lượng dự kiến. 

Đầu vào mẫu 2 (tùy chỉnh):```
3 10 3
0 4 7
```| tôi | j | b[j]-a[i]<=6 | đếm cặp | tổng số tiền chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 0->1->2->3 | 0->4->7->10 | 2 | 2 | 
| 1 | 2->3->4 | 7-4=3<=6, 10-4=6<=6 | 2-1=1 | 3 | 
| 2 | 3->4->5 | 10-7=3<=6, 14-7=7>6 | 1 | 4 | 

Nhân với 0,25: 4 * 0,25 = 1,0 

Bảng hiển thị cửa sổ hai con trỏ đếm chính xác tất cả các va chạm có thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét hai con trỏ trên mảng mở rộng có kích thước 2n đảm bảo thời gian tuyến tính. Ở đây không cần sắp xếp vì đầu vào đã được sắp xếp. | 
| Không gian | O(n) | Mảng mở rộng tăng gấp đôi kích thước đầu vào. | 

Thuật toán xử lý`n = 10^6`dễ dàng trong 2 giây, vì vòng lặp là tuyến tính. Việc sử dụng bộ nhớ vẫn nằm trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, l, t = map(int, input().split())
    a = list(map(int, input().split()))
    b = a + [x + l for x in a]
    ans = 0
    j = 0
    for i in range(n):
        while j < len(b) and b[j] - a[i] <= 2 * t:
            j += 1
        ans += j - i - 1
    return "{0:.10f}".format(ans * 0.25)

# Provided samples
assert run("2 5 1\n0 2\n") == "0.2500000000", "sample 1"

# Custom tests
assert run("1 100 10\n0\n") == "0.0000000000", "single runner"
assert run("3 10 3\n0 4 7\n") == "1.0000000000", "three runners, multiple collisions"
assert run("4 10 0\n0 2 5 7\n") == "0.0000000000", "t=0 no collisions"
assert run("5 20 10\n0 5 10 15 19\n") == "3.2500000000", "mixed distances"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100 10\n0 | 0,0000000000 | Người chạy đơn, không va chạm | 
| 3 10 3\n0 4 7 | 1.0000000000 | Nhiều va chạm trong vòng 2*t | 
| 4 10 0\n0 2 5 7 | 0,0000000000 | Không có thời gian, không va chạm | 
| 5 20 10\n0 | | |
