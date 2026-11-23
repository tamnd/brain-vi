---
title: "CF 455A - Nhàm chán"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Trong một lần di chuyển, chúng ta chọn một giá trị x hiện đang có, kiếm x điểm khi xóa một lần xuất hiện của nó và kết quả là tất cả các lần xuất hiện của x - 1 và x + 1 đều biến mất khỏi mảng."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 455
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 260 (Div. 1)"
rating: 1500
weight: 455
solve_time_s: 91
verified: true
draft: false
---

[CF 455A - Sự nhàm chán](https://codeforces.com/problemset/problem/455/A) 

**Đánh giá:** 1500 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Trong một lần di chuyển, chúng tôi chọn một giá trị`x`hiện có, kiếm được`x`hướng tới việc xóa một lần xuất hiện của nó và tất cả các lần xuất hiện của`x - 1`Và`x + 1`kết quả là biến mất khỏi mảng. 

Thoạt nhìn, hoạt động có vẻ như phụ thuộc vào từng vị trí trong mảng. Nó thực sự chỉ phụ thuộc vào các giá trị. Bất cứ khi nào chúng tôi quyết định giá trị đó`x`đáng để thực hiện, mỗi lần xuất hiện của`x`cuối cùng có thể được thu thập, bởi vì loại bỏ một`x`không loại bỏ cái khác`x`các giá trị. Xung đột duy nhất là với các giá trị lân cận`x - 1`Và`x + 1`. 

Đầu vào chứa tối đa`10^5`số và mỗi số nhiều nhất là`10^5`. Bất kỳ giải pháp nào cố gắng khám phá trực tiếp các chuỗi di chuyển khác nhau đều vô vọng. Ngay cả các thuật toán bậc hai cũng yêu cầu khoảng`10^10`trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(M)`, Ở đâu`M`là giá trị lớn nhất xuất hiện trong mảng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
3
2 2 2
```Câu trả lời đúng là`6`. 

Giải pháp bất cẩn có thể nghĩ đến việc chọn giá trị`2`chỉ cho`2`điểm một lần. Trong thực tế, sau khi loại bỏ các giá trị lân cận, cả ba lần xuất hiện của`2`vẫn có thể được thực hiện, mang lại`2 + 2 + 2 = 6`. 

Coi như:```
4
1 1 2 2
```Câu trả lời đúng là`4`. 

Lấy tất cả`1`s cho`2`điểm. Lấy tất cả`2`s cho`4`điểm. Chúng ta không thể lấy cả hai vì giá trị`1`Và`2`xung đột. Bất kỳ giải pháp nào xử lý các sự kiện một cách độc lập thay vì nhóm các giá trị bằng nhau sẽ tạo ra kết quả sai. 

Coi như:```
5
1 3 5 7 9
```Câu trả lời đúng là`25`. 

Không có giá trị nào liền kề nhau nên mọi giá trị đều có thể được lấy. Một giải pháp chỉ so sánh các vị trí mảng lân cận chứ không phải các giá trị số lân cận sẽ bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là nghĩ về mọi chuỗi chuyển động có thể xảy ra. Ở mỗi bước, chúng tôi chọn một giá trị, xóa các giá trị xung đột và tiếp tục đệ quy. Điều này đúng vì nó khám phá tất cả các kết quả pháp lý. 

Vấn đề là số lượng khả năng. Với tối đa`10^5`các phần tử, không gian tìm kiếm trở nên rất lớn. Ngay cả việc ghi nhớ các trạng thái cũng không thực tế vì tập hợp giá trị còn lại có thể khác nhau theo nhiều cách. 

Quan sát quan trọng là vị trí không quan trọng chút nào. Chỉ có tần số giá trị quan trọng. 

Giả sử giá trị`x`xuất hiện`cnt[x]`lần. Nếu chúng ta quyết định sử dụng giá trị`x`, cuối cùng chúng ta có thể thu thập```
x * cnt[x]
```điểm. 

Sau khi đưa ra quyết định đó, giá trị`x - 1`Và`x + 1`trở nên không có sẵn. Không có gì khác bị ảnh hưởng. 

Điều này biến vấn đề thành một vấn đề đơn giản hơn nhiều. Với mọi giá trị số nguyên`x`, chúng ta có một "ngôi nhà" đáng giá```
points[x] = x * cnt[x]
```Nếu chúng ta chiếm nhà`x`, chúng tôi không thể lấy nhà`x - 1`hoặc`x + 1`. 

Đây chính xác là cấu trúc giống như bài toán lập trình động cổ điển House Robber. 

Cho phép`dp[i]`là số điểm tối đa có thể đạt được bằng cách sử dụng các giá trị từ`1`bởi vì`i`. 

Đối với giá trị`i`, chỉ có hai khả năng. 

Nếu chúng ta bỏ qua nó, câu trả lời vẫn còn`dp[i - 1]`. 

Nếu chúng ta lấy nó, chúng ta sẽ đạt được`points[i]`và phải bỏ qua`i - 1`, cho```
dp[i - 2] + points[i]
```Vì vậy, sự tái phát trở thành:```
dp[i] = max(
    dp[i - 1],
    dp[i - 2] + points[i]
)
```Điều này làm giảm vấn đề thành một chương trình động tuyến tính đơn giản trên không gian giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP tối ưu | O(M) | O(M) | Đã chấp nhận | 

Đây`M`là giá trị tối đa xuất hiện ở đầu vào, nhiều nhất là`100000`. 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Đếm xem mỗi giá trị xuất hiện bao nhiêu lần. 
3. Với mọi giá trị`x`, tính tổng đóng góp của nó:```
points[x] = x * cnt[x]
```Nếu chúng ta chọn giá trị`x`, đây là tổng số điểm chúng ta có thể kiếm được từ tất cả các lần xuất hiện của`x`. 
4. Hãy để`mx`là giá trị lớn nhất trong mảng. 
5. Tạo một mảng DP có kích thước`mx + 1`. 
6. Đặt:```
dp[0] = 0
dp[1] = points[1]
```7. Với mọi giá trị`i`từ`2`ĐẾN`mx`, tính:```
dp[i] = max(
    dp[i - 1],
    dp[i - 2] + points[i]
)
```Tùy chọn đầu tiên bỏ qua giá trị`i`. 

Tùy chọn thứ hai có giá trị`i`, vì vậy giá trị`i - 1`không thể sử dụng được 
8. Đầu ra`dp[mx]`. 

### Tại sao nó hoạt động 

Sau khi nhóm các số bằng nhau, mọi giá trị`i`trở thành một quyết định độc lập với phần thưởng`points[i]`. Hạn chế duy nhất là không thể chọn cả hai giá trị khác nhau. 

Khi tính toán`dp[i]`, mọi nghiệm hợp lệ trên các giá trị`1..i`hoặc loại trừ`i`hoặc bao gồm nó. Nếu loại trừ`i`, điểm số tốt nhất là chính xác`dp[i-1]`. Nếu nó bao gồm`i`, sau đó`i-1`phải được loại trừ, để lại điểm số tốt nhất`dp[i-2] + points[i]`. Đây là những khả năng duy nhất và việc tái diễn là điều tốt hơn. 

Bằng quy nạp trên`i`,`dp[i]`luôn lưu trữ câu trả lời tối ưu cho các giá trị lên tới`i`, Vì thế`dp[mx]`là đáp án tối ưu cho toàn bộ bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)

    points = [0] * (mx + 1)
    for x in a:
        points[x] += x

    if mx == 1:
        print(points[1])
        return

    dp = [0] * (mx + 1)
    dp[1] = points[1]

    for i in range(2, mx + 1):
        dp[i] = max(dp[i - 1], dp[i - 2] + points[i])

    print(dp[mx])

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên tổng hợp tất cả các giá trị bằng nhau. Thay vì lưu trữ tần số riêng biệt và sau đó nhân lên, việc triển khai sẽ tích lũy trực tiếp`x`vào trong`points[x]`cho mỗi lần xuất hiện. Sau khi xử lý mảng,`points[x]`đã bằng rồi`x * cnt[x]`. 

