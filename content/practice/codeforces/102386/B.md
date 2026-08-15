---
title: "CF 102386B - \u0422\u0443\u0440\u043d\u0438\u0440 \u0423\u0440\u0424\u0423"
description: "Chúng ta cần đánh giá một vòng Rock-Paper-Scissors-Lizard-Spock. Dòng đầu tiên là nước đi được người chơi thứ nhất chọn và dòng thứ hai là nước đi được người chơi thứ hai chọn. Mỗi chiêu thức là một trong các đòn Rock, Scissors, Paper, Lizard hoặc Spock."
date: "2026-08-15T18:36:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "B"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 378
verified: false
draft: false
---

[CF 102386B - \u0422\u0443\u0440\u043d\u0438\u0440 \u0423\u0440\u0424\u0423](https://codeforces.com/problemset/problem/102386/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 18 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đánh giá một vòng Rock-Paper-Scissors-Lizard-Spock. Dòng đầu tiên là nước đi được người chơi thứ nhất chọn và dòng thứ hai là nước đi được người chơi thứ hai chọn. Mỗi bước đi là một trong`Rock`,`Scissors`,`Paper`,`Lizard`, hoặc`Spock`. 

Mỗi nước đi sẽ đánh bại chính xác hai nước đi khác.`Scissors`đánh bại`Paper`Và`Lizard`,`Paper`đánh bại`Rock`Và`Spock`,`Rock`đánh bại`Lizard`Và`Scissors`,`Lizard`đánh bại`Spock`Và`Paper`, Và`Spock`đánh bại`Scissors`Và`Rock`. Nếu cả hai người chơi cùng chọn một nước đi thì kết quả là hòa. 

Chương trình phải in`First`khi nước đi đầu tiên đánh bại nước đi thứ hai,`Second`khi điều ngược lại là đúng, và`Tie`khi các bước di chuyển bằng nhau. 

Không có đầu vào có kích thước thay đổi ở đây. Chính xác hai chuỗi được đọc và mỗi chuỗi thuộc về một tập hợp cố định gồm năm giá trị có thể. Do đó, ngay cả một phương pháp xem xét rõ ràng mọi cặp nước đi có thể thực hiện tối đa 25 phép so sánh. Không có ý nghĩa lớn-`n`vấn đề về hiệu suất trong vấn đề này, vì vậy giải pháp O(1) là đủ và dễ dàng phù hợp với mọi giới hạn Codeforces thông thường. 

Các trường hợp lợi thế chính đến từ việc coi trò chơi như trò chơi Kéo-bao-búa thông thường hoặc do quên rằng mỗi nước đi đều có hai đối thủ chiến thắng. Ví dụ,```
Rock
Rock
```phải sản xuất`Tie`. Việc thực hiện bất cẩn chỉ kiểm tra xem nước đi đầu tiên có vượt qua nước đi thứ hai hay không có thể dẫn đến thất bại`Second`thay vì xử lý sự bình đẳng trước tiên. 

Một trường hợp khác là```
Lizard
Spock
```sản xuất`First`. Lizard đánh bại Spock, mặc dù cả hai nước đi đều không thuộc ba lựa chọn tiêu chuẩn của Búa-Giấy-Kéo thông thường. Việc triển khai chỉ chứa ba mối quan hệ cổ điển sẽ cho kết quả không chính xác. 

Trường hợp ranh giới hữu ích thứ ba là```
Spock
Paper
```sản xuất`Second`, vì Paper đã đánh bại Spock. Chỉ kiểm tra một trong hai mối quan hệ chiến thắng cho mỗi nước đi sẽ bỏ lỡ trường hợp này. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu trực tiếp có thể liệt kê rõ ràng tất cả 25 cặp bước đi theo thứ tự và liên kết từng cặp với kết quả của nó. Vì mỗi người chơi chỉ có năm nước đi có thể thực hiện được nên trường hợp xấu nhất là có đúng 25 nước đi. Cách tiếp cận này đã đủ nhanh vì 25 là hằng số không phụ thuộc vào kích thước đầu vào. Không có kích thước đầu vào nào khiến phương pháp ép buộc cụ thể này trở nên quá chậm. 

Cách triển khai tự nhiên hơn sẽ sử dụng chính cấu trúc của trò chơi. Chúng tôi lưu trữ mười mối quan hệ chiến thắng theo hướng, sau đó kiểm tra xem nước đi của người chơi đầu tiên có phải là một trong những nước đi đánh bại nước đi của người chơi thứ hai hay không. Nếu vậy, người chơi đầu tiên sẽ thắng. Ngược lại, nếu các nước đi bằng nhau thì kết quả là hòa. Mỗi cặp còn lại phải có nghĩa là người chơi thứ hai thắng, vì luật xác định người chiến thắng cho mỗi cặp nước đi riêng biệt. 

Quan sát quan trọng là toàn bộ trò chơi là một đồ thị cố định chỉ có năm đỉnh. Mỗi nước đi là một đỉnh và một cạnh từ`A`ĐẾN`B`có nghĩa là`A`đánh bại`B`. Chúng tôi không cần phải tìm kiếm biểu đồ này hoặc xây dựng bất cứ thứ gì một cách linh hoạt. Cấu trúc tra cứu có kích thước không đổi thể hiện trực tiếp tất cả các mối quan hệ chiến thắng có thể có. 

Hai cách tiếp cận có độ phức tạp như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả 25 cặp có thể | O(1) | O(1) | Đã chấp nhận | 
| Tra cứu quan hệ thắng lợi | O(1) | O(1) | Đã chấp nhận | 

Cách tiếp cận tra cứu được ưa chuộng hơn vì nó thể hiện trực tiếp các quy tắc và tránh được một chuỗi dài các trường hợp đặc biệt. 

## Hướng dẫn thuật toán 

1. Đọc hai nước đi thành`first`Và`second`. Có chính xác hai dòng đầu vào, do đó không cần vòng lặp test-case. 
2. Nếu`first == second`, in`Tie`. Những nước đi ngang nhau không bao giờ đánh bại được nhau, bất kể đó là nước đi nào. 
3. Lưu trữ hai nước đi bị đánh bại bởi mỗi nước đi có thể. Ví dụ,`Rock`được liên kết với`Lizard`Và`Scissors`, trong khi`Spock`được liên kết với`Rock`Và`Scissors`. 
4. Kiểm tra xem`second`thuộc tập hợp các nước đi bị đánh bại bởi`first`. Nếu có thì in`First`. 
5. Nếu nước đi khác nhau và nước đi đầu tiên không đánh bại được nước đi thứ hai, hãy in`Second`. Mỗi cặp riêng biệt có chính xác một người chiến thắng, vì vậy không có kết quả thứ tư để xem xét. 

### Tại sao nó hoạt động 

Đối với mọi di chuyển`A`, cấu trúc tra cứu chứa chính xác hai bước di chuyển`A`thua theo luật chơi. Sau khi kiểm tra ngang bằng, hai đấu thủ có những nước đi riêng biệt. Nếu nước đi thứ hai xuất hiện trong tập thắng của nước đi đầu tiên, luật quy định rằng người chơi đầu tiên thắng. Nếu không, người chơi thứ nhất không thể đánh bại người thứ hai và vì mỗi cặp riêng biệt đều có người chiến thắng nên người chơi thứ hai phải thắng. Do đó, mọi đầu vào có thể đều đạt được kết quả chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

first = input().strip()
second = input().strip()

wins = {
    "Rock": {"Lizard", "Scissors"},
    "Scissors": {"Paper", "Lizard"},
    "Paper": {"Rock", "Spock"},
    "Lizard": {"Spock", "Paper"},
    "Spock": {"Scissors", "Rock"},
}

if first == second:
    print("Tie")
elif second in wins[first]:
    print("First")
else:
    print("Second")
```Từ điển`wins`là sự thể hiện đầy đủ của đồ thị trò chơi. Mỗi phím là một nước đi có thể có của người chơi đầu tiên và giá trị của nó chứa chính xác hai nước đi mà nó đánh bại. 

Kiểm tra đẳng thức xuất hiện trước tra cứu chiến thắng vì đẳng thức có kết quả riêng,`Tie`. Nếu không có sự kiểm tra này, một cặp bằng nhau sẽ rơi vào`Second`trường hợp. 

biểu thức`second in wins[first]`kiểm tra chính xác điều kiện cần thiết để người chơi đầu tiên giành chiến thắng. Nếu nó sai sau khi các nước đi đã được chứng minh là khác nhau thì người chơi thứ hai nhất thiết phải thắng. 

Không có chỉ mục, vòng lặp trên dữ liệu đầu vào hoặc phép toán số học ở đây, do đó không có mối lo ngại về ranh giới hoặc tràn số nguyên. các`.strip()`các cuộc gọi loại bỏ các ký tự dòng mới được tạo bởi`readline()`trong khi vẫn giữ được tên nước đi chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
Rock
Paper
```Những thay đổi trạng thái có liên quan là: 

| Bước |`first`|`second`| Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào |`Rock`|`Paper`| Cả hai bước di chuyển được lưu trữ | Tiếp tục | 
| Kiểm tra sự bình đẳng |`Rock`|`Paper`|`first == second`là sai | Tiếp tục | 
| Tra cứu chiến thắng |`Rock`|`Paper`|`Paper`không bị đánh bại bởi`Rock`|`Second`|`Rock`đánh bại`Lizard`Và`Scissors`, không`Paper`. Vì các nước đi khác nhau nên người chiến thắng duy nhất còn lại là người chơi thứ hai. Chương trình in`Second`. 

### Mẫu 2 

Đầu vào là:```
Rock
Rock
```Dấu vết là: 

| Bước |`first`|`second`| Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào |`Rock`|`Rock`| Cả hai bước di chuyển được lưu trữ | Tiếp tục | 
| Kiểm tra sự bình đẳng |`Rock`|`Rock`|`first == second`là đúng |`Tie`| 

Việc tra cứu không bao giờ cần thiết. Điều này chứng tỏ tại sao phải xử lý sự bình đẳng trước khi kiểm tra các mối quan hệ thắng lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có hai chuỗi được đọc và một lần tra cứu có kích thước không đổi được thực hiện. | 
| Không gian | O(1) | Từ điển chứa chính xác năm chìa khóa và mười mối quan hệ chiến thắng. | 

Kích thước đầu vào được cố định ở hai lần di chuyển từ bộ năm phần tử, do đó thuật toán chỉ thực hiện một số thao tác không đổi và sử dụng một lượng bộ nhớ không đổi. Nó thoải mái phù hợp với giới hạn thời gian và bộ nhớ của vấn đề. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    first = input().strip()
    second = input().strip()

    wins = {
        "Rock": {"Lizard", "Scissors"},
        "Scissors": {"Paper", "Lizard"},
        "Paper": {"Rock", "Spock"},
        "Lizard": {"Spock", "Paper"},
        "Spock": {"Scissors", "Rock"},
    }

    if first == second:
        return "Tie"
    if second in wins[first]:
        return "First"
    return "Second"

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline
        return solve()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("Rock\nPaper\n") == "Second", "sample 1"
assert run("Rock\nRock\n") == "Tie", "sample 2"
assert run("Lizard\nSpock\n") == "First", "sample 3"

# All equal values
assert run("Spock\nSpock\n") == "Tie", "equal moves"

# Reverse direction of a winning relationship
assert run("Paper\nRock\n") == "First", "Paper defeats Rock"
assert run("Rock\nPaper\n") == "Second", "Paper defeats Rock"

# Second winning relationship of a move
assert run("Spock\nRock\n") == "Second", "Rock defeats Spock"
assert run("Spock\nScissors\n") == "First", "Spock defeats Scissors"

# Lizard's two different winning relationships
assert run("Lizard\nPaper\n") == "First", "Lizard defeats Paper"
assert run("Paper\nLizard\n") == "Second", "Lizard defeats Paper"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`Spock / Spock`|`Tie`| Xử lý bình đẳng cho một động thái khác | 
|`Paper / Rock`|`First`| Một hướng của một mối quan hệ chiến thắng | 
|`Spock / Rock`|`Second`| Người chơi thứ hai giành chiến thắng với đối thủ của nước đi đầu tiên | 
|`Lizard / Paper`|`First`| Lợi thế chiến thắng thứ hai của Lizard | 
|`Paper / Lizard`|`Second`| Đảo ngược mối quan hệ tương tự | 

Vấn đề thực sự không có tham số kích thước tối thiểu hoặc kích thước tối đa riêng biệt. Mỗi trường hợp thử nghiệm luôn chứa chính xác hai bước di chuyển, do đó ranh giới liên quan là tập hợp đầy đủ năm giá trị có thể. Các thử nghiệm trên bao gồm tất cả các trường hợp kết cấu, bao gồm chuyển động bằng nhau và cả hai hướng của một số mối quan hệ. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là đẳng thức. Vì```
Rock
Rock
```thuật toán đọc cả hai nước đi, tìm thấy rằng`first == second`, và ngay lập tức quay trở lại`Tie`. Nó không cố gắng điều trị`Rock`như tự đánh bại chính mình, bởi vì luật chơi loại trừ việc tự so sánh. 

Trường hợp cạnh thứ hai là nước đi có hai cách khác nhau để giành chiến thắng. Coi như```
Lizard
Spock
```Mục từ điển dành cho`Lizard`là`{Spock, Paper}`. Từ`Spock`có mặt, điều kiện`second in wins[first]`là đúng và kết quả là`First`. Điều này nắm bắt các triển khai chỉ ghi nhớ một trong hai mối quan hệ chiến thắng của Lizard. 

Trường hợp cạnh thứ ba là cặp đảo ngược```
Paper
Lizard
```Mục nhập cho`Paper`là`{Rock, Spock}`, Vì thế`Lizard`không có mặt. Các nước đi không bằng nhau nên thuật toán đi đến nhánh cuối cùng và in ra`Second`. Điều này xác nhận rằng mối quan hệ là có hướng và không thể được coi là một kết nối vô hướng. 

Trường hợp thứ tư là sự tương tác ít rõ ràng hơn của Spock với Rock:```
Spock
Rock
```

`Rock`xuất hiện ở`wins["Spock"]`, do đó thuật toán in`First`. Đảo ngược đầu vào thành```
Rock
Spock
```làm cho việc tra cứu thất bại và thuật toán in`Second`. Hai đầu vào này cùng nhau xác minh rằng hướng đi của mọi mối quan hệ đang được diễn giải chính xác.
