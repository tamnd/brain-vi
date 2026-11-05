---
title: "CF 119B - Trước kỳ thi"
description: "Chúng ta có n định lý và mỗi định lý được gán một giá trị thành thạo. Bài thi có đúng k thẻ khác nhau. Mỗi thẻ chứa chính xác tầng (n / k) định lý riêng biệt và không có định lý nào xuất hiện trong nhiều hơn một thẻ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 119
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 90"
rating: 1900
weight: 119
solve_time_s: 122
verified: true
draft: false
---

[CF 119B - Trước kỳ thi](https://codeforces.com/problemset/problem/119/B) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, triển khai, sắp xếp 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`định lý, và mỗi định lý có một giá trị thành thạo được gán cho nó. Đề thi có nội dung chính xác`k`thẻ khác nhau. Mỗi thẻ chứa chính xác`floor(n / k)`định lý riêng biệt và không có định lý nào xuất hiện trong nhiều hơn một thẻ. Một số định lý có thể vẫn không được sử dụng nếu`n`không chia hết cho`k`. 

Một số học sinh đã tiết lộ chính xác các bộ định lý trên thẻ của mình. Những lá bài được tiết lộ này được đảm bảo là có giá trị và những lá bài giống hệt nhau có thể xuất hiện nhiều lần vì các học sinh khác nhau có thể rút cùng một lá bài. 

Vasya có thể nhận được bất kỳ một trong những`k`thẻ. Một số thẻ đã được biết đến từ các báo cáo, trong khi các thẻ còn lại chưa được biết. Chúng tôi phải tính toán mức độ thông thạo trung bình nhỏ nhất và lớn nhất có thể có trong số tất cả các thẻ mà Vasya vẫn có thể nhận được, có tính đến mọi lần hoàn thành hợp lệ của các thẻ chưa xác định. 

Những hạn chế là rất nhỏ. Cả hai`n`và số lượng thẻ báo cáo nhiều nhất`100`. Điều này có nghĩa là ngay cả các thuật toán bậc hai hoặc bậc ba cũng hoàn toàn an toàn. Việc tìm kiếm toàn diện trên tất cả các phân vùng định lý vẫn không thể thực hiện được vì số cách để phân vùng tập hợp tăng lên cực kỳ nhanh, nhưng bất cứ điều gì dựa trên việc sắp xếp hoặc gán tham lam đều không đáng kể trong giới hạn. 

Phần khó khăn là hiểu được thông tin nào thực sự quan trọng. Chúng ta không biết chính xác các quân bài chưa biết, chỉ biết mỗi định lý thuộc về nhiều nhất một quân bài và mỗi quân bài có kích thước cố định. Việc thực hiện bất cẩn có thể cho rằng mọi định lý vô hình đều có thể được kết hợp một cách tùy ý với mọi định lý vô hình khác. Điều đó là sai vì các thẻ phải tạo thành một phân vùng. 

Hãy xem xét ví dụ này:```
6 2
1 2 3 100 100 100
1
1 2 3
```Mỗi thẻ có kích thước`3`. Một thẻ đã được biết đến:`{1,2,3}`. Thẻ còn lại phải có`{4,5,6}`. Tối thiểu và tối đa đều là`100`. Nếu chúng ta bỏ qua cấu trúc phân vùng và thử các tập hợp con tùy ý, chúng ta có thể nghĩ sai về các giá trị trung bình như`(1+2+100)/3`là có thể. 

Một trường hợp tinh vi khác xảy ra khi một số thẻ được lặp lại trong báo cáo:```
4 2
1 2 10 20
2
1 2
2 1
```Hai báo cáo này mô tả cùng một thẻ. Vẫn chỉ có một thẻ được biết đến, không phải hai. Một giải pháp có lỗi tính cả hai một cách độc lập sẽ cho rằng không còn thẻ nào chưa xác định. 

Trường hợp cạnh quan trọng cuối cùng là khi`n`không chia hết cho`k`.```
7 3
1 2 3 4 5 6 7
0
```Mỗi kích thước thẻ là`2`, vậy chỉ`6`định lý được sử dụng trong thẻ. Một định lý vẫn chưa được sử dụng. Chiến lược tối ưu để tối đa hóa giá trị trung bình là loại bỏ định lý nhỏ nhất và tạo một thẻ từ cặp lớn nhất còn lại. Để giảm thiểu, thay vào đó chúng tôi loại bỏ định lý lớn nhất. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra mọi phân vùng hợp lệ của các định lý vào các thẻ, sau đó tính toán tất cả các mức trung bình có thể có của thẻ. Điều này hoạt động về mặt khái niệm vì các ràng buộc rất nhỏ, nhưng số lượng phân vùng sẽ bùng nổ về mặt tổ hợp. Chia đều`100`các phần tử thành các nhóm có kích thước xung quanh`50`có kích thước lớn về mặt thiên văn. Không có số lượng cắt tỉa tiết kiệm phương pháp này. 

Quan sát quan trọng là thành phần chính xác của tất cả các thẻ chưa biết không quan trọng. Chúng tôi chỉ quan tâm đến mức trung bình nhỏ nhất và lớn nhất có thể có của một thẻ. 

Giả sử mỗi thẻ chứa`m = floor(n / k)`định lý. Một số định lý đã được sửa chữa vì chúng xuất hiện trong các thẻ đã biết. Các định lý còn lại được phân phối miễn phí giữa các thẻ chưa biết theo bất kỳ cách hợp lệ nào. 

Để giảm thiểu mức trung bình có thể có của Vasya, chúng tôi muốn một thẻ chưa biết chứa các giá trị định lý nhỏ nhất có sẵn. Vì tất cả các thẻ chưa biết đều có cùng kích thước`m`, mức trung bình tối thiểu có thể có giữa các thẻ chưa biết chỉ đơn giản là mức trung bình của`m`giá trị định lý nhỏ nhất chưa được sử dụng. 

Tương tự, mức trung bình tối đa có thể có giữa các thẻ chưa biết là mức trung bình của`m`giá trị định lý không được sử dụng lớn nhất. 

Chúng ta cũng phải so sánh với những lá bài đã biết, vì Vasya cũng có thể nhận được một trong những lá bài đó. Mức trung bình của họ là cố định và có thể đạt được. 

Điều này làm giảm vấn đề đối với việc ghi sổ đơn giản: 

1. Xác định tất cả các thẻ đã biết riêng biệt. 
2. Đánh dấu tất cả các định lý xuất hiện trong các thẻ đó là đã được sử dụng. 
3. Tính trung bình của các thẻ đã biết. 
4. Sắp xếp các giá trị định lý không sử dụng. 
5. Xây dựng thẻ chưa biết tốt nhất và tồi tệ nhất có thể một cách tham lam từ những thái cực. 

Sự lựa chọn tham lam có hiệu quả vì không có ràng buộc bổ sung nào giữa các quân bài chưa biết ngoài sự rời rạc. Nếu chúng ta muốn một thẻ càng nhỏ càng tốt thì việc lấy các giá trị sẵn có nhỏ nhất trên toàn cầu luôn là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`, sau đó tính`m = n // k`, đó là số lượng định lý trên mỗi thẻ. 
2. Đọc tất cả các giá trị thành thạo định lý vào một mảng. 
3. Xử lý phiếu báo cáo. Vì cùng một thẻ có thể xuất hiện nhiều lần theo các thứ tự khác nhau, hãy sắp xếp từng cách trình bày thẻ và lưu trữ nó trong một bộ để chỉ giữ các thẻ riêng biệt. 
4. Đối với mỗi thẻ riêng biệt đã biết: 

1. Tính trình độ trung bình của nó. 
2. Cập nhật các câu trả lời tối thiểu và tối đa toàn cầu bằng cách sử dụng mức trung bình này. 
3. Đánh dấu tất cả các định lý của nó là đã được sử dụng. 

Những thẻ này được đảm bảo là có thể thực hiện được vì chúng đã được quan sát rõ ràng. 
5. Tập hợp tất cả các giá trị định lý chưa sử dụng vào một mảng riêng và sắp xếp nó. 
6. Hãy để`remaining_cards`là số thẻ chưa biết:`remaining_cards = k - number_of_distinct_known_cards`. 
7. Nếu có ít nhất một thẻ không xác định: 

1. Giá trị trung bình của lá bài chưa biết nhỏ nhất có thể đạt được bằng cách lấy`m`giá trị nhỏ nhất chưa sử dụng. 
2. Giá trị trung bình của lá bài chưa biết lớn nhất có thể đạt được bằng cách lấy`m`giá trị không sử dụng lớn nhất. 
3. Cập nhật các câu trả lời tối thiểu và tối đa chung với các giá trị này. 
8. In giá trị tối thiểu và tối đa cuối cùng. 

### Tại sao nó hoạt động 

Các thẻ đã biết là cố định và phải giữ nguyên chính xác như được quan sát. Mọi định lý chưa sử dụng chỉ thuộc về các thẻ chưa biết và hạn chế duy nhất đối với các thẻ chưa biết là chúng phân chia các định lý chưa sử dụng thành các nhóm có kích thước`m`. 

Để giảm thiểu trung bình cộng của một số thẻ chưa biết, bất kỳ định lý nào có trong thẻ đó phải càng nhỏ càng tốt. Việc thay thế bất kỳ định lý đã chọn nào bằng một định lý nhỏ hơn chưa được sử dụng chỉ có thể làm giảm giá trị trung bình. Việc lặp lại đối số trao đổi này cuối cùng sẽ tạo ra thẻ bao gồm phần nhỏ nhất trên toàn cầu`m`những giá trị không được sử dụng. 

Đối số tương tự được áp dụng đối xứng cho mức tối đa bằng cách sử dụng các giá trị lớn nhất. 

Bởi vì bất kỳ sự phân chia nào của các định lý không được sử dụng thành các nhóm có kích thước bằng nhau đều hợp lệ, nên các thẻ cực trị này luôn có thể xây dựng được. Do đó, thuật toán tính toán chính xác mức trung bình tối thiểu và tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    q = int(input())

    unique_cards = set()

    for _ in range(q):
        card = tuple(sorted(map(int, input().split())))
        unique_cards.add(card)

    m = n // k

    used = [False] * n

    INF = float('inf')
    mn = INF
    mx = -INF

    for card in unique_cards:
        total = 0

        for x in card:
            total += a[x - 1]
            used[x - 1] = True

        avg = total / m

        mn = min(mn, avg)
        mx = max(mx, avg)

    remaining = []

    for i in range(n):
        if not used[i]:
            remaining.append(a[i])

    remaining.sort()

    unknown_cards = k - len(unique_cards)

    if unknown_cards > 0:
        low_avg = sum(remaining[:m]) / m
        high_avg = sum(remaining[-m:]) / m

        mn = min(mn, low_avg)
        mx = max(mx, high_avg)

    print(f"{mn:.10f} {mx:.10f}")

solve()
```Chi tiết quan trọng đầu tiên là loại bỏ các thẻ được báo cáo trùng lặp. Hai học sinh có thể nhận được cùng một thẻ và thứ tự định lý bên trong các dòng đầu vào là tùy ý. Sắp xếp các chỉ số định lý và lưu trữ chúng dưới dạng bộ dữ liệu trong một tập hợp sẽ mang lại một biểu diễn chuẩn. 

Điểm tinh tế thứ hai là chỉ đánh dấu các định lý đã sử dụng từ các thẻ riêng biệt. Nếu chúng tôi xử lý các báo cáo trùng lặp một cách độc lập, chúng tôi có thể nghĩ sai rằng nhiều định lý được cố định hơn thực tế. 

Các giá trị định lý chưa được sử dụng còn lại đủ để mô tả đặc điểm của mọi lá bài chưa biết. Vì tất cả các thẻ chưa biết đều có kích thước giống nhau nên giá trị trung bình tối thiểu đến từ giá trị nhỏ nhất`m`giá trị và giá trị tối đa từ giá trị lớn nhất`m`. 

Thuật toán sử dụng phép chia dấu phẩy động trực tiếp vì độ chính xác yêu cầu chỉ`1e-6`. của Python`float`là quá đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 3
7 15 0 19 10 5 12
2
1 6
7 4
```Đây`m = 7 // 3 = 2`. 

Các thẻ được biết đến khác biệt là`{1,6}`Và`{4,7}`. 

| Bước | Các định lý đã sử dụng | Trung bình được biết đến | Tối thiểu hiện tại | Tối đa hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | {} | - | thông tin | -inf | 
| Thẻ {1,6} | {1,6} | (7+5)/2 = 6 | 6 | 6 | 
| Thẻ {4,7} | {1,4,6,7} | (19+12)/2 = 15,5 | 6 | 15,5 | 

Các giá trị định lý không được sử dụng là:```
15 0 10
```Đã sắp xếp:```
0 10 15
```Thẻ không xác định vẫn tồn tại bởi vì chỉ`2`các thẻ riêng biệt được biết đến từ`3`. 

| Tính toán | Kết quả | 
| --- | --- | 
| Trung bình tối thiểu chưa biết | (0 + 10) / 2 = 5 | 
| Trung bình tối đa chưa biết | (10 + 15) / 2 = 12,5 | 

Câu trả lời cuối cùng:```
5.0000000000 15.5000000000
```Dấu vết này chứng tỏ rằng cả những quân bài đã biết và những quân bài chưa biết đều phải được xem xét. Thẻ tốt nhất có thể đã tồn tại trong số những thẻ được quan sát. 

### Ví dụ 2 

đầu vào:```
7 3
1 2 3 4 5 6 7
0
```Không có thẻ nào được biết đến.`m = 2`. 

Các giá trị không được sử dụng sau khi sắp xếp:```
1 2 3 4 5 6 7
```| Tính toán | Kết quả | 
| --- | --- | 
| Trung bình tối thiểu có thể | (1 + 2) / 2 = 1,5 | 
| Trung bình tối đa có thể | (6 + 7) / 2 = 6,5 | 

Một định lý vẫn chưa được sử dụng vì chỉ`6`định lý tham gia vào thẻ. 

Câu trả lời cuối cùng:```
1.5000000000 6.5000000000
```Ví dụ này chứng tỏ tại sao các định lý còn sót lại phải được bỏ qua một cách chính xác khi`n`không chia hết cho`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các giá trị định lý không sử dụng chiếm ưu thế | 
| Không gian | O(n) | Mảng đã sử dụng và các giá trị còn lại | 

Với`n ≤ 100`, giải pháp này cực kỳ nhanh chóng. Ngay cả một thuật toán kém hiệu quả hơn nhiều cũng có thể vượt qua một cách dễ dàng, vì vậy việc triển khai tập trung vào sự rõ ràng và chính xác. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    q = int(input())

    unique_cards = set()

    for _ in range(q):
        card = tuple(sorted(map(int, input().split())))
        unique_cards.add(card)

    m = n // k

    used = [False] * n

    INF = float('inf')
    mn = INF
    mx = -INF

    for card in unique_cards:
        total = 0

        for x in card:
            total += a[x - 1]
            used[x - 1] = True

        avg = total / m

        mn = min(mn, avg)
        mx = max(mx, avg)

    remaining = []

    for i in range(n):
        if not used[i]:
            remaining.append(a[i])

    remaining.sort()

    unknown_cards = k - len(unique_cards)

    if unknown_cards > 0:
        low_avg = sum(remaining[:m]) / m
        high_avg = sum(remaining[-m:]) / m

        mn = min(mn, low_avg)
        mx = max(mx, high_avg)

    return f"{mn:.10f} {mx:.10f}"

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""7 3
7 15 0 19 10 5 12
2
1 6
7 4
"""
) == "5.0000000000 15.5000000000"

# minimum-size input
assert run(
"""1 1
42
0
"""
) == "42.0000000000 42.0000000000"

# duplicate known cards
assert run(
"""4 2
1 2 10 20
2
1 2
2 1
"""
) == "1.5000000000 15.0000000000"

# all equal values
assert run(
"""6 3
5 5 5 5 5 5
0
"""
) == "5.0000000000 5.0000000000"

# leftover unused theorem
assert run(
"""7 3
1 2 3 4 5 6 7
0
"""
) == "1.5000000000 6.5000000000"

print("All tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 42`|`42 42`| Đầu vào nhỏ nhất có thể | 
| Báo cáo trùng lặp |`1.5 15`| Chống trùng lặp các thẻ đã biết | 
| Tất cả các giá trị bằng nhau |`5 5`| Mức trung bình ổn định bất kể nhóm | 
|`n % k != 0`|`1.5 6.5`| Xử lý đúng các định lý chưa sử dụng | 

## Vỏ cạnh 

### Báo cáo thẻ trùng lặp 

đầu vào:```
4 2
1 2 10 20
2
1 2
2 1
```Mỗi kích thước thẻ là`2`. 

Cả hai báo cáo đều mô tả cùng một thẻ sau khi sắp xếp. Thuật toán chỉ lưu trữ một thẻ riêng biệt. 

Trung bình thẻ đã biết:```
(1 + 2) / 2 = 1.5
```Các giá trị không được sử dụng:```
10 20
```Trung bình thẻ không xác định:```
15
```Câu trả lời cuối cùng:```
1.5000000000 15.0000000000
```Nếu không loại bỏ sự trùng lặp, chương trình sẽ nhầm tưởng rằng tất cả các thẻ đều đã được biết. 

### Định lý còn sót lại chưa sử dụng 

đầu vào:```
7 3
1 2 3 4 5 6 7
0
```Mỗi thẻ chứa`2`định lý, vậy chỉ`6`định lý tham gia vào thẻ. 

Thuật toán sắp xếp các giá trị không sử dụng và chỉ lấy giá trị nhỏ nhất hoặc lớn nhất`2`các giá trị khi xây dựng các thẻ cực trị. Một định lý đương nhiên bị bỏ qua vì nó không thể thuộc về bất kỳ lá bài nào. 

tối thiểu:```
(1 + 2) / 2 = 1.5
```Tối đa:```
(6 + 7) / 2 = 6.5
```Điều này phù hợp với các phân vùng khả thi thực tế. 

### Tất cả các thẻ đã được biết đến 

đầu vào:```
6 3
1 2 3 4 5 6
3
1 2
3 4
5 6
```Tất cả ba thẻ riêng biệt đã được sửa. 

Trung bình của họ là:```
1.5, 3.5, 5.5
```Không còn quân bài nào chưa biết nên thuật toán bỏ qua bước xây dựng tham lam. 

Câu trả lời cuối cùng:```
1.5000000000 5.5000000000
```Điều này xác nhận giải pháp không vô tình sử dụng các thẻ không xác định không tồn tại.
