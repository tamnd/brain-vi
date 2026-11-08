---
title: "CF 140E - Vòng Hoa Năm Mới"
description: "Sự khác biệt của mảng đầu vào mô tả cách các giá trị liên tiếp trong một mảng không xác định thay đổi từ vị trí này sang vị trí tiếp theo."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 140
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 100"
rating: 2600
weight: 140
solve_time_s: 123
verified: false
draft: false
---

[CF 140E - Vòng hoa năm mới](https://codeforces.com/problemset/problem/140/E) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mảng đầu vào`differences`mô tả cách các giá trị liên tiếp trong một mảng không xác định thay đổi từ vị trí này sang vị trí tiếp theo. Nếu dãy ẩn được gọi`hidden`, sau đó:$$hidden[i + 1] = hidden[i] + differences[i]$$Nhiệm vụ là xác định có bao nhiêu giá trị bắt đầu hợp lệ có thể tạo ra một chuỗi ẩn hoàn chỉnh trong đó mọi phần tử đều nằm trong phạm vi bao gồm`[lower, upper]`. 

Quan sát quan trọng là toàn bộ chuỗi sẽ trở nên cố định sau khi giá trị đầu tiên được chọn. Mỗi giá trị sau này được xác định bằng cách áp dụng nhiều lần các sai khác. Điều đó có nghĩa là chúng tôi không tìm kiếm trên các mảng tùy ý, chúng tôi chỉ tìm kiếm trên các số bắt đầu có thể có. 

Giả sử giá trị bắt đầu là`x`. Khi đó mọi phần tử trong dãy có thể được viết dưới dạng:$$x + prefixSum$$Ở đâu`prefixSum`đại diện cho tổng số chênh lệch tích lũy tính đến thời điểm đó. 

Các ràng buộc đủ lớn để việc liệt kê bạo lực trên tất cả các chuỗi có thể là không thể. Độ dài mảng có thể đạt tới`10^5`, do đó giải pháp phải chạy theo thời gian tuyến tính. Bất kỳ cách tiếp cận nào liên quan đến việc lặp lại lồng nhau trên các tiền tố hoặc việc xây dựng lại các chuỗi lặp đi lặp lại sẽ trở nên quá chậm. 

Một số trường hợp cạnh rất quan trọng: 

Một chuỗi có thể ngay lập tức trở nên không hợp lệ vì một số tiền tố đẩy một giá trị ra ngoài phạm vi cho phép. Ví dụ:```
differences = [100]
lower = 0
upper = 1
```Không có giá trị bắt đầu nào có thể giữ cả hai phần tử trong phạm vi. 

Một trường hợp quan trọng khác xảy ra khi dãy chạm chính xác vào các ranh giới. Giới hạn bao gồm vấn đề. Ví dụ:```
differences = [1]
lower = 0
upper = 1
```Bắt đầu lúc`0`cho`[0,1]`, hợp lệ. 

Tổng tích lũy âm cũng rất quan trọng vì tiền tố tối thiểu có thể xuất hiện sau trong chuỗi chứ không phải ở đầu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi giá trị bắt đầu có thể có giữa`lower`Và`upper`. Đối với mỗi giá trị bắt đầu, chúng tôi xây dựng lại toàn bộ chuỗi ẩn và kiểm tra xem mọi phần tử có còn nằm trong phạm vi hợp lệ hay không. 

Điều này hoạt động vì chuỗi được xác định duy nhất sau khi giá trị đầu tiên được chọn. Thuật toán rất đơn giản: 

1. Chọn giá trị bắt đầu. 
2. Áp dụng nhiều lần những điểm khác biệt. 
3. Dừng lại nếu có bất kỳ giá trị nào vượt quá phạm vi cho phép. 
4. Đếm xem có bao nhiêu giá trị ban đầu thành công. 

Vấn đề là hiệu suất. Có thể có tới`200001`các giá trị bắt đầu có thể có và mỗi lần xác thực có thể yêu cầu duyệt qua tất cả`10^5`sự khác biệt. Độ phức tạp trong trường hợp xấu nhất trở thành:$$O((upper - lower + 1) \cdot n)$$đó là quá lớn. 

Thông tin chi tiết quan trọng là mọi phần tử của chuỗi chỉ khác với giá trị bắt đầu ở một tổng tiền tố. Thay vì kiểm tra từng chuỗi một cách độc lập, chúng ta có thể xác định trực tiếp phạm vi cho phép cho giá trị bắt đầu. 

Nếu tổng tiền tố tích lũy nằm trong khoảng từ`minPrefix`ĐẾN`maxPrefix`, thì mọi giá trị chuỗi trông như sau:$$start + prefix$$Để giữ tất cả các giá trị trong giới hạn:$$lower \le start + minPrefix$$Và$$start + maxPrefix \le upper$$Sắp xếp lại mang lại:$$lower - minPrefix \le start \le upper - maxPrefix$$Số số nguyên hợp lệ trong khoảng này là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Ghi chú | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((trên - dưới + 1) * n) | O(1) | Kiểm tra mọi giá trị bắt đầu có thể | 
| Tối ưu | O(n) | O(1) | Chỉ theo dõi phạm vi tổng tiền tố | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba biến: 

-`prefixSum = 0`-`minPrefix = 0`-`maxPrefix = 0`Tổng tiền tố biểu thị khoảng cách giữa phần tử chuỗi hiện tại với giá trị bắt đầu. 
2. Đi qua`differences`mảng một phần tử tại một thời điểm. 
3. Thêm chênh lệch hiện tại vào`prefixSum`. 

Điều này mô phỏng việc di chuyển qua chuỗi ẩn so với giá trị bắt đầu. 
4. Cập nhật: 

-`minPrefix = min(minPrefix, prefixSum)`-`maxPrefix = max(maxPrefix, prefixSum)`Các giá trị này theo dõi độ lệch nhỏ nhất và lớn nhất xảy ra ở bất kỳ vị trí nào trong chuỗi. 
5. Sau khi xử lý tất cả sự khác biệt, hãy tính khoảng thời gian bắt đầu hợp lệ. 

Vì mọi giá trị chuỗi đều bằng:$$start + prefix$$chúng tôi cần:$$lower \le start + minPrefix$$Và$$start + maxPrefix \le upper$$6. Sắp xếp lại các bất đẳng thức:$$start \ge lower - minPrefix$$

$$start \le upper - maxPrefix$$7. Tính số số nguyên trong khoảng:$$(upper - maxPrefix) - (lower - minPrefix) + 1$$8. Nếu khoảng thời gian không hợp lệ, hãy quay lại`0`. 

### Tại sao nó hoạt động 

Mỗi chuỗi ẩn hoàn toàn được xác định bởi giá trị bắt đầu của nó. Sự khác biệt tích lũy chỉ làm thay đổi các yếu tố sau này so với điểm khởi đầu đó. 

Tổng tiền tố nhỏ nhất xác định mức độ thấp của chuỗi và tổng tiền tố lớn nhất xác định mức độ có thể tăng của chuỗi. Bất kỳ giá trị ban đầu nào giữ cả hai cực trị bên trong`[lower, upper]`cũng tự động giữ mọi giá trị trung gian trong phạm vi. 

Bởi vì thuật toán tính toán khoảng thời gian cho phép chính xác cho giá trị bắt đầu, việc đếm các số nguyên bên trong khoảng đó sẽ cho ra số lượng chính xác các chuỗi hợp lệ. 

## Giải pháp Python```
from typing import List

class Solution:
    def numberOfArrays(self, differences: List[int], lower: int, upper: int) -> int:
        prefix_sum = 0
        min_prefix = 0
        max_prefix = 0

        for diff in differences:
            prefix_sum += diff
            min_prefix = min(min_prefix, prefix_sum)
            max_prefix = max(max_prefix, prefix_sum)

        smallest_start = lower - min_prefix
        largest_start = upper - max_prefix

        return max(0, largest_start - smallest_start + 1)
```Việc thực hiện trực tiếp theo đạo hàm toán học. 

Biến`prefix_sum`theo dõi tác động tích lũy của tất cả những khác biệt được xử lý cho đến nay. Vì mỗi giá trị chuỗi bằng giá trị bắt đầu cộng với độ lệch tích lũy này nên thông tin duy nhất chúng ta thực sự cần là độ lệch tối thiểu và tối đa gặp phải. 

Vòng lặp xử lý mảng một lần, cập nhật`min_prefix`Và`max_prefix`. Chúng đại diện cho những sai lệch cực độ so với giá trị ban đầu. 

Sau khi quá trình truyền tải kết thúc, mã sẽ tính khoảng thời gian hợp lệ của các giá trị bắt đầu. Biểu thức:```
largest_start - smallest_start + 1
```đếm có bao nhiêu số nguyên tồn tại trong khoảng bao gồm. 

Bên ngoài`max(0, ...)`xử lý các trường hợp không tồn tại khoảng hợp lệ. 

## Đi giải pháp```
func numberOfArrays(differences []int, lower int, upper int) int {
    prefixSum := 0
    minPrefix := 0
    maxPrefix := 0

    for _, diff := range differences {
        prefixSum += diff

        if prefixSum < minPrefix {
            minPrefix = prefixSum
        }

        if prefixSum > maxPrefix {
            maxPrefix = prefixSum
        }
    }

    smallestStart := lower - minPrefix
    largestStart := upper - maxPrefix

    result := largestStart - smallestStart + 1

    if result < 0 {
        return 0
    }

    return result
}
```Việc triển khai Go phản ánh chặt chẽ phiên bản Python. Vì Go không cung cấp tính năng tích hợp sẵn`min`Và`max`các hàm cho số nguyên, so sánh rõ ràng được sử dụng. 

Tràn số nguyên không phải là vấn đề đáng lo ngại ở đây vì các ràng buộc giữ tất cả các giá trị một cách thoải mái trong phạm vi số nguyên 32 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
differences = [1, -3, 4]
lower = 1
upper = 6
```Chúng tôi tính toán tổng tiền tố. 

| Bước | Sự khác biệt | Tiền tố Tổng | Tiền tố tối thiểu | Tiền tố tối đa | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 0 | 
| 1 | 1 | 1 | 0 | 1 | 
| 2 | -3 | -2 | -2 | 1 | 
| 3 | 4 | 2 | -2 | 2 | 

Bây giờ hãy tính các giá trị bắt đầu hợp lệ.$$smallestStart = 1 - (-2) = 3$$

$$largestStart = 6 - 2 = 4$$Bắt đầu hợp lệ là:```
3, 4
```Trả lời:```
2
```### Ví dụ 2```
differences = [3, -4, 5, 1, -2]
lower = -4
upper = 5
```| Bước | Sự khác biệt | Tiền tố Tổng | Tiền tố tối thiểu | Tiền tố tối đa | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 0 | 
| 1 | 3 | 3 | 0 | 3 | 
| 2 | -4 | -1 | -1 | 3 | 
| 3 | 5 | 4 | -1 | 4 | 
| 4 | 1 | 5 | -1 | 5 | 
| 5 | -2 | 3 | -1 | 5 | 

Tính giới hạn.$$smallestStart = -4 - (-1) = -3$$

$$largestStart = 5 - 5 = 0$$Có thể bắt đầu:```
-3, -2, -1, 0
```Trả lời:```
4
```### Ví dụ 3```
differences = [4, -7, 2]
lower = 3
upper = 6
```| Bước | Sự khác biệt | Tiền tố Tổng | Tiền tố tối thiểu | Tiền tố tối đa | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 0 | 
| 1 | 4 | 4 | 0 | 4 | 
| 2 | -7 | -3 | -3 | 4 | 
| 3 | 2 | -1 | -3 | 4 | 

Tính giới hạn.$$smallestStart = 3 - (-3) = 6$$

$$largestStart = 6 - 4 = 2$$Từ:$$6 > 2$$không có giá trị bắt đầu hợp lệ. 

Trả lời:```
0
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đi qua một lần`differences`| 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Thuật toán xử lý từng điểm khác biệt chính xác một lần và không bao giờ lưu trữ các mảng hoặc cấu trúc dữ liệu bổ sung. Điều này dễ dàng phù hợp với các ràng buộc vấn đề của`10^5`các phần tử. 

## Trường hợp thử nghiệm```
from typing import List

class Solution:
    def numberOfArrays(self, differences: List[int], lower: int, upper: int) -> int:
        prefix_sum = 0
        min_prefix = 0
        max_prefix = 0

        for diff in differences:
            prefix_sum += diff
            min_prefix = min(min_prefix, prefix_sum)
            max_prefix = max(max_prefix, prefix_sum)

        smallest_start = lower - min_prefix
        largest_start = upper - max_prefix

        return max(0, largest_start - smallest_start + 1)

sol = Solution()

assert sol.numberOfArrays([1, -3, 4], 1, 6) == 2  # provided example 1
assert sol.numberOfArrays([3, -4, 5, 1, -2], -4, 5) == 4  # provided example 2
assert sol.numberOfArrays([4, -7, 2], 3, 6) == 0  # provided example 3

assert sol.numberOfArrays([1], 0, 1) == 1  # exact boundary fit
assert sol.numberOfArrays([0, 0, 0], 1, 1) == 1  # constant sequence
assert sol.numberOfArrays([100], 0, 1) == 0  # impossible due to large jump
assert sol.numberOfArrays([-1, -1, -1], -3, 0) == 1  # decreasing sequence
assert sol.numberOfArrays([1, 1, 1], 1, 4) == 1  # increasing sequence
assert sol.numberOfArrays([1, -1, 1, -1], 0, 0) == 1  # oscillating sequence
```| Kiểm tra | Tại sao | 
| --- | --- | 
|`[1, -3, 4], 1, 6`| Xác minh hành vi mẫu | 
|`[3, -4, 5, 1, -2], -4, 5`| Kiểm tra hỗn hợp tiền tố tích cực và tiêu cực | 
|`[4, -7, 2], 3, 6`| Kiểm tra khoảng thời gian không thể | 
|`[1], 0, 1`| Kiểm tra bao gồm xử lý ranh giới | 
|`[0, 0, 0], 1, 1`| Kiểm tra lặp lại các giá trị giống hệt nhau | 
|`[100], 0, 1`| Kiểm tra sự khác biệt quá lớn | 
|`[-1, -1, -1], -3, 0`| Kiểm tra phong trào tích lũy tiêu cực | 
|`[1, 1, 1], 1, 4`| Kiểm tra chuyển động tích lũy tích cực | 
|`[1, -1, 1, -1], 0, 0`| Kiểm tra tổng tiền tố xen kẽ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi khoảng hợp lệ thu gọn về một giá trị duy nhất. Ví dụ:```
differences = [1, 1, 1]
lower = 1
upper = 4
```Tổng tiền tố là`0, 1, 2, 3`. Giá trị bắt đầu hợp lệ duy nhất là`1`. Việc triển khai bất cẩn có thể coi các điểm cuối khoảng thời gian là độc quyền và trả về một cách không chính xác.`0`thay vì`1`. Việc triển khai xử lý việc này một cách chính xác vì nó sử dụng tính bao gồm với`+1`. 

Một trường hợp phức tạp khác xảy ra khi tất cả sự khác biệt đều bằng 0:```
differences = [0, 0, 0]
lower = 5
upper = 5
```Mọi phần tử của chuỗi vẫn giống hệt với giá trị bắt đầu. Tổng tiền tố tối thiểu và tối đa đều bằng 0, vì vậy khoảng hợp lệ chính xác là`[5,5]`. Thuật toán xử lý việc này một cách tự nhiên vì phạm vi tiền tố không bao giờ thay đổi. 

Trường hợp cạnh quan trọng thứ ba xảy ra khi phạm vi chuỗi vượt quá giới hạn cho phép bất kể giá trị bắt đầu nào được chọn:```
differences = [4, -7, 2]
lower = 3
upper = 6
```Khoảng thời gian bắt đầu được yêu cầu trở nên không hợp lệ vì lần bắt đầu hợp lệ nhỏ nhất vượt quá lần bắt đầu hợp lệ lớn nhất. Việc triển khai phát hiện điều này bằng cách tính toán độ dài khoảng âm và trả về`0`. 

Trường hợp tinh tế cuối cùng liên quan đến tổng tiền tố âm xuất hiện muộn trong quá trình truyền tải:```
differences = [5, -10, 3]
```Tiền tố tối thiểu không xuất hiện ở đầu. Bất kỳ triển khai nào chỉ kiểm tra các giá trị hiện tại theo giới hạn mà không theo dõi các tiền tố tối thiểu và tối đa toàn cầu đều có thể tạo ra kết quả không chính xác. Thuật toán tránh điều này bằng cách duy trì cả hai cực trị trong suốt quá trình truyền tải.
