---
title: "CF 106102L - Nhắn tin"
description: "Chúng tôi nhận được một số tin nhắn văn bản. Một tin nhắn được coi là một \"trận chiến\" nếu nó chứa ít nhất ba chữ cái liên tiếp a, không phân biệt chữ hoa chữ thường. Điều đó có nghĩa là các chuỗi như aaa, AaA, aAA và AAA đều đủ điều kiện."
date: "2026-06-25T11:50:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106102
codeforces_index: "L"
codeforces_contest_name: "AGM 2025, Final Round, Day 1"
rating: 0
weight: 106102
solve_time_s: 36
verified: true
draft: false
---

[CF 106102L - Nhắn tin](https://codeforces.com/problemset/problem/106102/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một số tin nhắn văn bản. Một tin nhắn được coi là một "trận chiến" nếu nó chứa ít nhất ba chữ cái liên tiếp`a`, bỏ qua trường hợp. Điều đó có nghĩa là các trình tự như`aaa`,`AaA`,`aAA`, Và`AAA`tất cả đều đủ điều kiện. 

Đối với mỗi tin nhắn, chúng ta phải xác định xem liệu việc chạy như vậy có tồn tại ở bất kỳ đâu trong văn bản hay không. 

Nếu một lượt chạy đủ điều kiện xuất hiện, chúng tôi sẽ in:```
Love is war!
```Nếu không, chúng tôi in:```
Just friends <3
```Tin nhắn có thể chứa dấu cách và các ký tự khác, vì vậy toàn bộ mỗi dòng phải được đọc dưới dạng một chuỗi hoàn chỉnh. 

Những hạn chế là nhỏ. Mỗi tin nhắn dài tối đa 100 ký tự, do đó, ngay cả việc quét đơn giản từng ký tự cũng đủ nhanh. Nếu có`n`tin nhắn, tổng công việc tỷ lệ thuận với tổng số ký tự trên tất cả các tin nhắn. 

Chi tiết triển khai chính gây ra lỗi là việc so khớp không phân biệt chữ hoa chữ thường. Một trình tự như:```
AAa
```phải được xử lý giống hệt như:```
aaa
```Một lỗi dễ mắc phải khác là đặt lại số đếm liên tiếp không chính xác khi gặp lỗi không`a`tính cách. 

Hãy xem xét thông điệp:```
aabaa
```Trận đấu liên tiếp dài nhất`a`có độ dài 2, vì vậy đầu ra đúng là:```
Just friends <3
```Một giải pháp chỉ đếm tổng số`a`các nhân vật sẽ phân loại không chính xác đây là một trận chiến. 

Như một ví dụ khác:```
xAAAy
```chứa ba liên tiếp`a`các chữ cái sau khi chuyển sang chữ thường, vì vậy kết quả đúng là:```
Love is war!
```Việc triển khai phân biệt chữ hoa chữ thường sẽ bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là kiểm tra mọi chuỗi con có thể có độ dài ít nhất là ba và kiểm tra xem tất cả các ký tự trong chuỗi con đó có phải là chữ cái hay không`a`dưới sự so sánh không phân biệt chữ hoa chữ thường. 

Đối với một tin nhắn dài`m`, có các chuỗi con O(m²) và việc kiểm tra từng chuỗi con có thể mất thêm thời gian. Mặc dù các giới hạn ở đây rất nhỏ và điều này vẫn có thể vượt qua nhưng nó thực hiện nhiều công việc hơn mức cần thiết. 

Cấu trúc của vấn đề mang lại một quan sát đơn giản hơn. Chúng tôi thực sự không quan tâm cuộc chạy bắt đầu hay kết thúc ở đâu. Chúng ta chỉ cần biết liệu có bất kỳ lần chạy liên tiếp nào`a`đạt chiều dài 3. 

Điều đó gợi ý rằng hãy quét thư một lần trong khi vẫn duy trì độ dài hiện tại của quá trình chạy liên tục.`a`nhân vật. Khi chúng ta nhìn thấy một`a`(sau khi chuyển ký tự sang chữ thường), chúng ta tăng độ dài chạy. Bất kỳ ký tự nào khác sẽ dừng quá trình chạy và đặt lại bộ đếm về 0. 

Khi bộ đếm đếm đến số 3, chúng ta đã biết câu trả lời cho tin nhắn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(1) | Không cần thiết | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

Đây`m`biểu thị độ dài của một tin nhắn. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Lặp lại`n`lần, mỗi lần đọc một dòng tin nhắn hoàn chỉnh. 
3. Khởi tạo bộ đếm`run = 0`. 
4. Quét từng ký tự tin nhắn. 
5. Chuyển ký tự hiện tại thành chữ thường. 
6. Nếu nhân vật là`'a'`, tăng`run`bằng 1. Nếu không thì đặt`run = 0`. 
7. Nếu`run`trở thành ít nhất là 3, đánh dấu tin nhắn là một trận chiến và ngừng quét tin nhắn đó. 
8. Đầu ra`"Love is war!"`nếu một trận chiến được tìm thấy. Nếu không thì xuất ra`"Just friends <3"`. 

### Tại sao nó hoạt động 

Biến`run`luôn lưu trữ độ dài của hậu tố hiện tại bao gồm toàn bộ các chuỗi liên tiếp`a`ký tự, bỏ qua trường hợp. 

Bất cứ khi nào không`a`ký tự xuất hiện, bất kỳ chuỗi liên tiếp nào kết thúc ở vị trí trước đó đều bị hỏng, do đó việc đặt lại`run`về 0 là đúng. 

Một thông báo chứa một trận chiến hợp lệ chính xác khi một số lượt chạy liên tiếp`a`đạt chiều dài ít nhất là ba. Thuật toán phát hiện sự kiện đó lần đầu tiên`run >= 3`và nếu điều đó không bao giờ xảy ra thì không có lượt chạy đủ điều kiện nào tồn tại ở bất kỳ đâu trong tin nhắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

for _ in range(n):
    s = input().rstrip("\n")

    run = 0
    battle = False

    for ch in s:
        if ch.lower() == 'a':
            run += 1
            if run >= 3:
                battle = True
                break
        else:
            run = 0

    if battle:
        print("Love is war!")
    else:
        print("Just friends <3")
```Dòng đầu tiên đọc số lượng tin nhắn. Mỗi dòng tiếp theo được xử lý độc lập. 

quầy`run`theo dõi trình tự liên tiếp hiện tại của`a`nhân vật. sử dụng`ch.lower()`đảm bảo rằng các phiên bản chữ hoa và chữ thường được xử lý giống hệt nhau. 

Sớm`break`không bắt buộc phải có tính chính xác nhưng nó tránh được việc phải làm thêm khi đã biết câu trả lời. 

sử dụng`rstrip("\n")`chỉ xóa dòng mới ở cuối do đầu vào tạo ra, đồng thời giữ lại mọi khoảng trắng là một phần của chính thông báo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Tin nhắn đầu vào:```
THIS MEANS WAAAAAARRRRRRRRRR
```| Nhân vật | Chữ thường | chạy | 
| --- | --- | --- | 
| W | w | 0 | 
| A | một | 1 | 
| A | một | 2 | 
| A | một | 3 | 

Ở lần thứ ba liên tiếp`a`, điều kiện được thỏa mãn và câu trả lời trở thành:```
Love is war!
```Ví dụ này cho thấy chữ hoa phải được xử lý giống như chữ thường. 

### Ví dụ 2 

Tin nhắn đầu vào:```
what is a aardvark
```| Nhân vật | Chữ thường | chạy | 
| --- | --- | --- | 
| một | một | 1 | 
| không gian | không gian | 0 | 
| một | một | 1 | 
| một | một | 2 | 
| r | r | 0 | 

Lượt chạy liên tiếp dài nhất có độ dài 2 nên không có trận chiến nào tồn tại. 

Đầu ra:```
Just friends <3
```Ví dụ này chứng tỏ tại sao việc đếm tổng số lần xuất hiện của`a`là không đủ. Chỉ những lần xuất hiện liên tiếp mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi ký tự được kiểm tra một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Vì mỗi tin nhắn được quét chính xác một lần và các tin nhắn đều ngắn nên giải pháp dễ dàng phù hợp với mọi giới hạn cuộc thi hợp lý. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    out = []

    for _ in range(n):
        s = input().rstrip("\n")

        run = 0
        battle = False

        for ch in s:
            if ch.lower() == 'a':
                run += 1
                if run >= 3:
                    battle = True
                    break
            else:
                run = 0

        out.append("Love is war!" if battle else "Just friends <3")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# sample-style cases
assert run(
"""5
How are you doing Kaguya?
THIS MEANS WAAAAAARRRRRRRRRR
oops sorry wrong person
LlLmmAAaOoo that's soooo funnnyyy
what is a aardvark
"""
) == (
"""Just friends <3
Love is war!
Just friends <3
Love is war!
Just friends <3"""
)

# minimum size
assert run("1\na\n") == "Just friends <3"

# exactly three consecutive letters
assert run("1\naaa\n") == "Love is war!"

# mixed case
assert run("1\nAaA\n") == "Love is war!"

# separated runs
assert run("1\naa baa aa\n") == "Just friends <3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`Just friends <3`| Tin nhắn kích thước tối thiểu | 
|`aaa`|`Love is war!`| Ngưỡng chính xác là 3 | 
|`AaA`|`Love is war!`| So khớp không phân biệt chữ hoa chữ thường | 
|`aa baa aa`|`Just friends <3`| Chạy cách nhau bởi không`a`nhân vật | 

## Vỏ cạnh 

Hãy xem xét thông điệp:```
AaA
```Thuật toán chuyển đổi từng ký tự thành chữ thường trong khi quét. 

Các giá trị của`run`trở nên:```
1 -> 2 -> 3
```Càng sớm càng`run`đạt tới 3, thuật toán xuất ra:```
Love is war!
```Điều này xử lý chính xác cách viết hoa hỗn hợp. 

Bây giờ hãy xem xét:```
aabaa
```Quá trình quét tiến hành như sau:```
a -> run = 1
a -> run = 2
b -> run = 0
a -> run = 1
a -> run = 2
```Bộ đếm không bao giờ đạt tới 3, vì vậy kết quả là:```
Just friends <3
```Điều này xác nhận rằng chỉ những lần xuất hiện liên tiếp mới quan trọng. 

Cuối cùng, hãy xem xét:```
aaaa
```Bộ đếm phát triển như sau:```
1 -> 2 -> 3
```Thuật toán ngay lập tức nhận ra trận chiến và ngừng quét. Các lần chạy dài hơn sẽ được xử lý tự động vì mọi lần chạy có độ dài 4, 5 hoặc nhiều hơn đều nhất thiết phải đi qua độ dài 3 trước tiên.
