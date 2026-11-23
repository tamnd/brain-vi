---
title: "CF 469A - Tôi Muốn Trở Thành Chàng Trai"
description: "Chúng ta có một trò chơi với các cấp độ được đánh số từ 1 đến n. Bé X có thể hoàn thành một số tập hợp con của các cấp độ này và Bé Y có thể hoàn thành một tập hợp con khác. Họ quyết định hợp tác, có nghĩa là một cấp độ được coi là đạt nếu ít nhất một trong số họ có thể hoàn thành nó."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 469
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 268 (Div. 2)"
rating: 800
weight: 469
solve_time_s: 98
verified: true
draft: false
---

[CF 469A - Tôi Muốn Trở Thành Chàng Trai](https://codeforces.com/problemset/problem/469/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một trò chơi với các cấp độ được đánh số từ 1 đến _n_. Bé X có thể hoàn thành một số tập hợp con của các cấp độ này và Bé Y có thể hoàn thành một tập hợp con khác. Họ quyết định hợp tác, có nghĩa là một cấp độ được coi là đạt nếu ít nhất một trong số họ có thể hoàn thành nó. 

Nhiệm vụ chỉ đơn giản là xác định xem khả năng kết hợp của họ có bao gồm mọi cấp độ của trò chơi hay không. Nếu mọi cấp độ từ 1 đến _n_ đều có thể được X hoặc Y hoàn thành, chúng tôi sẽ in`"I become the guy."`. Mặt khác, ít nhất một cấp độ vẫn không thể thực hiện được đối với cả hai người chơi và chúng tôi in`"Oh, my keyboard!"`. 

Những hạn chế là rất nhỏ. Số cấp độ nhiều nhất là 100, do đó, ngay cả những cách tiếp cận tương đối kém hiệu quả cũng sẽ hoạt động thoải mái trong thời gian giới hạn. Chúng tôi không cần cấu trúc dữ liệu phức tạp hoặc thuật toán nâng cao. Việc triển khai đơn giản để kiểm tra phạm vi bao phủ của tất cả các cấp là quá đủ. 

Một vài trường hợp đặc biệt rất dễ bị bỏ qua. 

Giả sử một người chơi không thể vượt qua cấp độ nào cả. 

đầu vào:```
1
0
1 1
```Đầu ra đúng là:```
I become the guy.
```Mặc dù X không đóng góp gì nhưng chỉ có Y mới đảm bảo được mức độ duy nhất. 

Một trường hợp khác là khi cả hai người chơi liệt kê một số cấp độ giống nhau. 

đầu vào:```
4
2 1 2
2 2 3
```Đầu ra đúng là:```
Oh, my keyboard!
```Cấp độ 1, 2 và 3 được đề cập nhưng thiếu cấp độ 4. Việc triển khai bất cẩn chỉ đếm số lượng số đã được đọc có thể khiến cho bốn mục nhập ngụ ý bốn cấp độ được bao phủ một cách không chính xác. 

Trường hợp lợi thế cuối cùng xảy ra khi cả hai người chơi đều không hoàn thành mọi cấp độ riêng lẻ mà cùng nhau thực hiện. 

đầu vào:```
4
2 1 2
2 3 4
```Đầu ra đúng là:```
I become the guy.
```Việc kiểm tra từng người chơi riêng biệt sẽ thất bại ở đây. Chúng ta phải xem xét sự kết hợp của các cấp độ có thể tiếp cận được của họ. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là kiểm tra mọi cấp độ từ 1 đến _n_. Đối với mỗi cấp độ, hãy quét danh sách X và danh sách Y để xem liệu một trong hai người chơi có thể hoàn thành nó hay không. Nếu một mức độ nào đó không có trong cả hai danh sách thì câu trả lời là phủ định. 

Cách tiếp cận này đúng vì nó xác minh rõ ràng điều kiện mà vấn đề yêu cầu: mọi cấp độ đều phải được đáp ứng. Với tối đa 100 cấp độ và tối đa 100 mục nhập trên mỗi danh sách, trường hợp xấu nhất thực hiện khoảng 100 × 200 = 20.000 so sánh, tốc độ này đã đủ nhanh. 

Chúng tôi có thể làm cho giải pháp trở nên rõ ràng hơn bằng cách quan sát rằng chúng tôi chỉ quan tâm liệu một cấp độ có xuất hiện ít nhất một lần trong danh sách của cả hai người chơi hay không. Sự xuất hiện trùng lặp không thành vấn đề. Điều này tự nhiên gợi ý sử dụng một bộ. 

Chúng tôi chèn mọi cấp độ mà X có thể vượt qua vào một tập hợp, sau đó chèn mọi cấp độ mà Y có thể vượt qua vào cùng một tập hợp. Bộ này tự động lưu trữ mỗi cấp độ chỉ một lần. Nếu kích thước của tập hợp trở thành _n_ thì mọi cấp độ đều được biểu thị. Nếu không, một số cấp độ bị thiếu. 

Phương pháp brute-force hoạt động vì nó trực tiếp kiểm tra mức độ bao phủ theo cấp độ. Quan sát quan trọng là mức độ bao phủ hoàn toàn giống với sự kết hợp giữa các bộ cấp độ của hai người chơi. Việc thể hiện sự kết hợp đó một cách rõ ràng cho phép chúng tôi trả lời câu hỏi bằng một so sánh kích thước duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(p + q)) | O(1) | Đã chấp nhận | 
| Tối ưu | O(p + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số cấp độ,`n`. 
2. Đọc dòng chữ của Bé X. Số đầu tiên là`p`, theo sau là`p`cấp độ X có thể hoàn thành. 
3. Đọc dòng chữ của Tiểu Y. Số đầu tiên là`q`, theo sau là`q`cấp độ Y có thể hoàn thành. 
4. Tạo một tập hợp trống. 
5. Chèn tất cả các cấp độ từ danh sách của X vào tập hợp. 

Bộ này chỉ giữ các cấp độ duy nhất, đó chính xác là những gì chúng ta cần. 
6. Chèn tất cả các cấp độ từ danh sách của Y vào cùng một bộ. 

Sau bước này, bộ này thể hiện sự kết hợp các cấp độ mà một trong hai người chơi có thể vượt qua. 
7. So sánh kích thước của bộ với`n`. 

Nếu tập hợp chứa chính xác`n`cấp độ khác nhau, mọi cấp độ từ 1 đến`n`được che phủ. 
8. In`"I become the guy."`nếu tất cả các cấp độ được bảo hiểm. Nếu không thì in`"Oh, my keyboard!"`. 

### Tại sao nó hoạt động 

Bộ này chứa chính xác các cấp độ mà ít nhất một người chơi có thể vượt qua. Mọi cấp độ được chèn bởi X hoặc Y đều xuất hiện trong tập hợp và không có cấp độ nào khác xuất hiện ở đó. 

Nếu kích thước đặt bằng`n`, sau đó tất cả các cấp độ từ 1 đến`n`có mặt trong liên minh, nghĩa là người chơi có thể cùng nhau hoàn thành toàn bộ trò chơi. 

Nếu kích thước cài đặt nhỏ hơn`n`, ít nhất một cấp độ bị thiếu trong liên minh. Không người chơi nào có thể hoàn thành cấp độ đó nên việc hoàn thành toàn bộ trò chơi là không thể. 

Vì thuật toán kiểm tra chính xác phép kết theo yêu cầu của câu lệnh bài toán nên nó luôn tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    x_data = list(map(int, input().split()))
    y_data = list(map(int, input().split()))

    levels = set()

    for level in x_data[1:]:
        levels.add(level)

    for level in y_data[1:]:
        levels.add(level)

    if len(levels) == n:
        print("I become the guy.")
    else:
        print("Oh, my keyboard!")

solve()
```Dòng đầu tiên ghi tổng số cấp độ. Hai dòng tiếp theo chứa thông tin của người chơi. Giá trị đầu tiên trên mỗi dòng là một số đếm và bản thân nó không phải là số cấp độ, vì vậy chúng tôi bỏ qua nó bằng cách cắt từ chỉ mục 1 trở đi. 

Một bộ được sử dụng vì chúng tôi chỉ quan tâm liệu một cấp độ có được bao phủ hay không chứ không phải nó xuất hiện bao nhiêu lần. Nếu cả hai người chơi đều có thể vượt qua cấp độ 2 thì bộ vẫn chỉ lưu trữ một bản sao của 2. 

Sự so sánh cuối cùng kiểm tra xem liệu tập hợp có chứa tất cả`n`cấp độ. Vì các cấp độ được đánh số từ 1 đến`n`, có`n`các cấp độ được bảo hiểm riêng biệt có nghĩa là mọi cấp độ đều được bảo hiểm. 

Một chi tiết tinh tế là tránh sử dụng các giá trị đếm`p`Và`q`như các cấp độ. Việc đưa chúng vào một cách vô tình sẽ tạo ra câu trả lời sai ở một số thông tin đầu vào. sử dụng`x_data[1:]`Và`y_data[1:]`tránh được sai lầm đó một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 1 2 3
2 2 4
```| Bước | Bộ hiện tại | 
| --- | --- | 
| Bắt đầu | {} | 
| Thêm 1 | {1} | 
| Thêm 2 | {1, 2} | 
| Thêm 3 | {1, 2, 3} | 
| Thêm 2 | {1, 2, 3} | 
| Thêm 4 | {1, 2, 3, 4} | 

Trạng thái cuối cùng: 

| n | Kích thước của bộ | 
| --- | --- | 
| 4 | 4 | 

Đầu ra:```
I become the guy.
```Dấu vết này cho thấy mức độ trùng lặp 2 được tập hợp tự động bỏ qua như thế nào. Sau khi kết hợp khả năng của cả hai người chơi, tất cả bốn cấp độ đều được hoàn thành. 

### Ví dụ 2 

đầu vào:```
4
2 1 2
1 3
```| Bước | Bộ hiện tại | 
| --- | --- | 
| Bắt đầu | {} | 
| Thêm 1 | {1} | 
| Thêm 2 | {1, 2} | 
| Thêm 3 | {1, 2, 3} | 

Trạng thái cuối cùng: 

| n | Kích thước của bộ | 
| --- | --- | 
| 4 | 3 | 

Đầu ra:```
Oh, my keyboard!
```Cấp 4 không bao giờ xuất hiện trong danh sách của cả hai người chơi. Bộ này chỉ chứa ba cấp độ riêng biệt nên việc hoàn thành trò chơi là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(p + q) | Mỗi cấp độ được liệt kê sẽ được chèn vào tập hợp một lần | 
| Không gian | O(n) | Bộ này có thể lưu trữ tất cả các cấp độ trò chơi | 

Từ`n ≤ 100`, thuật toán chạy về cơ bản ngay lập tức. Cả thời gian và mức sử dụng bộ nhớ đều thấp hơn nhiều so với giới hạn của vấn đề. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())

    x_data = list(map(int, input().split()))
    y_data = list(map(int, input().split()))

    levels = set(x_data[1:]) | set(y_data[1:])

    if len(levels) == n:
        return "I become the guy."
    return "Oh, my keyboard!"

