---
title: "CF 106488C - Baba đi thi"
description: "Vấn đề là tối đa hóa lợi nhuận khi nướng bánh. Có sẵn một lượng bột cố định. Một chiếc bánh có thể được làm mà không cần nhân hoặc bằng một trong một số loại nhân."
date: "2026-06-25T08:46:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106488
codeforces_index: "C"
codeforces_contest_name: "XXX Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 106488
solve_time_s: 34
verified: true
draft: false
---

[CF 106488C - Baba đi thi](https://codeforces.com/problemset/problem/106488/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề là tối đa hóa lợi nhuận khi nướng bánh. Có sẵn một lượng bột cố định. Một chiếc bánh có thể được làm mà không cần nhân hoặc bằng một trong một số loại nhân. Mỗi chiếc bánh nhồi tiêu tốn một ít bột và một lượng nhân cụ thể, sau đó mang lại một số tiền nhất định. Bánh không nhồi chỉ tiêu tốn bột và có chi phí cũng như phần thưởng cố định riêng. Mục tiêu là chọn ra số lượng bánh mỗi loại để nướng sao cho tổng doanh thu càng lớn càng tốt. 

Đầu vào cung cấp số lượng bột có sẵn, số loại nhân cũng như yêu cầu và phần thưởng của từng loại bánh. Đối với mỗi loại nhân, chúng ta biết có bao nhiêu nhân, một chiếc bánh tiêu thụ bao nhiêu nhân, cần bao nhiêu bột và kiếm được bao nhiêu tiền. Đầu ra là thu nhập tối đa có thể có sau khi sử dụng bất kỳ sự kết hợp nào của các loại bánh. 

Số lượng bột nhiều nhất là 1000 và có nhiều nhất 10 loại nhân. Giới hạn bột nhỏ là đầu mối chính. Một giải pháp phụ thuộc vào lượng bột có thể tạo ra trạng thái lập trình động. Giải pháp thử từng số lượng bánh có thể có của mỗi loại sẽ tăng theo cấp số nhân theo số lượng loại nhân nên không phù hợp. 

Một sai lầm phổ biến là xử lý từng chất nhồi một cách độc lập và tối đa hóa mọi lựa chọn chất nhồi riêng biệt. Điều này không thành công vì tất cả các loại bánh đều có chung nguồn bột. Ví dụ:```
Input:
10 1 10 100
5 5 4 100

Output:
200
```Lựa chọn tốt nhất là nướng hai chiếc bánh nhồi, dùng 8 bột và 10 nhân. Một lựa chọn tham lam chỉ dựa trên việc nhồi mật độ lợi nhuận có thể dự trữ bột không chính xác cho những chiếc bánh không nhân hoặc chọn ít bánh nhân hơn. 

Một trường hợp khó khăn khác là khi nhân bánh không thể tạo thành một chiếc bánh hoàn chỉnh. Ví dụ:```
Input:
5 1 1 1
3 4 10 100

Output:
5
```Có nhân nhưng không đủ bột để làm một chiếc bánh nhồi. Hành động duy nhất có thể làm là làm năm cái bánh đơn giản. Một giải pháp chỉ kiểm tra tính khả dụng của chất nhồi và bỏ qua các yêu cầu về bột sẽ tạo ra câu trả lời sai. 

Trường hợp ranh giới cuối cùng là khi tài nguyên còn sót lại không thể được sử dụng. Ví dụ:```
Input:
7 1 3 5
4 4 2 10

Output:
15
```Hai chiếc bánh nhồi sử dụng 4 bột và 8 nhân, nhưng thực tế chỉ có thể làm được một chiếc bánh nhồi vì số lượng nhân có hạn. Các tài nguyên còn lại không thể được chuyển đổi thành một phần bun khác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi số lượng bánh có thể có cho mọi loại nhân. Đối với mỗi loại nhân, chúng ta có thể chọn bất kỳ vị trí nào từ số 0 đến mức tối đa mà số nhân có sẵn cho phép. Sau khi chọn hết bánh nhồi, chúng ta đổ bột bánh còn lại vào bánh thường. Điều này đúng vì mọi kế hoạch nướng hợp lệ đều tương ứng với một số kết hợp được bảng liệt kê xem xét. 

Vấn đề là số lượng kết hợp. Số lượng nhồi có thể cho phép tối đa 100 lựa chọn và có thể có 10 kiểu nhồi. Trong trường hợp xấu nhất, khám phá mọi khả năng có nghĩa là xung quanh$100^{10}$sự kết hợp, vượt xa những gì mà bất kỳ giới hạn thời gian của cuộc thi nào có thể xử lý được. 

Quan sát quan trọng là bột là nguồn tài nguyên duy nhất được chia sẻ bởi mọi loại bánh. Nguồn nhân nhồi là cục bộ cho các loại bánh riêng của chúng và số lượng bột chỉ là 1000. Thay vì theo dõi từng quyết định nhồi riêng biệt, chúng tôi có thể xử lý từng loại nhân nhồi và duy trì lợi nhuận tốt nhất cho mỗi lượng bột có thể đã bỏ ra. 

Đối với một loại nhân, chúng ta có một số lựa chọn giống hệt nhau: nướng không bánh, một bánh, hai bánh, v.v. cho đến khi hết nhân hoặc bột. Đây là một quá trình chuyển đổi ba lô giới hạn. Sau khi xử lý tất cả các loại bánh nhồi, phần bột còn lại luôn có thể được chuyển thành bánh bao thường, vì vậy câu trả lời cuối cùng có được bằng cách kiểm tra mọi lượng bột có thể được sử dụng cho bánh nhồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100^m) | O(1) | Quá chậm | 
| Tối ưu | O(m * n * 100) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng lập trình động trong đó`dp[x]`đại diện cho số tiền tối đa có thể đạt được sau khi sử dụng chính xác`x`gram bột trên bánh nhồi được chế biến cho đến nay. Ban đầu, chỉ có thể sử dụng bột không, vì vậy`dp[0] = 0`và mọi trạng thái khác là không thể. 
2. Xử lý từng loại nhồi một cách độc lập. Đối với cách nhồi hiện tại, hãy tính số lượng bánh tối đa có thể làm được từ nó. Giới hạn này xuất phát từ cả lượng nhân có sẵn và lượng bột có sẵn. 
3. Hãy thử mọi số lượng bánh có thể được làm bằng loại nhân này. Nếu chúng ta làm`k`bánh bao, họ tiêu thụ`k * c`bột và đưa`k * d`tiền bạc. Cập nhật trạng thái lập trình động tiếp theo từ trạng thái trước đó bằng cách thêm lựa chọn này. 
4. Thay thế trạng thái cũ bằng trạng thái mới tính toán sau khi hoàn thành kiểu nhồi này. Điều này ngăn cản việc sử dụng cùng một thứ nhiều lần hơn mức tồn tại. 
5. Sau khi xử lý xong tất cả các loại nhân, hãy xem xét mọi lượng bột có thể dùng để làm bánh nhồi. Nếu như`x`bột đã được sử dụng, phần còn lại`n - x`bột có thể tạo ra`(n - x) / c0`bánh đơn giản, mang lại thêm lợi nhuận`((n - x) / c0) * d0`. 
6. Lấy giá trị lớn nhất có thể`x`. Điều này bao gồm mọi sự kết hợp hợp lệ giữa bánh nhồi và bánh thường. 

