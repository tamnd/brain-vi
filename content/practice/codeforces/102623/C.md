---
title: "CF 102623C - Bảng cheat"
description: "Bảng cheat có dung lượng cố định được đo bằng ký tự. Setsuna có một bộ sưu tập các từ khóa, nhưng các từ khóa lặp lại chỉ quan trọng một lần vì trang cuối cùng không thể chứa các từ khóa trùng lặp."
date: "2026-08-04T17:11:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "C"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 73
verified: true
draft: false
---

[CF 102623C - Bảng ghi chú](https://codeforces.com/problemset/problem/102623/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bảng cheat có dung lượng cố định được đo bằng ký tự. Setsuna có một bộ sưu tập các từ khóa, nhưng các từ khóa lặp lại chỉ quan trọng một lần vì trang cuối cùng không thể chứa các từ khóa trùng lặp. Nếu một số từ khóa được đặt trên trang tính, chúng cần một khoảng cách duy nhất giữa các từ lân cận, do đó tổng độ dài chiếm dụng là tổng độ dài từ khóa đã chọn cộng với một dấu phân cách cho mỗi kết nối giữa hai từ khóa đã chọn. 

Nhiệm vụ là chọn số lượng từ khóa khác nhau lớn nhất có thể có tổng độ dài viết không vượt quá khoảng trống có sẵn. 

Số lượng từ khóa tối đa là 1000 và độ dài mỗi từ khóa tối đa là 100. Điều này có nghĩa là đầu vào đủ nhỏ để sắp xếp và quét tuyến tính, nhưng nó không phù hợp để thử mọi tập hợp con. Với 1000 từ khóa khác nhau, số lượng lựa chọn có thể là 2^1000, vượt xa những gì có thể khám phá trong thời gian giới hạn. 

Có một số chi tiết có thể gây ra câu trả lời sai. Những từ trùng lặp không được tính hai lần. Ví dụ:```
n = 10, m = 3
hello hello hi
```Câu trả lời đúng là 2 vì những từ có sẵn chỉ`hello`Và`hi`. Việc triển khai bất cẩn xử lý từng từ đầu vào một cách riêng biệt có thể đếm được ba từ khóa có thể có. 

Khoảng cách giữa các từ cũng ảnh hưởng đến câu trả lời. Ví dụ:```
n = 7, m = 2
abc defg
```Đáp án đúng là 1. Hai từ ghép lại cần 3 + 1 + 4 = 8 ký tự nên không thể vừa nhau. Việc triển khai chỉ so sánh tổng độ dài từ với`n`sẽ trả lại không chính xác 2. 

Thứ tự của chữ hoa và chữ thường rất quan trọng. Ví dụ:```
n = 5, m = 2
A a
```Câu trả lời đúng là 2 vì đây là những từ khóa khác nhau và chúng khớp chính xác với một khoảng trắng: 1 + 1 + 1 = 3 ký tự. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ xem xét mọi nhóm từ khóa có thể có, loại bỏ các từ khóa trùng lặp trong nhóm, tính toán tổng độ dài của nó và giữ lại nhóm lớn nhất phù hợp. Cách tiếp cận này đúng vì nó kiểm tra mọi câu trả lời có thể. Tuy nhiên, số lượng tập hợp con tăng theo cấp số nhân. Ngay cả khi chỉ có 60 từ khóa, vẫn có thể có hơn 10^18 nhóm nhỏ, khiến chiến lược này không thể sử dụng được. 

Quan sát chính là mọi từ khóa đều có cấu trúc chi phí giống nhau ngoại trừ độ dài của nó. Nếu chúng ta quyết định đặt`k`từ khóa trên bảng cheat, khoảng trắng đóng góp chính xác`k - 1`nhân vật. Mục tiêu không phải là tối đa hóa tổng độ dài được sử dụng mà là tối đa hóa số lượng từ khóa được chọn. Vì mỗi từ khóa được chọn đều đóng góp chính xác một từ khóa cho câu trả lời nên lựa chọn tốt nhất luôn là dành khoảng trống có sẵn cho những từ khóa ngắn nhất. 

Sau khi loại bỏ các từ khóa trùng lặp, hãy sắp xếp các từ khóa còn lại theo độ dài. Lấy từ khóa từ ngắn nhất đến dài nhất sẽ cho số lượng tối đa có thể. Nếu một từ khóa dài hơn có thể phù hợp tại một thời điểm nào đó, thì việc thay thế bất kỳ từ khóa ngắn hơn nào đã chọn bằng từ khóa đó sẽ không làm tăng số lượng từ đã chọn và chỉ có thể tiêu tốn nhiều không gian hơn. Thứ tự tham lam này nắm bắt mọi lựa chọn hữu ích mà không cần phải thử kết hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * m) | O(m) | Quá chậm | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ khóa và chèn chúng vào một bộ. Bộ này loại bỏ các từ khóa lặp lại vì việc viết cùng một từ khóa nhiều lần không bao giờ được phép và không bao giờ làm tăng câu trả lời. 
2. Chuyển đổi các từ khóa riêng biệt thành một danh sách và sắp xếp nó theo độ dài theo thứ tự tăng dần. Từ khóa ngắn hơn được ưu tiên vì mỗi từ khóa được chọn có giá trị như nhau nhưng những từ khóa ngắn hơn sẽ tiêu tốn ít không gian hơn. 
3. Giữ một biến đại diện cho số lượng ký tự hiện đang được sử dụng và một biến khác cho số lượng từ khóa đã chọn. Ban đầu cả hai giá trị đều bằng 0. 
4. Lặp lại các từ khóa đã được sắp xếp. Đối với từ khóa hiện tại, hãy tính toán khoảng trống bổ sung cần thiết. Nếu chưa có từ khóa nào được chọn thì chi phí chỉ là độ dài của nó. Nếu không, chi phí sẽ là chiều dài của nó cộng thêm một vì cần có một không gian ngăn cách. 
5. Thêm từ khóa nếu tổng số mới vẫn nằm trong giới hạn bảng ghi chú. Tăng số lượng đã chọn và tiếp tục. Nếu từ khóa không phù hợp, hãy ngừng kiểm tra các từ khóa tiếp theo vì tất cả các từ khóa còn lại ít nhất cũng dài như vậy. 

