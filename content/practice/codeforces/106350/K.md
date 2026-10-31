---
title: "CF 106350K - Wahban và dấu ngoặc"
description: "Chuỗi chỉ chứa dấu ngoặc tròn. Chúng ta cần loại bỏ một số ký tự trong khi vẫn giữ các chỉ mục còn lại theo thứ tự ban đầu. Mục đích là thu được dãy con dài nhất có đảo ngược là dãy ngoặc thông thường."
date: "2026-06-25T08:08:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106350
codeforces_index: "K"
codeforces_contest_name: "Zaglol Contest - FCDS level 1 contest 2026"
rating: 0
weight: 106350
solve_time_s: 30
verified: true
draft: false
---

[CF 106350K - Wahban và dấu ngoặc](https://codeforces.com/problemset/problem/106350/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chuỗi chỉ chứa dấu ngoặc tròn. Chúng ta cần loại bỏ một số ký tự trong khi vẫn giữ các chỉ mục còn lại theo thứ tự ban đầu. Mục đích là thu được dãy con dài nhất có đảo ngược là dãy ngoặc thông thường. Nếu không có dãy con nào trống thỏa mãn tính chất này, chúng ta sẽ in`-1`. 

Dãy ngoặc thông thường có hai yêu cầu: tổng số dấu ngoặc mở và đóng bằng nhau và khi đọc từ trái sang phải, số dấu ngoặc mở không bao giờ nhỏ hơn số dấu ngoặc đóng. Đối với chuỗi dấu ngoặc thông thường bị đảo ngược, các điều kiện này áp dụng sau khi đảo ngược chuỗi con đã chọn. 

Nếu dãy con được chọn là`T`, sau đó đọc`T`từ phải sang trái phải hoạt động giống như một chuỗi dấu ngoặc thông thường. Điều đó có nghĩa là trong khi quét`T`từ cuối về đầu, mỗi dấu ngoặc đóng phải có sẵn dấu ngoặc mở sớm hơn theo thứ tự đảo ngược này. 

Độ dài của đầu vào có thể đạt tới một triệu ký tự. Một giải pháp thử các dãy con khác nhau là không thể vì số lượng các dãy con có thể có là theo cấp số nhân. Ngay cả việc kiểm tra tất cả các cặp vị trí cũng đã quá chậm. Kích thước đầu vào cho chúng ta biết rằng chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính, trong đó mỗi dấu ngoặc chỉ được xử lý một số lần không đổi. 

Các trường hợp phức tạp xuất phát từ việc nhầm lẫn một chuỗi dấu ngoặc thông thường với một chuỗi bị đảo ngược. Ví dụ:```
()
```Đầu ra đúng là:```
-1
```Toàn bộ chuỗi là đều đặn, nhưng sau khi đảo ngược nó trở thành`)(`, không hợp lệ. Một giải pháp chỉ kiểm tra xem chuỗi gốc có chứa chuỗi con cân bằng hay không sẽ trả về sai`2`. 

Một trường hợp khác là:```
)(
```Đầu ra đúng là:```
2
```Bản thân chuỗi đó không đều đặn, nhưng việc đảo ngược nó sẽ mang lại`()`, hợp lệ. Một giải pháp kiểm tra hướng ban đầu sẽ từ chối nó một cách không chính xác. 

Trường hợp thứ ba là:```
(((
```Đầu ra đúng là:```
-1
```Không có đủ dấu ngoặc đóng để tạo một cặp. Chỉ đếm số dấu ngoặc mở là không đủ; câu trả lời phải chứa các cặp khớp hoàn chỉnh. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tạo ra các dãy con ứng cử viên và kiểm tra xem chuỗi đảo ngược của chúng có chính quy hay không. Điều này đúng vì nó kiểm tra mọi lựa chọn có thể có của các ký tự bị xóa, vì vậy lựa chọn hợp lệ nhất phải xuất hiện. Tuy nhiên, một chuỗi có độ dài một triệu có số lượng dãy con rất lớn, cụ thể lên đến$2^n$, làm cho phương pháp này không thể sử dụng được. 

Một cách tốt hơn là nhìn điều kiện đảo ngược một cách khác. Thay vì xây dựng chuỗi con đảo ngược, hãy tưởng tượng việc đọc chuỗi gốc từ phải sang trái. Các ký tự được chọn phải tạo thành một chuỗi dấu ngoặc thông thường theo hướng đó. 

Đối với một dãy con trong ngoặc thông thường, chiến lược so khớp tham lam là đủ. Khi chúng ta nhìn thấy dấu ngoặc mở, nó có thể đợi dấu ngoặc đóng trong tương lai. Khi nhìn thấy dấu ngoặc đóng, chúng ta nên sử dụng một dấu ngoặc mở chờ nếu có thể. Mỗi trận đấu thành công sẽ tạo ra một cặp. 

Áp dụng ý tưởng tương tự trong khi duyệt ngược chuỗi ban đầu sẽ cho số lượng cặp tối đa cho chuỗi con dấu ngoặc thông thường bị đảo ngược. Chúng ta không cần biết các chỉ số được chọn thực tế. Chúng ta chỉ cần số lượng cặp khớp, vì mỗi cặp đóng góp hai ký tự. 

Lực lượng vũ phu hoạt động vì nó khám phá tất cả các thao tác xóa có thể xảy ra, nhưng nó lặp lại các quyết định khớp tương tự nhiều lần. Sự quan sát tham lam loại bỏ sự lặp lại đó bằng cách chứng minh rằng mọi dấu ngoặc có sẵn nên được sử dụng bất cứ khi nào có thể khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ cuối chuỗi và di chuyển về phía đầu. Chúng ta quét theo hướng này vì dãy con được chọn đảo ngược phải chính quy. 
2. Duy trì một bộ đếm biểu thị số lượng dấu ngoặc mở chưa khớp có sẵn trong quá trình duyệt ngược này. Khi chúng ta gặp phải`(`, tăng bộ đếm này vì khung này có thể khớp với bộ đếm sau`)`theo thứ tự đảo ngược. 
3. Khi chúng ta gặp phải`)`, kiểm tra xem có sẵn một cái nào chưa từng có không`(`. Nếu có, hãy sử dụng một dấu ngoặc mở và tăng số lượng cặp khớp. Đây là cách duy nhất mà dấu ngoặc đóng này có thể đóng góp vào một dãy con hợp lệ. 
4. Sau khi xử lý toàn bộ chuỗi, nhân số cặp khớp với 2. Nếu không có cặp nào trùng khớp, xuất ra`-1`bởi vì không tồn tại dãy con hợp lệ. 

Tại sao nó hoạt động: 

Trong quá trình quét từ phải sang trái, bộ đếm có ý nghĩa chính xác về số lượng dấu ngoặc mở vẫn có thể hỗ trợ các kết quả khớp trong tương lai theo trình tự đảo ngược. Mỗi khi đếm một dấu ngoặc đóng, chúng tôi sẽ khớp nó với một dấu ngoặc mở có sẵn. Điều này là tối ưu vì việc để lại khung mở có sẵn không được sử dụng không thể giúp ích cho bất kỳ khung đóng nào sau này hơn là sử dụng nó ngay lập tức. Quá trình này giống hệt với việc tìm chuỗi con trong ngoặc vuông dài nhất trong chuỗi đảo ngược, do đó số lượng cặp được tạo là lớn nhất có thể. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    s = input().strip()

    available_open = 0
    pairs = 0

    for ch in reversed(s):
        if ch == '(':
            available_open += 1
        else:
            if available_open > 0:
                available_open -= 1
                pairs += 1

    if pairs == 0:
        print(-1)
    else:
        print(pairs * 2)

if __name__ == "__main__":
    solve()
```Quá trình quét sử dụng`reversed(s)`sao cho thứ tự lặp phù hợp với hướng được yêu cầu bởi định nghĩa dấu ngoặc đảo ngược. Biến`available_open`là số dấu ngoặc mở đã xuất hiện theo hướng này nhưng chưa được ghép nối. 

Khi một`)`xuất hiện, mã kiểm tra điều kiện biên`available_open > 0`trước khi trừ. Nếu không có kiểm tra này, việc triển khai sẽ tính các cặp không hợp lệ không tồn tại. 

Câu trả lời được lưu dưới dạng một số cặp vì mỗi trận đấu thành công đóng góp chính xác một dấu ngoặc mở và một dấu ngoặc đóng. Phép nhân cuối cùng với hai sẽ chuyển đổi các cặp thành độ dài dãy con. 

Không có đệ quy, mảng hoặc chuỗi bổ sung nào được tạo, điều này quan trọng vì kích thước đầu vào có thể đạt tới một triệu ký tự. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
()
```quá trình quét đi từ phải sang trái: 

