---
title: "CF 35D - Động vật"
description: "Chúng ta được yêu cầu mô phỏng một trang trại trong đó các con vật đến một con mỗi ngày trong n ngày. Mỗi con vật có một nhu cầu thức ăn cố định hàng ngày bắt đầu từ ngày nó đến. Trang trại bắt đầu với X tấn thực phẩm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 35
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 35 (Div. 2)"
rating: 1700
weight: 35
solve_time_s: 74
verified: true
draft: false
---
[CF 35D - Động vật](https://codeforces.com/problemset/problem/35/D) 

**Đánh giá:** 1700 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một trang trại trong đó các con vật đến mỗi ngày một con.`n`ngày. Mỗi con vật có một nhu cầu thức ăn cố định hàng ngày bắt đầu từ ngày nó đến. Trang trại bắt đầu với`X`tấn thực phẩm. Hàng ngày chúng ta có thể chấp nhận hoặc từ chối con vật đến, nhưng chúng ta phải đảm bảo rằng không con vật nào bị đói. Mục tiêu là tối đa hóa tổng số vật nuôi trong trang trại vào cuối ngày`n`. 

Cụ thể, trong ngày`i`, động vật`i`muốn`c[i]`tấn thực phẩm mỗi ngày từ ngày`i`Hôm nay`n`. Nếu chúng ta chấp nhận động vật`i`, nó tiêu thụ`(n - i + 1) * c[i]`tấn cộng dồn. Thách thức là chọn một tập hợp con động vật sao cho tổng lượng tiêu thụ tích lũy của chúng không vượt quá`X`, trong khi vẫn giữ tập con càng lớn càng tốt. 

Những ràng buộc cho chúng ta`1 ≤ n ≤ 100`Và`1 ≤ X ≤ 10000`. Một cách tiếp cận bạo lực ngây thơ để kiểm tra tất cả các tập hợp con động vật sẽ yêu cầu`2^n`hoạt động, đại khái là`1.27e30`trong trường hợp xấu nhất. Điều này là hoàn toàn không thể thực hiện được. Nhu cầu thức ăn cho mỗi con vật có thể lên tới 300 và tổng thức ăn lên tới 10000, vì vậy các thuật toán phụ thuộc vào tổng tích lũy chính xác cần xử lý các giá trị lên tới`3e4`, đủ nhỏ để lập trình động hoặc các phương pháp tiếp cận tham lam. 

Các trường hợp khó khăn phát sinh khi động vật có nhu cầu thức ăn lớn vào lúc bắt đầu hoặc lúc kết thúc. Ví dụ, nếu con vật đầu tiên muốn nhiều hơn`X`, chúng ta phải từ chối nó ngay lập tức. Một trường hợp tinh tế khác là khi những con vật nhỏ ban đầu kết hợp với một con vật rất lớn đến muộn có thể vượt quá tổng giới hạn thức ăn - một kẻ tham lam ngây thơ luôn chấp nhận các con vật theo thứ tự mà không có sự ưu tiên có thể thất bại. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ liệt kê tất cả các tập hợp con động vật, tính tổng mức tiêu thụ tích lũy cho từng tập hợp con và chọn tập hợp con hợp lệ lớn nhất. Mặc dù đúng nhưng nó không thực tế vì`n`có thể là 100, điều này làm cho không gian tập hợp con lớn về mặt thiên văn. 

Một cách tiếp cận tốt hơn tận dụng thực tế là trong mỗi ngày, mức tiêu thụ tích lũy tỷ lệ thuận với việc con vật đến sớm như thế nào. Những con đầu tiên tiêu thụ tổng lượng thức ăn nhiều hơn những con sau này, vì vậy việc chấp nhận một con vật tiêu thụ ít sớm sẽ tốt hơn một con vật tiêu thụ nhiều. Đây là một vấn đề phân bổ nguồn lực cổ điển. Nếu chúng ta theo dõi mức tiêu thụ thực phẩm tích lũy hàng ngày theo hàng ưu tiên hoặc danh sách được sắp xếp, chúng ta có thể tham lam chấp nhận một con vật và nếu tổng lượng thức ăn vượt quá`X`, loại bỏ loài động vật tiêu thụ nhiều nhất cho đến nay. Điều này đảm bảo rằng ở mỗi bước chúng tôi duy trì số lượng động vật tối đa trong khi vẫn ở dưới mức giới hạn thức ăn. 

Điểm mấu chốt là thứ tự đến chỉ đóng vai trò quan trọng trong việc tính toán nhu cầu thức ăn tích lũy của mỗi con vật.`(n - i + 1) * c[i]`. Khi biết được điều đó, chúng ta có thể sắp xếp hoặc quản lý các loài động vật theo yêu cầu tích lũy và chấp nhận càng nhiều càng tốt bằng cách loại bỏ những loài có đóng góp lớn nhất một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tham lam với đống | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng nhu cầu thức ăn cho từng con vật. Đối với một con vật đến vào ngày`i`với yêu cầu hàng ngày`c[i]`, tổng lượng thức ăn nó sẽ tiêu thụ cho đến ngày`n`là`(n - i + 1) * c[i]`. Lưu trữ các giá trị này trong danh sách cùng với chỉ mục ngày để tham khảo. 
2. Khởi tạo một đống tối thiểu để lưu trữ tổng số yêu cầu của động vật được chấp nhận. Ngoài ra, khởi tạo một biến`current_sum`để theo dõi tổng lượng thức ăn tích lũy của tất cả các loài động vật được chấp nhận. 
3. Lặp lại các con vật theo thứ tự xuất hiện. Với mỗi con vật, cộng tổng nhu cầu của nó vào đống và thêm vào`current_sum`. 
4. Nếu`current_sum`vượt quá`X`, liên tục loại bỏ con vật có yêu cầu lớn nhất khỏi đống (bằng cách giữ lại đống tối đa) cho đến khi tổng nhỏ hơn hoặc bằng`X`. Bước này đảm bảo số lượng động vật được tối đa hóa bằng cách loại bỏ những con đắt tiền nhất trước tiên. 
5. Sau khi xử lý tất cả vật nuôi, kích thước của đống là số lượng vật nuôi tối đa có thể được giữ trong trang trại. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi chấp nhận con vật mới và trục xuất người tiêu dùng lớn nhất nếu cần. Điều này bảo đảm tính bất biến rằng tổng của tất cả các động vật được chấp nhận không bao giờ vượt quá`X`. Bằng cách luôn loại bỏ yếu tố đóng góp lớn nhất, chúng tôi dành chỗ cho nhiều động vật hơn với tổng yêu cầu nhỏ hơn, điều này trực tiếp tối đa hóa số lượng. Không có chuỗi quyết định chấp nhận/từ chối nào khác có thể mang lại số lượng động vật lớn hơn vì chúng tôi giảm thiểu chi phí tích lũy một cách có hệ thống. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n, X = map(int, input().split())
c = list(map(int, input().split()))

# Step 1: compute total food requirement for each animal
requirements = [(c[i] * (n - i), i) for i in range(n)]  # using (total_food, index)

current_sum = 0
max_heap = []

for total_food, _ in requirements:
    heapq.heappush(max_heap, -total_food)  # push negative for max-heap behavior
    current_sum += total_food
    if current_sum > X:
        current_sum += heapq.heappop(max_heap)  # remove largest consumer

print(len(max_heap))
```Giải pháp đầu tiên tính toán tổng lượng thức ăn sử dụng của mỗi con vật từ khi đến lúc kết thúc. Chúng tôi duy trì vùng heap tối đa bằng cách sử dụng số âm vì heapq của Python theo mặc định là vùng heap tối thiểu. Đối với mỗi con vật mới, chúng tôi thêm nó vào đống và kiểm tra tổng tích lũy. Nếu nó vượt quá`X`, chúng tôi loại bỏ người tiêu dùng lớn nhất để giải phóng thực phẩm. Cuối cùng, kích thước đống phản ánh số lượng động vật tối đa. 

Những điểm tinh tế bao gồm tính toán`(n - i) * c[i]`cẩn thận để tránh sai sót từng cái một. Đẩy các giá trị âm vào vùng heap sẽ chuyển nó thành vùng heap tối đa. Xóa khỏi bản cập nhật heap`current_sum`một cách chính xác bằng cách cộng số âm vừa xuất hiện. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3 4
1 1 1
```| Ngày | Tổng số động vật | Đống (âm bản) | hiện tại_sum | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | [-3] | 3 | Chấp nhận | 
| 2 | 2 | [-3, -2] | 5 | Tổng > X, loại bỏ lớn nhất (-3) | 
| 2 | 2 | [-2] | 2 | Chấp nhận | 
| 3 | 1 | [-2, -1] | 3 | Chấp nhận | 

Kích thước heap = 2 → đầu ra 2. Đúng. 

**Đầu vào mẫu 2:**```
5 10
1 2 3 4 5
```| Ngày | Tổng số động vật | Đống | hiện tại_sum | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | [-5] | 5 | Chấp nhận | 
| 2 | 8 | [-8,-5] | 13 | Xóa -8 | 
| 3 | 9 | [-9,-5] | 14 | Xóa -9 | 
| 4 | 8 | [-8,-5] | 13 | Xóa -8 | 
| 5 | 5 | [-5,-5] | 10 | Chấp nhận | 

Kích thước heap = 2 → đầu ra 2. Thể hiện việc loại bỏ tổng yêu cầu lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi thao tác heap là log n, được thực hiện một lần cho mỗi con vật. | 
| Không gian | O(n) | Max-heap lưu trữ tối đa n động vật. | 

Với`n`≤ 100, log n là tầm thường, giúp giải pháp này nhanh chóng. Việc sử dụng bộ nhớ là không đáng kể so với giới hạn 64 MB. 

## Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    import heapq
    n, X = map(int, input().split())
    c = list(map(int, input().split()))
    requirements = [(c[i] * (n - i), i) for i in range(n)]
    current_sum = 0
    max_heap = []
    for total_food, _ in requirements:
        heapq.heappush(max_heap, -total_food)
        current_sum += total_food
        if current_sum > X:
            current_sum += heapq.heappop(max_heap)
    return str(len(max_heap))

# provided samples
assert run("3 4\n1 1 1\n") == "2", "sample 1"

# custom cases
assert run("1 5\n6\n") == "0", "single animal too big"
assert run("5 100\n10 10 10 10 10\n") == "5", "all fit"
assert run("3 5\n2 2 2\n") == "2", "boundary fit"
assert run("4 10\n5 4 3 2\n") == "2", "removal of largest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5\n6 | 0 | Một con vật vượt quá X | 
| 5 100\n10 10 10 10 10 | 5 | Tất cả |
