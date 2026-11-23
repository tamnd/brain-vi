---
title: "CF 55C - Ăn hay chết"
description: "Chúng tôi có một mạng lưới. Một số ô chứa bánh nướng và một số bánh nướng có thể dùng chung một ô."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 55
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 51"
rating: 1900
weight: 55
solve_time_s: 103
verified: true
draft: false
---
[CF 55C - Ăn hay chết](https://codeforces.com/problemset/problem/55/C) 

**Xếp hạng:** 1900 
**Thẻ:** trò chơi 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một$n \times m$lưới. Một số ô chứa bánh nướng và một số bánh nướng có thể dùng chung một ô. 

Mỗi lượt có hai giai đoạn. Đầu tiên, Volodya chọn một chiếc bánh và di chuyển nó từng ô theo một trong bốn hướng. Nếu chiếc bánh đã ở trên ranh giới, anh ta có thể di chuyển nó ra ngoài bàn cờ qua một cạnh của đường viền và ngay lập tức giành chiến thắng trong toàn bộ trò chơi. Sau đó, nếu trò chơi chưa kết thúc, Vlad chặn đúng một cạnh viền đơn vị của bàn cờ. Sau khi bị chặn, cạnh đó không bao giờ có thể được sử dụng nữa để di chuyển chiếc bánh ra ngoài. 

Trò chơi xoay quanh việc liệu Volodya cuối cùng có thể giành chiến thắng hay không nếu cả hai người chơi đều chơi hoàn hảo. 

Kích thước bảng tối đa là 100 và có tối đa 100 bánh nướng. Những giới hạn này rất nhỏ, đó là tín hiệu cho thấy khó khăn thực sự không phải là độ phức tạp khi triển khai mà là việc tìm ra quan sát chính xác về lý thuyết trò chơi. Việc tìm kiếm mạnh mẽ trên các trạng thái trò chơi vẫn không thể thực hiện được vì trạng thái bao gồm các vị trí của tất cả các bánh cùng với tập hợp các cạnh biên giới đã bị chặn. Thậm chí một$100 \times 100$bảng có 400 cạnh viền, do đó, chỉ riêng số lượng tập hợp con cạnh bị chặn có thể là rất lớn. 

Phần nguy hiểm của vấn đề này là nó trông giống như việc tìm đường, nhưng trò chơi thực tế chỉ phụ thuộc vào việc tiếp cận biên giới. Một ý tưởng ngây thơ như "nếu chiếc bánh nào đó có thể chạm tới ranh giới thì Volodya sẽ thắng" là hoàn toàn sai lầm vì mọi lối ra biên giới cuối cùng đều có thể bị chặn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một bảng không có bánh nướng:```
2 2 0
```Câu trả lời đúng là`NO`bởi vì Volodya không có gì để di chuyển. Bất kỳ triển khai nào chỉ kiểm tra kích thước bảng hoặc kích thước đường viền mà không xử lý$k=0$riêng biệt sẽ thất bại. 

Một trường hợp tinh tế khác là khi một chiếc bánh đã bắt đầu ở đường viền:```
2 2 1
1 2
```Câu trả lời là`YES`. Volodya thắng ngay ở nước đi đầu tiên trước khi Vlad chặn được bất cứ thứ gì. Việc quên thứ tự di chuyển dẫn đến kết quả sai. 

Trường hợp phức tạp quan trọng nhất là khi có nhiều bánh nhưng tất cả đều nằm sâu bên trong bảng:```
5 5 20
3 3
3 3
...
```Ngay cả với nhiều bánh nướng, câu trả lời vẫn có thể là`NO`. Vlad chỉ chặn một cạnh biên mỗi lượt, nhưng Volodya cũng cần thời gian để đưa bánh nướng đến ranh giới. Số lượng quyết định không chỉ là số lượng bánh nướng mà là liệu có đủ số lượng bánh nướng có thể đe dọa lối thoát nhanh hơn Vlad có thể phong ấn chúng hay không. 

Điều tinh tế cuối cùng là các ô ranh giới khác nhau đóng góp số lượng lối thoát khác nhau. Các ô góc có hai cạnh viền dẫn ra ngoài, trong khi các ô ranh giới không góc có một cạnh. Việc xử lý mọi ô ranh giới đều làm mất thông tin quan trọng như nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa toàn bộ trạng thái trò chơi. Chúng tôi sẽ theo dõi tất cả các vị trí hình tròn và tất cả các cạnh viền bị chặn, sau đó chạy phân tích tối đa hóa hoặc ngược dòng. Điều này đúng về mặt lý thuyết vì trò chơi là hữu hạn và mang tính quyết định. 

Vấn đề là số lượng trạng thái bùng nổ ngay lập tức. Hội đồng quản trị có$2n + 2m$các cạnh viền. Mỗi cạnh có thể bị chặn hoặc không, cho khoảng$2^{2n+2m}$khả năng ngay cả trước khi xem xét vị trí chiếc bánh. Với$n=m=100$, điều này hoàn toàn vô vọng. 

Điểm quan trọng là trò chơi chỉ quan tâm đến một điều: Vlad cần bao nhiêu lượt để phong ấn hoàn toàn bàn cờ. 

Mỗi bước di chuyển của Vlad đều chặn chính xác một cạnh viền. Tổng số cạnh biên là:$$2n + 2m$$Nếu Volodya có thể buộc một số chiếc bánh đi đến một lối ra không bị chặn trước khi tất cả các cạnh này bị phong ấn thì anh ta sẽ thắng. 

Bây giờ hãy nghĩ từ góc độ của một chiếc bánh duy nhất. Giả sử khoảng cách tối thiểu của nó đến bất kỳ ô biên nào là$d$. Vậy thì Volodya cần chính xác$d$di chuyển để đặt nó vào ranh giới, và một động thái nữa để rời khỏi bảng. Để chiếc bánh đó có thể trốn thoát sau$d+1$Volodya di chuyển. 

Trong khi đó, sau mỗi nước đi đó, Vlad chặn được một cạnh. Sau đó$t$quay lại, Vlad đã chặn chính xác$t$các cạnh viền. 

Cái nhìn sâu sắc quan trọng là một chiếc bánh không cần tất cả các lối thoát để luôn mở. Nó chỉ cần một cạnh có thể sử dụng được tại thời điểm nó chạm đến ranh giới. Vì toàn bộ bảng chỉ có$2n+2m$tổng số lần thoát, Vlad cuối cùng cũng hết khả năng chặn. 

Giải pháp chính thức còn đơn giản hóa việc này hơn nữa. Nếu tồn tại ít nhất một chiếc bánh có khoảng cách đến ranh giới gần nhất nhiều nhất là 4 thì Volodya sẽ thắng. Nếu không Vlad sẽ thắng. 

Tại sao 4? Bởi vì tổng số lối ra riêng biệt xung quanh bất kỳ khu vực địa phương nào là quá lớn để Vlad có thể bịt kín đủ nhanh. Một chiếc bánh đủ gần ranh giới luôn có thể buộc truy cập vào một số cạnh vẫn còn mở trước khi Vlad có thể đóng mọi khả năng. Nếu tất cả các bánh ở xa hơn 4, Vlad có đủ thời gian để chuẩn bị phòng thủ hoàn chỉnh. 

Vì vậy, trò chơi chuyển sang kiểm tra xem một chiếc bánh nào đó có nằm trong khoảng cách 4 tính từ đường biên hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước bảng và số lượng bánh nướng. 
2. Đối với mỗi chiếc bánh ở vị trí$(x,y)$, tính khoảng cách tối thiểu của nó tới bất kỳ ranh giới bảng nào. 

Bốn khoảng cách đó là:$$x-1,\ n-x,\ y-1,\ m-y$$Mức tối thiểu của chúng cho chúng ta biết cần bao nhiêu bước di chuyển để lần đầu tiên đến được ô ranh giới. 
3. Nếu khoảng cách tối thiểu này nhiều nhất là 4 cho bất kỳ chiếc bánh nào, hãy in ngay`YES`. 

Một chiếc bánh gần ranh giới như thế này luôn có thể buộc phải trốn thoát trước khi Vlad phong ấn mọi mép biên giới có liên quan. 
4. Nếu tất cả các bánh có khoảng cách lớn hơn 4, hãy in`NO`. 

Vlad có đủ thời gian để tổ chức phong tỏa hoàn toàn trước khi bất kỳ chiếc bánh nào trở nên nguy hiểm. 

### Tại sao nó hoạt động 

Trò chơi được điều khiển hoàn toàn bằng thời gian. 

Một chiếc bánh ở khoảng cách$d$từ nhu cầu ranh giới$d+1$chuyển động để trốn thoát. Vlad nhận được chính xác một cạnh bị chặn sau mỗi lần di chuyển. Nếu như$d \le 4$, chiếc bánh đạt đến vùng ranh giới đủ nhanh để vẫn có những lối thoát mở không thể tránh khỏi. Vlad không thể đồng thời phong ấn tất cả các lối thoát có thể có trong vài lượt như vậy. 

Nếu mỗi chiếc bánh có khoảng cách ít nhất là 5 thì Volodya cần nhiều lượt trước khi mọi nỗ lực trốn thoát trở nên ngay lập tức. Vlad có thể sử dụng thời gian này để chặn các lối thoát một cách chiến lược và cuối cùng ngăn chặn mọi lối thoát có thể xảy ra. Vì chỉ có các cạnh biên giới mới quan trọng nên chuyển động bên trong không còn hữu ích nữa khi chu vi được kiểm soát. 

Sự phân đôi đó khớp chính xác với điều kiện được thuật toán kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())

