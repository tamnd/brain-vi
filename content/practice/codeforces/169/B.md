---
title: "CF 169B - Thay chữ số"
description: "Chúng ta được cung cấp một số thập phân dưới dạng một chuỗi và một chuỗi khác chứa các chữ số bổ sung mà chúng ta có thể sử dụng để thay thế. Mỗi chữ số từ chuỗi thứ hai có thể được sử dụng nhiều nhất một lần. Đối với mỗi chữ số được chọn, chúng ta có thể thay thế bất kỳ vị trí nào trong số ban đầu."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 169
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2012 Round 2 (Unofficial Div. 2 Edition)"
rating: 1100
weight: 169
solve_time_s: 92
verified: true
draft: false
---

[CF 169B - Thay thế chữ số](https://codeforces.com/problemset/problem/169/B) 

**Đánh giá:** 1100 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số thập phân dưới dạng một chuỗi và một chuỗi khác chứa các chữ số bổ sung mà chúng ta có thể sử dụng để thay thế. Mỗi chữ số từ chuỗi thứ hai có thể được sử dụng nhiều nhất một lần. Đối với mỗi chữ số được chọn, chúng ta có thể thay thế bất kỳ vị trí nào trong số ban đầu. 

Mục tiêu là tối đa hóa giá trị số cuối cùng. 

Chi tiết quan trọng là thứ tự các chữ số trong chuỗi thay thế không quan trọng. Chúng ta có thể tự do chọn bất kỳ chữ số thay thế nào và đặt nó vào bất kỳ vị trí nào của số. Vì các chữ số lớn hơn ở các vị trí quan trọng hơn đóng góp nhiều hơn vào giá trị cuối cùng, điều này ngay lập tức gợi ý một chiến lược tham lam tập trung vào các vị trí ngoài cùng bên trái. 

Kích thước đầu vào đạt$10^5$chữ số cho cả hai chuỗi. Bất kỳ giải pháp nào liên tục xây dựng lại chuỗi hoặc thử nhiều kết hợp thay thế sẽ không thành công. Một thuật toán bậc hai sẽ thực hiện xung quanh$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét đầu vào này:```
999
123
```Câu trả lời vẫn phải là:```
999
```Việc triển khai bất cẩn có thể buộc phải sử dụng tất cả các chữ số thay thế và vô tình làm giảm số lượng. 

Một trường hợp tế nhị khác là:```
1000
999
```Câu trả lời tốt nhất là:```
9991
```chữ số`9`nên được sử dụng ở các vị trí sớm nhất trước tiên vì các chữ số trước đó có ý nghĩa cao hơn. Thay thế các chữ số cuối cùng trước tiên sẽ cho kết quả tồi tệ hơn. 

Bản sao cũng quan trọng:```
1234
3321
```Câu trả lời đúng là:```
3334
```Sau khi sắp xếp các chữ số thay thế giảm dần, chúng ta nên tham lam sử dụng chữ số hữu ích lớn nhất mỗi lần. Sử dụng chữ số nhỏ hơn quá sớm sẽ lãng phí cơ hội. 

Một kịch bản phức tạp cuối cùng là khi các chữ số thay thế bằng chữ số hiện tại:```
555
555
```Câu trả lời vẫn là:```
555
```Việc thay thế bằng một chữ số bằng nhau sẽ không thay đổi gì cả, vì vậy việc này vô hại nhưng không cần thiết. Thuật toán tham lam đương nhiên sẽ bỏ qua các thao tác như vậy. 

## Phương pháp tiếp cận 

Giải pháp vũ phu sẽ thử mọi cách có thể để gán các chữ số thay thế cho các vị trí. Đối với mỗi chữ số trong bộ thay thế, chúng ta có thể sử dụng hoặc bỏ qua nó và nếu sử dụng nó, chúng ta có thể đặt nó vào bất kỳ vị trí nào của số ban đầu. Số lượng các khả năng bùng nổ theo cấp số nhân. Ngay cả với chiều dài 20, không gian tìm kiếm vẫn trở nên khổng lồ. 

Chúng ta có thể đơn giản hóa suy nghĩ bằng cách tập trung vào giá trị vị trí. Chữ số ngoài cùng bên trái thống trị mọi chữ số ở bên phải của nó. Tăng chữ số đầu tiên lên 1 luôn tốt hơn tăng bất kỳ chữ số nào sau đó lên 9. 

Sự quan sát đó thay đổi hoàn toàn vấn đề. Thay vì khám phá các kết hợp, chúng ta nên luôn cố gắng cải thiện vị trí sớm nhất có thể bằng cách sử dụng chữ số thay thế lớn nhất hiện có. 

Giả sử chúng ta sắp xếp tất cả các chữ số thay thế theo thứ tự giảm dần. Sau đó chúng ta quét số ban đầu từ trái sang phải. Tại mỗi vị trí, nếu chữ số thay thế lớn nhất chưa được sử dụng hiện tại lớn hơn chữ số hiện tại thì chúng ta sẽ thay thế nó. Nếu không, chúng tôi bỏ qua vị trí. 

Tại sao điều này là tối ưu? Bởi vì nếu một chữ số có thể cải thiện vị trí nào đó thì sử dụng nó sớm hơn luôn tốt hơn là để dành cho sau này. Và nếu chữ số lớn nhất còn lại không thể cải thiện vị trí hiện tại thì không có chữ số nhỏ hơn nào có thể cải thiện được vị trí đó. 

Điều này biến vấn đề thành một đường chuyền tham lam đơn giản sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam tối ưu | O(n + m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số gốc dưới dạng danh sách các ký tự để có thể sửa đổi các chữ số tại chỗ. 
2. Đọc các chữ số thay thế và sắp xếp chúng theo thứ tự giảm dần. 
3. Duy trì một con trỏ`j`đến chữ số thay thế chưa sử dụng lớn nhất hiện tại. 
4. Quét số gốc từ trái sang phải. 
5. Đối với từng vị trí: 

1. Nếu tất cả các chữ số thay thế đã được sử dụng, hãy dừng lại. 
2. So sánh chữ số hiện tại với chữ số thay thế lớn nhất chưa được sử dụng. 
3. Nếu chữ số thay thế lớn hơn, hãy thay thế chữ số hiện tại và di chuyển`j`phía trước. 
4. Nếu không, giữ nguyên chữ số hiện tại. 
6. In số đã sửa đổi. 

Ý tưởng chính là các vị trí trước đó chiếm ưu thế hơn các vị trí sau. Bất cứ khi nào chúng ta có thể cải thiện một vị trí, việc trì hoãn sự cải thiện đó không thể giúp ích được gì. Sử dụng ngay chữ số lớn nhất còn lại luôn là tối ưu. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng tiền tố được xử lý đã lớn nhất có thể. 

Giả sử chúng ta đang kiểm tra vị trí`i`. Nếu chữ số thay thế lớn nhất chưa được sử dụng lớn hơn chữ số hiện tại thì mọi giải pháp tối ưu đều phải đặt một số chữ số cải thiện ở vị trí này. Sử dụng số lớn nhất là tối ưu vì chữ số lớn hơn ở vị trí trước đó luôn đánh bại mọi cách sắp xếp có chữ số nhỏ hơn ở đó. 

Nếu chữ số thay thế lớn nhất chưa được sử dụng không lớn hơn chữ số hiện tại thì không có chữ số nào còn lại có thể cải thiện vị trí này. Bỏ qua nó là bắt buộc. 

Bởi vì mọi quyết định đều tối ưu cục bộ và không thể gây tổn hại đến các vị trí trong tương lai nên chiến lược tham lam sẽ tạo ra số lượng tối đa trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = list(input().strip())
s = sorted(input().strip(), reverse=True)

j = 0

for i in range(len(a)):
    if j == len(s):
        break

    if s[j] > a[i]:
        a[i] = s[j]
        j += 1

print("".join(a))
```Số ban đầu được chuyển đổi thành danh sách vì chuỗi Python là bất biến. Cập nhật trực tiếp các ký tự trong danh sách sẽ hiệu quả và tránh việc phải xây dựng lại các chuỗi nhiều lần. 

Các chữ số thay thế được sắp xếp theo thứ tự giảm dần nên chữ số lớn nhất hiện có luôn dễ dàng truy cập. Đây là bước chuẩn bị tham lam cốt lõi. 

Con trỏ`j`theo dõi chữ số thay thế nào hiện không được sử dụng. Mỗi chữ số thay thế chỉ có thể được sử dụng một lần, do đó con trỏ chỉ tiến lên khi việc thay thế thực sự xảy ra. 

Vòng lặp xử lý các chữ số từ trái sang phải vì các vị trí trước đó có giá trị hơn. Nếu chữ số thay thế hiện tại không lớn hơn chữ số hiện tại thì việc thay thế sẽ không cải thiện kết quả. Vì tất cả các chữ số thay thế còn lại thậm chí còn nhỏ hơn nên không có lý do gì để thử thay thế thêm cho vị trí này. 

Thuật toán không bao giờ tạo ra các số 0 đứng đầu vì việc thay thế chỉ xảy ra khi chữ số mới lớn hơn chữ số hiện có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1024
010
```Các chữ số thay thế được sắp xếp:```
100
```| Vị trí | Chữ số hiện tại | Thay thế chưa sử dụng lớn nhất | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | Bỏ qua | 1024 | 
| 1 | 0 | 1 | Thay thế | 1124 | 
| 2 | 2 | 0 | Bỏ qua | 1124 | 
| 3 | 4 | 0 | Bỏ qua | 1124 | 

Câu trả lời cuối cùng:```
1124
```Dấu vết này cho thấy tại sao không nên sử dụng các chữ số bằng nhau. Thay thế cái đầu tiên`1`với người khác`1`không thay đổi gì và lãng phí một cơ hội thay thế. 

### Ví dụ 2 

đầu vào:```
1000
999
```Các chữ số thay thế được sắp xếp:```
999
```| Vị trí | Chữ số hiện tại | Thay thế chưa sử dụng lớn nhất | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 9 | Thay thế | 9000 | 
| 1 | 0 | 9 | Thay thế | 9900 | 
| 2 | 0 | 9 | Thay thế | 9990 | 
| 3 | 0 | Không có | Dừng lại | 9990 | 

Đợi đã, điều này không tối ưu. Chúng ta vẫn còn một chữ số gốc chưa được chạm vào ở cuối, cho biết:```
9990
```Nhưng chúng ta có thể làm tốt hơn bằng cách thay thế phần còn lại cuối cùng`0`quá: 

| Vị trí | Chữ số hiện tại | Thay thế chưa sử dụng lớn nhất | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 3 | 0 | 9 | Thay thế | 9999 | 

Câu trả lời cuối cùng:```
9999
```Điều này chứng tỏ rằng mọi sự thay thế cải tiến nên được sử dụng ngay lập tức ở vị trí sớm nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log m) | Sắp xếp các chữ số thay thế có giá O(m log m), quá trình quét là tuyến tính | 
| Không gian | O(m) | Mảng chữ số thay thế được sắp xếp lưu trữ m ký tự | 

Với$n, m \le 10^5$, việc sắp xếp chiếm ưu thế trong thời gian chạy. Xung quanh$10^5 \log 10^5$các hoạt động dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a = list(input().strip())
    s = sorted(input().strip(), reverse=True)

    j = 0

    for i in range(len(a)):
        if j == len(s):
            break

        if s[j] > a[i]:
            a[i] = s[j]
            j += 1

    print("".join(a))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue()

# provided sample
assert run("1024\n010\n") == "1124\n", "sample 1"

# minimum size
assert run("1\n9\n") == "9\n", "single digit replacement"

# no replacement helps
assert run("999\n123\n") == "999\n", "all replacements smaller"

# repeated digits
assert run("1234\n3321\n") == "3334\n", "duplicate replacement digits"

# leading digit improvement
assert run("1000\n9999\n") == "9999\n", "replace earliest positions first"

# equal digits should not waste replacements
assert run("555\n555\n") == "555\n", "equal digits skipped"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 9`|`9`| Kích thước đầu vào tối thiểu | 
|`999 / 123`|`999`| Những thay thế làm giảm giá trị sẽ bị bỏ qua | 
|`1234 / 3321`|`3334`| Các chữ số thay thế trùng lặp được xử lý chính xác | 
|`1000 / 9999`|`9999`| Ưu tiên các vị trí trước đó | 
|`555 / 555`|`555`| Các chữ số bằng nhau được bỏ qua | 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi chữ số thay thế đều nhỏ hơn các chữ số ban đầu:```
987
111
```Các chữ số thay thế được sắp xếp vẫn còn`111`. Tại mọi vị trí, chữ số thay thế hiện tại không lớn hơn chữ số hiện tại, do đó thuật toán không bao giờ thực hiện thay thế. Đầu ra vẫn còn:```
987
```Điều này khẳng định rằng quy luật tham lam không bao giờ làm giảm số lượng. 

Bây giờ hãy xem xét nhiều chữ số thay thế lặp đi lặp lại:```
1111
999
```Thuật toán xử lý các vị trí từ trái qua phải: 

| Vị trí | Hiện tại | Thay thế | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 1 | 9 | 9111 | 
| 1 | 1 | 9 | 9911 | 
| 2 | 1 | 9 | 9991 | 

Câu trả lời cuối cùng là:```
9991
```Dấu vết cho thấy mỗi chữ số thay thế được sử dụng đúng một lần và luôn được áp dụng ở vị trí còn lại có giá trị nhất. 

Cuối cùng, kiểm tra các chữ số bằng nhau:```
4444
4444
```Ở mọi vị trí, chữ số thay thế bằng chữ số hiện tại. điều kiện`s[j] > a[i]`lần nào cũng thất bại nên số lượng vẫn không thay đổi:```
4444
```Điều này tránh lãng phí việc thay thế những thao tác không cải thiện được câu trả lời.
