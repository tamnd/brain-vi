---
title: "CF 104790A - \\texttt{nâng cấp apt}"
description: "Chúng tôi được cung cấp một bộ sưu tập các gói phần mềm, mỗi gói có kích thước tải xuống đã biết. Tại một thời điểm nào đó trong quá trình nâng cấp, chúng tôi nhận thấy rằng chính xác một số gói đã hoàn tất quá trình tải xuống, trong khi có thể tải xuống tối đa một số gói cố định tại…"
date: "2026-06-28T13:54:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104790
codeforces_index: "A"
codeforces_contest_name: "2023 Benelux Algorithm Programming Contest (BAPC 23)"
rating: 0
weight: 104790
solve_time_s: 58
verified: true
draft: false
---

[CF 104790A - \\texttt{apt nâng cấp}](https://codeforces.com/problemset/problem/104790/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các gói phần mềm, mỗi gói có kích thước tải xuống đã biết. Tại một thời điểm nào đó trong quá trình nâng cấp, chúng tôi nhận thấy rằng chính xác một số gói đã hoàn tất tải xuống, trong khi có thể tải xuống tối đa một số gói cố định cùng lúc. Quá trình tải xuống được thực hiện song song nên bất cứ lúc nào`k`các gói đang được tải xuống tích cực và các gói đã hoàn thành không còn hoạt động nữa. 

Điểm mấu chốt là chỉ báo tiến trình không phân biệt hoàn hảo giữa “đã tải xuống đầy đủ nhưng chưa được đánh dấu là hoàn thành” và “vẫn đang tiến hành”, vì vậy tại thời điểm quan sát, một số lượt tải xuống đang hoạt động có thể đóng góp hiệu quả kích thước đầy đủ của chúng vào tiến trình được hiển thị mặc dù chúng chưa chính thức hoàn thành. 

Chúng tôi được yêu cầu tính toán phần tối đa có thể có trong tổng kích thước tải xuống có thể đã được hoàn thành tại thời điểm chúng tôi quan sát chính xác`m`các gói đã hoàn thành, dưới sự ràng buộc tối đa`k`các gói được tải xuống đồng thời và thứ tự tải xuống hoàn toàn không xác định được. 

Nói cách khác, chúng ta có thể tự do lựa chọn gói nào đã hoàn tất, gói nào hiện đang được tải xuống và gói nào chưa bắt đầu, miễn là chính xác.`m`đã hoàn thành và nhiều nhất là`k`đang hoạt động. Chúng tôi muốn sắp xếp việc này theo cách tối đa hóa tổng lượng dữ liệu đã tải xuống cho đến nay, bao gồm cả các gói đã hoàn thành và những gói hiện đang được thực hiện (có thể coi là gần như hoàn chỉnh). 

Kích thước đầu vào`n`có thể lên đến`100000`, do đó, bất kỳ giải pháp nào liên quan đến việc sắp xếp đều khả thi, nhưng bất kỳ giải pháp bậc hai hoặc liên quan đến mô phỏng lặp lại trên các hoán vị thì không. Một giải pháp thử tất cả các tập hợp con của các gói đã hoàn thành và đang thực hiện sẽ quá chậm vì điều đó sẽ bùng nổ về mặt tổ hợp. 

Trường hợp cạnh tinh tế xuất hiện khi`k`lớn so với`n`. Trong trường hợp đó, tất cả các gói còn lại sau khi chọn những gói đã hoàn thành vẫn có thể được xem xét đang tiến hành, nghĩa là về cơ bản mọi thứ đều có thể đóng góp vào tổng tiến độ. 

Một trường hợp quan trọng khác là khi`m = 0`. Sau đó, không có gì được hoàn thành đầy đủ, và chỉ đến`k`các gói có thể đóng góp kích thước đầy đủ của chúng dưới dạng quá trình tải xuống đang diễn ra. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể để lựa chọn`m`gói đã hoàn thành và lên đến`k`tải xuống hoạt động từ những cái còn lại. Đối với mỗi cấu hình, chúng tôi sẽ tính toán tổng kích thước đóng góp và theo dõi mức tối đa. Về nguyên tắc, điều này đúng nhưng hoàn toàn không khả thi vì số cách chọn tập hoàn thiện và tập tích cực là tổ hợp trong`n`. 

Nhận xét quan trọng là cấu trúc của bài toán hoàn toàn không phụ thuộc vào thứ tự thời gian. Chúng tôi chỉ quan tâm đến việc có bao nhiêu gói hàng được đếm đầy đủ (`m`) và bao nhiêu gói bổ sung vẫn có thể đóng góp đầy đủ vì chúng đang được tiến hành (`k`). Điều này có nghĩa là chúng ta chỉ đơn giản chọn một tập hợp kích thước`m + k`từ`n`các gói đóng góp đầy đủ vào tiến độ quan sát được. 

Để tối đa hóa tổng tiến độ, chúng ta phải luôn chọn các gói lớn nhất cho cả bộ đã hoàn thiện và bộ đang hoàn thiện, vì mỗi gói đều đóng góp kích thước đầy đủ của nó trong cả hai trường hợp. Không có hình phạt hoặc sự đánh đổi một phần: mỗi gói được chọn đóng góp độc lập và đầy đủ vào tử số của tỷ lệ phần trăm. 

Do đó, chiến lược tối ưu giảm xuống còn việc sắp xếp tất cả các kích cỡ gói hàng theo thứ tự giảm dần và tính tổng kích thước lớn nhất.`m + k`các giá trị. Mẫu số là tổng của tất cả các kích cỡ gói. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chọn các bộ đã hoàn thành và đang thực hiện) | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp và đưa lên hàng đầu`m + k`| O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Hãy để kích thước gói được lưu trữ trong một mảng. 