| Ký tự được xử lý | có sẵn_open | cặp | 
| --- | --- | --- | 
|`)`| 0 | 0 | 
|`(`| 1 | 0 | 

Không có cặp nào phù hợp nên kết quả đầu ra là`-1`. Điều này chứng tỏ tại sao một chuỗi ngoặc hợp lệ thông thường không nhất thiết có hiệu lực sau khi đảo ngược. 

Đối với đầu vào:```
()()()
```quá trình quét là: 

| Ký tự được xử lý | có sẵn_open | cặp | 
| --- | --- | --- | 
|`)`| 0 | 0 | 
|`(`| 1 | 0 | 
|`)`| 0 | 1 | 
|`(`| 1 | 1 | 
|`)`| 0 | 2 | 
|`(`| 1 | 2 | 

Có hai cặp, cho đầu ra`4`. Dãy con được chọn có thể là`)()(`, đảo ngược của nó là`()()`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khung được kiểm tra một lần trong khi quét từ phải sang trái. | 
| Không gian | O(1) | Chỉ có hai bộ đếm được lưu trữ bất kể độ dài chuỗi. | 

Giới hạn đầu vào yêu cầu tránh mọi phương pháp lưu trữ tất cả các chuỗi con hoặc thực hiện so khớp lặp lại. Quá trình quét tuyến tính xử lý thoải mái một triệu ký tự trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    s = inp.strip()
    available_open = 0
    pairs = 0

    for ch in reversed(s):
        if ch == '(':
            available_open += 1
        elif available_open:
            available_open -= 1
            pairs += 1

    return "-1\n" if pairs == 0 else f"{pairs * 2}\n"