for _ in range(k):
    x, y = map(int, input().split())

    dist = min(x - 1, n - x, y - 1, m - y)

    if dist <= 4:
        print("YES")
        sys.exit()

print("NO")
```Việc triển khai có chủ ý rất nhỏ vì phần khó là khám phá thuộc tính trò chơi. 

Đối với mỗi chiếc bánh, chúng tôi tính toán số bước tối thiểu cần thiết để đến bất kỳ ô biên nào. Vì tọa độ có chỉ số 1 nên khoảng cách trên cùng là`x - 1`, khoảng cách đáy là`n - x`, khoảng cách bên trái là`y - 1`, và khoảng cách đúng là`m - y`. 

Khi tìm thấy một chiếc bánh có khoảng cách lớn nhất là 4, chúng ta có thể dừng lại ngay lập tức và in`YES`. Những chiếc bánh nướng không còn quan trọng nữa vì Volodya chỉ cần một lần trốn thoát thành công. 

Sớm`sys.exit()`tránh việc xử lý không cần thiết và giữ cho mã đơn giản. 

Một lỗi thường gặp là nhầm lẫn giữa "khoảng cách đến ô ranh giới" với "khoảng cách rời khỏi bảng". Định lý đã giải thích điều này một cách chính xác. Chúng tôi chỉ kiểm tra xem khoảng cách ranh giới có tối đa là 4 hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 1
1 2
```Dấu vết: 

