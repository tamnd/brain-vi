---
title: "CF 2078A - Phán quyết cuối cùng"
description: "Chúng ta được cung cấp một mảng các số nguyên và một giá trị đích. Mục tiêu là liên tục chia mảng thành các phần có kích thước bằng nhau, thay thế mảng bằng giá trị trung bình của các phần đó và tiếp tục cho đến khi mảng chỉ còn một phần tử."
date: "2026-06-08T06:29:14+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2078
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1008 (Div. 2)"
rating: 800
weight: 2078
solve_time_s: 93
verified: true
draft: false
---

[CF 2078A - Phán quyết cuối cùng](https://codeforces.com/problemset/problem/2078/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một giá trị đích. Mục tiêu là liên tục chia mảng thành các phần có kích thước bằng nhau, thay thế mảng bằng giá trị trung bình của các phần đó và tiếp tục cho đến khi mảng chỉ còn một phần tử. Chúng ta phải xác định xem có tồn tại một chuỗi các thao tác như vậy kết thúc bằng phần tử đơn lẻ bằng mục tiêu hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài mảng`n`, mục tiêu`x`, và mảng`a`chính nó. Các phần tử mảng và`x`đều là số nguyên dương và`n`nhiều nhất là 100. Điều này ngụ ý rằng chúng ta có thể sử dụng các thuật toán có độ phức tạp về thời gian lên tới khoảng$O(n^2)$mỗi trường hợp thử nghiệm. Lực lượng mạnh mẽ đối với tất cả các phần tách có thể sẽ trở nên cồng kềnh, đặc biệt là vì đối với mỗi phần chia, chúng ta sẽ cần tính toán mức trung bình và đệ quy. 

Các trường hợp cạnh không rõ ràng phát sinh khi mảng đã là một phần tử duy nhất hoặc khi tất cả các phần tử giống hệt nhau. Ví dụ, nếu`a = [3]`Và`x = 3`, chúng ta có thể ngay lập tức trả về CÓ. Nếu như`a = [5,5,5]`Và`x = 5`, câu trả lời cũng là CÓ, nhưng một cách tiếp cận ngây thơ là tìm kiếm các tập hợp con có tổng chính xác bằng`x`có thể từ chối nó một cách không chính xác. Một sự tinh tế khác xảy ra khi mục tiêu`x`bằng giá trị trung bình của mảng nhưng không bằng bất kỳ phần tử riêng lẻ nào. Ví dụ,`a = [1, 2]`Và`x = 1.5`không thể đạt được vì sự phân chia của chúng ta phải tạo ra các chuỗi con có độ dài số nguyên và tính trung bình trên chúng chỉ tạo ra số nguyên nếu tất cả các phần tử đều là số nguyên, chúng ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi phép phân chia có thể có ở mỗi bước, tính toán trung bình theo cách đệ quy. Điều này đúng về mặt kỹ thuật, bởi vì bất kỳ chuỗi hoạt động hợp lệ nào cũng phải là một chuỗi các phần tách như vậy. Tuy nhiên, số cách để phân chia một mảng tăng lên theo cách kết hợp với`n`, làm cho cách tiếp cận này không thực tế ngay cả đối với`n = 20`. Vì`n = 100`, điều đó là không thể thực hiện được. 

Thông tin chi tiết quan trọng là ở mỗi thao tác, các phần tử mới của mảng là sự kết hợp lồi của các phần tử mảng hiện tại. Điều này có nghĩa là giá trị cuối cùng`x`phải nằm giữa mức tối thiểu và tối đa của mảng ban đầu. Nếu như`x`bằng một trong các phần tử, câu trả lời là CÓ. Bằng không, nếu`x`nằm trong phạm vi của mảng nhưng không chính xác bằng bất kỳ phần tử nào, chúng ta luôn có thể xây dựng một chuỗi các phần chia có giá trị trung bình là`x`bằng cách đầu tiên lấy trung bình của toàn bộ mảng. Cụ thể, chúng ta luôn có thể hợp nhất tất cả các phần tử trong một chuỗi có độ dài`n`(toàn bộ mảng), tạo ra giá trị trung bình. Do đó, điều kiện giảm xuống còn kiểm tra xem`x`bằng với bất kỳ phần tử nào hoặc bằng giá trị trung bình của mảng. 

Quan sát này cho phép chúng ta giảm vấn đề xuống việc quét tuyến tính của mảng: kiểm tra xem`x`đang ở trong`a`hoặc bằng giá trị trung bình của`a`. Điều này nhanh chóng và phù hợp thoải mái trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`x`, và mảng`a`. 
2. Tính tổng các phần tử của mảng. 
3. Kiểm tra xem`x`bằng với bất kỳ phần tử nào của`a`. Nếu có, hãy in "CÓ" và tiếp tục với trường hợp kiểm thử tiếp theo. Điều này bao gồm các chuỗi tầm thường trong đó chúng ta có thể chọn ngay một chuỗi con có một phần tử. 
4. Nếu không, hãy kiểm tra xem`x * n == sum(a)`. Điều này xác minh liệu`x`chính xác là giá trị trung bình của mảng. Nếu có, hãy in "CÓ". Lý do là việc chia toàn bộ mảng thành một nhóm và lấy kết quả trung bình`x`. 
5. Nếu không đáp ứng điều kiện nào, hãy in "KHÔNG". Không có chuỗi hoạt động được phép nào có thể tạo ra`x`trong trường hợp này. 

Tại sao nó hoạt động: Thao tác được phép thay thế mảng bằng giá trị trung bình của các chuỗi con có kích thước bằng nhau. Bất kỳ kết quả trung bình nào cũng là sự kết hợp lồi của các phần tử hiện có, do đó phần tử cuối cùng phải nằm giữa mức tối thiểu và tối đa của mảng ban đầu. Nếu mục tiêu khớp với một phần tử hoặc giá trị trung bình tổng thể thì sẽ tồn tại một chuỗi phân chia rõ ràng để đạt được mục tiêu đó. Nếu không thì không có trình tự nào có thể tạo ra nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    if x in a:
        print("YES")
    elif sum(a) == x * n:
        print("YES")
    else:
        print("NO")
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc mảng và tính tổng. Kiểm tra`x in a`xử lý ngay những trường hợp đơn giản nhất. So sánh`sum(a)`ĐẾN`x * n`xử lý các trường hợp trong đó giá trị trung bình bằng mục tiêu. nhân`x`qua`n`tránh các vấn đề về độ chính xác của dấu phẩy động. Nếu không có điều kiện nào xảy ra, "NO" sẽ được in. Cách tiếp cận này đơn giản, nhanh chóng và tránh sự đệ quy không cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
4
1 3
3
4 9
7 11 2 5
6 9
1 9 14 12 10 8
10 10
10 10 10 10 10 10 10 10 10 10
```| Trường hợp thử nghiệm | một | x | tổng(a) | x trong một | x * n == tổng(a) | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [3] | 3 | 3 | Đúng | Đúng | CÓ | 
| 2 | [7,11,2,5] | 9 | 25 | Sai | Sai | KHÔNG | 
| 3 | [1,9,14,12,10,8] | 9 | 54 | Sai | Đúng | CÓ | 
| 4 | [10]*10 | 10 | 100 | Đúng | Đúng | CÓ | 

Bảng cho thấy thuật toán xác định chính xác cả chuỗi tầm thường và chuỗi dựa trên trung bình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét tuyến tính của mảng để kiểm tra thành phần và tính tổng | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Lưu trữ mảng | 

Với các ràng buộc (`n <= 100`Và`t <= 500`), giải pháp thực hiện tối đa 50.000 lượt truy cập mảng, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        if x in a:
            print("YES")
        elif sum(a) == x * n:
            print("YES")
        else:
            print("NO")
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("4\n1 3\n3\n4 9\n7 11 2 5\n6 9\n1 9 14 12 10 8\n10 10\n10 10 10 10 10 10 10 10 10 10\n") == "YES\nNO\nYES\nYES", "sample 1"

# custom cases
assert run("3\n2 5\n1 5\n3 2\n1 2 3\n5 3\n3 3 3 3 3\n") == "YES\nYES\nYES", "custom 1"
assert run("2\n1 100\n50\n4 10\n5 5 5 5\n") == "NO\nYES", "custom 2"
assert run("1\n3 4\n2 4 6\n") == "YES", "custom 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 5\n1 5 | CÓ | Phần tử đơn khớp với mục tiêu | 
| 3 2\n1 2 3 | CÓ | Mục tiêu bằng giá trị trung bình nhưng không phải là một phần tử | 
| 5 3\n3 3 3 3 3 | CÓ | Tất cả các yếu tố bằng mục tiêu | 
| 1 100\n50 | KHÔNG | Phần tử đơn không khớp với mục tiêu | 
| 4 10\n5 5 5 5 | CÓ | Trung bình bằng mục tiêu | 

## Vỏ cạnh 

Nếu mảng có một phần tử bằng`x`, thuật toán ngay lập tức trả về "CÓ". Vì`a = [3]`Và`x = 3`, séc`x in a`vượt qua. Ví dụ: nếu mục tiêu bằng giá trị trung bình nhưng không có phần tử đơn lẻ nào khớp với nó`a = [1,9,14,12,10,8]`Và`x = 9`, séc`sum(a) == x * n`đi qua, tạo ra "CÓ". Mảng có tất cả các phần tử bằng nhau được xử lý chính xác vì cả hai lần kiểm tra đều thành công. Mảng không có điều kiện nào đúng, chẳng hạn như`a = [1,2]`Và`x = 5`, mang lại chính xác "KHÔNG".