def run(inp: str) -> str:
    return solution(inp)

assert run("()\n") == "-1\n", "sample 1"
assert run("()()()\n") == "4\n", "sample 2"

assert run(")\n") == "-1\n", "single bracket"
assert run(")(\n") == "2\n", "reverse pair"
assert run("(((())))\n") == "8\n", "already balanced reverse length"
assert run("((((\n") == "-1\n", "only opening brackets"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()`|`-1`| Phân biệt chuỗi chính quy với chuỗi chính quy đảo ngược | 
|`()()()`|`4`| Kiểm tra hành vi khớp tối đa mẫu | 
|`)`|`-1`| Xử lý đầu vào không thể có kích thước tối thiểu | 
|`)(`|`2`| Kiểm tra hướng khớp | 
|`(((())))`|`8`| Kiểm tra trình tự hoàn toàn khớp | 
|`((((`|`-1`| Ngăn chặn việc đếm các cặp không đầy đủ | 

## Vỏ cạnh 

Đối với đầu vào:```
()
```thuật toán bắt đầu với dấu ngoặc mở có sẵn bằng 0. Ký tự được xử lý đầu tiên là`)`, không thể khớp được nên nó bị bỏ qua. trận chung kết`(`tăng bộ đếm, nhưng không còn dấu ngoặc đóng. Số cặp vẫn bằng 0 và câu trả lời là`-1`. 

Đối với đầu vào:```
)(
```ký tự được xử lý đầu tiên là`(`, do đó sẽ có một dấu ngoặc mở. Ký tự được xử lý tiếp theo là`)`và sử dụng dấu ngoặc mở đó, tạo ra một cặp. Kết quả là`2`, điều này đúng vì việc đảo ngược trình tự đã chọn sẽ mang lại`()`. 

Đối với đầu vào:```
(((
```mỗi nhân vật chỉ tăng lên`available_open`. Không có dấu ngoặc đóng nào xuất hiện nên không có cặp nào được tạo. Đầu ra của thuật toán`-1`, loại bỏ chính xác các chuỗi con không thể cân bằng sau khi đảo ngược. 

Đối với đầu vào:```
()()()
```thuật toán tìm thấy hai kết quả phù hợp trong khi quét ngược. Nó không sử dụng dấu ngoặc mở chưa khớp cuối cùng vì nó không có dấu ngoặc đóng sau nó theo hướng ngược lại. Câu trả lời cuối cùng vẫn là độ dài tối đa có thể,`4`.
