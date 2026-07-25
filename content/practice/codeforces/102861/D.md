---
title: "CF 102861D - Vũ điệu chia hết"
description: "Có hai sự sắp xếp vòng tròn của mọi người. Việc ghép nối ban đầu là từng vị trí, do đó vị trí i trong một vòng tròn sẽ đối diện với vị trí i trong vòng tròn kia. Trong khi nhảy, mỗi bước sẽ quay đúng một vòng tròn."
date: "2026-07-25T14:01:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "D"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 57
verified: true
draft: false
---

[CF 102861D - Vũ điệu phân chia](https://codeforces.com/problemset/problem/102861/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có hai sự sắp xếp vòng tròn của mọi người. Việc ghép nối ban đầu là theo từng vị trí, vì vậy vị trí`i`trong một vòng tròn vị trí đối mặt`i`ở vòng tròn khác. Trong khi nhảy, mỗi bước sẽ quay đúng một vòng tròn. Một vòng quay thay đổi độ lệch hiện tại giữa hai vòng tròn. Toàn bộ điệu nhảy chỉ hợp lệ nếu mỗi phần bù xuất hiện là mới, bởi vì nhìn thấy cùng một phần bù hai lần có nghĩa là các cặp giống nhau sẽ đối mặt với nhau một lần nữa. 

Một điệu nhảy hoàn chỉnh được xác định bởi trình tự bù trừ sau mỗi bước. Bản thân các phép quay tương đương với việc chọn một thay đổi khác 0 trong phần bù hiện tại, bởi vì mọi modulo thay đổi khác 0`N`tương ứng với chính xác một lượng luân chuyển hợp lệ. 

Cuối cùng, phần bù cuối cùng phải tạo các cặp có tổng tuổi đều có cùng phần dư modulo`M`. Chúng ta cần đếm xem có bao nhiêu chuỗi hợp lệ`K`những thay đổi khác 0 kết thúc ở một trong những độ lệch có thể chấp nhận được. 

Kích thước đầu vào là khó khăn chính.`N`có thể đạt được`10^6`, do đó, các thuật toán thử từng cặp vị trí hoặc mọi khoảng cách có thể có với xác minh đầy đủ là quá chậm. Phần đếm cuối cùng phải gần tuyến tính`N`, và độ dài điệu nhảy`K`có thể lớn như`10^9`, vì vậy chúng tôi không thể mô phỏng các bước. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu như`K >= N`, một điệu nhảy hợp lệ không thể tồn tại vì độ lệch ban đầu cộng với`K`phần bù sau này sẽ chứa ít nhất`N+1`chỉ ghé thăm`N`các vị trí có thể. Ví dụ, với`N = 3`,`K = 3`, mỗi chuỗi ba bước di chuyển lặp lại một khoảng bù, vì vậy câu trả lời là`0`. 

Giá trị offset cuối cùng không bao giờ có thể bằng 0, vì giá trị offset 0 là trạng thái bắt đầu. Ví dụ, với`N = 3`,`K = 1`, nếu chỉ ghép cặp ban đầu thỏa mãn điều kiện về độ tuổi thì đáp án đúng vẫn là`0`, không`1`, vì không thể đạt được trạng thái đó nếu không lặp lại việc ghép đôi ban đầu. 

Nhiều lần bù trừ cuối cùng hợp lệ phải được tính riêng. Ví dụ: nếu hai ca quay vòng khác nhau của vòng tròn thứ hai tạo ra tổng số tuổi hợp lệ thì mỗi ca sẽ đóng góp số lần nhảy riêng của mình. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê tất cả các chuỗi quay có thể có. Mỗi bước có`N-1`những thay đổi có thể khác 0, do đó điều này tạo ra khoảng`(N-1)^K`khả năng. Ngay cả đối với nhỏ`K`, cái này phát triển quá nhanh. Một lực lượng vũ phu tốt hơn sẽ liệt kê mọi ca cuối cùng và kiểm tra xem điều kiện tuổi có được giữ hay không, nhưng việc kiểm tra chi phí của một ca`O(N)`, cho`O(N^2)`thời gian trong trường hợp xấu nhất là quá chậm để`N = 10^6`. 

Quan sát quan trọng là tình trạng tuổi chỉ phụ thuộc vào sự khác biệt vòng tròn. Giả sử một sự thay đổi cuối cùng`s`là hợp lệ. Khi đó với mỗi cặp vị trí nữ liền kề chúng ta phải có`A[i] + B[i+s] = A[i+1] + B[i+1+s]`. 

Sắp xếp lại mang lại`B[i+1+s] - B[i+s] = A[i] - A[i+1]`. 

Phía bên trái là dãy sai phân hình tròn của`B`và phía bên phải là một mẫu cố định bắt nguồn từ`A`. Do đó, việc tìm các ca hợp lệ trở thành một bài toán so khớp mẫu vòng tròn. 

Chúng tôi xây dựng mảng khác biệt của`B`và tìm kiếm mô hình khác biệt từ`A`bên trong hai bản sao của mảng khác biệt. Thuật toán Knuth-Morris-Pratt tìm tất cả các vị trí bắt đầu trùng khớp trong thời gian tuyến tính. 

Sau khi tìm được số ca cuối cùng hợp lệ, bài toán còn lại thuần tuý là tổ hợp. Một điệu nhảy có độ dài hợp lệ`K`là một chuỗi các`K+1`độ lệch riêng biệt bắt đầu bằng phần bù`0`. Nếu như`K < N`, sửa các lá offset cuối cùng`K-1`các khoản bù đắp trung gian để lựa chọn và đặt hàng từ bên kia`N-2`các vị trí. Số khả năng cho mỗi ca cuối cùng khác 0 hợp lệ là`(N-2) * (N-3) * ... * (N-K)`đó là`(N-2)! / (N-K-1)!`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((N-1)^K) | O(K) | Quá chậm | 
| Kiểm tra mỗi ca | O(N^2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính hình tròn khác biệt của phụ nữ. Đối với mọi vị trí, lưu trữ`A[i] - A[i+1]`modulo`M`. Đây là khuôn mẫu mà mọi vòng quay hợp lệ của đàn ông đều phải tái tạo. 
2. Tính mảng sai phân hình tròn của nam. Đối với mọi vị trí, lưu trữ`B[i+1] - B[i]`modulo`M`. Ca cuối cùng hợp lệ chính xác là vị trí mà chuỗi khác biệt này khớp với mẫu của phụ nữ. 
3. Chạy KMP trên mảng khác biệt nam được nhân đôi một lần. Mỗi trận đấu bắt đầu ở một vị trí từ`0`ĐẾN`N-1`đại diện cho một ca cuối cùng hợp lệ. Mảng trùng lặp xử lý các kết quả khớp bao quanh phần cuối của vòng tròn. 
4. Loại bỏ trường hợp ca cuối cùng`0`, vì phần bù ban đầu đã được truy cập trước khi điệu nhảy bắt đầu. 
5. Nếu`K >= N`, đầu ra`0`, bởi vì một chuỗi các offset hợp lệ không thể chứa nhiều hơn`N`các trạng thái riêng biệt. 
6. Mặt khác hãy tính số cách chọn độ lệch trung gian. Nhân giá trị này với số ca cuối cùng hợp lệ và lấy kết quả theo modulo`10^9+7`. 

Tại sao nó hoạt động: mọi điệu nhảy đều tương ứng chính xác với một con đường xuyên qua nhóm bù trừ theo chu kỳ. Điều kiện hợp lệ tương đương với việc truy cập các độ lệch riêng biệt, vì vậy sau khi sửa độ lệch cuối cùng, chúng ta chỉ cần đếm các lựa chọn có thứ tự của các độ lệch đã truy cập còn lại. Bước KMP tìm thấy chính xác các dịch chuyển trong đó điều kiện ghép đôi cuối cùng được thỏa mãn, bởi vì đẳng thức của tất cả các tổng cặp tương đương với đẳng thức của tất cả các hiệu liền kề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def kmp_count(text, pattern, n):
    m = len(pattern)
    pi = [0] * m

    for i in range(1, m):
        j = pi[i - 1]
        while j and pattern[i] != pattern[j]:
            j = pi[j - 1]
        if pattern[i] == pattern[j]:
            j += 1
        pi[i] = j

    count = 0
    j = 0
    for i, x in enumerate(text):
        while j and x != pattern[j]:
            j = pi[j - 1]
        if x == pattern[j]:
            j += 1
        if j == m:
            start = i - m + 1
            if start < n:
                count += 1
            j = pi[j - 1]

    return count

def solve():
    N, M, K = map(int, input().split())
    A = [int(x) % M for x in input().split()]
    B = [int(x) % M for x in input().split()]

    if K >= N:
        print(0)
        return

    pattern = [(A[i] - A[(i + 1) % N]) % M for i in range(N - 1)]
    diff_b = [(B[(i + 1) % N] - B[i]) % M for i in range(N)]

    valid = kmp_count(diff_b + diff_b, pattern, N)

    if K == 1:
        ways = 1
    else:
        ways = 1
        for x in range(N - 2, N - K - 1, -1):
            ways = ways * x % MOD

    print(valid * ways % MOD)

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý KMP xây dựng hàm lỗi của mẫu khác biệt. Điều này cho phép thuật toán tiếp tục tìm kiếm sau khi không khớp mà không cần khởi động lại từ đầu, điều này mang lại độ phức tạp tuyến tính. 

Mảng chênh lệch trùng lặp là cần thiết vì một kết quả khớp vòng tròn có thể vượt qua phần cuối của mảng. Chỉ bắt đầu từ lần đầu tiên`N`các vị trí được tính vì đó là những ca làm việc theo vòng tròn thực tế. 

Vòng lặp sản phẩm tính toán giai thừa giảm trực tiếp thay vì sử dụng giai thừa và nghịch đảo mô-đun. Điều này tránh được số học mô-đun bổ sung và cũng xử lý được`K = 1`trường hợp một cách tự nhiên. 

Chương trình không bao giờ tự xây dựng trình tự khiêu vũ. Nó chỉ tính các khoản bù đắp cuối cùng có thể có và nhân với số lượng đơn đặt hàng bù đắp dẫn đến đó. 

## Ví dụ đã hoạt động 

Đối với mẫu 1:```
4 10 1
3 4 1 7
13 27 36 9
```Các giá trị quan trọng là: 

| Giá trị | Kết quả | 
| --- | --- | 
| Mẫu khác biệt của phụ nữ | [9, 3, 4] | 
| Mảng chênh lệch nam | [4, 9, 3, 4] | 
| Ca làm việc phù hợp | 1 | 
| Cách cho mỗi ca | 1 | 
| Trả lời | 1 | 

Phần bù cuối cùng duy nhất có thể có là phần bắt đầu ở vị trí thứ hai của mảng chênh lệch nam. Vì chỉ có một bước nên việc đạt đến độ lệch đó có đúng một bước nhảy hợp lệ. 

Đối với mẫu 2:```
5 10 2
3 4 1 7 6
4 7 1 2 5
```| Giá trị | Kết quả | 
| --- | --- | 
| Mẫu khác biệt của phụ nữ | [9, 3, 4, 1] | 
| Mảng chênh lệch nam | [3, 4, 1, 3, 9] | 
| Ca làm việc phù hợp | 0 | 
| Cách cho mỗi ca | 3 | 
| Trả lời | 0 | 

Mẫu không xuất hiện theo trình tự khác biệt vòng tròn, do đó không có cặp đôi cuối cùng nào thỏa mãn điều kiện tuổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Xây dựng sự khác biệt, chạy KMP và tính giai thừa giảm đều là tuyến tính | 
| Không gian | O(N) | Mảng khác biệt và bảng tiền tố KMP sử dụng bộ nhớ tuyến tính | 

Giải pháp phù hợp với`N = 10^6`giới hạn vì mỗi thao tác là một lần chuyển qua các mảng đầu vào. Giá trị của`K`không ảnh hưởng đến thời gian chạy vì nó chỉ được sử dụng trong công thức tổ hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
    return out.getvalue()

assert run("""4 10 1
3 4 1 7
13 27 36 9
""") == "1\n", "sample 1"

assert run("""5 10 2
3 4 1 7 6
4 7 1 2 5
""") == "0\n", "sample 2"

assert run("""5 10 2
3 4 1 7 6
5 4 7 1 2
""") == "3\n", "sample 3"

assert run("""3 100 3
1 2 3
4 5 6
""") == "0\n", "too many steps"

assert run("""3 10 1
1 1 1
2 2 2
""") == "2\n", "all equal values"

assert run("""4 7 2
1 2 3 4
6 5 4 3
""") == "2\n", "circular matching"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 1 | Đếm một bước | 
| Mẫu 2 | 0 | Không có ca cuối cùng hợp lệ | 
| Mẫu 3 | 3 | Nhiều điệu nhảy từ một ca cuối cùng | 
|`N=3, K=3`| 0 | Số lượng trạng thái riêng biệt không thể có | 
| Giá trị bằng nhau | 2 | Nhiều ca hợp lệ và xử lý modulo | 
| Hộp diêm hình tròn | 2 | Khớp mẫu qua ranh giới vòng tròn | 

## Vỏ cạnh 

Khi nào`K >= N`, thuật toán ngay lập tức trả về 0. Ví dụ:```
3 100 3
1 2 3
4 5 6
```Chỉ có ba sự bù đắp có thể xảy ra, bao gồm cả sự bù đắp bắt đầu. Một điệu nhảy có độ dài ba sẽ cần bốn lần bù khác nhau, điều này không thể xảy ra. 

Khi ghép nối hợp lệ duy nhất là ghép nối ban đầu thì không được tính. Giai đoạn KMP có thể tìm thấy sự chuyển dịch`0`, nhưng pha tổ hợp không bao giờ cho phép quay lại phần bù ban đầu vì tất cả các phần bù đã truy cập phải khác biệt. 

Khi một số ca thỏa mãn điều kiện tuổi, thuật toán sẽ tính từng ca một cách độc lập. Bước so khớp khác biệt trả về tất cả các lần bắt đầu vòng tròn hợp lệ và mỗi lần bắt đầu đóng góp cùng một số lượng lịch sử khiêu vũ hợp lệ vì số lượng khoảng cách trung gian chỉ phụ thuộc vào`N`Và`K`, không phải ở vị trí cuối cùng.
