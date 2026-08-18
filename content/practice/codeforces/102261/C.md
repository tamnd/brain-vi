---
title: "CF 102261C - \u0418\u043d\u0442\u0435\u0440\u0435\u0441\u043d\u0430\u044f \u0438\u0433\u0440\u0430"
description: "Petya có một dãy N thẻ, mỗi thẻ chứa một số nguyên không âm. Trước khi trò chơi bắt đầu, Vasya chọn điểm mục tiêu K. Petya sau đó lật các lá bài từ trái sang phải."
date: "2026-08-17T20:45:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102261
codeforces_index: "C"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102261
solve_time_s: 75
verified: true
draft: false
---

[CF 102261C - \u0418\u043d\u0442\u0435\u0440\u0435\u0441\u043d\u0430\u044f \u0438\u0433\u0440\u0430](https://codeforces.com/problemset/problem/102261/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Petya có một dãy N thẻ, mỗi thẻ chứa một số nguyên không âm. Trước khi trò chơi bắt đầu, Vasya chọn điểm mục tiêu K. Petya sau đó lật các lá bài từ trái sang phải. 

Lá bài chia hết cho 5 mang lại cho Vasya một điểm, trong khi lá bài chia hết cho 3 mang lại cho Petya một điểm. Một số chia hết cho cả 3 và 5 sẽ không cho ai một điểm, cũng như một số không chia hết cho cả 2. Trò chơi kết thúc ngay lập tức khi một người chơi đạt được K điểm. Nếu tất cả các lá bài đã được xử lý mà không một trong hai người chơi đạt K, người chơi có số điểm lớn hơn sẽ thắng và số điểm bằng nhau sẽ hòa. 

Đầu vào cho K, điểm mục tiêu, theo sau là N và giá trị thẻ N. Đầu ra cần thiết là`Petya`,`Vasya`, hoặc`Draw`, tùy thuộc vào người chiến thắng trong trò chơi tuần tự chính xác này. 

Ràng buộc quan trọng là N <= 10^6. Một giải pháp về cơ bản phải tuyến tính về số lượng thẻ vì ngay cả O(N log N) cũng không cần thiết ở đây, trong khi mọi phép tính bậc hai sẽ yêu cầu khoảng 10^12 phép tính trong trường hợp xấu nhất. Giá trị của mỗi lá bài tối đa là 1000, vì vậy việc kiểm tra khả năng chia hết cho hai số cố định 3 và 5 có thể được thực hiện bằng các phép tính số dư theo thời gian không đổi. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý trở nên sai lầm. Đầu tiên, một số chia hết cho cả 3 và 5 sẽ không có điểm. Ví dụ,```
1 1
15
```sản xuất`Draw`, không`Petya`hoặc`Vasya`. Vì 15 chia hết cho cả hai số nên điểm số không thay đổi. 

Thứ hai, thứ tự của các quân bài rất quan trọng vì trò chơi sẽ dừng ngay lập tức. Ví dụ,```
2 3
3 5 3
```sản xuất`Petya`. Petya nhận được điểm từ quân bài đầu tiên và quân bài thứ ba, nhưng sau quân bài thứ ba, anh ta đạt được 2 điểm và thắng. Một giải pháp chỉ đơn giản là đếm tất cả các quân bài và so sánh tổng số sẽ giải quyết được trường hợp này, nhưng cách tiếp cận đó không thể tái tạo các trường hợp một người chơi đạt K trước khi các quân bài sau đó được xử lý. 

Thứ ba, số 0 chia hết cho mọi số nguyên dương. Như vậy,```
1 1
0
```sản xuất`Draw`, vì số 0 chia hết cho cả 3 và 5 và không ai được điểm. Coi số 0 là chia hết cho cả hai sẽ cho kết quả không chính xác`Petya`hoặc`Vasya`tùy theo việc thực hiện. 

## Phương pháp tiếp cận 

Việc triển khai bạo lực trực tiếp có thể mô phỏng trò chơi, nhưng giả sử khả năng chia hết được xử lý bằng cách tìm kiếm chung trên các ước số có thể có cho mỗi thẻ. Đối với thẻ có giá trị x, phương pháp như vậy có thể kiểm tra tối đa x ứng viên trước khi quyết định thẻ hoạt động như thế nào. Vì x có thể là 1000 và có thể có 10^6 thẻ nên trường hợp xấu nhất đạt tới khoảng 10^9 phép chia. Điều đó vượt xa những gì giới hạn một giây có thể chịu đựng được. 

Mô phỏng lực lượng vũ phu là chính xác về mặt khái niệm vì mỗi lá bài đều ảnh hưởng đến điểm số chính xác theo thuộc tính chia hết của nó và việc xử lý các lá bài theo thứ tự sẽ tái tạo trò chơi thực tế. Vấn đề không phải là bản thân mô phỏng. Sự lãng phí xuất phát từ việc coi một bài kiểm tra tính chia hết rất đơn giản như một phép tìm kiếm số học tổng quát. 

Quan sát quan trọng là các ước số liên quan duy nhất đã được cố định trước: 3 và 5. Chúng ta không cần khám phá xem một số có chia hết cho chúng hay không. Hai phép toán còn lại`x % 3`Và`x % 5`, xác định hoàn toàn tác dụng của lá bài. 

Có một quan sát hữu ích khác về điều kiện dừng. Thời điểm một trong hai điểm đạt đến K, câu trả lời đã được biết trước nên không có lý do gì để kiểm tra các lá bài sau đó. Trong trường hợp xấu nhất, chúng tôi vẫn xử lý tất cả N thẻ, nhưng độ phức tạp là O(N), tối ưu vì bản thân đầu vào chứa N giá trị. 

Cách tiếp cận kết quả chỉ đơn giản là quét từ trái sang phải. Đối với mỗi lá bài, trước tiên hãy kiểm tra trường hợp nó chia hết cho cả 3 và 5, vì trường hợp đó không có điểm. Ngược lại, tăng điểm của Petya nếu chia hết cho 3 hoặc tăng điểm của Vasya nếu chia hết cho 5. Sau mỗi thẻ ghi điểm, hãy kiểm tra xem người chơi tương ứng đã đạt đến K hay chưa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm ước chung | O(N · tối đa(A)) | O(1) | Quá chậm | 
| Mô phỏng trực tiếp với`% 3`Và`% 5`| O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc K và N, sau đó xử lý thẻ từ thẻ đầu tiên đến thẻ cuối cùng. Thứ tự phải được giữ nguyên vì đạt K sẽ kết thúc trò chơi ngay lập tức. 
2. Duy trì hai quầy,`petya`Và`vasya`, ban đầu cả hai đều bằng không. Chúng thể hiện điểm chính xác sau khi tất cả các thẻ được xử lý cho đến nay. 
3. Với mỗi giá trị`x`, trước tiên hãy kiểm tra xem`x`chia hết cho cả 3 và 5. Nếu đúng thì giữ nguyên cả hai bộ đếm. Điều kiện này phải được kiểm tra trước khi kiểm tra khả năng chia của từng cá nhân vì lá bài như vậy không mang lại điểm cho cả hai người chơi. 
4. Nếu`x`chia hết cho 3 nhưng không chia hết cho 5, tăng dần`petya`. Kiểm tra ngay xem`petya == K`. Nếu có thì in`Petya`và chấm dứt vì những lá bài sau không thể thay đổi được người thắng cuộc. 
5. Nếu`x`chia hết cho 5 nhưng không chia hết cho 3, tăng dần`vasya`. Kiểm tra ngay xem`vasya == K`. Nếu có thì in`Vasya`và chấm dứt vì lý do tương tự. 
6. Nếu tất cả N thẻ đã được xử lý mà không có người chơi nào đạt được K, hãy so sánh điểm cuối cùng của hai người. Điểm lớn hơn quyết định người chiến thắng, trong khi điểm bằng nhau tạo ra`Draw`. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của chuỗi thẻ,`petya`Và`vasya`chính xác là số điểm mà trò chơi thực sự sẽ có tại thời điểm đó, miễn là trò chơi chưa kết thúc. Mỗi thẻ rơi vào đúng một loại liên quan: chia hết cho cả hai, chỉ chia hết cho 3, chỉ chia hết cho 5 hoặc không chia hết cho cả hai. Thuật toán áp dụng chính xác quy tắc cho danh mục đó. Bởi vì nó kiểm tra mục tiêu ngay sau mỗi lần tăng điểm, nên nó cũng dừng lại ở đúng lá bài mà trò chơi thực sự sẽ dừng. Nếu không đạt được mục tiêu nào thì bộ đếm cuối cùng là điểm số cuối cùng thực tế, do đó việc so sánh chúng sẽ cho ra kết quả cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    K, N = map(int, input().split())

    petya = 0
    vasya = 0

    for _ in range(N):
        x = int(input().split()[0]) if False else None

    # The official input places all N numbers on the next line.
    # Re-read using a direct iterator for fast processing.
```Đầu vào chứa tất cả các giá trị thẻ N trên dòng tiếp theo, do đó, cách triển khai rõ ràng là lặp lại trực tiếp trên dòng đó:```python
import sys
input = sys.stdin.readline

def solve():
    K, N = map(int, input().split())

    petya = 0
    vasya = 0

    cards = map(int, input().split())

    for x in cards:
        divisible_by_3 = (x % 3 == 0)
        divisible_by_5 = (x % 5 == 0)

        if divisible_by_3 and divisible_by_5:
            continue

        if divisible_by_3:
            petya += 1
            if petya == K:
                print("Petya")
                return

        elif divisible_by_5:
            vasya += 1
            if vasya == K:
                print("Vasya")
                return

    if petya > vasya:
        print("Petya")
    elif vasya > petya:
        print("Vasya")
    else:
        print("Draw")

if __name__ == "__main__":
    solve()
```Hai biến đầu tiên lưu trữ điểm hiện tại. Chúng không bao giờ cần vượt quá K vì hàm sẽ trả về ngay lập tức khi điểm đạt đến mục tiêu. 

Hai biểu thức boolean tính toán các thuộc tính chia hết một lần cho mỗi thẻ. Trường hợp kết hợp được xử lý trước tiên, điều này rất cần thiết cho các giá trị như 0, 15, 30 và mọi bội số khác của 15. 

các`elif`Cấu trúc cũng đảm bảo rằng quân bài chỉ chia hết cho 3 sẽ mang lại chính xác một điểm cho Petya, trong khi quân bài chỉ chia hết cho 5 sẽ mang lại chính xác một điểm cho Vasya. Một số chia hết cho không đến nhánh nào. 

Kiểm tra đẳng thức đối với K là đủ vì bộ đếm chỉ tăng một. Điểm không thể nhảy từ K-1 lên giá trị lớn hơn K. Hàm trả về ngay khi xác định được người chiến thắng nên các lá bài còn lại không ảnh hưởng đến kết quả. 

Giải pháp tuân theo định dạng đầu vào trực tiếp và không lưu trữ toàn bộ mảng thẻ.`map`tạo ra các số nguyên được chuyển đổi một cách lười biếng từ các mã thông báo đầu vào được phân tách, giữ cho cấu trúc dữ liệu bổ sung của thuật toán có kích thước không đổi một cách hiệu quả. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là:```
3 10
1 2 3 4 5 6 7 8 9 10
```Mục tiêu là 3. Giá trị chia hết cho 3 sẽ cho điểm Petya và giá trị chia cho 5 sẽ cho điểm Vasya. 

| Thẻ | Giá trị | Điểm Petya | Điểm Vasya | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | Không có điểm | 
| 2 | 2 | 0 | 0 | Không có điểm | 
| 3 | 3 | 1 | 0 | Điểm Petya | 
| 4 | 4 | 1 | 0 | Không có điểm | 
| 5 | 5 | 1 | 1 | Điểm Vasya | 
| 6 | 6 | 2 | 1 | Điểm Petya | 
| 7 | 7 | 2 | 1 | Không có điểm | 
| 8 | 8 | 2 | 1 | Không có điểm | 
| 9 | 9 | 3 | 1 | Petya đạt K | 

Petya đạt ba điểm ở lá bài thứ chín nên thuật toán trả về ngay lập tức với`Petya`. Lá bài cuối cùng không bao giờ liên quan đến kết quả. 

Đối với Mẫu 2, đầu vào là:```
4 16
1 2 3 4 5 6 7 8 9 10 15 20 25 24 21 18
```Ở đây K là 4. Những quân bài chia hết cho cả 3 và 5, chẳng hạn như 15, không được cho điểm. 

| Thẻ | Giá trị | Điểm Petya | Điểm Vasya | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | Không có điểm | 
| 2 | 2 | 0 | 0 | Không có điểm | 
| 3 | 3 | 1 | 0 | Điểm Petya | 
| 4 | 4 | 1 | 0 | Không có điểm | 
| 5 | 5 | 1 | 1 | Điểm Vasya | 
| 6 | 6 | 2 | 1 | Điểm Petya | 
| 7 | 7 | 2 | 1 | Không có điểm | 
| 8 | 8 | 2 | 1 | Không có điểm | 
| 9 | 9 | 3 | 1 | Điểm Petya | 
| 10 | 10 | 3 | 2 | Điểm Vasya | 
| 11 | 15 | 3 | 2 | Cả hai đều chia hết, không có điểm | 
| 12 | 20 | 3 | 3 | Điểm Vasya | 
| 13 | 25 | 3 | 4 | Vasya đạt K | 

Vasya đạt bốn điểm ở lá bài 13 nên trò chơi kết thúc ở đó và câu trả lời là`Vasya`. Ba lá bài cuối cùng không quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi thẻ được kiểm tra một lần và sử dụng một số phép tính số học không đổi. | 
| Không gian | O(1) phụ trợ | Chỉ K, N và hai bộ đếm điểm được lưu trữ rõ ràng. | 

Với N tối đa 10^6, quá trình quét tuyến tính thực hiện tối đa một triệu lần lặp lại thẻ, phù hợp với các ràng buộc. Không có sự sắp xếp, phép lặp lồng nhau hoặc mảng phụ tỷ lệ với N. Việc kiểm tra số dư trực tiếp cũng tránh mọi sự phụ thuộc vào giá trị số ngoài số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def solve():
    K, N = map(int, input().split())

    petya = 0
    vasya = 0

    for x in map(int, input().split()):
        by3 = (x % 3 == 0)
        by5 = (x % 5 == 0)

        if by3 and by5:
            continue

        if by3:
            petya += 1
            if petya == K:
                print("Petya")
                return

        elif by5:
            vasya += 1
            if vasya == K:
                print("Vasya")
                return

    if petya > vasya:
        print("Petya")
    elif vasya > petya:
        print("Vasya")
    else:
        print("Draw")

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        old_stdout = sys.stdout
        sys.stdout = io.StringIO()

        try:
            solve()
            return sys.stdout.getvalue().strip()
        finally:
            sys.stdout = old_stdout
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("""3 10
1 2 3 4 5 6 7 8 9 10
""") == "Petya", "sample 1"

assert run("""4 16
1 2 3 4 5 6 7 8 9 10 15 20 25 24 21 18
""") == "Vasya", "sample 2"

assert run("""3 5
3 5 15 15 15
""") == "Draw", "sample 3"

# Minimum-size input, zero is divisible by both 3 and 5.
assert run("""1 1
0
""") == "Draw", "zero gives nobody a point"

# K is reached exactly on the final card.
assert run("""2 3
5 3 6
""") == "Petya", "Petya reaches K on the final card"

# All cards are divisible by both 3 and 5.
assert run("""1 4
0 15 30 45
""") == "Draw", "all cards give no points"

# Vasya reaches K before a later Petya-scoring card.
assert run("""2 4
5 10 3 3
""") == "Vasya", "immediate stopping"

# Large N. Petya reaches K very early, while the remaining cards are irrelevant.
large_input = "1000 1000000\n" + "3 " * 1000000
assert run(large_input) == "Petya", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`Draw`| Đầu vào tối thiểu và hành vi chia hết đặc biệt bằng 0 | 
|`2 3 / 5 3 6`|`Petya`| Đạt K chính xác ở lá bài cuối cùng | 
|`1 4 / 0 15 30 45`|`Draw`| Mỗi quân bài chia hết cho cả 3 và 5 | 
|`2 4 / 5 10 3 3`|`Vasya`| Trò chơi dừng ngay lập tức khi Vasya đạt K | 
|`1000 1000000 / 3 ...`|`Petya`| N tối đa và chấm dứt sớm | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là lá bài chia hết cho cả 3 và 5. Hãy xem xét:```
1 1
15
```Vì`x = 15`, cả hai`x % 3 == 0`Và`x % 5 == 0`là đúng. Điều kiện kết hợp được thực thi`continue`, vì vậy cả hai điểm vẫn bằng 0. Bây giờ tất cả các thẻ đã hết, điểm số bằng nhau và kết quả là`Draw`. Nếu không có séc tổng hợp, cùng một lá bài có thể trao điểm không chính xác cho một trong các người chơi. 

Trường hợp cạnh thứ hai bằng 0:```
1 1
0
```Số 0 thỏa mãn cả hai phép thử chia hết vì 0 là bội số của mọi số nguyên dương. Do đó, thuật toán xử lý nó chính xác như 15 và không cho điểm. Điểm cuối cùng là 0 và 0 nên kết quả là`Draw`. 

Trường hợp cạnh thứ ba là chấm dứt ngay lập tức:```
2 4
5 10 3 3
```Sau lá bài đầu tiên, điểm là 0 và 1. Sau lá bài thứ hai, chúng là 0 và 2, do đó Vasya đạt đến K và thuật toán in ra`Vasya`ngay lập tức. Hai lá bài còn lại sẽ cho Petya hai điểm, nhưng chúng không bao giờ được xem xét vì trò chơi thực sự đã kết thúc. 

Trường hợp cạnh thứ tư đạt K chính xác ở cuối:```
2 3
5 3 6
```Điểm sau hai lá bài đầu tiên là 1 cho Vasya và 1 cho Petya. Lá bài cuối cùng là 6, chia hết cho 3 nhưng không chia hết cho 5 nên Petya trở thành người chơi đầu tiên đạt 2 điểm. Thuật toán in`Petya`vào thời điểm đó thay vì lọt vào vòng so sánh điểm số cuối cùng. 

Trường hợp cạnh thứ năm là khi không có ai đạt đến K và điểm số cuối cùng quyết định người chiến thắng. Ví dụ,```
5 4
3 5 6 5
```mang lại cho Petya hai điểm từ 3 và 6, trong khi Vasya nhận được hai điểm từ hai điểm 5. Cả hai đều không đạt đến năm, vì vậy các quân bài kết thúc với số điểm bằng nhau và kết quả là`Draw`. Phép so sánh cuối cùng chỉ được sử dụng sau khi toàn bộ chuỗi đã được xử lý mà không có người chiến thắng trước đó.
