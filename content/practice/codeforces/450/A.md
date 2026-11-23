---
title: "CF 450A - Jzzhu và trẻ em"
description: "Chúng tôi có một dòng trẻ em. Con à, tổng cộng con muốn một [i] viên kẹo. Mỗi khi có một em đến đầu hàng thì sẽ nhận được chính xác m chiếc kẹo. Nếu trẻ vẫn chưa nhận đủ kẹo sau lần phân phát đó, trẻ sẽ di chuyển về cuối hàng."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 450
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 257 (Div. 2)"
rating: 1000
weight: 450
solve_time_s: 94
verified: true
draft: false
---

[CF 450A - Jzzhu và Trẻ em](https://codeforces.com/problemset/problem/450/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng trẻ em. Đứa trẻ`i`muốn`a[i]`tổng số kẹo. Mỗi khi một đứa trẻ đến đầu hàng, chúng sẽ nhận được chính xác`m`kẹo. 

Nếu trẻ vẫn chưa nhận đủ kẹo sau lần phân phát đó, trẻ sẽ di chuyển về cuối hàng. Nếu không, họ sẽ rời đi vĩnh viễn. Quá trình tiếp tục cho đến khi không còn con nào. 

Nhiệm vụ là xác định xem đứa trẻ nào rời đi cuối cùng. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 đứa trẻ và mỗi đứa trẻ muốn có nhiều nhất 100 cái kẹo. Ngay cả một mô phỏng trực tiếp cũng dễ dàng phù hợp với giới hạn thời gian vì tổng số lần phân phối kẹo là nhỏ. Tuy nhiên, có một quan sát rõ ràng hơn giúp tránh việc mô phỏng hàng đợi. 

Số lượng quan trọng đối với mỗi đứa trẻ là chúng cần bao nhiêu lượt trước khi rời đi. Nếu một đứa trẻ muốn 7 viên kẹo và`m = 3`, các em cần ba lần lên phía trước vì dãy kẹo nhận được là 3, 6, 9. Nói chung, trẻ`i`nhu cầu$$\left\lceil \frac{a_i}{m} \right\rceil$$rẽ. 

Một trường hợp tinh tế xảy ra khi một số trẻ yêu cầu số lượt chơi như nhau. Ví dụ:```
3 2
4 4 4
```Mỗi đứa trẻ cần hai lượt. Lần khởi hành cuối cùng của họ diễn ra theo thứ tự ban đầu nên đứa trẻ thứ 3 rời đi cuối cùng. Giải pháp chỉ tìm thấy số lượt quay cần thiết tối đa và trả về lần xuất hiện đầu tiên sẽ đưa ra kết quả không chính xác là 1. 

Một lỗi dễ mắc phải khác là xử lý sai bội số chính xác của`m`.```
2 3
3 4
```Con 1 chỉ cần một lượt, không cần hai lượt. Số lượt đúng là 1 và 2 nên đáp án là 2. Sử dụng phép chia số nguyên thông thường`a[i] // m`sẽ tạo ra kết quả không chính xác. 

Trường hợp góc cuối cùng là đầu vào tối thiểu:```
1 100
1
```Chỉ có một đứa trẻ nên câu trả lời là 1 bất kể số kẹo. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là mô phỏng hàng đợi chính xác như mô tả. Lưu trữ cặp`(child_index, remaining_candies)`. Liên tục loại bỏ con phía trước, trừ đi`m`kẹo và gửi chúng về nhà hoặc đặt chúng ở cuối hàng đợi. 

Mô phỏng này là chính xác vì nó tuân theo quy trình theo đúng nghĩa đen. Theo những ràng buộc nhất định, nó cũng đủ nhanh. Mỗi đứa trẻ có thể yêu cầu nhiều nhất$$\left\lceil \frac{100}{1} \right\rceil = 100$$lượt nên tổng số thao tác xếp hàng nhiều nhất là khoảng 10.000. 

Tuy nhiên, có một cách đơn giản hơn để nghĩ về quá trình này. 

Một em ra về sau khi đã nhận đủ lô đủ size`m`. Điều duy nhất quan trọng là số lượt truy cập họ cần:$$t_i = \left\lceil \frac{a_i}{m} \right\rceil$$Những đứa trẻ cần được thăm khám nhiều hơn sẽ ở trong hàng đợi lâu hơn. Đứa trẻ cuối cùng rời đi là đứa trẻ có giá trị lớn nhất của`t_i`. 

Khi có nhiều con có cùng giá trị lớn nhất thì con ở dòng ban đầu sẽ rời đi sau. Họ hoàn thành chuyến thăm cuối cùng sau tất cả những đứa trẻ trước đó có cùng yêu cầu. 

Điều này có nghĩa là chúng ta chỉ cần tìm vị trí cuối cùng có số lượt yêu cầu là tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hàng đợi Brute Force | O(n · max(aᵢ)) | O(n) | Đã chấp nhận | 
| Đếm lượt tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, và mảng kẹo yêu cầu. 
2. Với mỗi em, tính số lượt cần thực hiện:$$t_i = \left\lceil \frac{a_i}{m} \right\rceil$$Trong số học số nguyên, điều này có thể được viết là:$$t_i = \frac{a_i + m - 1}{m}$$sử dụng cách chia tầng. 
3. Theo dõi số lượt quay lớn nhất được thấy cho đến nay và chỉ số con tương ứng. 
4. Khi số lượt của trẻ lớn hơn hoặc bằng số tối đa hiện tại, hãy cập nhật câu trả lời cho trẻ này. 

sử dụng`>=`là điều cần thiết. Nếu hai em có số lượt chơi bằng nhau thì em nào về sau sẽ về muộn hơn và ghi đè lên câu trả lời trước đó. 
5. Sau khi xử lý tất cả các phần tử con, xuất ra chỉ mục đã lưu trữ. 

### Tại sao nó hoạt động 

Đối với mỗi đứa trẻ,`t_i`thể hiện chính xác số lần đứa trẻ đó phải đến trước khi rời đi. 

Bất cứ đứa trẻ nào có thân hình nhỏ hơn`t_i`rời đi sớm hơn một đứa trẻ có kích thước lớn hơn`t_i`, bởi vì họ kết thúc sau ít lượt giao bóng hơn. 

Nếu hai đứa trẻ có cùng một`t_i`, thứ tự tương đối của chúng không bao giờ thay đổi. Trong chuyến thăm cuối cùng của họ, đứa trẻ bắt đầu muộn hơn trong hàng đợi ban đầu cũng sẽ đến chuyến thăm đó muộn hơn. Do đó trong số tất cả những đứa trẻ có số lượt quay tối đa, đứa cuối cùng theo thứ tự ban đầu là đứa trẻ cuối cùng về nhà. 

Thuật toán tìm thấy rõ ràng lần xuất hiện cuối cùng của số lượt quay tối đa, chính xác là phần tử con được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    best_turns = -1
    answer = -1

    for i, candies in enumerate(a, start=1):
        turns = (candies + m - 1) // m

        if turns >= best_turns:
            best_turns = turns
            answer = i

    print(answer)

solve()
```Giải pháp xử lý mỗi đứa trẻ chính xác một lần. 

biểu hiện`(candies + m - 1) // m`tính toán trần của`candies / m`sử dụng số học số nguyên. Điều này tránh các phép toán dấu phẩy động và xử lý chính xác các giá trị không phải là bội số chính xác của`m`. 

Việc so sánh sử dụng`>=`còn hơn là`>`. Đây là chi tiết triển khai quan trọng nhất. Khi hai trẻ yêu cầu số lượt quay bằng nhau thì trẻ đi sau phải trở thành đáp án hiện tại vì trẻ đó về muộn hơn. 

Các chỉ số được tạo ra với`enumerate(..., start=1)`vì bài toán đánh số trẻ em bắt đầu từ 1 chứ không phải 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 3 1 4 2
```| Con | Kẹo Muốn | Lần lượt cần thiết | Tối đa hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 3 | 2 | 2 | 2 | 
| 3 | 1 | 1 | 2 | 2 | 
| 4 | 4 | 2 | 2 | 4 | 
| 5 | 2 | 1 | 2 | 4 | 

Đầu ra:```
4
```Trẻ 2 và 4 đều cần hai lượt, là mức tối đa. Con 4 xuất hiện muộn hơn nên con 4 ra về cuối cùng. 

### Ví dụ 2 

đầu vào:```
3 3
9 6 7
```| Con | Kẹo Muốn | Lần lượt cần thiết | Tối đa hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 3 | 3 | 1 | 
| 2 | 6 | 2 | 3 | 1 | 
| 3 | 7 | 3 | 3 | 3 | 

Đầu ra:```
3
```Trẻ em 1 và 3 đều cần ba lượt. Đứa trẻ sau, số 3, thực hiện lần khởi hành cuối cùng sau đó và trở thành đáp án. 

Ví dụ này chứng minh tại sao các mối quan hệ phải cập nhật câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đứa trẻ được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ bên cạnh mảng đầu vào | 

Với`n ≤ 100`, giải pháp này thấp hơn nhiều so với giới hạn thời gian và bộ nhớ. Ngay cả phương pháp mô phỏng cũng có thể thực hiện dễ dàng, nhưng việc quan sát đếm lượt sẽ giảm vấn đề xuống còn một lần quét tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    best_turns = -1
    answer = -1

    for i, candies in enumerate(a, start=1):
        turns = (candies + m - 1) // m

        if turns >= best_turns:
            best_turns = turns
            answer = i

    return str(answer)

# provided sample
assert run("5 2\n1 3 1 4 2\n") == "4", "sample 1"

# minimum size
assert run("1 100\n1\n") == "1", "single child"

# all equal values
assert run("3 2\n4 4 4\n") == "3", "last occurrence of maximum"

# exact multiples of m
assert run("2 3\n3 4\n") == "2", "ceiling division"

# maximum turns tie
assert run("4 1\n100 100 100 100\n") == "4", "all need same turns"

# larger maximum appears earlier
assert run("5 4\n8 3 3 3 3\n") == "1", "unique maximum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 100 / 1`|`1`| Kích thước đầu vào tối thiểu | 
|`3 2 / 4 4 4`|`3`| Xử lý hòa, lần xuất hiện cuối cùng thắng | 
|`2 3 / 3 4`|`2`| Phân chia trần đúng cách | 
|`4 1 / 100 100 100 100`|`4`| Hộp đựng cà vạt lớn | 
|`5 4 / 8 3 3 3 3`|`1`| Số lượt quay tối đa duy nhất | 

## Vỏ cạnh 

Hãy xem xét:```
3 2
4 4 4
```Mỗi đứa trẻ cần$$\left\lceil \frac{4}{2} \right\rceil = 2$$rẽ. Thuật toán xử lý trẻ em từ trái sang phải. Trẻ 1 đặt giá trị tối đa là 2. Trẻ 2 khớp với giá trị tối đa và trở thành câu trả lời. Con 3 cũng trùng khớp với đáp án tối đa và trở thành đáp án cuối cùng. Đầu ra là:```
3
```phù hợp với quá trình xếp hàng thực tế. 

Bây giờ hãy xem xét:```
2 3
3 4
```Số lượt lần lượt là:$$\left\lceil \frac{3}{3} \right\rceil = 1$$Và$$\left\lceil \frac{4}{3} \right\rceil = 2$$Thuật toán đưa ra con 2. Trường hợp này xác nhận rằng bội số chính xác của`m`không được làm tròn thêm thời gian. 

Cuối cùng, hãy xem xét:```
1 100
1
```Đứa trẻ độc thân cần một lượt. Quá trình quét chỉ truy cập một phần tử và trả về chỉ mục 1. Vì không có phần tử con nào khác tồn tại nên phần tử đó nhất thiết phải là phần tử cuối cùng rời đi.