| Vị trí bánh | Khoảng cách | Khoảng cách tối thiểu | Quyết định | 
| --- | --- | --- | --- | 
| (1,2) | 0,1,1,0 | 0 | CÓ | 

Chiếc bánh đã bắt đầu ở ranh giới. Volodya ngay lập tức di chuyển nó ra ngoài trước khi Vlad kịp chặn bất cứ thứ gì. 

### Ví dụ 2 

đầu vào:```
15 15 2
8 8
10 10
```Dấu vết: 

| Vị trí bánh | Khoảng cách | Khoảng cách tối thiểu | Quyết định | 
| --- | --- | --- | --- | 
| (8,8) | 7,7,7,7 | 7 | tiếp tục | 
| (10,10) | 9,5,9,5 | 5 | tiếp tục | 

Không có chiếc bánh nào có khoảng cách nhiều nhất là 4 nên đáp án là`NO`. 

Ví dụ này chứng minh rằng ngay cả những chiếc bánh tương đối trung tâm vẫn hoạt động quá chậm. Vlad có đủ lượt để chuẩn bị một vòng phòng thủ hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k)$| Mỗi chiếc bánh được chế biến một lần | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Với tối đa 100 chiếc bánh, chương trình sẽ chạy ngay lập tức. Việc sử dụng bộ nhớ không đáng kể vì chúng tôi không bao giờ lưu trữ toàn bộ bảng hoặc mô phỏng trò chơi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    for _ in range(k):
        x, y = map(int, input().split())

        dist = min(x - 1, n - x, y - 1, m - y)

        if dist <= 4:
            print("YES")
            return

    print("NO")

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
assert run(
"""2 2 1
1 2
"""
) == "YES", "sample 1"