# provided sample
assert run("4\n3 1 2 3\n2 2 4\n") == "I become the guy.", "sample 1"

# minimum size
assert run("1\n0\n1 1\n") == "I become the guy.", "minimum n"

# missing one level
assert run("4\n2 1 2\n1 3\n") == "Oh, my keyboard!", "level 4 missing"

# one player covers everything
assert run("5\n5 1 2 3 4 5\n0\n") == "I become the guy.", "single player sufficient"

# maximum-style coverage
assert run(
    "100\n50 " + " ".join(map(str, range(1, 51))) +
    "\n50 " + " ".join(map(str, range(51, 101))) + "\n"
) == "I become the guy.", "full coverage across both players"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`, chỉ có Y mới biết cấp 1 |`I become the guy.`| Đầu vào kích thước tối thiểu | 
| Cấp 1,2,3 đã xong, cấp 4 còn thiếu |`Oh, my keyboard!`| Thiếu phát hiện mức độ | 
| X bao gồm tất cả các cấp độ, Y không bao gồm |`I become the guy.`| Một người chơi thôi là đủ | 
| Chia vùng phủ sóng trên 100 cấp độ |`I become the guy.`| Trường hợp ranh giới lớn | 

## Vỏ cạnh 

Hãy xem xét trường hợp một người chơi không đóng góp gì. 

đầu vào:```
1
0
1 1
```Bộ này bắt đầu trống. X không thêm cấp độ nào. Y thêm cấp 1, tạo ra`{1}`. Kích thước đã đặt trở thành 1, bằng`n`, do đó thuật toán in ra:```
I become the guy.
```Bây giờ hãy xem xét các mức độ chồng chéo. 

đầu vào:```
4
2 1 2
2 2 3
```Sau khi xử lý X, tập hợp này là`{1, 2}`. Quá trình xử lý Y thêm lại cấp 2 và cấp 3, tạo ra`{1, 2, 3}`. Mức độ trùng lặp không làm tăng kích thước đã đặt. Vì kích thước cuối cùng là 3 thay vì 4 nên thuật toán sẽ in chính xác:```
Oh, my keyboard!
```Cuối cùng, hãy xem xét một trường hợp cần hợp tác. 

đầu vào:```
4
2 1 2
2 3 4
```X đóng góp`{1, 2}`và Y đóng góp`{3, 4}`. Công đoàn của họ trở thành`{1, 2, 3, 4}`. Kích thước đặt bằng`n`, do đó đầu ra là:```
I become the guy.
```Điều này chứng tỏ tại sao chúng ta phải đánh giá phạm vi bao phủ kết hợp thay vì kiểm tra từng người chơi riêng lẻ.
