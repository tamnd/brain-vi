---
title: "CF 106394B - Đồng tiền bị nguyền rủa"
description: "Alice tìm thấy một căn phòng chứa n cái rương. Mỗi rương có một giá trị được viết bằng a[i]. Giá trị dương có nghĩa là việc mở rương đó sẽ mang lại cho Alice những đồng xu, trong khi giá trị âm có nghĩa là rương chứa những đồng xu bị nguyền rủa và làm giảm tổng số tiền của cô ấy."
date: "2026-06-25T10:09:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106394
codeforces_index: "B"
codeforces_contest_name: "RUCP x WiCS Mini-Contest"
rating: 0
weight: 106394
solve_time_s: 34
verified: true
draft: false
---

[CF 106394B - Đồng xu bị nguyền rủa](https://codeforces.com/problemset/problem/106394/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Alice tìm thấy một căn phòng chứa`n`rương. Mỗi rương có một giá trị được viết bằng`a[i]`. Giá trị dương có nghĩa là việc mở rương đó sẽ mang lại cho Alice những đồng xu, trong khi giá trị âm có nghĩa là rương chứa những đồng xu bị nguyền rủa và làm giảm tổng số tiền của cô ấy. Alice sở hữu chính xác`k`chìa khóa, vì vậy cô ấy có thể mở nhiều nhất`k`rương. Mục tiêu của cô là chọn những chiếc rương nào sẽ mở sao cho số xu cuối cùng của cô càng lớn càng tốt. 

Đầu vào chứa một số trường hợp thử nghiệm độc lập. Đối với mỗi trường hợp, chúng tôi nhận được số lượng rương, số lượng chìa khóa có sẵn và giá trị của mỗi rương. Đầu ra là số dư tối đa có thể sau khi Alice đưa ra lựa chọn tốt nhất. 

Những hạn chế là đầu mối chính. Tổng số rương trên tất cả các trường hợp thử nghiệm có thể đạt tới`2 * 10^5`, vì vậy giải pháp thử mọi tập hợp con của rương là không thể. Thậm chí kiểm tra tất cả các lựa chọn của`k`rương sẽ theo cấp số nhân. Một giải pháp gần tuyến tính hoặc`O(n log n)`mỗi trường hợp thử nghiệm là cần thiết. 

Các trường hợp cạnh khóa xuất phát từ thực tế là Alice không phải sử dụng mọi khóa. Việc thực hiện bất cẩn có thể cho rằng cô ấy phải luôn mở chính xác`k`rương. 

Ví dụ:```
Input
1
3 2
5 -10 -20

Output
5
```Alice có hai chiếc chìa khóa, nhưng việc mở bất kỳ chiếc rương bị nguyền rủa nào sẽ làm giảm kết quả của cô ấy. Lựa chọn đúng đắn là chỉ mở rương có giá trị`5`. Giải pháp chọn mù quáng hai rương sẽ tạo ra giá trị nhỏ hơn. 

Một trường hợp khác là khi mỗi rương đều bị nguyền rủa:```
Input
1
4 3
-1 -2 -3 -4

Output
0
```Alice bắt đầu với số tiền bằng 0 và có thể đơn giản tránh sử dụng chìa khóa của mình. Sắp xếp các giá trị và luôn lấy giá trị đầu tiên`k`các phần tử không kiểm tra dấu của chúng sẽ khiến câu trả lời là phủ định một cách sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi bộ rương mà Alice có thể mở, tính tổng giá trị của nó và giữ mức tối đa. Điều này đúng vì mọi quyết định có thể đều được xem xét. Tuy nhiên, số lượng tập hợp con là`2^n`, điều này trở nên không thể ngay cả đối với vài chục rương. Với`n = 2 * 10^5`, cách tiếp cận này vượt xa thời gian có sẵn. 

Cách tốt hơn để xem xét vấn đề là tập trung vào những gì góp phần tạo nên câu trả lời. Mở rương có giá trị dương sẽ làm tăng số dư cuối cùng. Mở rương có giá trị âm sẽ giảm giá trị đó. Vì không có yêu cầu phải sử dụng tất cả các khóa nên các giá trị âm không bao giờ có ích. 

Hoạt động hữu ích duy nhất là chọn các giá trị dương lớn nhất. Nếu có ít hơn`k`rương tích cực, Alice lấy tất cả chúng. Nếu có nhiều hơn`k`, cô ấy nên giữ những gì tốt nhất`k`giá trị dương vì mỗi khóa được sử dụng cho giá trị nhỏ hơn sẽ ngăn cản việc nhận phần thưởng lớn hơn. 

Quan sát này biến vấn đề thành một vấn đề sắp xếp. Chúng tôi thu thập tất cả các giá trị dương, sắp xếp chúng từ lớn nhất đến nhỏ nhất và cộng nhiều nhất`k`của họ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

Bước sắp xếp chiếm ưu thế trong thời gian chạy. Phần còn lại của thuật toán chỉ quét mảng và tính tổng các giá trị đã chọn. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và xử lý từng test case một cách độc lập. 
2. Kiểm tra mọi giá trị ở rương và chỉ giữ lại các giá trị lớn hơn 0. Đây là những chiếc rương duy nhất có thể cải thiện khả năng giữ thăng bằng của Alice. 
3. Sắp xếp các giá trị dương còn lại theo thứ tự giảm dần. Những yếu tố đầu tiên sau khi sắp xếp là những lựa chọn có lợi nhất. 
4. Thêm cái đầu tiên`k`giá trị từ danh sách được sắp xếp này hoặc tất cả các giá trị có sẵn nếu có ít hơn`k`. 
5. In tổng kết quả. 

Lý do khiến sự lựa chọn tham lam này có hiệu quả là vì mỗi rương được chọn sẽ sử dụng một chìa khóa. Nếu một giải pháp chứa rương dương nhỏ hơn trong khi rương dương lớn hơn không được chọn, việc hoán đổi chúng sẽ tăng lên hoặc giữ cho tổng số không thay đổi. Việc lặp lại quá trình trao đổi này chỉ để lại những giá trị dương lớn nhất được chọn. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến rằng sau khi xem xét các rương dương có sẵn, các giá trị được chọn luôn là phần thưởng cao nhất có thể có cho các khóa được sử dụng. Rương âm chỉ có thể làm giảm tổng số, vì vậy việc loại bỏ chúng không bao giờ gây tổn hại. Tập được chọn cuối cùng chính xác là tập có tổng lớn nhất có thể. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        positive = [x for x in a if x > 0]
        positive.sort(reverse=True)

        ans.append(str(sum(positive[:k])))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Tính năng hiểu danh sách sẽ lọc ra tất cả các rương bị nguyền rủa vì các giá trị âm không bao giờ có thể là một phần của giải pháp tối ưu. Việc sắp xếp đặt phần thưởng cao nhất lên đầu tiên, phù hợp với lý luận tham lam từ phần thuật toán. 

lát cắt`positive[:k]`cũng là điều kiện biên ngăn cản việc sử dụng nhiều khóa hơn số khóa mà Alice sở hữu. Nếu ít hơn`k`rương có lợi nhuận tồn tại, Python tự động trả về toàn bộ danh sách. 

Tổng sử dụng số nguyên Python, do đó không có rủi ro tràn ngay cả khi giá trị tích lũy vượt quá phạm vi 32 bit hoặc 64 bit. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
Input
3
2 1
5 10
2 1
5 -10
3 3
1 -2 3
```Trường hợp thử nghiệm đầu tiên chọn một rương vì có một chìa khóa. 

| Bước | Giá trị dương | Giá trị được sắp xếp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Đọc rương | 5, 10 | 10, 5 | 0 | 
| Lấy 1 giá trị đầu tiên | 10 | 10 | 10 | 

Thuật toán chọn phần thưởng lớn nhất hiện có. 

Trường hợp thử nghiệm thứ hai chứng minh rằng những chiếc rương bị nguyền rủa bị bỏ qua. 

| Bước | Giá trị dương | Giá trị được sắp xếp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Đọc rương | 5 | 5 | 0 | 
| Lấy 1 giá trị đầu tiên | 5 | 5 | 5 | 

Rương âm không bao giờ được xem xét. 

Đối với trường hợp thử nghiệm thứ ba: 

| Bước | Giá trị dương | Giá trị được sắp xếp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Đọc rương | 1, 3 | 3, 1 | 0 | 
| Lấy 3 giá trị đầu tiên | 3, 1 | 3, 1 | 4 | 

Chỉ có hai rương hữu dụng nên cả hai đều được mở mặc dù Alice có ba chìa khóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các giá trị rương dương chiếm ưu thế trong công việc | 
| Không gian | O(n) | Danh sách các giá trị dương có thể chứa mọi rương | 

Tổng số rương trên tất cả các trường hợp thử nghiệm được giới hạn bởi`2 * 10^5`, do đó việc sắp xếp tất cả các giá trị dương được thu thập vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    res = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        pos = [x for x in a if x > 0]
        pos.sort(reverse=True)
        res.append(str(sum(pos[:k])))

    return "\n".join(res)

# provided samples
assert solution("""3
2 1
5 10
2 1
5 -10
3 3
1 -2 3
""") == """10
5
4""", "samples"

# minimum size
assert solution("""1
1 1
-7
""") == "0", "all negative minimum"

# all equal values
assert solution("""1
5 3
4 4 4 4 4
""") == "12", "equal positives"

# boundary where k exceeds positive count
assert solution("""1
6 5
8 -1 3 -2 0 4
""") == "15", "ignore non-positive values"

# large k with mixed values
assert solution("""1
7 2
-5 100 -10 50 20 1 -1
""") == "150", "take largest positives only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / -7`|`0`| Alice có thể chọn không sử dụng chìa khóa | 
|`1 / 5 3 / 4 4 4 4 4`|`12`| Các giá trị dương bằng nhau được xử lý chính xác | 
|`1 / 6 5 / 8 -1 3 -2 0 4`|`15`| Giá trị âm và 0 bị bỏ qua | 
|`1 / 7 2 / -5 100 -10 50 20 1 -1`|`150`| Chỉ có điều tốt nhất`k`phần thưởng được chọn | 

## Vỏ cạnh 

Khi tất cả các rương đều bị nguyền rủa, danh sách tích cực sẽ trống rỗng. Vì:```
Input
1
4 3
-1 -2 -3 -4
```thuật toán loại bỏ mọi giá trị trong quá trình lọc. Việc sắp xếp không làm gì cả và tổng các giá trị đã chọn là`0`, phù hợp với việc Alice không chọn rương. 

Khi có ít rương tích cực hơn chìa khóa:```
Input
1
5 4
6 -2 3 -8 1
```danh sách được lọc là`[6, 3, 1]`. Sau khi sắp xếp nó vẫn còn`[6, 3, 1]`và việc lấy bốn phần tử đầu tiên chỉ cần lấy cả ba giá trị. Câu trả lời là`10`. 

Khi có nhiều rương có lợi hơn chìa khóa:```
Input
1
5 2
9 7 5 3 1
```danh sách được sắp xếp là`[9, 7, 5, 3, 1]`. Chỉ có hai giá trị đầu tiên được sử dụng, cho`16`. Bất kỳ sự thay thế nào có giá trị nhỏ hơn sẽ làm giảm kết quả, vì vậy lựa chọn tham lam là tối ưu.
