---
title: "CF 106189A - Giấy dán tường"
description: "Igor có n bức tường. Mỗi bức tường cao đúng 3 mét và bức tường thứ i có chiều rộng ai. Cửa hàng bán cuộn giấy dán tường cũng cao 3 mét nên chiều cao không bao giờ gây lãng phí. Mỗi cuộn có chiều rộng chính xác là k mét."
date: "2026-06-25T06:47:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106189
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2025"
rating: 0
weight: 106189
solve_time_s: 34
verified: true
draft: false
---

[CF 106189A - Hình nền](https://codeforces.com/problemset/problem/106189/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Igor có`n`những bức tường. Mỗi bức tường cao đúng 3 mét, và`i`-bức tường có chiều rộng`a_i`. 

Cửa hàng bán cuộn giấy dán tường cũng cao 3 mét nên chiều cao không bao giờ gây lãng phí. Mỗi cuộn bao gồm chính xác`k`mét chiều rộng. 

Đối với một bức tường có chiều rộng`a_i`, Igor hình nền nó theo một cách rất cụ thể. Anh ấy sử dụng càng nhiều cuộn có chiều rộng tối đa càng tốt. Nếu một số chiều rộng nhỏ hơn`k`còn lại, anh ta cắt miếng cần thiết từ một cuộn mới và ngay lập tức loại bỏ phần còn lại của cuộn đó. Thức ăn thừa từ một bức tường không thể được tái sử dụng cho bức tường khác. 

Nhiệm vụ là xác định tổng số cuộn cần mua. 

Các ràng buộc đủ lớn để chúng ta không thể mô phỏng bất cứ điều gì phức tạp. Chúng tôi có tới`10^6`các bức tường và mỗi chiều rộng có thể lớn bằng`10^9`. Bất kỳ giải pháp nào cũng phải xử lý từng bức tường một cách độc lập trong thời gian không đổi. Quét tuyến tính trên các bức tường là hoàn toàn an toàn, trong khi mọi thứ bậc hai là không thể. 

Nguồn gốc chính của sai lầm là việc diễn giải cách thức hoạt động của các cuộn từng phần. 

Coi như:```
n = 1, k = 5
a = [13]
```Bức tường cần hai cuộn đầy đủ cho 10 mét và một cuộn bổ sung cho 3 mét còn lại. Câu trả lời là`3`, không`2`, vì 2 mét còn lại từ cuộn cuối cùng đó không thể tái sử dụng được. 

Một sai lầm dễ xảy ra khác khi chiều rộng của bức tường đã được chia cho`k`.```
n = 1, k = 5
a = [10]
```Câu trả lời là`2`, không`3`. Không cần cuộn thêm vì không có phần dư nào được che phủ. 

Trường hợp cạnh cuối cùng là`k = 1`.```
n = 1, k = 1
a = [7]
```Mỗi cuộn bao phủ chính xác một mét, vì vậy câu trả lời là`7`. Vì mọi chiều rộng đều chia hết cho 1 nên không bao giờ có bất kỳ cuộn nào được sử dụng một phần. 

Số lượng cuộn cần thiết cho một bức tường chính xác bằng trần nhà`a_i / k`. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để nghĩ về một bức tường là trừ đi nhiều lần`k`từ chiều rộng của nó cho đến khi không còn lại gì. Mỗi phép trừ tương ứng với một cuộn. Điều này đúng vì mỗi cuộn đóng góp nhiều nhất`k`mét chiều rộng. 

Đối với một bức tường có chiều rộng`a_i`, điều này đòi hỏi khoảng`a_i / k`lần lặp lại. Từ`a_i`có thể lớn như`10^9`, một bức tường có thể cần tới hàng tỷ thao tác. Với tối đa`10^6`tường, phương án này hoàn toàn không khả thi. 

Quan sát cho thấy phép trừ lặp đi lặp lại chỉ đơn giản là tính toán mức chia trần. 

Nếu chiều rộng của bức tường là`a_i`, sau đó:```
rolls = ceil(a_i / k)
```Khi`a_i`chia hết cho`k`, mỗi cuộn đều được sử dụng đầy đủ. 

Khi`a_i`để lại phần còn lại, tất cả các khối có chiều rộng hoàn chỉnh`k`sử dụng toàn bộ cuộn và cần thêm một cuộn cho phần còn lại. 

Vì vậy, câu trả lời trở thành:```
sum(ceil(a_i / k))
```Sử dụng số học số nguyên:```
ceil(a_i / k) = (a_i + k - 1) // k
```Chúng tôi đánh giá công thức này cho mọi bức tường và tích lũy kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Σ(a_i / k)) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. 
2. Đọc tất cả chiều rộng của tường. 
3. Khởi tạo`answer = 0`. 
4. Đối với mỗi chiều rộng tường`a`: 

Tính toán```
rolls = (a + k - 1) // k
```Đây là dạng số nguyên của`ceil(a / k)`. 
5. Thêm`rolls`ĐẾN`answer`. 
6. Sau khi xử lý tất cả các bức tường, in`answer`. 

### Tại sao nó hoạt động 