1. Tính tổng số tiền của tất cả các kích cỡ gói hàng. Đây sẽ là mẫu số của tỷ lệ phần trăm cuối cùng. 
2. Sắp xếp mảng theo thứ tự giảm dần để các gói lớn hơn được xem xét trước. 
3. Lấy cái đầu tiên`m + k`các phần tử từ danh sách được sắp xếp này. Chúng thể hiện sự lựa chọn tốt nhất có thể về các gói có thể đóng góp đầy đủ vào tiến độ quan sát được, dù ở dạng đã hoàn thành hoặc đang trong quá trình thực hiện nhưng được tính là hoàn thành một cách hiệu quả. 
4. Tổng hợp những`m + k`các phần tử để có được kích thước tải xuống tối đa có thể đạt được tại thời điểm quan sát. 
5. Chia số tiền này cho tổng số tiền và nhân với 100 để chuyển nó thành phần trăm. 

Lý do bước 3 hợp lệ là vì chúng tôi có thể tự do gán vai trò cho các gói sau khi thực tế: gói lớn nhất`m`trở thành "hoàn thành" và lớn nhất tiếp theo`k`trở thành "hiện đang tải xuống nhưng được tính đầy đủ một cách hiệu quả". 

### Tại sao nó hoạt động 

Tại thời điểm quan sát, chính xác`m`các gói hàng đang ở trạng thái hoàn thiện và có thể lên tới`k`có thể được tích cực tải xuống. Mỗi lượt tải xuống đang hoạt động có thể đóng góp tối đa kích thước đầy đủ của nó vào tiến trình được hiển thị và các gói đã hoàn thành cũng đóng góp kích thước đầy đủ của chúng. 

Điều này có nghĩa là tổng tiến trình được tính được xác định hoàn toàn bằng cách chọn tối đa`m + k`các gói có kích thước đầy đủ được bao gồm trong tổng số. Vì không có ràng buộc nào về việc gói cụ thể nào phải được hoàn thành, chỉ có số lượng nên mọi sự phân công vai trò đều hợp lệ. Do đó, việc tối đa hóa số tiền sẽ giảm xuống việc chọn kích thước gói hàng lớn nhất hiện có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
sizes = list(map(int, input().split()))

total = sum(sizes)
sizes.sort(reverse=True)

take = min(n, m + k)
best = sum(sizes[:take])

