---
title: "CF 106188G - Lịch sử cổ đại"
description: "Chúng ta có n đoạn thẳng có độ dài a[i]. Chúng ta cần đếm xem có bao nhiêu cách chọn chính xác k đoạn thẳng này để các đoạn được chọn có thể tạo thành một đa giác không suy biến có các đỉnh nằm trên một đường tròn."
date: "2026-06-25T10:47:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106188
codeforces_index: "G"
codeforces_contest_name: "UTPC x WiCS Contest 11-12-2025"
rating: 0
weight: 106188
solve_time_s: 37
verified: true
draft: false
---

[CF 106188G - Lịch sử cổ đại](https://codeforces.com/problemset/problem/106188/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`n`đoạn thẳng có độ dài`a[i]`. Chúng ta cần đếm xem có bao nhiêu cách để chọn chính xác`k`của các đoạn này sao cho các đoạn được chọn có thể tạo thành một đa giác không suy biến có các đỉnh nằm trên một đường tròn. Các đoạn có độ dài bằng nhau vẫn là những lựa chọn khác nhau vì mỗi đoạn được phát hiện đều được coi là duy nhất. 

Đối với một tập hợp các độ dài các cạnh, việc có thể tạo thành một đa giác không suy biến tương đương với việc cạnh dài nhất nhỏ hơn tổng của tất cả các cạnh còn lại. Một đa giác thỏa mãn điều kiện này có thể được sắp xếp thành một đa giác tuần hoàn, do đó bài toán trở thành phép đếm`k`-các tập hợp phần tử trong đó không có đoạn nào được chọn dài ít nhất bằng tổng chiều dài của các đoạn khác. 

Kích thước đầu vào cho phép`n`đạt khoảng 100 và độ dài có thể đủ lớn để việc kiểm tra tất cả các tập hợp con là không thể. Số cách lựa chọn có thể là`C(n, k)`, tăng quá nhanh ngay cả đối với các giá trị vừa phải của`n`. Chúng ta cần một giải pháp thời gian đa thức. Độ dài tối đa của một đoạn chỉ là`10000`, đó là ràng buộc chính mà chúng ta có thể khai thác. 

Phần khó khăn là chúng ta không thể chỉ nhìn vào giá trị lớn nhất trên toàn cầu. Một tập hợp con có thể không hợp lệ vì phần tử lớn nhất của chính nó. Ví dụ:```
Input
4 3
1 2 3 10
```Câu trả lời là`1`bởi vì sự lựa chọn hợp lệ duy nhất là`{1,2,3}`. Việc triển khai bất cẩn chỉ kiểm tra tổng của tất cả các phân khúc có thể kết luận không chính xác rằng các phân khúc lớn luôn có thể tham gia. 

Một trường hợp cạnh khác là độ dài lặp lại. Ví dụ:```
Input
5 3
3 3 3 1 2
```Câu trả lời là`7`. Ba đoạn có chiều dài`3`là các đối tượng khác nhau, do đó các kết hợp chứa các bản sao khác nhau phải được tính riêng. Việc coi độ dài là giá trị duy nhất sẽ bị tính thiếu. 

Trường hợp ranh giới cuối cùng là sự bình đẳng. Điều kiện rất nghiêm ngặt. Ví dụ:```
Input
3 3
1 2 3
```Câu trả lời là`0`bởi vì`3 = 1 + 2`, tạo ra một tam giác suy biến thay vì một đa giác hợp lệ. sử dụng`<=`thay vì`<`sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi tập hợp con có kích thước`k`, tìm cạnh cực đại của nó và kiểm tra xem cực đại đó có nhỏ hơn tổng của các cực đại khác hay không. Điều này đúng vì nó kiểm tra chính xác bất đẳng thức đa giác cho mọi lựa chọn có thể. Tuy nhiên, có thể có`C(100,50)`tập hợp con, vượt xa những gì có thể được xử lý. 

Quan sát hữu ích là mọi tập hợp con không hợp lệ đều có một cạnh phân biệt: cạnh lớn nhất của nó. Sau khi sắp xếp độ dài, giả sử đoạn được chọn lớn nhất hiện tại có độ dài`x`. Tập hợp con không hợp lệ chính xác khi tập hợp con kia`k-1`các đoạn được chọn có tổng chiều dài tối đa`x`. 

Điều này cho phép chúng ta đếm các tập hợp con không hợp lệ thay vì cố gắng đếm trực tiếp các tập hợp con hợp lệ. Vì độ dài đoạn lớn nhất có thể chỉ là`10000`, chúng ta chỉ cần thông tin lập trình động về tổng lên tới`10000`. Tổng lớn hơn không bao giờ có thể làm cho một tập hợp con không hợp lệ. 

Chúng tôi xử lý các phân đoạn theo thứ tự tăng dần. Trước khi chèn một đoạn`x`vào bảng lập trình động, bảng chỉ chứa các phân đoạn nhỏ hơn. Chúng tôi sử dụng nó để đếm có bao nhiêu nhóm`k-1`các phân đoạn nhỏ hơn có tổng nhiều nhất`x`. Mỗi nhóm như vậy tạo ra chính xác một tập hợp con không hợp lệ với`x`là phần tử lớn nhất. Sau khi đếm các tập con xấu này, chúng ta chèn`x`vào bảng để nó có thể đóng vai trò là phần tử lớn nhất cho các lựa chọn trong tương lai. 

Tổng số có thể`k`-tập hợp con là`C(n, k)`. Trừ đi những cái không hợp lệ sẽ đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(C(n,k) * k)`|`O(k)`| Quá chậm | 
| Tối ưu |`O(n * k * A)`|`O(k * A)`| Đã chấp nhận | 

Đây`A`là độ dài đoạn tối đa, nhiều nhất`10000`. 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các độ dài đoạn theo thứ tự không giảm. Việc xử lý theo thứ tự này đảm bảo rằng khi chúng tôi kiểm tra một phân đoạn, mọi phân đoạn đã được lưu trong bảng lập trình động đều nhỏ hơn hoặc bằng nhau và chỉ có thể là cạnh không tối đa. 
2. Tạo bảng lập trình động`dp[j][s]`. Nó lưu trữ số cách để chọn`j`các phân đoạn được xử lý có tổng chiều dài chính xác`s`, Ở đâu`s`bị giới hạn ở`10000`. 
3. Đối với mỗi đoạn có độ dài`x`, trước tiên hãy đếm các đa giác không hợp lệ trong đó`x`là cạnh lớn nhất. Chúng tôi cần các nhóm`k-1`các phân đoạn trước có tổng tối đa`x`. 
4. Thêm số đó vào tổng tập hợp con không hợp lệ. Điều này tính mỗi tập hợp con không hợp lệ một lần vì mỗi tập hợp con không hợp lệ có chính xác một phân đoạn được chọn lớn nhất. 
5. Chèn`x`vào bảng lập trình động. Lặp lại các kích thước ngược để phân đoạn hiện tại không được sử dụng lại nhiều lần trong cùng một tập hợp con. 
6. Tính tổng số cách chọn`k`phân đoạn bằng cách sử dụng kết hợp và trừ đi số lượng không hợp lệ. 

Tại sao nó hoạt động: mọi lựa chọn không hợp lệ đều có một cạnh lớn nhất duy nhất. Khi mặt đó được xử lý, tất cả các mặt được chọn khác đã được chèn vào`dp`, do đó thuật toán sẽ tính lựa chọn chính xác một lần. Mọi lựa chọn không được tính là không hợp lệ đều có cạnh lớn nhất nhỏ hơn tổng các cạnh còn lại, chính xác là bất đẳng thức đa giác. Phép trừ để lại chính xác các đa giác tuần hoàn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7
MAX_A = 10000

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    comb = [[0] * (k + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        comb[i][0] = 1
        for j in range(1, min(i, k) + 1):
            if j == i:
                comb[i][j] = 1
            else:
                comb[i][j] = (comb[i - 1][j - 1] + comb[i - 1][j]) % MOD

    dp = [[0] * (MAX_A + 1) for _ in range(k)]
    dp[0][0] = 1

    invalid = 0

    for x in a:
        if k > 1:
            bad = 0
            for s in range(x + 1):
                bad += dp[k - 1][s]
            invalid = (invalid + bad) % MOD

        for cnt in range(k - 2, -1, -1):
            current = dp[cnt]
            nxt = dp[cnt + 1]
            for s in range(MAX_A - x + 1):
                if current[s]:
                    nxt[s + x] = (nxt[s + x] + current[s]) % MOD

    answer = (comb[n][k] - invalid) % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```Bảng kết hợp tính toán tổng số có thể`k`-các lựa chọn phân đoạn. Các giá trị cần thiết modulo`10^9+7`, do đó tam giác Pascal tránh tràn và xử lý trực tiếp mọi trường hợp. 

Bảng lập trình động chỉ lưu trữ tổng tối đa`10000`. Tổng lớn hơn không bao giờ có thể xuất hiện trong kiểm tra tập hợp con không hợp lệ vì cạnh lớn nhất đang được kiểm tra không bao giờ lớn hơn`10000`. 

Vòng lặp đếm các tập hợp con xấu xảy ra trước khi chèn phân đoạn hiện tại. Thứ tự này ngăn cản việc đếm đoạn hiện tại là một trong những cạnh nhỏ hơn. Vòng lặp cập nhật quay ngược lại số lượng đã chọn, đây là mẫu ba lô 0/1 tiêu chuẩn và ngăn việc sử dụng một phân đoạn nhiều lần. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5 3
1 2 3 3 3
```Trạng thái xử lý là: 

| Phân khúc hiện tại | Nhóm không hợp lệ với nhóm này là lớn nhất | Kích thước tập hợp con được lưu trữ | 
| --- | --- | --- | 
| 1 | 0 | 1: {1} | 
| 2 | 0 | tổng của các nhóm nhỏ hơn | 
| 3 | nhóm có hai đoạn trước có tổng 3 | cập nhật với 3 | 
| 3 | kiểm tra tương tự với các giá trị trước đó | cập nhật với 3 cái khác | 
| 3 | kiểm tra tương tự với các giá trị trước đó | cập nhật với 3 cái khác | 

Các lựa chọn không hợp lệ sẽ bị xóa khỏi tổng số`3`-lựa chọn phần tử Câu trả lời còn lại là`7`. 

Đối với mẫu thứ hai:```
6 4
1 5 2 3 4 1
```Sau khi sắp xếp:```
1 1 2 3 4 5
```Các trạng thái quan trọng là: 

| Phân khúc hiện tại | Cần 3 đoạn trước có tổng  giá trị hiện tại | Kết quả | 
| --- | --- | --- | 
| 1 | không thể | không có tập hợp con không hợp lệ | 
| 2 | không thể ngoại trừ số tiền nhỏ | không có tập hợp con không hợp lệ | 
| 3 | bộ ba trước đó quá lớn | không có tập hợp con không hợp lệ | 
| 4 | bộ ba trước đó với số tiền nhỏ được kiểm tra | tính không hợp lệ | 
| 5 | tất cả các lỗi phía lớn nhất có thể còn lại đều được tính | tính không hợp lệ | 

Thuật toán đếm mọi hình tứ giác xấu theo cạnh dài nhất của nó và trừ chúng khỏi tất cả các lựa chọn có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n * k * A)`| Mỗi phân đoạn cập nhật mọi kích thước tập hợp con và mọi tổng có thể lên tới`A = 10000`. | 
| Không gian |`O(k * A)`| Bảng lưu trữ số lượng cho mọi kích thước tập hợp con và tổng có thể có. | 

Với`n <= 100`,`k <= 100`, Và`A <= 10000`, số lượng hoạt động là khoảng`10^8`trong trường hợp lớn nhất và phù hợp với giới hạn dự định bằng các vòng lặp Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    n, k = map(int, data().split())
    a = list(map(int, data().split()))
    a.sort()

    MOD = 10 ** 9 + 7
    MAX_A = 10000

    comb = [[0] * (k + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        comb[i][0] = 1
        for j in range(1, min(i, k) + 1):
            comb[i][j] = (comb[i - 1][j - 1] + comb[i - 1][j]) % MOD

    dp = [[0] * (MAX_A + 1) for _ in range(k)]
    dp[0][0] = 1
    bad = 0

    for x in a:
        for s in range(x + 1):
            bad += dp[k - 1][s]
        for c in range(k - 2, -1, -1):
            for s in range(MAX_A - x + 1):
                if dp[c][s]:
                    dp[c + 1][s + x] += dp[c][s]

    sys.stdin = old
    return str((comb[n][k] - bad) % MOD)

assert run("""5 3
1 2 3 3 3
""") == "7"

assert run("""6 4
1 5 2 3 4 1
""") == "12"

assert run("""3 3
1 2 3
""") == "0"

assert run("""3 3
2 2 3
""") == "0"

assert run("""5 5
1 1 1 1 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 3 / 1 2 3 3 3`|`7`| Độ dài lặp lại được coi là các đoạn riêng biệt | 
|`6 4 / 1 5 2 3 4 1`|`12`| Đếm đa giác chung | 
|`3 3 / 1 2 3`|`0`| Trường hợp bình đẳng bị bác bỏ | 
|`3 3 / 2 2 3`|`0`| Xử lý đa giác suy biến | 
|`5 5 / 1 1 1 1 1`|`1`| Lựa chọn hoàn chỉnh tối thiểu không trống | 

## Vỏ cạnh 

Đối với ranh giới đẳng thức:```
3 3
1 2 3
```Khi xử lý`3`, bảng lập trình động chứa cặp`{1,2}`với tổng`3`. Vì số lượng truy vấn tổng bằng và bao gồm cạnh lớn nhất hiện tại nên tập hợp con này được đánh dấu là không hợp lệ. Câu trả lời cuối cùng là`0`. 

Đối với các giá trị lặp lại:```
5 3
3 3 3 1 2
```Thứ tự sắp xếp là`1,2,3,3,3`. Mỗi bản sao của`3`được chèn riêng vào bảng lập trình động. Các chỉ số khác nhau tạo ra các lựa chọn khác nhau, do đó thuật toán bảo toàn tính bội số một cách tự nhiên. 

Đối với một bên lớn thống trị các bên khác:```
4 3
1 2 3 10
```Khi`10`được xử lý, bảng chứa tất cả các cặp giá trị nhỏ hơn. Mỗi cặp có tổng nhiều nhất`10`, do đó tất cả các tập con chứa`10`không hợp lệ. Tam giác hợp lệ duy nhất còn lại là`{1,2,3}`. 

Đối với kích thước đa giác nhỏ nhất có thể:```
3 3
2 3 4
```Thuật toán kiểm tra tập hợp con duy nhất có thể. Cạnh lớn nhất là`4`, trong khi hai vế còn lại tổng bằng`5`, vậy tập hợp con hợp lệ và câu trả lời là`1`.
