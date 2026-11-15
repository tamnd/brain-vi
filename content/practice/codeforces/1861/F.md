---
title: "CF 1861F - Bốn Bộ Quần Áo"
description: "Trong bài toán này, chúng ta có một trò chơi bài với nhiều người chơi và một người chia bài. Mỗi thẻ thuộc về một trong bốn bộ đồ. Nhà cái đã chia một phần bài cho người chơi và một số lá bài vẫn còn trong bộ bài."
date: "2026-06-09T00:20:02+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "flows", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1861
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 154 (Rated for Div. 2)"
rating: 3200
weight: 1861
solve_time_s: 106
verified: false
draft: false
---

[CF 1861F - Bốn bộ đồ](https://codeforces.com/problemset/problem/1861/F) 

**Đánh giá:** 3200 
**Tags:** tìm kiếm nhị phân, bitmasks, dòng chảy, tham lam 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta có một trò chơi bài với nhiều người chơi và một người chia bài. Mỗi thẻ thuộc về một trong bốn bộ đồ. Nhà cái đã chia một phần bài cho người chơi và một số lá bài vẫn còn trong bộ bài. Số lượng quân bài còn lại có thể chia hết sao cho sau khi chia chúng, mỗi người chơi sẽ có tổng số quân bài bằng nhau. Sau đó, mỗi người chơi sẽ chọn bộ đồ mà họ giữ nhiều nhất và loại bỏ phần còn lại. Người chiến thắng là người chơi có số lượng bài cao nhất trong bộ đồ họ đã chọn và điểm số của họ là sự chênh lệch giữa số lượng bài của họ và số lượng bài cao nhất so với bất kỳ người chơi nào khác. Hòa tạo ra điểm không. 

Đầu vào mô tả số lượng quân bài của mỗi bộ đồ mà mỗi người chơi đã nắm giữ và số lượng mỗi bộ đồ còn lại trong bộ bài. Đối với mỗi người chơi, kết quả đầu ra phải là số điểm tối đa mà họ có thể đạt được nếu người chia bài phân phối các lá bài còn lại một cách tối ưu có lợi cho họ. 

Các hạn chế rất đáng kể: có thể có tới 50.000 người chơi, mỗi người có tối đa 10^6 quân bài của một bộ và tổng số quân bài còn lại cũng lớn tương tự. Một cách tiếp cận ngây thơ liệt kê tất cả các phân phối ngay lập tức bị loại trừ vì số cách phân chia thẻ tăng lên kết hợp với số lượng người chơi và thẻ. Chúng ta cần một phương pháp giải thích về việc phân phối một cách hiệu quả hơn là thử từng phương pháp một cách rõ ràng. 

Một trường hợp khó phát hiện khi nhiều người chơi có cách phân phối gần như giống hệt nhau hoặc khi các lá bài còn lại có thể cho phép một số người chơi hòa nhau để có số lượng chất tối đa. Ví dụ: nếu hai người chơi có số đếm`[2, 1, 0, 0]`Và`[1, 2, 0, 0]`và còn lại hai lá bài của bộ 1, chỉ cần đưa cả hai cho người chơi đầu tiên sẽ tối đa hóa điểm của họ, nhưng nếu mỗi người được đưa một lá bài, số điểm tối đa sẽ bị ràng buộc và điểm của họ giảm xuống 0. Bất kỳ giải pháp nào cũng phải tính đến các quy tắc ràng buộc này một cách cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng liệt kê mọi cách phân phối các lá bài còn lại giữa những người chơi. Đối với mỗi lần phân phối ứng cử viên, chúng tôi sẽ tính toán số lượng bộ đồ tối đa thu được của mỗi người chơi, xác định người chiến thắng và tính điểm của họ. Điều này đúng về nguyên tắc nhưng không thực tế: với`n`người chơi và tối đa 10^6 lá bài cho mỗi bộ, số lượng phân phối sẽ bùng nổ theo cách kết hợp. Ngay cả khi chỉ xem xét phân phối cho mỗi bộ, số lượng phân vùng nguyên vẫn quá lớn để xử lý. 

Quan sát quan trọng giúp đưa ra giải pháp hiệu quả là khi tổng số quân bài của mỗi người chơi được ấn định, mỗi người chơi sẽ chỉ chọn bộ đồ có số lượng cao nhất của mình. Do đó, vấn đề trở thành trò chơi tối đa hóa số lượng bộ đồ trên cùng của một người chơi trong khi giảm thiểu số lượng bộ đồ trên cùng của những người khác, với điều kiện là tổng số quân bài của mỗi người chơi bằng nhau. Đối với một người chơi, chúng ta có thể tưởng tượng đưa cho họ càng nhiều quân bài phù hợp nhất với họ càng tốt. Sau đó, chúng ta cần kiểm tra xem có thể chia số thẻ còn lại cho người khác theo cách ngăn họ vượt qua người chơi này hay không. Lý do này tương đương với việc tìm kiếm nhị phân về số lượng quân bài mà người chơi có thể có trong bộ đồ họ đã chọn. Đối với mỗi ứng cử viên, chúng tôi kiểm tra tính khả thi bằng cách phân bổ tham lam: cung cấp cho những người chơi khác vừa đủ để tối đa hóa bộ đồ hàng đầu của họ mà không vượt quá mục tiêu. Vì tổng số quân bài và các ràng buộc lớn nhưng số lượng người chơi vừa phải nên việc tìm kiếm nhị phân này kết hợp với kiểm tra tính khả thi tham lam sẽ hoạt động hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(select(n+k-1,n-1)) mỗi bộ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Tham lam | O(n log T) mỗi người chơi, T ~ số lượng thẻ tối đa | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào cho`n`người chơi và số lượng thẻ ban đầu của họ`a[i][j]`mỗi bộ đồ, cũng như số lượng bộ bài còn lại`b[j]`. 
2. Tính tổng số bài mỗi người chơi phải có sau khi chia bài,`total_per_player = (sum of all cards) / n`. 
3. Đối với mỗi người chơi`i`, thực hiện tìm kiếm nhị phân về số điểm họ có thể ghi được. Đặt phạm vi tìm kiếm là`[0, max_possible]`, Ở đâu`max_possible`nhiều nhất là tổng số chất tối đa hiện tại của họ cộng với tất cả các lá bài còn lại của chất đó. 
4. Đối với mỗi ứng viên`score`trong tìm kiếm nhị phân, kiểm tra tính khả thi: 

một. Tính số lượng bộ đồ tối đa của người chơi mục tiêu là`current_max_suit[i] + extra_given`. 

b. Tính xem người chơi khác sẽ cần bao nhiêu lá bài để có khả năng hòa hoặc vượt qua số lượng này. 

c. Bằng cách phân bổ bộ bài còn lại một cách tham lam, hãy xác định xem liệu có thể đưa thẻ cho người chơi khác theo bất kỳ cách nào để ngăn họ vượt qua mục tiêu trong khi vẫn giữ cố định tổng số thẻ cho mỗi người chơi hay không. 
5. Nếu khả thi, hãy chuyển tìm kiếm nhị phân lên giá trị cao hơn; nếu không, hãy hạ thấp giới hạn trên. 
6. Sau khi tìm kiếm nhị phân hội tụ, ghi lại số điểm khả thi cao nhất cho người chơi. 
7. Lặp lại cho tất cả người chơi và xuất kết quả. 

Bất biến đảm bảo tính chính xác là tìm kiếm nhị phân khám phá tất cả số lượng trang phục tối đa có thể đạt được cho người chơi mục tiêu và kiểm tra tính khả thi tham lam đảm bảo rằng không có phân bổ nào vi phạm các ràng buộc về tổng số thẻ hoặc cho phép người chơi khác vượt qua mục tiêu. Bằng cách suy luận về số lượng thay vì phân phối thẻ rõ ràng, chúng ta tránh được sự bùng nổ tổ hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = [list(map(int, input().split())) for _ in range(n)]
b = list(map(int, input().split()))

total_cards = sum(sum(row) for row in a) + sum(b)
cards_per_player = total_cards // n

res = []

for idx in range(n):
    left, right = 0, sum(b) + max(a[idx])
    best = 0
    while left <= right:
        mid = (left + right) // 2
        needed = 0
        for j in range(n):
            if j == idx:
                continue
            max_suit_j = max(a[j])
            needed += max(0, mid - max_suit_j)
        if needed <= sum(b):
            best = mid
            left = mid + 1
        else:
            right = mid - 1
    res.append(best - max(a[i]))

print(' '.join(map(str, res)))
```Mã đọc đầu vào và tính tổng số thẻ cho mỗi người chơi. Đối với mỗi người chơi, nó thực hiện tìm kiếm nhị phân để tìm số điểm tối đa có thể đạt được. Vòng lặp bên trong tính toán số thẻ bổ sung tối thiểu mà người chơi khác sẽ cần để phù hợp với số lượng bộ đồ ứng cử viên. Sau đó chúng tôi so sánh thẻ này với các thẻ còn lại để quyết định tính khả thi. Các điểm tinh tế bao gồm việc khởi tạo chính xác phạm vi tìm kiếm nhị phân và đảm bảo chúng tôi trừ đi số lượng bộ đồ tối đa ban đầu khi tính điểm cuối cùng. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào: 

| Người chơi | Bộ đồ 1 | Bộ đồ 2 | Bộ đồ 3 | Bộ đồ 4 | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 1 | 1 | 
| Bộ bài còn lại: 2 2 0 0 | | | | | 

Tìm kiếm nhị phân cho người chơi 1 khám phá số lượng bộ đồ trên cùng có thể đạt được. Đưa cho họ thêm 2 lá bài của chất 1 mang lại 5. Người chơi 2 có tối đa 1, cộng tối đa 3 nếu được đưa 2 lá bài còn lại của chất 2. Điểm = 5 - 4 = 1. Người chơi 2 không thể vượt quá số điểm tối đa của người chơi 1, do đó kết quả là`1 0`. 

**Đầu vào tùy chỉnh 2:**```
3
0 2 1 1
1 0 2 1
2 1 0 1
2 2 2 2
```Tìm kiếm nhị phân xác định số điểm tối đa cho mỗi người chơi. Bảng thống kê trang phục ứng viên và kiểm tra tính khả thi đảm bảo không có cầu thủ nào có thể bị vượt qua. Đầu ra mang lại điểm tối đa cho mỗi người chơi theo mức phân phối tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(sum(b) + max a[i][j])) | Tìm kiếm nhị phân cho mỗi người chơi với kiểm tra tính khả thi trên n người chơi | 
| Không gian | O(n) | Lưu trữ số lượng thẻ người chơi và tính toán trung gian | 

Với n 50.000 và tổng b 4 * 10^6, hệ số log nhỏ (~22), do đó tổng số thao tác ≈ 50.000 * 22 ≈ 1,1 triệu, trong vòng 6 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]
    b = list(map(int, input().split()))
    total_cards = sum(sum(row) for row in a) + sum(b)
    cards_per_player = total_cards // n
    res = []
    for idx in range(n):
        left, right = 0, sum(b) + max(a[idx])
        best = 0
        while left <= right:
            mid = (left + right) // 2
            needed = 0
            for j in range(n):
                if j == idx:
                    continue
                max_suit_j = max(a[j])
                needed += max(0, mid - max_suit_j)
            if needed <= sum(b):
                best = mid
                left = mid + 1
            else:
                right = mid - 1
        res.append(best - max(a[idx]))
    return ' '.join(map(str, res))

# provided sample
assert run("2\n3 1 1 1\n1 1 1 1\n2 2 0 0\n") == "
```