Điều bất biến là sau khi xử lý một số kiểu nhồi,`dp[x]`chứa doanh thu tốt nhất có thể trong số tất cả các lựa chọn sử dụng chính xác`x`bột trên các loại nhồi đã qua chế biến. Mọi lựa chọn khả thi cho lần nhồi tiếp theo đều sử dụng số bánh bao bằng 0 hoặc một số số bánh bao hợp lệ, vì vậy quá trình chuyển đổi sẽ xem xét mọi cách mà bất biến có thể tiếp tục được giữ. Sau kiểu nhồi cuối cùng, việc thêm cách sử dụng bánh mì trơn tốt nhất có thể cho mỗi lượng bột còn lại sẽ mang lại kế hoạch nướng hoàn chỉnh tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, c0, d0 = map(int, input().split())

    stuffings = []
    for _ in range(m):
        a, b, c, d = map(int, input().split())
        stuffings.append((a, b, c, d))

    neg = -10**18
    dp = [neg] * (n + 1)
    dp[0] = 0

    for a, b, c, d in stuffings:
        ndp = [neg] * (n + 1)

        limit = a // b
        for used in range(n + 1):
            if dp[used] == neg:
                continue

            for cnt in range(limit + 1):
                dough = used + cnt * c
                if dough > n:
                    break
                ndp[dough] = max(ndp[dough], dp[used] + cnt * d)

        dp = ndp

    ans = 0
    for used in range(n + 1):
        if dp[used] == neg:
            continue
        plain = (n - used) // c0
        ans = max(ans, dp[used] + plain * d0)

    print(ans)

if __name__ == "__main__":
    solve()
```Phân tích cú pháp đầu vào lưu trữ mọi kiểu nhồi dưới dạng một bộ vì mỗi kiểu được xử lý riêng trong quá trình chuyển đổi lập trình động. 

Mảng`dp`sử dụng một giá trị không thể cho các trạng thái không thể truy cập được. Điều này tránh việc vô tình sử dụng trạng thái không thể tạo được. giá trị`-10**18`nằm dưới bất kỳ câu trả lời có thể nào một cách an toàn, do đó, nó hoạt động như âm vô cực mà không gây lo ngại tràn trong Python. 

Đối với mỗi kiểu nhồi, một mảng mới được tạo. Đây là phần ngăn cản việc liên tục lấy cùng một thứ vượt quá số lượng có sẵn. Vòng lặp bên trong thử mọi số lượng bánh có thể có từ phần nhồi hiện tại và ranh giới`dough > n`dừng công việc không cần thiết khi vượt quá giới hạn bột. 

Vòng lặp cuối cùng tách biệt với quá trình chuyển đổi nhồi vì bánh đơn giản có sẵn không giới hạn. Sau khi biết được lượng bột dùng để làm bánh nhồi, việc lựa chọn bánh đơn giản tốt nhất là điều xác định. Phép chia số nguyên xử lý bột thừa một cách chính xác vì không thể làm được những chiếc bánh chưa hoàn chỉnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 2 2 1
7 3 2 100
12 3 1 10
```Các trạng thái quan trọng sau khi xử lý từng loại nhồi là: 