# no pies
assert run(
"""5 5 0
"""
) == "NO", "no pies"

# pie exactly distance 4
assert run(
"""10 10 1
5 5
"""
) == "YES", "distance exactly 4"

# pie too far
assert run(
"""20 20 1
10 10
"""
) == "NO", "deep interior"

# multiple pies, one winning
assert run(
"""30 30 3
15 15
20 20
5 10
"""
) == "YES", "one reachable pie"

# smallest board
assert run(
"""1 1 1
1 1
"""
) == "YES", "already on boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 5 0`|`NO`| Xử lý chính xác dữ liệu nhập trống | 
|`10 10`với chiếc bánh`(5,5)`|`YES`| Khoảng cách đúng 4 là thắng | 
|`20 20`với chiếc bánh`(10,10)`|`NO`| Bánh nướng nội thất thua | 
| Nhiều chiếc bánh với một đóng |`YES`| Chỉ cần một chiếc bánh chiến thắng | 
|`1 1 1`|`YES`| Bảng nhỏ nhất có thể | 

## Vỏ cạnh 

Xét trường hợp bảng trống:```
2 2 0
```Thuật toán không bao giờ đi vào vòng lặp trên bánh nướng, do đó nó in trực tiếp`NO`. 

Điều này phù hợp với trò chơi vì Volodya không có động thái hợp pháp. 

Bây giờ hãy xem xét một chiếc bánh đã ở trên ranh giới:```
3 3 1
1 2
```Khoảng cách tính được là:$$0,\ 2,\ 1,\ 1$$Tối thiểu là 0, tối đa là 4, do đó thuật toán sẽ in`YES`. 

Volodya thắng ngay lập tức bằng cách di chuyển chiếc bánh ra ngoài trước khi Vlad hành động. 

Tiếp theo, hãy xem xét một chiếc bánh chính xác ở ngưỡng tới hạn:```
10 10 1
5 5
```Khoảng cách là:$$4,\ 5,\ 4,\ 5$$Khoảng cách tối thiểu là 4, vì vậy câu trả lời là`YES`. 

Điều này kiểm tra điều kiện biên bao gồm. sử dụng`< 4`thay vì`<= 4`sẽ thất bại ở đây. 

Cuối cùng, hãy xem xét một chiếc bánh vừa vượt quá ngưỡng:```
12 12 1
6 6
```Khoảng cách là:$$5,\ 6,\ 5,\ 6$$Khoảng cách tối thiểu là 5 nên thuật toán in`NO`. 

Đây là cấu hình bên trong bị hao hụt nhỏ nhất và bắt từng lỗi một xung quanh điểm cắt.
