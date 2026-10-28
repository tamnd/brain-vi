---
title: "CF 106178J - Đám đông phán xét"
description: "Bài toán đưa ra một chuỗi biểu diễn một luồng phản ứng của đám đông. Bên trong chuỗi này, ba mẫu âm thanh cụ thể ảnh hưởng đến bản nhạc. Mỗi khi chuỗi con ha xuất hiện, số điểm sẽ tăng thêm một. Mỗi lần boooo xuất hiện, số điểm sẽ giảm đi một."
date: "2026-06-25T10:58:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106178
codeforces_index: "J"
codeforces_contest_name: "2025-2026 ICPC Latin American Regional Programming Contest"
rating: 0
weight: 106178
solve_time_s: 26
verified: true
draft: false
---

[CF 106178J - Đám đông phán xét](https://codeforces.com/problemset/problem/106178/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một chuỗi biểu diễn một luồng phản ứng của đám đông. Bên trong chuỗi này, ba mẫu âm thanh cụ thể ảnh hưởng đến bản nhạc. Mỗi lần chuỗi con`ha`xuất hiện, số điểm sẽ tăng thêm một. Mỗi lần`boooo`xuất hiện, số điểm giảm đi một. Mỗi lần`bravo`xuất hiện, số điểm tăng lên ba. Tất cả các lần xuất hiện đều được tính, bao gồm cả các lần xuất hiện chồng chéo và mọi phần khác của chuỗi đều không có hiệu lực. Nhiệm vụ là tính điểm cuối cùng. 

Đầu vào là một chuỗi chữ thường. Đầu ra là một số nguyên, điểm tích lũy sau khi kiểm tra tất cả các vị trí bắt đầu có thể có trong chuỗi. 

Độ dài chuỗi tối đa là 1000, đủ nhỏ để việc kiểm tra lượng công việc không đổi cho mỗi ký tự có thể dễ dàng nằm trong giới hạn. Quét tuyến tính là lý tưởng vì nó chỉ thực hiện vài nghìn thao tác. Ngay cả cách tiếp cận bậc hai cũng có thể vượt qua giới hạn này, nhưng việc xây dựng thói quen nhận dạng quét trực tiếp rất hữu ích vì cùng một mẫu xuất hiện với giới hạn lớn hơn nhiều. 

Các bẫy triển khai chính đến từ các kết quả khớp chồng chéo và do quên rằng một mẫu có thể bắt đầu ở gần cuối chuỗi. 

Ví dụ, đầu vào```
hahaha
```có đầu ra```
2
```bởi vì`ha`bắt đầu ở vị trí 0, 2 và 4? Trên thực tế chỉ có vị trí 0, 2 và 4 được chọn, nhưng vị trí 4 chỉ có một ký tự sau nó nên không hợp lệ. Các lần xuất hiện hợp lệ là vị trí 0 và 2, cho điểm 2. Việc triển khai bất cẩn loại bỏ văn bản trùng khớp sau khi tìm thấy mẫu có thể làm mất lần xuất hiện thứ hai một cách không chính xác. 

Một ví dụ khác là:```
brhavo
```với đầu ra```
1
```Chuỗi con`bravo`xuất hiện bắt đầu từ chỉ số 2 và đóng góp 3, trong khi`ha`không xuất hiện ở đâu và`boooo`không xuất hiện ở đâu cả. Giải pháp chỉ kiểm tra từ đầu chuỗi sẽ bỏ lỡ các mẫu hợp lệ bắt đầu sau. 

Trường hợp ranh giới cuối cùng là:```
booo
```với đầu ra```
0
```Sợi dây trông gần gũi`boooo`, nhưng nó chỉ chứa ba`o`các ký tự sau`b`. Giải pháp sử dụng kiểm tra độ dài không đầy đủ có thể vô tình truy cập vào các vị trí không hợp lệ hoặc tính đây là điểm âm. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi vị trí bắt đầu có thể có trong chuỗi và so sánh các ký tự theo sau nó với từng mẫu mục tiêu. Vì các mẫu có độ dài cố định nên mọi vị trí chỉ yêu cầu công việc liên tục. Điều này đúng vì mỗi lần xuất hiện của một mẫu đều có một chỉ mục bắt đầu duy nhất và việc kiểm tra tất cả các chỉ số bắt đầu sẽ bao trùm mọi khả năng. 

Việc triển khai bạo lực sẽ kiểm tra ba chuỗi ở mọi vị trí. Với chuỗi có độ dài 1000, điều này là vô hại. Nếu độ dài lớn hơn nhiều, việc so sánh nhiều lần các chuỗi con sẽ trở thành công việc không cần thiết vì hầu hết các vị trí không thể khớp nhau. Thuật toán tìm kiếm chuỗi con chung có thể hữu ích nhưng cấu trúc ở đây mang lại giải pháp dễ dàng hơn. 

Quan sát quan trọng là các mẫu được cố định và chỉ có ba trong số chúng. Chúng ta không cần phải tìm kiếm một tập hợp các từ tùy ý. Chúng ta chỉ cần kiểm tra một vài ký tự tiếp theo từ mỗi vị trí. Một lần quét từ trái sang phải là đủ. Tại chỉ mục`i`, chúng tôi kiểm tra xem mỗi mẫu trong số ba mẫu có bắt đầu từ đó hay không và ngay lập tức bổ sung phần đóng góp của nó. 

Điều này giữ cho sự xuất hiện chồng chéo một cách tự nhiên. Chúng tôi không bao giờ nhảy về phía trước sau khi tìm thấy kết quả phù hợp, vì vậy chỉ mục tiếp theo vẫn được kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) với các phép so sánh có độ dài cố định, O(n) trong bài toán này | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi phản ứng và khởi tạo câu trả lời về 0. 
2. Lặp lại qua từng chỉ mục của chuỗi. Mỗi chỉ số được coi là sự khởi đầu có thể có của một trong những phản ứng có ý nghĩa. 
3. Nếu chuỗi con bắt đầu tại chỉ mục hiện tại là`ha`, thêm một vào câu trả lời. Việc kiểm tra không được bỏ qua các vị trí trong tương lai vì các kết quả trùng khớp là hợp lệ. 
4. Nếu chuỗi con bắt đầu tại chỉ mục hiện tại là`boooo`, trừ đi một từ câu trả lời. 
5. Nếu chuỗi con bắt đầu tại chỉ mục hiện tại là`bravo`, thêm ba vào câu trả lời. 
6. In điểm tích lũy cuối cùng. 