| Bước | Bột đã qua sử dụng | Lợi nhuận tốt nhất | 
| --- | --- | --- | 
| Ban đầu | 0 | 0 | 
| Sau khi nhồi 1 | 4 | 200 | 
| Sau khi nhồi 1 | 10 | 200 | 
| Sau khi nhồi 2 | 8 | 240 | 
| Sau khi nhồi 2 | 10 | 241 | 

Trạng thái bánh nhồi tốt nhất sử dụng 10 bột và kiếm được 241. Không còn bột cho bánh thường, vì vậy câu trả lời cuối cùng là 241. 

Dấu vết này cho thấy tại sao DP giữ số lượng bột khác nhau thay vì chỉ giữ lợi nhuận cao nhất. Một quốc gia có ít lợi nhuận trước mắt hơn có thể dành chỗ cho những lựa chọn tốt hơn trong tương lai. 

### Mẫu 2 

đầu vào:```
100 1 25 50
15 5 20 10
```| Bước | Bột đã qua sử dụng | Lợi nhuận tốt nhất | 
| --- | --- | --- | 
| Ban đầu | 0 | 0 | 
| Sau khi nhồi | 20 | 10 | 
| Sau khi nhồi | 40 | 20 | 
| Sau khi nhồi | 60 | 30 | 
| Bánh tráng cuối cùng | 100 | 200 | 

Bánh nhồi không đủ lãi so với bánh thường. Phép tính cuối cùng sẽ lấp đầy chính xác phần bột chưa sử dụng bằng bốn chiếc bánh đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n * 100) | Đối với mỗi loại nhân, mỗi trạng thái bột sẽ thử mọi số lượng nhân có thể. | 
| Không gian | O(n) | Chỉ có mảng trạng thái bột hiện tại và tiếp theo được lưu trữ. | 

Đây,`m`nhiều nhất là 10 và`n`nhiều nhất là 1000. Số lượng thao tác là khoảng một triệu, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided samples
assert solution("""10 2 2 1
7 3 2 100
12 3 1 10
""") == "241\n", "sample 1"

assert solution("""100 1 25 50
15 5 20 10
""") == "200\n", "sample 2"

# minimum dough, only plain buns
assert solution("""1 1 1 7
1 1 1 1
""") == "7\n", "minimum size"

# stuffing unavailable for a full bun
assert solution("""5 1 1 1
3 4 10 100
""") == "5\n", "cannot use stuffing"

# all choices have the same value
assert solution("""10 2 2 2
10 2 2 4
10 2 2 4
""") == "20\n", "equal values"

# leftover dough and stuffing boundary
assert solution("""7 1 3 5
4 4 2 10
""") == "15\n", "boundary handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp bột tối thiểu | 7 | Xử lý lượng tài nguyên nhỏ nhất. | 
| Trường hợp nhồi không đủ | 5 | Ngăn chặn việc tạo ra những chiếc bánh nhồi không thể thực hiện được. | 
| Lựa chọn có giá trị bằng nhau | 20 | Kiểm tra xem tất cả các chuyển đổi tương đương có được xem xét hay không. | 
| Trường hợp tài nguyên còn sót lại | 15 | Kiểm tra phép tính plain-bun cuối cùng và phép chia số nguyên. | 

## Vỏ cạnh 

Đối với trường hợp có nhân nhồi nhưng không tạo được bánh nhồi:```
5 1 1 1
3 4 10 100
```DP thấy rằng loại nhân chỉ có thể làm ra những chiếc bánh không vì mỗi chiếc bánh nhồi cần 10 viên bột trong khi chỉ có 5 viên. Trạng thái duy nhất có thể truy cập là`dp[0] = 0`. Bước cuối cùng thêm năm chiếc bánh đơn giản, tạo thành 5 chiếc. 

Đối với trường hợp lựa chọn lợi nhuận tham lam không thành công:```
10 1 10 100
5 5 4 100
```Thuật toán không quyết định cục bộ. Nó có cả hai khả năng: dành bột cho bánh nhân và tiết kiệm bột cho bánh thường. Nó đánh giá sự kết hợp cuối cùng và phát hiện ra rằng hai chiếc bánh nhồi cho 200, tốt hơn một chiếc bánh bao thường. 

Đối với trường hợp tài nguyên còn sót lại:```
7 1 3 5
4 4 2 10
```Quá trình chuyển đổi nhồi tạo ra các trạng thái để tạo ra một chiếc bánh bao không hoặc một chiếc bánh bao. Hai chiếc bánh nhồi là không thể vì chỉ có bốn đơn vị nhân. Trạng thái có một chiếc bánh nhồi sử dụng hai miếng bột và kiếm được 10. Còn lại 5 miếng bột, tạo ra một chiếc bánh mì trơn cho 5 chiếc bánh khác, đưa ra câu trả lời cuối cùng là 15.
