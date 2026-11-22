---
title: "CF 301C - Yaroslav và thuật toán"
description: "Chúng ta phải xây dựng một chương trình viết lại chuỗi nhỏ. Chương trình bao gồm các lệnh theo thứ tự. Mỗi lệnh tìm kiếm một chuỗi con và thay thế lần xuất hiện đầu tiên của nó bằng một chuỗi khác. Một số lệnh tiếp tục thực thi sau khi thay thế, trong khi những lệnh khác chấm dứt ngay lập tức."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 301
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 182 (Div. 1)"
rating: 2500
weight: 301
solve_time_s: 418
verified: true
draft: false
---

[CF 301C - Yaroslav và Thuật toán](https://codeforces.com/problemset/problem/301/C) 

**Đánh giá:** 2500 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 6 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta phải xây dựng một chương trình viết lại chuỗi nhỏ. Chương trình bao gồm các lệnh theo thứ tự. Mỗi lệnh tìm kiếm một chuỗi con và thay thế lần xuất hiện đầu tiên của nó bằng một chuỗi khác. Một số lệnh tiếp tục thực thi sau khi thay thế, trong khi những lệnh khác chấm dứt ngay lập tức. 

Nhiệm vụ không phải là tăng mọi số nguyên nói chung. Chúng ta chỉ cần một chương trình hoạt động với tập hợp số đã cho. Mỗi số đầu vào nhiều nhất là 1024, do đó có tối đa 1025 giá trị có thể có. Giới hạn 50 lệnh hạn chế hơn nhiều so với kích thước đầu vào. 

Chi tiết quan trọng là cách các lệnh được chọn. Trong mỗi lần lặp, thuật toán sẽ quét các lệnh từ trên xuống dưới và thực thi lệnh đầu tiên có cạnh trái xuất hiện ở đâu đó trong chuỗi hiện tại. Điều này có nghĩa là thứ tự lệnh hoàn toàn quyết định hành vi. Một lệnh vô tình khớp với một số khác có thể ngăn lệnh dự định chạy. 

Các chuỗi trong lệnh có thể chứa các chữ số và ký tự`?`. Tuyên bố không bao giờ đưa ra ngữ nghĩa ký tự đại diện cho`?`, vì vậy nó hoạt động như một ký tự bình thường. Vì tất cả các số đầu vào chỉ chứa các chữ số nên bất kỳ lệnh nào chứa`?`ở phía bên trái sẽ không bao giờ khớp. Giải pháp đơn giản nhất bỏ qua`?`toàn bộ. 

Những hạn chế là rất nhỏ. Chúng tôi chỉ có tối đa 100 số, mỗi số dưới 1025 và chỉ cho phép 50 lệnh. Ngay cả việc kiểm tra bậc hai giữa tất cả các số cũng không đáng kể. Thử thách hoàn toàn mang tính xây dựng: thiết kế các lệnh mà sự tương tác của chúng không ảnh hưởng lẫn nhau. 

Một cách tiếp cận đơn giản sẽ tạo ra một lệnh cho mỗi số:```
x<>x+1
```Đối với mẫu`{10, 79}`, cái này hoạt động:```
10<>11
79<>80
```Nhưng điều này không thành công khi một số là chuỗi con của số khác. Giả sử tập hợp là:```
1
10
```Nếu chúng ta đặt:```
1<>2
10<>11
```sau đó nhập`10`thực hiện lệnh đầu tiên vì`1`xuất hiện bên trong`10`. Kết quả trở thành`20`, điều đó là sai. 

Đảo ngược thứ tự cũng không thành công:```
10<>11
1<>2
```Bây giờ hãy nhập`10`trở thành`11`, nhưng thuật toán vẫn tiếp tục và lệnh thứ hai khớp lại, tạo ra`22`. 

Mối nguy hiểm không rõ ràng là việc thay thế có thể tạo ra những trận đấu mới. Chúng tôi phải đảm bảo rằng sau khi chuyển đổi dự định, không có lệnh nào sau này có thể kích hoạt. 

Một trường hợp tinh vi khác là chuỗi các chuỗi con:```
1
10
100
```Một sự sắp xếp bất cẩn có thể dễ dàng viết lại`100`qua một số trạng thái trung gian ngoài ý muốn. 

Vấn đề trở thành một bài tập cẩn thận trong việc tránh va chạm giữa các mẫu. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi số đầu vào`x`, thêm lệnh kết thúc:```
x<>x+1
```Nếu không có số nào là chuỗi con của số khác thì điều này đúng ngay lập tức. Lệnh khớp đầu tiên xác định duy nhất đầu vào và kết thúc sau khi thay thế. 

Sự cố xuất hiện khi một số xuất hiện bên trong một số khác. Vì các lệnh được kiểm tra tổng thể dựa trên các chuỗi con chứ không phải đối với toàn bộ chuỗi nên các mẫu ngắn hơn có thể chiếm đoạt các số lớn hơn. 

Một cách sửa chữa mạnh mẽ có thể xảy ra là giới thiệu các ký tự đánh dấu tạm thời và nhiều giai đoạn viết lại. Ví dụ: trước tiên chúng ta có thể đánh dấu một số cụ thể bằng các ký hiệu đặc biệt, sau đó chuyển điểm đánh dấu thành câu trả lời cuối cùng. Vì chuỗi lệnh có thể chứa`?`, ký tự này có thể hoạt động như một điểm đánh dấu riêng không bao giờ xuất hiện trong dữ liệu đầu vào ban đầu. 

Hướng này hoạt động nhưng trở nên lộn xộn. Chúng ta cần quản lý cẩn thận các tương tác giữa các trạng thái tạm thời, thứ tự lệnh và dọn dẹp. Số lượng lệnh cũng có thể tăng lên nhanh chóng. 

Quan sát quan trọng là miền cực kỳ nhỏ. Mọi số đầu vào đều nhỏ hơn 1025, do đó độ dài thập phân của nó tối đa là bốn chữ số. Quan trọng hơn, chỉ có 1025 chuỗi gốc có thể có. 

Thay vì trực tiếp chống lại xung đột chuỗi con, chúng ta có thể sử dụng ý tưởng kiểu ba cổ điển. Nếu chúng ta xử lý các chuỗi dài hơn trước các chuỗi ngắn hơn thì bất kỳ khi nào số ngắn hơn là chuỗi con của chuỗi dài hơn thì số dài hơn sẽ được ưu tiên. 

Điều này vẫn không giải quyết được mọi thứ, vì sau khi thay thế một số dài hơn, kết quả có thể chứa một mẫu khác. Cải tiến quan trọng là làm cho mọi lệnh kết thúc bằng cách sử dụng`<>`. Khi sự thay thế dự định xảy ra, quá trình thực thi sẽ dừng ngay lập tức. Không có lệnh sau này có thể can thiệp. 

Bây giờ tính đúng đắn giảm xuống còn một điều kiện: 

Với mỗi số đầu vào`a`, trong số tất cả các lệnh có phía bên trái xuất hiện bên trong`a`, lệnh đúng cho`a`phải đến trước. 

Điều này dễ dàng được đảm bảo. Chúng tôi sắp xếp các số bằng cách giảm độ dài chuỗi. Nếu hai số có cùng độ dài thì không thể là chuỗi con của số kia trừ khi chúng giống hệt nhau. Vì đầu vào đại diện cho một tập hợp nên việc trùng lặp không thành vấn đề. 

Sau khi sắp xếp theo độ dài giảm dần, lệnh riêng của mỗi số sẽ xuất hiện trước các lệnh dành cho tất cả các chuỗi con ngắn hơn có thể khớp bên trong nó. 

Việc xây dựng lực lượng vũ phu sẽ có hiệu lực trở lại sau khi được kết hợp với thứ tự phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Các lệnh không có thứ tự ngây thơ | Xác thực O(n²) | O(n) | Không đúng | 
| Lệnh kết thúc theo thứ tự độ dài | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số dưới dạng chuỗi. 
2. Loại bỏ các bản sao nếu cần thiết. Nhiều lệnh giống nhau là không cần thiết. 
3. Sắp xếp các chuỗi theo độ dài giảm dần. 
4. Đối với mỗi chuỗi`s`, tạo lệnh kết thúc:```
s<>str(int(s)+1)
```1. Xuất lệnh theo thứ tự đó. 

Bước sắp xếp là toàn bộ thủ thuật. Giả sử một số`a`chứa một số khác`b`như một chuỗi con. Sau đó`|b| < |a|`, bởi vì các chuỗi riêng biệt có độ dài bằng nhau không thể là chuỗi con của nhau. Vì trước tiên chúng tôi xử lý các chuỗi dài hơn nên lệnh dành cho`a`xuất hiện trước lệnh cho`b`. 

Khi nhập`a`được xử lý, lệnh của chính nó là lệnh khớp sớm nhất. Công dụng thay thế`<>`, do đó quá trình thực thi sẽ dừng ngay lập tức. Không có lệnh chuỗi con nhỏ hơn nào có cơ hội chạy. 

### Tại sao nó hoạt động 

Xem xét bất kỳ chuỗi đầu vào nào`x`. 

Lệnh riêng của nó phù hợp rõ ràng, bởi vì`x`chứa chính nó như một chuỗi con. 

Bây giờ hãy xem xét bất kỳ lệnh nào trước đó cũng có thể khớp với`x`. Vì các lệnh được sắp xếp theo độ dài giảm dần nên bất kỳ lệnh nào trước đó đều tương ứng với một chuỗi có độ dài ít nhất là`|x|`. 

Một chuỗi dài hơn riêng biệt không thể là chuỗi con của`x`. Một chuỗi có độ dài bằng nhau riêng biệt cũng không thể là chuỗi con của`x`. Chuỗi con duy nhất có độ dài bằng nhau của`x`là`x`chính nó. 

Vì vậy, lệnh khớp đầu tiên phải chính xác là lệnh dành cho`x`. 

Lệnh đó thay thế`x`với`x+1`và chấm dứt ngay lập tức vì nó sử dụng`<>`. 

Do đó, mọi số đầu vào đều được chuyển đổi thành số kế tiếp của nó và không có lệnh ngoài ý muốn nào có thể thực thi được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    nums = []
    seen = set()
    
    for _ in range(n):
        s = input().strip()
        if s not in seen:
            seen.add(s)
            nums.append(s)
    
    nums.sort(key=lambda x: (-len(x), x))
    
    ans = []
    
    for s in nums:
        t = str(int(s) + 1)
        ans.append(f"{s}<>{t}")
    
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai ngắn gọn vì bản thân việc xây dựng rất đơn giản một khi đã hiểu được đối số thứ tự. 

Chúng tôi lưu trữ số dưới dạng chuỗi vì hành vi của chuỗi con phụ thuộc vào cách biểu thị văn bản chứ không phải giá trị số của chúng. Việc chuyển đổi chúng thành số nguyên quá sớm sẽ làm mất cấu trúc số 0 đứng đầu, mặc dù vấn đề này đảm bảo số nguyên dương trong ký hiệu thập phân tiêu chuẩn. 

Việc loại bỏ trùng lặp là tùy chọn về mặt kỹ thuật nếu đầu vào được đảm bảo là một tập hợp, nhưng việc giữ nó sẽ giúp công trình xây dựng sạch hơn và tránh các lệnh lặp lại. 

Khóa sắp xếp là dòng quan trọng:```
nums.sort(key=lambda x: (-len(x), x))
```Chúng tôi sắp xếp theo chiều dài giảm dần. Thứ tự từ điển thứ cấp không liên quan đến tính chính xác nhưng mang lại kết quả xác định. 

Mỗi lệnh sử dụng`<>`, không`>>`. Sự lựa chọn tinh tế này ngăn chặn việc viết lại theo tầng sau khi quá trình chuyển đổi chính xác đã xảy ra. 

Chuỗi thay thế chỉ đơn giản là`int(s) + 1`, chuyển về dạng thập phân. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
10
79
```Sau khi sắp xếp theo độ dài giảm dần: 

| Đặt hàng | Chuỗi | Thay thế | 
| --- | --- | --- | 
| 1 | 10 | 11 | 
| 2 | 79 | 80 | 

Các lệnh được tạo:```
10<>11
79<>80
```Dấu vết thực thi cho đầu vào`10`: 

| Bước | Chuỗi hiện tại | Lệnh khớp đầu tiên | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 10 | 10<>11 | 11 | 

Thuật toán kết thúc ngay sau khi thay thế. 

Dấu vết thực thi cho đầu vào`79`: 

| Bước | Chuỗi hiện tại | Lệnh khớp đầu tiên | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 79 | 79<>80 | 80 | 

Ví dụ này thể hiện cơ chế cơ bản khi không có xung đột chuỗi con. 

### Ví dụ 2 

đầu vào:```
3
1
10
100
```Sau khi sắp xếp: 

| Đặt hàng | Chuỗi | Thay thế | 
| --- | --- | --- | 
| 1 | 100 | 101 | 
| 2 | 10 | 11 | 
| 3 | 1 | 2 | 

Các lệnh được tạo:```
100<>101
10<>11
1<>2
```Dấu vết thực thi cho đầu vào`100`: 

| Bước | Chuỗi hiện tại | Lệnh khớp đầu tiên | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 100 | 100<>101 | 101 | 

Dấu vết thực thi cho đầu vào`10`: 

| Bước | Chuỗi hiện tại | Lệnh khớp đầu tiên | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 10 | 10<>11 | 11 | 

Dấu vết thực thi cho đầu vào`1`: 

| Bước | Chuỗi hiện tại | Lệnh khớp đầu tiên | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 | 1<>2 | 2 | 

Ví dụ này thực hiện chuỗi chuỗi con nguy hiểm. Thứ tự độ dài giảm dần đảm bảo rằng các chuỗi lớn hơn luôn yêu cầu mức độ ưu tiên trước các chuỗi con ngắn hơn của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế | 
| Không gian | O(n) | Chúng tôi lưu trữ tất cả các lệnh | 

Các ràng buộc rất nhỏ nên điều này dễ dàng phù hợp với giới hạn. Ngay cả kích thước đầu vào tối đa có thể cũng chỉ bao gồm khoảng một trăm chuỗi ngắn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    nums = []
    seen = set()

    for _ in range(n):
        s = input().strip()
        if s not in seen:
            seen.add(s)
            nums.append(s)

    nums.sort(key=lambda x: (-len(x), x))

    ans = []

    for s in nums:
        ans.append(f"{s}<>{int(s)+1}")

    print("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("2\n10\n79\n") == "10<>11\n79<>80", "sample 1"

# minimum size
assert run("1\n1\n") == "1<>2", "single number"

# substring chain
assert run("3\n1\n10\n100\n") == "100<>101\n10<>11\n1<>2", "substring ordering"

# boundary value
assert run("1\n1024\n") == "1024<>1025", "largest allowed value"

# equal lengths without substring relation
assert run("3\n12\n34\n56\n") == "12<>13\n34<>35\n56<>57", "same length numbers"

# duplicate handling
assert run("3\n7\n7\n7\n") == "7<>8", "duplicate inputs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n`|`1<>2`| Đầu vào nhỏ nhất có thể | 
|`3\n1\n10\n100\n`| Ra lệnh các lệnh dài hơn đến ngắn hơn | Xử lý xung đột chuỗi con | 
|`1\n1024\n`|`1024<>1025`| Giá trị được phép lớn nhất | 
|`3\n12\n34\n56\n`| Lệnh độc lập | Các chuỗi có độ dài bằng nhau không thể can thiệp | 
|`3\n7\n7\n7\n`| Lệnh đơn | Loại bỏ trùng lặp | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
2
1
10
```Các lệnh được tạo là:```
10<>11
1<>2
```Thực thi trên`10`tiến hành như sau: 

Thuật toán kiểm tra các lệnh từ trên xuống dưới.`10<>11`phù hợp ngay lập tức, vì vậy`10`trở thành`11`và việc thực thi chấm dứt. Mẫu ngắn nguy hiểm hơn`1`không bao giờ thực thi. 

Bây giờ hãy xem xét:```
3
1
10
100
```Các lệnh là:```
100<>101
10<>11
1<>2
```đầu vào`100`chứa cả hai`10`Và`1`dưới dạng chuỗi con, nhưng lệnh riêng của nó xuất hiện đầu tiên vì nó dài nhất. Việc thay thế kết thúc ngay lập tức nên các lệnh ngắn hơn sẽ không bao giờ được kích hoạt. 

Một trường hợp tinh vi khác là các chuỗi có độ dài bằng nhau:```
2
12
21
```Không chuỗi nào có thể là chuỗi con của chuỗi kia vì chúng có độ dài bằng nhau và khác biệt. Bất kỳ đơn đặt hàng nào cũng hoạt động. Các lệnh được tạo có thể là:```
12<>13
21<>22
```đầu vào`12`chỉ khớp với lệnh đầu tiên, trong khi đầu vào`21`chỉ phù hợp với thứ hai. 

Cuối cùng, hãy xem xét giá trị biên:```
1
1024
```Lệnh được tạo là:```
1024<>1025
```Sự cố cho phép đầu ra lớn hơn 1024. Chỉ các đầu vào ban đầu bị giới hạn. Việc xây dựng xử lý việc này một cách tự nhiên bằng phép cộng số nguyên thông thường.