Tại sao nó hoạt động: 

Sau khi sắp xếp, thuật toán luôn xem xét các từ khóa từ rẻ nhất đến đắt nhất. Giả sử một câu trả lời tối ưu chứa một từ khóa dài hơn trong khi tồn tại một từ khóa ngắn hơn chưa được chọn. Việc thay thế từ khóa dài hơn bằng từ khóa ngắn hơn sẽ giữ nguyên số lượng từ khóa được chọn và không làm tăng không gian sử dụng. Việc lặp lại việc trao đổi này có nghĩa là luôn có một giải pháp tối ưu chứa các từ khóa có sẵn ngắn nhất. Thuật toán xây dựng chính xác tập hợp này nên số mà nó trả về là lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    words = input().split()

    words = list(set(words))
    words.sort(key=len)

    used = 0
    answer = 0

    for word in words:
        add = len(word)
        if answer > 0:
            add += 1

        if used + add <= n:
            used += add
            answer += 1
        else:
            break

    print(answer)

if __name__ == "__main__":
    solve()
```Việc chuyển đổi tập hợp xử lý các từ khóa trùng lặp trước khi bắt đầu bất kỳ phép tính nào. Điều này là cần thiết vì các bản sao không thể xuất hiện cùng nhau trên bảng ghi chú. 

Sắp xếp theo`len`đặt các từ theo thứ tự chính xác cần thiết cho sự lựa chọn tham lam. Trong quá trình quét,`used`lưu trữ kích thước hiện tại đầy đủ của bảng cheat, bao gồm tất cả các khoảng trắng đã được đặt giữa các từ. 

Biến`answer`cũng cho chúng ta biết liệu có cần dấu phân cách trước từ tiếp theo hay không. Từ được chọn đầu tiên không có khoảng trắng trước nó, trong khi mỗi từ được chọn sau đó sẽ thêm một ký tự phụ. 

Khi một từ không khớp, vòng lặp sẽ kết thúc ngay lập tức. Tất cả các từ sau này đều có độ dài bằng hoặc lớn hơn nên chúng cũng không thể khớp được. Điều này tránh việc kiểm tra không cần thiết và cũng tránh được những sai sót liên quan đến việc đếm dấu phân cách. 

Số nguyên Python không gặp vấn đề tràn ở đây vì tổng kích thước tối đa có thể chỉ khoảng 101000 ký tự. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, sau khi loại bỏ các từ khóa trùng lặp, các từ khóa là:```
myworld lusto KR12138 oneman233 SetsunaQAQ
```Thứ tự sắp xếp theo độ dài là`lusto`,`myworld`,`KR12138`,`oneman233`,`SetsunaQAQ`. 

| Từ khóa hiện tại | Đã thêm độ dài | Ký tự được sử dụng | Số đã chọn | 
| --- | --- | --- | --- | 
| ham muốn | 5 | 5 | 1 | 
| thế giới của tôi | 8 | 13 | 2 | 
| KR12138 | 8 | 21 | 3 | 
| oneman233 | 10 | 31 | 4 | 
| SetsunaQAQ | 11 | 42 | 4 | 

Từ cuối cùng không phù hợp vì tổng số sẽ vượt quá 40. Dấu vết cho thấy tại sao việc sắp xếp theo độ dài là đủ: bốn từ ngắn nhất đầu tiên chính xác là bốn lựa chọn tốt nhất. 

Đối với mẫu thứ hai:```
n = 7
^_^ ^_^
```Sau khi loại bỏ các từ khóa trùng lặp, chỉ còn lại một từ khóa. 

| Từ khóa hiện tại | Đã thêm độ dài | Ký tự được sử dụng | Số đã chọn | 
| --- | --- | --- | --- | 
| ^_^ | 3 | 3 | 1 | 

Kết quả là 1 vì từ khóa trùng lặp không tạo thêm lựa chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc xóa các bản sao cần có thời gian dự kiến ​​tuyến tính và việc sắp xếp các từ khóa riêng biệt sẽ chiếm ưu thế trong độ phức tạp | 
| Không gian | O(m) | Bộ và danh sách lưu trữ các từ khóa riêng biệt | 

Số lượng từ khóa đầu vào tối đa chỉ là 1000 nên việc sắp xếp dễ dàng trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ vì mỗi từ khóa có độ dài giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, m = map(int, sys.stdin.readline().split())
    words = sys.stdin.readline().split()

    words = list(set(words))
    words.sort(key=len)

    used = 0
    answer = 0

    for word in words:
        add = len(word) + (1 if answer > 0 else 0)
        if used + add <= n:
            used += add
            answer += 1
        else:
            break

    sys.stdin = old_stdin
    return str(answer)

assert solve("40 5\nmyworld lusto KR12138 oneman233 SetsunaQAQ\n") == "4", "sample 1"
assert solve("7 2\n^_^ ^_^\n") == "1", "sample 2"

assert solve("1 1\na\n") == "1", "minimum size"
assert solve("10 3\nhello hello hi\n") == "2", "duplicates"
assert solve("7 2\nabc defg\n") == "1", "separator boundary"
assert solve("1000 1000\n" + " ".join(["x"] * 1000) + "\n") == "1", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`40 5`đầu vào mẫu | 4 | Lựa chọn tham lam thông thường với nhiều độ dài khác nhau | 
|`7 2`đầu vào trùng lặp | 1 | Loại bỏ trùng lặp | 
|`1 1`với`a`| 1 | Công suất nhỏ nhất có thể | 
|`10 3`với`hello hello hi`| 2 | Xử lý từ khóa lặp lại | 
|`7 2`với`abc defg`| 1 | Kế toán tách đúng | 
| 1000 từ một ký tự giống hệt nhau | 1 | Các giá trị bằng nhau và hành vi được thiết lập | 

## Vỏ cạnh 

Từ khóa lặp lại sẽ được giảm xuống còn một lựa chọn duy nhất trước khi sắp xếp. Vì:```
10 3
hello hello hi
```bộ chỉ chứa`hello`Và`hi`. Thuật toán sắp xếp chúng như`hi`,`hello`, mất`hi`, và sau đó lấy`hello`vì tổng cộng là 8 ký tự bao gồm cả dấu phân cách. Đầu ra là 2. 

Dấu phân cách có thể là lý do khiến một từ không khớp. Vì:```
7 2
abc defg
```thuật toán đầu tiên chấp nhận`abc`, sử dụng 3 ký tự. Từ khóa tiếp theo cần 1 dấu cách cộng với 4 ký tự, vì vậy tổng số mới sẽ là 8. Từ khóa này bị từ chối, tạo ra 1. 

Từ khóa chữ hoa và chữ thường vẫn tách biệt vì tập hợp so sánh chính xác các chuỗi. Vì:```
5 2
A a
```danh sách được sắp xếp chứa cả hai từ. Câu đầu tiên sử dụng 1 ký tự và câu thứ hai thêm 1 dấu phân cách cộng với 1 ký tự, do đó câu trả lời trở thành 2. 

Từ khóa đầu tiên có điều kiện biên đặc biệt vì nó không cần khoảng trắng trước đó. Việc triển khai xử lý việc này bằng cách chỉ thêm dấu phân cách khi ít nhất một từ đã được chọn. 

Bạn có thể điều chỉnh độ dài bài xã luận hoặc làm cho phần giải thích mang phong cách cuộc thi hơn nếu cần.
