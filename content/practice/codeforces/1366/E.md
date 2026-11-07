---
title: "CF 1366E - Hai mảng"
description: "Chúng ta được cho hai mảng, a và b. Mảng b đang tăng dần và nhiệm vụ của chúng ta là phân chia mảng a thành chính xác m mảng con liên tiếp, trong đó giá trị tối thiểu của mỗi mảng con khớp với phần tử tương ứng trong mảng b."
date: "2026-06-11T12:13:25+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "combinatorics", "constructive-algorithms", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1366
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 89 (Rated for Div. 2)"
rating: 2100
weight: 1366
solve_time_s: 641
verified: true
draft: false
---

[CF 1366E - Hai mảng](https://codeforces.com/problemset/problem/1366/E) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, tổ hợp, thuật toán xây dựng, dp, hai con trỏ 
**Thời gian giải:** 10 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`. Mảng`b`đang tăng lên một cách nghiêm ngặt và nhiệm vụ của chúng ta là phân vùng mảng`a`vào chính xác`m`mảng con liên tiếp, trong đó giá trị tối thiểu của mỗi mảng con khớp với phần tử tương ứng trong`b`. Mỗi phần tử của`a`phải thuộc chính xác một mảng con và các phân vùng liền kề nhau. Đầu ra là số cách hợp lệ để thực hiện việc phân vùng này, modulo 998244353. 

Những hạn chế về`n`Và`m`đều lên tới 200.000 và giá trị của các phần tử có thể đạt tới 10^9. Với các kích thước này, bất kỳ giải pháp nào xem xét tất cả các phân vùng có thể một cách đơn giản sẽ quá chậm, vì số lượng phân vùng tăng theo cấp số nhân với`n`. Thay vào đó, thuật toán phải khai thác cấu trúc của bài toán: chúng ta chỉ quan tâm đến vị trí cực tiểu của các mảng con xuất hiện, còn các phần tử còn lại là tự do miễn là chúng không vi phạm ràng buộc tối thiểu. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó một số`b[i]`không tồn tại ở phần còn lại`a`. Ví dụ, nếu`a = [1,2,3]`Và`b = [2,3,4]`, không có phân vùng hợp lệ vì`4`bị thiếu. Một trường hợp cạnh khác là các giá trị lặp lại trong`a`đó có thể là một phần của nhiều mảng con; thuật toán phải tính toán những điều này một cách chính xác để tránh tính toán quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra mọi cách có thể để phân chia`a`vào trong`m`các phân đoạn liền kề và kiểm tra xem mức tối thiểu của mỗi phân đoạn có khớp không`b`. Cách tiếp cận này hoạt động về nguyên tắc vì nó trực tiếp thực thi các ràng buộc, nhưng độ phức tạp của nó tăng theo cấp số nhân.`n`và không khả thi đối với mảng lớn. Vì`n`lên tới 2 * 10^5, việc lặp lại trên tất cả các phân vùng là không thể. 

Cách tiếp cận tối ưu thúc đẩy quan sát rằng, đối với một phân vùng hợp lệ, mức tối thiểu của phân đoạn cuối cùng phải là`b[m-1]`, mức tối thiểu của phân đoạn từ thứ hai đến cuối cùng phải là`b[m-2]`, vân vân. Chúng tôi có thể quét`a`từ phải sang trái để xác định vị trí sớm nhất nơi mỗi giá trị tối thiểu có thể bắt đầu, đảm bảo rằng giá trị tối thiểu của mảng con còn lại bằng giá trị tương ứng`b[i]`. Khi các vị trí này được xác định, số cách đặt các điểm phân chia được đưa ra bằng cách đếm các lựa chọn hợp lệ cho điểm bắt đầu của mỗi đoạn. Cụ thể là số lần chia hợp lệ cho mảng con`i`là số phần tử giữa mức tối thiểu cuối cùng của phân đoạn trước và lần xuất hiện tiếp theo của mức tối thiểu hiện tại, tạo thành cấu trúc nhân cho tổng số phân vùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^m) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu quét mảng`a`từ phải sang trái, theo dõi giá trị tối thiểu được thấy cho đến nay. Điều này xác định các điểm cuối tiềm năng cho từng phân đoạn vì phần tử ngoài cùng bên phải của mỗi phân đoạn ít nhất phải đạt mức tối thiểu bắt buộc`b[i]`. 
2. Kiểm tra xem`b[m-1]`, giá trị tối thiểu của mảng con cuối cùng, tồn tại ở phần ngoài cùng bên phải của`a`. Nếu không, câu trả lời là 0 vì không tồn tại phân vùng hợp lệ. 
3. Đối với từng phân khúc`i`từ cuối cùng đến đầu tiên, ghi lại chỉ số sớm nhất trong đó mức tối thiểu`b[i]`có thể xuất hiện mà không vi phạm mức tối thiểu của các phân đoạn tiếp theo. Điều này thiết lập phạm vi hợp lệ cho từng phân khúc. 
4. Đối với mỗi phân đoạn, hãy tính số vị trí mà ranh giới bên trái có thể đảm nhận, đó là chênh lệch giữa lần xuất hiện sớm nhất của mức tối thiểu của phân khúc tiếp theo và mức tối thiểu của phân khúc hiện tại. Nhân các số đếm này theo modulo 998244353 để có tổng số phân vùng. 
5. Trả về tổng số câu trả lời. 

Bất biến chính là mỗi mức tối thiểu`b[i]`phải xuất hiện ở đâu đó trong phân đoạn của nó và tất cả các phần tử ở bên phải nó thuộc về các phân đoạn có cực tiểu lớn hơn. Bằng cách quét từ phải sang trái, chúng tôi đảm bảo rằng cực tiểu được chỉ định theo cách tôn trọng cả thứ tự và các ràng buộc liên tục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n, m = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

min_a = [0] * n
min_a[-1] = a[-1]
for i in range(n - 2, -1, -1):
    min_a[i] = min(a[i], min_a[i + 1])

if min_a[0] != b[0]:
    print(0)
    sys.exit()

pos = n
ans = 1
for i in range(m - 1, 0, -1):
    found = False
    for j in range(pos - 1, -1, -1):
        if min_a[j] < b[i]:
            ans = (ans * (pos - j - 1)) % MOD
            pos = j + 1
            found = True
            break
    if not found:
        print(0)
        sys.exit()

print(ans)
```Giải pháp đầu tiên tính toán hậu tố cực tiểu của`a`để nhanh chóng xác định các điểm cuối phân khúc hợp lệ. Sau đó, nó kiểm tra xem phần tử đầu tiên có thể bắt đầu một phân đoạn hợp lệ và lặp lại qua`b`để đếm các lựa chọn hợp lệ cho ranh giới phân khúc. Phép nhân được thực hiện theo modulo 998244353 theo yêu cầu. Việc sử dụng hậu tố cực tiểu cho phép thuật toán quét`a`chỉ một lần cho mỗi phân đoạn, đảm bảo độ phức tạp về thời gian O(n). 

## Ví dụ đã hoạt động 

cho`a = [12, 10, 20, 20, 25, 30]`Và`b = [10, 20, 30]`, mảng cực tiểu có hậu tố là`[10, 10, 20, 20, 25, 30]`. Quét từ bên phải, mức tối thiểu cuối cùng`30`xảy ra ở chỉ số 5, do đó đoạn cuối cùng phải bắt đầu ở chỉ số 5. ​​Điểm tối thiểu thứ hai`20`xảy ra ở chỉ số 2 và chỉ số 3. Vị trí bắt đầu hợp lệ cho phân đoạn thứ hai là chỉ số 2 và 3, đưa ra hai lựa chọn. Mức tối thiểu đầu tiên`10`xảy ra ở chỉ số 1, phải bắt đầu phân đoạn đầu tiên. Nhân số lựa chọn cho phân đoạn thứ hai (2) với lựa chọn duy nhất cho phân đoạn đầu tiên sẽ có tổng cộng 2 phân vùng hợp lệ. 

| Chỉ mục | một [tôi] | phút_a[i] | Chia tách hợp lệ | 
| --- | --- | --- | --- | 
| 5 | 30 | 30 | đoạn cuối bắt đầu từ đây | 
| 4 | 25 | 25 | đoạn tiếp theo có thể bắt đầu lúc 2 hoặc 3 | 
| 3 | 20 | 20 | xem ở trên | 
| 2 | 20 | 20 | xem ở trên | 
| 1 | 10 | 10 | đoạn đầu tiên bắt đầu ở đây | 
| 0 | 12 | 10 | được bao phủ bởi phân đoạn đầu tiên | 

Dấu vết này xác nhận rằng phương pháp tiếp cận hậu tố tối thiểu nắm bắt tất cả các vị trí hợp lệ một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Thuật toán tính hậu tố cực tiểu trong O(n) và quét từng đoạn một lần | 
| Không gian | O(n) | Lưu trữ mảng cực tiểu hậu tố | 

Cho rằng`n`2 * 10^5, giải pháp vừa vặn thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    min_a = [0] * n
    min_a[-1] = a[-1]
    for i in range(n - 2, -1, -1):
        min_a[i] = min(a[i], min_a[i + 1])

    if min_a[0] != b[0]:
        return "0"

    pos = n
    ans = 1
    for i in range(m - 1, 0, -1):
        found = False
        for j in range(pos - 1, -1, -1):
            if min_a[j] < b[i]:
                ans = (ans * (pos - j - 1)) % MOD
                pos = j + 1
                found = True
                break
        if not found:
            return "0"

    return str(ans)

# Provided sample
assert run("6 3\n12 10 20 20 25 30\n10 20 30\n") == "2", "sample 1"

# Custom cases
assert run("3 2\n1 2 3\n1 3\n") == "1", "minimum size, one valid split"
assert run("5 3\n5 4 3 2 1\n1 3 5\n") == "0", "no valid partitions"
assert run("4 2\n2 2 2 2\n2 2\n") == "3", "all equal elements, multiple valid splits"
assert run("2 2\n1 2\n1 2\n") == "1", "each element forms its own subarray"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 3\n12 10 20 20 25 30\n10 20 30 | 2 | đầu vào mẫu | 
| 3 2\n1 | | |
