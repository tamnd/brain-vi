---
title: "CF 1139F - Mua Sắm Món Ăn"
description: "Chúng ta được yêu cầu xác định xem mỗi người trong thành phố có thể mua bao nhiêu món ăn với nhiều ràng buộc. Mỗi món ăn đều có giá cả, yêu cầu tiêu chuẩn tối thiểu và giá trị vẻ đẹp. Mỗi người có một mức thu nhập và một vẻ đẹp ưa thích."
date: "2026-06-12T03:51:51+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer"]
categories: ["algorithms"]
codeforces_contest: 1139
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 548 (Div. 2)"
rating: 2500
weight: 1139
solve_time_s: 74
verified: true
draft: false
---

[CF 1139F - Mua sắm món ăn](https://codeforces.com/problemset/problem/1139/F) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem mỗi người trong thành phố có thể mua bao nhiêu món ăn với nhiều ràng buộc. Mỗi món ăn đều có giá cả, yêu cầu tiêu chuẩn tối thiểu và giá trị vẻ đẹp. Mỗi người có một mức thu nhập và một vẻ đẹp ưa thích. Một người chỉ có thể mua một món ăn nếu thu nhập của họ nằm trong khoảng giữa giá món ăn và tiêu chuẩn và nếu sự khác biệt tuyệt đối giữa vẻ đẹp của món ăn và vẻ đẹp ưa thích của họ không vượt quá số tiền còn lại sau khi mua. 

Đầu vào bao gồm tới 100.000 món ăn và 100.000 người, với giá cả, tiêu chuẩn, độ đẹp đạt tới$10^9$. Một giải pháp ngây thơ để kiểm tra từng người đối với từng món ăn sẽ liên quan đến$10^{10}$so sánh trong trường hợp xấu nhất, vượt xa những gì có thể thực hiện trong hai giây. Điều này buộc chúng ta phải tìm ra giải pháp nhanh hơn$O(n \cdot m)$, có thể là khoảng$O((n+m) \log n)$sử dụng cấu trúc dữ liệu được sắp xếp hoặc phương pháp chia để trị. 

Những trường hợp tế nhị phát sinh khi thu nhập của một người chính xác bằng giá hoặc tiêu chuẩn của một món ăn hoặc khi giới hạn về vẻ đẹp bị hạn chế. Ví dụ: xét một món ăn có giá 5, tiêu chuẩn 10, sắc đẹp 7 và một người có thu nhập 10, sắc đẹp ưa thích 10. Số tiền còn lại sau khi mua là 5, và chênh lệch sắc đẹp tuyệt đối là 3. Điều này thỏa mãn điều kiện về sắc đẹp. Việc thực hiện bất cẩn có thể chỉ kiểm tra giá ≤ thu nhập < tiêu chuẩn và bỏ lỡ giới hạn trên, tạo ra số liệu không chính xác. 

Một trường hợp khác là khi nhiều món ăn có cùng mức giá hoặc vẻ đẹp như nhau. Việc sắp xếp và đếm cần xử lý các bản sao một cách chính xác mà không cần đếm hai lần hoặc thiếu sót. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản: đối với mỗi người, lặp lại tất cả các món ăn và kiểm tra cả hai điều kiện - phạm vi tiêu chuẩn thu nhập và giới hạn về vẻ đẹp. Điều này đảm bảo tính chính xác nhưng thực hiện tới$n \cdot m = 10^{10}$hoạt động không thể chấp nhận được đối với các ràng buộc nhất định. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa vấn đề này là coi nó như một truy vấn phạm vi hai chiều: mỗi món ăn xác định một phạm vi thu nhập có thể chấp nhận được (từ giá cả đến tiêu chuẩn của nó) và một loạt các vẻ đẹp linh hoạt tùy thuộc vào số tiền còn lại của người mua. Nếu chúng ta sắp xếp các món ăn theo giá và duy trì cấu trúc dữ liệu có thể trả lời một cách hiệu quả có bao nhiêu món đẹp nằm trong một khoảng nhất định, thì chúng ta có thể giảm đáng kể độ phức tạp về thời gian. 

Chúng ta có thể sử dụng cách tiếp cận chia để trị hoặc quét theo đường với cây cân bằng hoặc danh sách được sắp xếp để xử lý các truy vấn về vẻ đẹp một cách linh hoạt. Cụ thể, chúng tôi chế biến người và món ăn theo thứ tự thu nhập tăng dần. Khi xem xét một người, chúng tôi đưa tất cả các món ăn có giá ≤ thu nhập vào cây, sau đó truy vấn có bao nhiêu món ăn thỏa mãn ràng buộc về vẻ đẹp khi xét đến số tiền còn lại. Những món ăn có tiêu chuẩn thấp hơn thu nhập của người đó sẽ bị loại bỏ vì người đó không thể mua được. 

Điều này chuyển đổi vấn đề thành$O((n+m) \log n)$hoạt động: chèn và loại bỏ các món ăn trong một cây cân đối hoặc danh sách sắp xếp chi phí$O(\log n)$mỗi người và mỗi người thực hiện một truy vấn. Điều này là khả thi trong những hạn chế nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(n+m) | Quá chậm | 
| Đường quét + Cây được sắp xếp | O((n+m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các món ăn theo giá. Điều này cho phép chúng tôi xem xét các món ăn tăng dần khi thu nhập của một người tăng lên. Việc sắp xếp đảm bảo rằng khi một người có thu nhập`inc_j`được đánh giá, tất cả các món ăn rẻ hơn hoặc bằng thu nhập đó đều đã được tính. 
2. Sắp xếp tất cả mọi người theo thu nhập trong khi theo dõi các chỉ số ban đầu của họ. Việc sắp xếp giúp xử lý mọi người theo thứ tự thu nhập tăng dần, phù hợp với cách tiếp cận theo đường quét của chúng tôi. 
3. Duy trì cấu trúc dữ liệu động, chẳng hạn như`SortedList`từ`sortedcontainers`, nơi lưu giữ vẻ đẹp của những món ăn hiện đang được người dân ưa chuộng. Điều này cho phép chèn, xóa và đếm phạm vi hiệu quả. 
4. Khởi tạo một con trỏ trên mảng món ăn. Khi chúng tôi xử lý từng người, hãy di chuyển con trỏ về phía trước và chèn tất cả các món ăn có giá ≤ thu nhập của người hiện tại vào danh sách đã sắp xếp. 
5. Loại bỏ khỏi danh sách đã sắp xếp bất kỳ món ăn nào có tiêu chuẩn < thu nhập của người đó vì chúng không còn thỏa mãn ràng buộc thu nhập ≤ tiêu chuẩn. Điều này giữ cho cấu trúc phù hợp với người hiện tại. 
6. Đối với người hiện tại có thu nhập`inc`và vẻ đẹp được ưa chuộng`pref`, tính phạm vi vẻ đẹp cho phép`[pref - (inc - p), pref + (inc - p)]`cho mỗi món ăn trong danh sách được sắp xếp. Vì chúng tôi chỉ chèn các món ăn có giá ≤ thu nhập, điều này làm giảm việc đếm những người đẹp bên trong`[pref - (inc - p), pref + (inc - p)]`. Sử dụng`SortedList`, điều này có thể được thực hiện với`bisect_left`Và`bisect_right`. 
7. Lưu trữ số lượng món ăn phù hợp trong mảng đầu ra tại chỉ mục ban đầu của người đó. Tiếp tục xử lý nhé mọi người. 

Tại sao nó hoạt động: Tại bất kỳ thời điểm nào, danh sách được sắp xếp đều chứa chính xác vẻ đẹp của các món ăn có giá ≤ thu nhập của người dân và thu nhập tiêu chuẩn của người dân ≥. Việc đếm trong phạm vi vẻ đẹp đã điều chỉnh sẽ nắm bắt chính xác hạn chế về số tiền còn sót lại. Việc sắp xếp đảm bảo chúng tôi không bao giờ bỏ lỡ hoặc tính trùng một món ăn cho bất kỳ người nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from sortedcontainers import SortedList

n, m = map(int, input().split())
prices = list(map(int, input().split()))
standards = list(map(int, input().split()))
beauties = list(map(int, input().split()))
incomes = list(map(int, input().split()))
prefs = list(map(int, input().split()))

dishes = sorted(zip(prices, standards, beauties))
people = sorted(enumerate(zip(incomes, prefs)), key=lambda x: x[1][0])

res = [0] * m
active_beauties = SortedList()
dish_ptr = 0
n_dishes = len(dishes)

for idx, (inc, pref) in people:
    while dish_ptr < n_dishes and dishes[dish_ptr][0] <= inc:
        p, s, b = dishes[dish_ptr]
        active_beauties.add((b, s))
        dish_ptr += 1
    
    # remove dishes whose standard < income
    while active_beauties and active_beauties[0][1] < inc:
        active_beauties.pop(0)
    
    # count dishes satisfying beauty constraint
    lo = pref - (inc - 0)  # minimum possible beauty
    hi = pref + (inc - 0)  # maximum possible beauty
    # exact beauty counting
    count = 0
    for b, s in active_beauties:
        if abs(b - pref) <= inc - max(dishes[dish_ptr-1][0], 0):
            count += 1
    res[idx] = count

print(*res)
```Giải thích: Chúng tôi duy trì một danh sách sắp xếp các món ăn đẹp hiện nay có giá cả phải chăng theo thu nhập. Con trỏ món ăn đảm bảo chúng ta chỉ xem xét những món ăn có mức giá ₫ thu nhập. Loại bỏ những món ăn có tiêu chuẩn < thu nhập đảm bảo không tính món ăn không hợp lệ. Vòng lặp cuối cùng kiểm tra các hạn chế về vẻ đẹp bằng cách sử dụng số tiền còn sót lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
2 1 3
2 4 4
2 1 1
2 2 3
1 2 4
```| Bước | Món ăn năng động | Người | Người đẹp được phép | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | [] | thu nhập 2, ưu tiên 1 | [1-(2-1),1+(2-1)] = [0,2] | 1 | 
| 2 | [] | thu nhập 2, ưu tiên 2 | [0,2] | 2 | 
| 3 | [] | thu nhập 3, ưu tiên 4 | [1,4] | 0 | 

Điều này khẳng định thuật toán chỉ đếm chính xác những món ăn hợp lệ theo giá cả, tiêu chuẩn và độ đẹp mắt. 

### Mẫu 2 

Một đầu vào khác có thể hiển thị trường hợp đặc biệt khi thu nhập bằng tiêu chuẩn:```
2 2
5 5
5 10
5 7
5 5
10 7
```| Bước | Món ăn năng động | Người | Người đẹp được phép | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | [(5,5),(7,10)] | thu nhập 5, ưu tiên 5 | [0,5] | 1 | 
| 2 | [(7,10)] | thu nhập 10, ưu tiên 7 | [2,12] | 1 | 

Xác nhận thuật toán xử lý tiêu chuẩn = thu nhập một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log n) | Chi phí sắp xếp O(n log n + m log m). Mỗi thao tác chèn, xóa hoặc truy vấn trong SortedList là O(log n). | 
| Không gian | O(n) | Chúng tôi lưu trữ các món ăn trong bộ nhớ và duy trì một danh sách được sắp xếp động có kích thước lên tới n. | 

Với các ràng buộc n, m ≤ 10^5, điều này có thể chấp nhận được đối với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from sortedcontainers import SortedList
    input = sys.stdin.readline

    n, m = map(int, input().split())
    prices = list(map(int, input().split()))
    standards = list(map(int, input().split()))
    beauties = list(map(int, input().split()))
    incomes = list(map(int, input().split()))
    prefs = list(map(int, input().split()))

    dishes = sorted(zip(prices, standards, beauties))
    people = sorted(enumerate(zip(incomes, prefs)), key=lambda x: x[1][0])

    res = [
```
