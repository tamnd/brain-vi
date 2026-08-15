---
title: "CF 102428M - Dãy núi"
description: "Con đường này có N điểm ngắm cảnh theo thứ tự gặp phải khi đi về phía đỉnh núi. Độ cao của chúng tạo thành một mảng không giảm nên việc tiến về phía trước không bao giờ cần phải xuống dốc. Cặp đôi có thể chọn bất kỳ quan điểm nào làm điểm xuất phát của họ."
date: "2026-08-12T07:23:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "M"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 67
verified: true
draft: false
---

[CF 102428M - Dãy núi](https://codeforces.com/problemset/problem/102428/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đường mòn chứa`N`các quan điểm theo thứ tự gặp phải khi đi về phía đỉnh núi. Độ cao của chúng tạo thành một mảng không giảm nên việc tiến về phía trước không bao giờ cần phải xuống dốc. 

Cặp đôi có thể chọn bất kỳ quan điểm nào làm điểm xuất phát của họ. Từ đó, họ lần lượt truy cập các điểm quan sát theo thứ tự mảng tăng dần. Một bước chuyển từ quan điểm`i`để quan điểm`i + 1`chỉ được phép khi độ cao tăng`A[i + 1] - A[i]`nhiều nhất là`X`. Ngay khi một mức tăng liền kề lớn hơn`X`, cuộc đi bộ phải dừng lại. Nhiệm vụ là tìm số lượng điểm quan sát liên tiếp lớn nhất có thể được ghé thăm. 

Hệ quả chính của thứ tự không giảm là mọi mức tăng có thể xảy ra đều tương ứng với một đoạn liền kề của mảng. Chúng ta chỉ cần tìm đoạn dài nhất có chênh lệch độ cao liền kề nhiều nhất là`X`. 

Đây`N`nhiều nhất là`1000`, vì vậy ngay cả một`O(N^2)`giải pháp thực hiện tối đa khoảng nửa triệu phép so sánh liền kề trong trường hợp xấu nhất. Đó là đủ nhỏ cho những hạn chế này. Tuy nhiên, cấu trúc của vấn đề mang lại một`O(N)`giải pháp đơn giản hơn và có khả năng mở rộng hơn. Độ cao và`X`giới hạn cũng đủ nhỏ để số nguyên Python không có vấn đề tràn. 

Có một số trường hợp đặc biệt có thể bộc lộ sai sót trong logic chuyển tiếp. Thứ nhất, với một quan điểm, không có động thái nào để thực hiện nên câu trả lời phải là`1`. Ví dụ,```
1 0
500
```có câu trả lời`1`. Một giải pháp khởi tạo độ dài tốt nhất của nó bằng 0 sẽ trả về 0 không chính xác. 

Trường hợp cạnh thứ hai là`X = 0`. Vì mảng không giảm nên các bước di chuyển duy nhất được phép là giữa các điểm quan sát có độ cao chính xác bằng nhau. Ví dụ,```
5 0
10 10 10 11 11
```có câu trả lời`3`, bởi vì ba góc nhìn đầu tiên tạo thành đoạn hợp lệ dài nhất. Việc thực hiện bất cẩn bằng cách sử dụng`< X`thay vì`<= X`sẽ từ chối ngay cả những chuyển động có độ cao bằng nhau. 

Trường hợp thứ ba xảy ra khi mọi chuyển đổi đều được cho phép. Ví dụ,```
4 3
2 3 5 8
```có câu trả lời`4`. Quan điểm cuối cùng phải được tính khi một phân đoạn hợp lệ đến cuối mảng. Việc triển khai chỉ cập nhật câu trả lời khi gặp phải chuyển đổi không hợp lệ có thể vô tình bỏ lỡ trường hợp này. 

Cuối cùng, một quá trình chuyển đổi không hợp lệ sẽ phá vỡ hoàn toàn phân đoạn hiện tại. Vì```
5 2
1 2 10 11 12
```câu trả lời là`3`, từ`10, 11, 12`. Một khi nhảy từ`2`ĐẾN`10`quá lớn, đoạn hợp lệ trước nó không thể được mở rộng qua bước nhảy đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi quan điểm ban đầu có thể. Bắt đầu từ chỉ mục`i`, chúng tôi đi bộ về phía đỉnh trong khi chênh lệch độ cao liên tiếp vẫn còn nhiều nhất`X`, đếm xem có thể tiếp cận được bao nhiêu quan điểm. Số lượng tối đa trên tất cả các vị trí bắt đầu là câu trả lời. Điều này đúng vì mỗi chuyến đi bộ có thể có chính xác một điểm xuất phát và từ điểm xuất phát đó chỉ có một hướng để đi theo. 

Trong trường hợp xấu nhất, khi mọi chuyển đổi đều hợp lệ, bắt đầu từ chỉ mục`0`séc`N - 1`chuyển tiếp, bắt đầu từ chỉ mục`1`séc`N - 2`, vân vân. Tổng cộng là`(N - 1) + (N - 2) + ... + 1 = N(N - 1)/2`sự so sánh, đó là`499,500`khi`N = 1000`. Điều đó hoàn toàn có thể quản lý được đối với ràng buộc đã nêu, vì vậy giải pháp vũ phu thực sự có thể chấp nhận được ở đây. Điểm yếu của nó là lặp đi lặp lại quá trình kiểm tra chuyển tiếp giống nhau nhiều lần. 

Quan sát loại bỏ sự lặp lại này là quá trình chuyển đổi có giá trị hoặc không hợp lệ, độc lập với nơi bắt đầu tăng vọt hiện tại. Nếu như`A[i + 1] - A[i] <= X`, một đoạn hợp lệ có thể tiếp tục đi qua ranh giới đó. Nếu sự khác biệt lớn hơn`X`, không một chuyến đi bộ nào có thể vượt qua ranh giới đó cả. Vì vậy, chúng ta có thể quét mảng một lần và duy trì độ dài của đoạn hợp lệ liên tiếp hiện tại. 

Bất cứ khi nào quá trình chuyển đổi hợp lệ, phân đoạn hiện tại sẽ tăng theo một quan điểm. Bất cứ khi nào nó không hợp lệ, đoạn hiện tại phải khởi động lại ở điểm xem tiếp theo, có độ dài là`1`. Giá trị lớn nhất từng đạt được là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Được chấp nhận cho N ≤ 1000 | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`current = 1`Và`answer = 1`. Một quan điểm duy nhất luôn là một chuyến đi bộ hợp lệ vì không cần di chuyển. 
2. Quét từng cặp liền kề`A[i - 1]`Và`A[i]`. 
3. Nếu`A[i] - A[i - 1] <= X`, mở rộng phân đoạn hợp lệ hiện tại bằng cách đặt`current += 1`. Điều này hợp lệ vì cặp đôi có thể chuyển từ quan điểm trước đó sang quan điểm hiện tại. 
4. Nếu không, hãy đặt`current = 1`. Không thể vượt qua quá trình chuyển đổi giữa hai quan điểm này, do đó, phân đoạn hợp lệ duy nhất kết thúc ở quan điểm hiện tại bắt đầu ở chính quan điểm hiện tại. 
5. Sau khi xử lý từng góc nhìn, cập nhật`answer = max(answer, current)`. Phân đoạn hiện tại có thể là phân đoạn dài nhất được xem cho đến nay, bao gồm cả thời điểm đến điểm quan sát cuối cùng. 
6. In`answer`. 

### Tại sao nó hoạt động 

Giữ nguyên bất biến đó`current`chính xác là số điểm quan sát liên tiếp tối đa trong đoạn kết thúc ở chỉ số hiện tại có mỗi lần tăng độ cao liền kề nhiều nhất`X`. Nếu quá trình chuyển đổi hiện tại được cho phép, phân đoạn hợp lệ trước đó có thể được mở rộng thêm một. Nếu quá trình chuyển đổi bị cấm thì không có phân đoạn hợp lệ nào kết thúc ở điểm nhìn hiện tại có thể chứa điểm nhìn trước đó, vì vậy độ dài tốt nhất có thể của nó là chính xác`1`. Vì mỗi lần đi bộ đường dài có thể là một đoạn liền kề và mọi ranh giới đều được kiểm tra, tận dụng tối đa`current`qua quá trình quét sẽ cung cấp số lượng điểm quan sát tối đa có thể được truy cập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    current = 1
    answer = 1

    for i in range(1, n):
        if a[i] - a[i - 1] <= x:
            current += 1
        else:
            current = 1

        answer = max(answer, current)

    print(answer)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên ghi số điểm nhìn và mức tăng độ cao tối đa cho phép. Dòng thứ hai chứa độ cao của điểm quan sát đã được sắp xếp.`current`đại diện cho phân đoạn hợp lệ kết thúc ở vị trí hiện tại. Nó bắt đầu lúc`1`bởi vì quan điểm đầu tiên luôn có thể được ghé thăm bởi chính nó.`answer`cũng bắt đầu lúc`1`vì lý do tương tự. 

Vòng lặp bắt đầu tại chỉ mục`1`bởi vì quá trình chuyển đổi đòi hỏi hai quan điểm. biểu thức`a[i] - a[i - 1] <= x`cố tình sử dụng`<=`, vì mức tăng chính xác bằng`X`được cho phép. 

Khi chuyển tiếp quá lớn,`current`trở thành`1`, không`0`. Bản thân quan điểm hiện tại vẫn là điểm khởi đầu hoàn toàn hợp lệ. Đang cập nhật`answer`sau khi cả hai nhánh cũng xử lý một phân đoạn hợp lệ tiếp tục đến điểm quan sát cuối cùng. 

Không cần phải xử lý đặc biệt`N = 1`. Việc khởi tạo đã tạo ra câu trả lời đúng và vòng lặp chỉ thực hiện 0 lần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu có`X = 2`, sự khác biệt về độ cao là`11, 1, 77, 561, 5244, 0, 1, 2`. Chỉ có bốn quan điểm cuối cùng tạo thành một phân đoạn hợp lệ liên tục. 

| Chỉ mục | Độ cao | Sự khác biệt |`current`|`answer`| 
| --- | --- | --- | --- | --- | 
| 0 | 3 | - | 1 | 1 | 
| 1 | 14 | 11 | 1 | 1 | 
| 2 | 15 | 1 | 2 | 2 | 
| 3 | 92 | 77 | 1 | 2 | 
| 4 | 653 | 561 | 1 | 2 | 
| 5 | 5897 | 5244 | 1 | 2 | 
| 6 | 5897 | 0 | 2 | 2 | 
| 7 | 5898 | 1 | 3 | 3 | 
| 8 | 5900 | 2 | 4 | 4 | 

Câu trả lời cuối cùng là`4`. Dấu vết cho thấy lý do tại sao một quá trình chuyển đổi không hợp lệ lại đặt lại phân đoạn và tại sao một quá trình chuyển đổi lại chính xác bằng`X`được chấp nhận. 

### Mẫu 2 

đây`X = 0`, do đó chỉ có thể kết nối các độ cao liên tiếp bằng nhau. 

| Chỉ mục | Độ cao | Sự khác biệt |`current`|`answer`| 
| --- | --- | --- | --- | --- | 
| 0 | 3 | - | 1 | 1 | 
| 1 | 14 | 11 | 1 | 1 | 
| 2 | 15 | 1 | 1 | 1 | 
| 3 | 92 | 77 | 1 | 1 | 
| 4 | 653 | 561 | 1 | 1 | 
| 5 | 5897 | 5244 | 1 | 1 | 
| 6 | 5897 | 0 | 2 | 2 | 
| 7 | 5898 | 1 | 1 | 2 | 
| 8 | 5900 | 2 | 1 | 2 | 

Câu trả lời là`2`, tương ứng với hai điểm nhìn liên tiếp ở độ cao`5897`. Điều này thực hiện ranh giới bình đẳng cho`X = 0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi cặp liền kề được kiểm tra chính xác một lần. | 
| Không gian | O(N) | Mảng độ cao được lưu trữ, trong khi bản thân thuật toán sử dụng không gian bổ sung O(1). | 

Với`N ≤ 1000`, thuật toán chỉ thực hiện một số thao tác tuyến tính và sử dụng một lượng bộ nhớ rất nhỏ. Biểu diễn số nguyên của Python cũng làm cho tình trạng tràn không liên quan đến giới hạn độ cao đã cho. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    current = 1
    answer = 1

    for i in range(1, n):
        if a[i] - a[i - 1] <= x:
            current += 1
        else:
            current = 1

        answer = max(answer, current)

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("""9 2
3 14 15 92 653 5897 5897 5898 5900
""") == "4", "sample 1"

assert run("""9 0
3 14 15 92 653 5897 5897 5898 5900
""") == "2", "sample 2"

assert run("""9 8848
3 14 15 92 653 5897 5897 5898 5900
""") == "9", "sample 3"

# Minimum-size input
assert run("""1 0
500
""") == "1", "single viewpoint"

# All viewpoints have the same altitude
assert run("""6 0
100 100 100 100 100 100
""") == "6", "all equal"

# Boundary condition: difference exactly X is allowed
assert run("""5 3
2 5 8 11 15
""") == "4", "exactly X"

# Multiple breaks, longest segment is at the end
assert run("""7 2
1 2 10 11 12 20 21
""") == "3", "multiple breaks"

# Maximum-size input
assert run(
    "1000 0\n" + " ".join(["42"] * 1000) + "\n"
) == "1000", "maximum size"

# Off-by-one case: final valid segment reaches the end
assert run("""4 2
1 10 11 13
""") == "3", "final segment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 500`| 1 | Đầu vào tối thiểu và câu trả lời ban đầu | 
|`6 0 / 100 100 100 100 100 100`| 6 | Tất cả các chuyển đổi hợp lệ khi`X = 0`| 
|`5 3 / 2 5 8 11 15`| 4 | Một sự khác biệt chính xác bằng`X`được phép | 
|`7 2 / 1 2 10 11 12 20 21`| 3 | Một số chuyển tiếp không hợp lệ và khởi động lại | 
| 1000 giá trị bằng nhau với`X = 0`| 1000 | Kích thước đầu vào tối đa | 
|`4 2 / 1 10 11 13`| 3 | Đếm chính xác một đoạn kết thúc ở điểm nhìn cuối cùng | 

## Vỏ cạnh 

Đối với một quan điểm duy nhất, đầu vào```
1 0
500
```khởi tạo cả hai`current`Và`answer`ĐẾN`1`. Vòng lặp không có chuyển đổi nào để kiểm tra, vì vậy đầu ra vẫn giữ nguyên`1`, điều này đúng vì việc tham quan một điểm quan sát không cần phải di chuyển lên dốc. 

Vì`X = 0`, coi như```
5 0
10 10 10 11 11
```Sự chuyển đổi đầu tiên có sự khác biệt`0`, Vì thế`current`trở thành`2`. Thứ hai cũng có sự khác biệt`0`, cho`current = 3`. Sự chuyển tiếp từ`10`ĐẾN`11`có sự khác biệt`1`, Vì thế`current`đặt lại thành`1`. Cặp bằng nhau cuối cùng nâng nó lên`2`. Tối đa là`3`. 

Khi quá trình chuyển đổi đúng giới hạn cho phép thì không được phá vỡ phân đoạn. Vì```
5 3
2 5 8 11 15
```ba điểm khác biệt đầu tiên đều là`3`, Vì thế`current`phát triển từ`1`ĐẾN`4`. Sự khác biệt cuối cùng là`4`, thiết lập lại`current`ĐẾN`1`. Đầu ra là`4`. Đây là trường hợp phân biệt`<= X`từ`< X`. 

Đối với nhiều lần nghỉ giải lao,```
7 2
1 2 10 11 12 20 21
```sự khác biệt đầu tiên`1`tạo ra một đoạn có độ dài`2`. Sự khác biệt`8`phá vỡ nó, vì vậy quan điểm ở độ cao`10`bắt đầu một đoạn mới. Hai điểm khác biệt tiếp theo là`1`Và`1`, cho một đoạn có độ dài`3`. Sự khác biệt`8`phá vỡ nó một lần nữa và cặp cuối cùng có chiều dài`2`. Câu trả lời là do đó`3`. 

Khi đoạn dài nhất kết thúc, câu trả lời phải được cập nhật trong khi xử lý quan điểm cuối cùng của nó. TRONG```
4 2
1 10 11 13
```quá trình chuyển đổi đầu tiên bị gián đoạn ngay lập tức, sau đó là sự khác biệt`1`Và`2`mở rộng đoạn bắt đầu từ`10`chiều dài`3`. Bởi vì`answer`được cập nhật trên mỗi lần lặp lại, phân đoạn cuối cùng đó sẽ được tính ngay cả khi không có chuyển đổi không hợp lệ nào sau đó để kích hoạt một bản cập nhật riêng.