print((best / total) * 100)
```Giải pháp đầu tiên đọc đầu vào và tính toán tổng kích thước của tất cả các gói. Sau đó, nó sắp xếp kích thước gói theo thứ tự giảm dần để chúng ta có thể tham lam chọn những đóng góp có giá trị nhất trước tiên. 

Biến`take`đảm bảo chúng tôi không vượt quá số lượng gói có sẵn khi`m + k > n`. Kết quả cuối cùng được tính dưới dạng phần trăm dấu phẩy động. Sử dụng phép chia float của Python là đủ vì độ chính xác cần thiết chỉ là`1e-4`. 

Một cạm bẫy triển khai phổ biến là quên kẹp`m + k`qua`n`, điều này sẽ dẫn đến lỗi lập chỉ mục hoặc tính tổng không chính xác. Một vấn đề tế nhị khác là phép chia số nguyên, cần phải tránh khi tính tỷ lệ phần trăm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 2
10 25 30 15 20
```Kích thước được sắp xếp:`[30, 25, 20, 15, 10]`Chúng tôi lấy`m + k = 3`các phần tử. 

| Bước | Bộ được chọn | Tổng hợp | 
| --- | --- | --- | 
| Đã hoàn tất + đang trong quá trình lựa chọn | 30, 25, 20 | 75 | 

Tổng số tiền là`100`, vậy đáp án là`75%`. 

Điều này cho thấy chiến lược tốt nhất là coi các gói thầu lớn nhất là đã hoàn thành hoặc gần hoàn thành, nhằm tối đa hóa sự đóng góp. 

### Ví dụ 2 

đầu vào:```
5 0 4
4 2 7 1 3
```Kích thước được sắp xếp:`[7, 4, 3, 2, 1]`Chúng tôi lấy`m + k = 4`các phần tử. 

| Bước | Bộ được chọn | Tổng hợp | 
| --- | --- | --- | 
| Các yếu tố hàng đầu được chọn | 7, 4, 3, 2 | 16 | 

Tổng số tiền là`17`, vậy đáp án là`94.1176...%`. 

Trường hợp này chứng minh rằng khi không có gói nào được hoàn thành, tất cả đóng góp đều đến từ quá trình tải xuống đang diễn ra, nhưng lựa chọn tham lam tương tự vẫn được áp dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, tổng là tuyến tính | 
| Không gian | O(n) | Lưu trữ cho kích thước gói | 

Các ràng buộc cho phép lên đến`100000`các gói, do đó việc sắp xếp thoải mái phù hợp trong giới hạn và mức sử dụng bộ nhớ là tuyến tính theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    total = sum(a)
    a.sort(reverse=True)
    take = min(n, m + k)
    best = sum(a[:take])
    return str((best / total) * 100)

# provided samples (second sample)
assert abs(float(solve("5 0 4\n4 2 7 1 3\n")) - 94.117647059) < 1e-6

# custom: minimum case
assert solve("1 0 1\n5\n").startswith("100")

# all equal values
assert solve("4 2 2\n10 10 10 10\n").startswith("100")

# k = 0 case
assert solve("5 2 0\n1 2 3 4 5\n").startswith("60")

# m + k > n
assert solve("3 2 5\n1 2 3\n").startswith("100")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1 / 5`|`100%`| Độ chính xác của phần tử đơn | 
| tất cả đều bình đẳng |`100%`| tính đối xứng và phân công vai trò | 
|`k = 0`| chỉ một phần | không có đóng góp nào đang diễn ra | 
|`m + k > n`| toàn bộ số tiền | hành vi kẹp | 

## Vỏ cạnh 

Khi nào`m = 0`, thuật toán rút gọn về việc chọn giá trị lớn nhất`k`gói. Lựa chọn đã sắp xếp vẫn áp dụng trực tiếp và kết quả chỉ đơn giản là tổng của phần trên cùng.`k`kích thước trên tổng số. 

Khi`k = 0`, không có đóng góp nào đang diễn ra nên chỉ có phần đóng góp hàng đầu`m`gói được tính. Thuật toán vẫn hoạt động vì`m + k = m`. 

Khi`m + k >= n`, tất cả các gói đều được tính vào tổng nên câu trả lời luôn là`100%`. Bước kẹp đảm bảo chúng tôi không cố gắng vượt quá giới hạn mảng và logic tự nhiên sẽ chuyển sang tính tổng mọi thứ.