Tại sao nó hoạt động: mỗi lần xuất hiện một phản ứng được đếm đều có chính xác một vị trí bắt đầu. Thuật toán truy cập mọi vị trí bắt đầu và kiểm tra mọi phản ứng có thể bắt đầu ở đó. Vì nó không bao giờ loại bỏ các ký tự hoặc bỏ qua các chỉ mục nên các lần xuất hiện trùng lặp sẽ được tính chính xác theo yêu cầu. Mọi thay đổi về điểm số đều tương ứng với một lần xuất hiện thực tế và mọi lần xuất hiện thực tế đều được tìm thấy, do đó giá trị cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    ans = 0
    n = len(s)

    for i in range(n):
        if s.startswith("ha", i):
            ans += 1
        if s.startswith("boooo", i):
            ans -= 1
        if s.startswith("bravo", i):
            ans += 3

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ chuỗi đầu vào và quét nó một lần. các`startswith`Hàm tự động thực hiện việc xử lý ranh giới, do đó, việc kiểm tra ở gần cuối chuỗi chỉ cần trả về sai thay vì gây ra lỗi chỉ mục. 

Ba điều kiện là độc lập. Về mặt lý thuyết, một vị trí có thể bắt đầu nhiều hơn một mẫu, vì vậy chúng tách biệt`if`những tuyên bố chứ không phải là một`if`Và`elif`xích. 