Mảng lập trình động được lập chỉ mục theo giá trị chứ không phải theo vị trí trong mảng ban đầu. Đây là ý tưởng trung tâm của giải pháp. Sự lặp lại chỉ phụ thuộc vào hai trạng thái giá trị trước đó. 

Việc xử lý đặc biệt đối với`mx == 1`tránh truy cập`dp[1]`khi mảng DP có kích thước bằng một. Đây là điều kiện biên duy nhất cần được quan tâm rõ ràng. 

Số nguyên Python tự động xử lý các giá trị lớn. Câu trả lời tối đa có thể là xung quanh`10^10`, dễ dàng phù hợp với kiểu số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
```Tổng tần suất:```
points[1] = 1
points[2] = 2
```| tôi | điểm[i] | dp[i-2] | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | - | 1 | 
| 2 | 2 | 0 | 1 | 2 | 

Trả lời:```
2
```Lấy giá trị`2`tốt hơn là lấy giá trị`1`. Vì các giá trị liền kề nhau nên không thể chọn cả hai. 

### Ví dụ 2 

đầu vào:```
5
2 2 2 2 2
```Tổng tần suất:```
points[2] = 10
```| tôi | điểm[i] | dp[i-2] | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | - | - | 0 | 
| 2 | 10 | 0 | 0 | 10 | 

Trả lời:```
10
```Ví dụ này cho thấy tại sao các giá trị bằng nhau phải được nhóm lại. Một lần giá trị`2`được chọn, mỗi lần xuất hiện đều góp phần vào điểm số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M) | Một lần chuyển đổi DP cho mọi giá trị từ 1 đến giá trị tối đa | 
| Không gian | O(M) | Mảng`points`Và`dp`kích thước`M + 1`| 

Từ`M ≤ 100000`, thuật toán chỉ thực hiện khoảng một trăm nghìn chuyển đổi DP và dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)

    points = [0] * (mx + 1)
    for x in a:
        points[x] += x

    if mx == 1:
        return str(points[1])

    dp = [0] * (mx + 1)
    dp[1] = points[1]

    for i in range(2, mx + 1):
        dp[i] = max(dp[i - 1], dp[i - 2] + points[i])

    return str(dp[mx])

# provided sample
assert run("2\n1 2\n") == "2", "sample 1"

# minimum size
assert run("1\n1\n") == "1", "single element"

# all equal values
assert run("5\n2 2 2 2 2\n") == "10", "all equal"

# adjacent conflict
assert run("4\n1 1 2 2\n") == "4", "must choose better side"

# non-adjacent values
assert run("5\n1 3 5 7 9\n") == "25", "all values can be taken"

# classic mixed case
assert run("9\n1 2 1 3 2 2 2 3 3\n") == "10", "dp transition check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Đầu vào hợp lệ nhỏ nhất | 
|`5 / 2 2 2 2 2`|`10`| Tất cả các lần xuất hiện của một giá trị đều được thu thập | 
|`4 / 1 1 2 2`|`4`| Xung đột giá trị liền kề | 
|`5 / 1 3 5 7 9`|`25`| Tất cả các giá trị không liền kề đều có thể được chọn | 
|`9 / 1 2 1 3 2 2 2 3 3`|`10`| Hành vi DP chung | 

## Vỏ cạnh 

Hãy xem xét:```
3
2 2 2
```Tổng điểm là:```
points[2] = 6
```DP tính toán:```
dp[1] = 0
dp[2] = max(0, 6) = 6
```Đầu ra là`6`, thu thập chính xác mọi lần xuất hiện của giá trị`2`. 

Coi như:```
4
1 1 2 2
```Tổng số trở thành:```
points[1] = 2
points[2] = 4
```DP tính toán:```
dp[1] = 2
dp[2] = max(2, 4) = 4
```Đầu ra là`4`. Thuật toán nhận ra chính xác rằng việc lấy tất cả`2`s tốt hơn là lấy tất cả`1`S. 

Coi như:```
5
1 3 5 7 9
```Tổng số là:```
points[1] = 1
points[3] = 3
points[5] = 5
points[7] = 7
points[9] = 9
```Tất cả các giá trị trung gian đều có đóng góp bằng 0. Phép truy toán lặp đi lặp lại nhận mọi giá trị không xung đột, tạo ra:```
1 + 3 + 5 + 7 + 9 = 25
```Đầu ra là`25`, cho thấy khoảng cách giữa các giá trị được DP xử lý một cách tự nhiên.