Đối với bất kỳ chiều rộng tường nào`a`, viết:```
a = qk + r
```Ở đâu`0 ≤ r < k`. 

Nếu như`r = 0`, chính xác`q`cuộn đầy đủ bao phủ bức tường. 

Nếu như`r > 0`,`q`bìa cuộn đầy đủ`qk`chiều rộng và cần thêm một cuộn để che phần còn lại`r`chiều rộng. 

Vậy số cuộn giấy là:```
q            if r = 0
q + 1        if r > 0
```đó chính xác là`ceil(a / k)`. 

Vì các bức tường độc lập và phần thừa không thể được tái sử dụng giữa các bức tường nên câu trả lời tổng thể là tổng các giá trị này trên tất cả các bức tường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = map(int, input().split())

ans = 0
for x in a:
    ans += (x + k - 1) // k

print(ans)
```Chương trình thực hiện một lần đi qua chiều rộng của bức tường. 

biểu hiện`(x + k - 1) // k`thực hiện phép chia trần chỉ sử dụng số nguyên. Điều này tránh số học dấu phẩy động và hoạt động chính xác ngay cả đối với các giá trị rất lớn. 

Câu trả lời có thể trở nên lớn hơn`10^9`, vì có thể có tới`10^6`những bức tường. Số nguyên Python tự động xử lý các giá trị đó một cách an toàn. 

Không cần thêm mảng hoặc cấu trúc dữ liệu nào ngoài chính đầu vào, do đó mức sử dụng bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
10 13 20 21
```| Chiều rộng tường | Tính toán | Bánh cuộn | Tổng số chạy | 
| --- | --- | --- | --- | 
| 10 | (10 + 4) // 5 | 2 | 2 | 
| 13 | (13 + 4) // 5 | 3 | 5 | 
| 20 | (20 + 4) // 5 | 4 | 9 | 
| 21 | (21 + 4) // 5 | 5 | 14 | 

Đầu ra:```
14
```Ví dụ này chứa cả chiều rộng chia hết và không chia hết. Bức tường có chiều rộng 21 chứng tỏ tại sao phần còn lại cần thêm một cuộn. 

### Ví dụ 2 

đầu vào:```
4 8
8 16 32 33
```| Chiều rộng tường | Tính toán | Bánh cuộn | Tổng số chạy | 
| --- | --- | --- | --- | 
| 8 | (8 + 7) // 8 | 1 | 1 | 
| 16 | (16 + 7) // 8 | 2 | 3 | 
| 32 | (32 + 7) // 8 | 4 | 7 | 
| 33 | (33 + 7) // 8 | 5 | 12 | 

Đầu ra:```
12
```Ba bức tường đầu tiên chia đều cho 8. Bức tường cuối cùng có số dư là 1, vẫn cần phải cuộn thêm đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một phép tính thời gian không đổi trên mỗi bức tường | 
| Không gian | O(1) | Chỉ có một số biến được duy trì | 

Với tối đa`10^6`tường, quét tuyến tính chính xác là những gì các ràng buộc yêu cầu. Giải pháp dễ dàng phù hợp với giới hạn cuộc thi điển hình. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, k = map(int, input().split())
    arr = map(int, input().split())

    ans = 0
    for x in arr:
        ans += (x + k - 1) // k

    return str(ans)

# provided samples
assert run("4 5\n10 13 20 21\n") == "14", "sample 1"
assert run("3 1\n5 6 3\n") == "14", "sample 2"
assert run("4 8\n8 16 32 33\n") == "12", "sample 3"

# custom cases
assert run("1 5\n1\n") == "1", "minimum wall"
assert run("1 5\n10\n") == "2", "exact multiple"
assert run("3 7\n7 7 7\n") == "3", "all equal"
assert run("2 1000000\n1000000000 999999999\n") == "2000", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 / 1`|`1`| Bức tường khác 0 nhỏ nhất | 
|`1 5 / 10`|`2`| Chia hết chính xác | 
|`3 7 / 7 7 7`|`3`| Giá trị bằng nhau lặp lại | 
|`2 1000000 / 1000000000 999999999`|`2000`| Số học số lớn | 

## Vỏ cạnh 

Hãy xem xét:```
1 5
10
```Thuật toán tính toán:```
(10 + 4) // 5 = 2
```Không có phần còn lại nên không có cuộn bổ sung nào được thêm vào. Đầu ra là:```
2
```cái nào đúng. 

Bây giờ hãy xem xét:```
1 5
13
```Thuật toán tính toán:```
(13 + 4) // 5 = 3
```Hai cuộn dài 10 mét và cuộn thứ ba phủ 3 mét còn lại. Đầu ra là:```
3
```phù hợp với hạn chế của tuyên bố rằng một cuộn được sử dụng một phần không thể được sử dụng lại. 

Cuối cùng:```
1 1
7
```Thuật toán tính toán:```
(7 + 0) // 1 = 7
```Mỗi cuộn có độ dày chính xác là một mét, vì vậy cần có bảy cuộn. Đầu ra là:```
7
```Điều này xác nhận rằng công thức chia trần cũng xử lý chính xác chiều rộng cuộn nhỏ nhất có thể.