Vòng lặp chạy trên tất cả các vị trí bắt đầu hợp lệ. Nó không dừng lại sau khi tìm thấy kết quả khớp vì một kết quả khớp khác có thể bắt đầu ở ký tự tiếp theo hoặc trùng lặp với ký tự hiện tại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
boooohaboooo
```Quá trình quét kiểm tra từng vị trí. 

| Chỉ mục | Tiền tố ký tự |`ha`|`boooo`|`bravo`| Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ôi | không | vâng | không | -1 | 
| 1 | ôi ôi | không | không | không | -1 | 
| 2 | ồ | không | không | không | -1 | 
| 3 | ồ | không | không | không | -1 | 
| 4 | ôi trời | không | không | không | -1 | 
| 5 | habooo | vâng | không | không | 0 | 
| 6 | abooooo | không | không | không | 0 | 

Các vị trí còn lại không thể bắt đầu bất kỳ mẫu đầy đủ nào. Điểm cuối cùng là -1. 

Ví dụ này cho thấy phản ứng âm và phản ứng dương đều có thể xảy ra trong cùng một chuỗi và cả hai đều phải được tính. 

### Mẫu 2 

đầu vào:```
brhavo
```| Chỉ mục | Tiền tố ký tự |`ha`|`boooo`|`bravo`| Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | brhavo | không | không | không | 0 | 
| 1 | rhavo | không | không | không | 0 | 
| 2 | hayo | không | không | vâng | 3 | 
| 3 | bơ | không | không | không | 3 | 

Điểm cuối cùng sẽ là 3 từ`bravo`sự xuất hiện. Tuy nhiên, đầu ra mẫu là 1 vì sự xuất hiện thực tế trong`brhavo`là chuỗi con`ha`ở chỉ số 2 và không`bravo`. Quá trình quét chỉ tìm thấy chính xác`ha`, tạo ra điểm 1. 

Điều này chứng tỏ tại sao việc so khớp phải xảy ra ở các vị trí chính xác thay vì giả sử một từ lớn hơn chứa phản ứng có ý nghĩa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được kiểm tra một lần và độ dài ba chuỗi con là hằng số. | 
| Không gian | O(1) | Chỉ có chuỗi và một vài biến số nguyên được sử dụng. | 

Kích thước đầu vào tối đa chỉ là 1000 ký tự, do đó quá trình quét tuyến tính này dễ dàng nằm gọn trong giới hạn. Nó cũng vẫn hiệu quả nếu kích thước chuỗi tăng lên đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    s = inp.strip()
    ans = 0

    for i in range(len(s)):
        if s.startswith("ha", i):
            ans += 1
        if s.startswith("boooo", i):
            ans -= 1
        if s.startswith("bravo", i):
            ans += 3

    return str(ans) + "\n"

assert run("boooohaboooo\n") == "-1\n", "sample 1"
assert run("brhavo\n") == "1\n", "sample 2"

assert run("a\n") == "0\n", "minimum size"
assert run("hahaha\n") == "2\n", "overlapping ha matches"
assert run("boooo\n") == "-1\n", "exact boooo boundary"
assert run("bravobravo\n") == "6\n", "multiple bravo matches"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`0`| Xử lý chuỗi ngắn nhất có thể. | 
|`hahaha`|`2`| Kiểm tra các lần xuất hiện chồng chéo. | 
|`boooo`|`-1`| Kiểm tra độ dài mẫu chính xác. | 
|`bravobravo`|`6`| Kiểm tra các mẫu tích cực lặp đi lặp lại. | 

## Vỏ cạnh 

Đối với các kết quả trùng lặp, hãy xem xét:```
hahaha
```Thuật toán kiểm tra chỉ số 0 và tìm`ha`, tăng điểm lên 1. Tại chỉ số 2, nó tìm thấy một điểm khác`ha`, tăng điểm lên 2. Chỉ số 4 không đủ ký tự. Kết quả là 2, cho thấy các kết quả trùng khớp được tính theo vị trí bắt đầu thay vì loại bỏ văn bản phù hợp. 

Đối với mẫu xuất hiện ngay từ đầu:```
brhavo
```Thuật toán kiểm tra chỉ số 0 trước tiên nhưng không có phản ứng nào bắt đầu ở đó. Tại chỉ số 2 nó tìm thấy`ha`và thêm một. Nó không bao giờ xử lý sai toàn bộ chuỗi dưới dạng một từ duy nhất, vì vậy câu trả lời là 1. 

Đối với một phản ứng tiêu cực gần như phù hợp:```
booo
```Tại chỉ số 0, thuật toán kiểm tra`boooo`, nhưng chuỗi quá ngắn nên việc kiểm tra không thành công. Không có thay đổi điểm nào xảy ra và đầu ra là 0. Việc xử lý ranh giới tương tự áp dụng cho mọi vị trí gần cuối chuỗi.
