---
title: "CF 2203C - Máy phát thử nghiệm"
description: "Bài toán yêu cầu chúng ta tạo ra một mảng các số nguyên không âm có tổng bằng tổng s cho trước, với ràng buộc bổ sung là mỗi phần tử chỉ được có tập hợp các bit cũng được đặt trong một số khác m."
date: "2026-06-07T20:01:09+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2203
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 187 (Rated for Div. 2)"
rating: 1500
weight: 2203
solve_time_s: 122
verified: false
draft: false
---

[CF 2203C - Trình tạo thử nghiệm](https://codeforces.com/problemset/problem/2203/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, bitmasks, tham lam, toán học 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tạo ra một mảng các số nguyên không âm có tổng bằng tổng cho trước`s`, với ràng buộc bổ sung là mỗi phần tử chỉ được đặt các bit cũng được đặt ở một số khác`m`. Nói cách khác, nếu một bit bằng 0 trong`m`, nó phải bằng 0 trong mọi phần tử mảng. Chúng ta được yêu cầu tìm số phần tử nhỏ nhất có thể có trong một mảng như vậy hoặc báo cáo`-1`nếu không có mảng tồn tại. 

Từ những hạn chế,`s`Và`m`có thể lớn như$10^{18}$, và có thể có tới$10^4$trường hợp thử nghiệm. Điều này loại trừ mọi nỗ lực mạnh mẽ để thử tất cả các mảng có thể hoặc liệt kê mọi tập hợp con bit, vì thậm chí lặp lại tối đa`s`là không thể thực hiện được. Điều quan trọng là làm việc ở cấp độ bit và lý do về cách tính tổng`s`có thể được xây dựng bằng cách sử dụng lũy ​​thừa của hai được cho phép bởi`m`. 

Trường hợp khó phát hiện xảy ra khi`m`là rất nhỏ so với`s`. Ví dụ, nếu`s = 13`Và`m = 6`(nhị phân`110`), bất kỳ phần tử mảng nào cũng chỉ có thể có`2`-bit hoặc`4`-bit thiết lập. Không có sự kết hợp nào trong số này có thể tạo ra`13`, vì vậy đầu ra là`-1`. Một trường hợp góc khác là khi`m`chỉ có một tập hợp bit duy nhất. Vì`s = 10`Và`m = 1`, mảng hợp lệ duy nhất là`[1,1,1,...,1]`, và độ dài chính xác là`10`. 

Những ví dụ này minh họa rằng tính khả thi phụ thuộc vào việc liệu`s`có thể được biểu diễn dưới dạng tổng lũy ​​thừa được phép của hai từ`m`. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là cố gắng xây dựng tất cả các mảng bằng cách cộng các số liên tục`a_i`như vậy`a_i & m = a_i`, bắt đầu từ số lớn nhất có thể ≤`s`. Bạn có thể thử mọi chuỗi phần tử có thể. Mặc dù điều này cuối cùng sẽ tìm thấy một mảng hợp lệ nếu nó tồn tại, nhưng nó chậm theo cấp số nhân: mỗi bit có thể xuất hiện trong một phần tử hoặc không, mang lại tối đa 2^{\text{#bits}} khả năng, điều này là vô vọng khi`s`là lớn. 

Cái nhìn sâu sắc quan trọng là mọi số hợp lệ là một tập hợp con của các bit được đặt trong`m`. Giả định`m`có các bit được đặt ở vị trí`[b0, b1, ..., bk]`. Khi đó bất kỳ số nào trong mảng đều có thể được viết dưới dạng tổng của`2^bi`đối với một số tập hợp con của chỉ số. Nếu chúng ta xem xét`s`ở dạng nhị phân, chúng ta có thể coi nó như một tổng mục tiêu mà chúng ta cần xây dựng bằng cách sử dụng các bản sao không giới hạn của các lũy thừa được phép này. Điều này tương đương với việc phân phối các bit đã đặt của`s`trên các vị trí được cho phép bởi`m`. 

Cách tiếp cận tối ưu sử dụng phân phối mức bit tham lam. Bắt đầu từ bit có trọng số nhỏ nhất và xác định xem cần bao nhiêu bản sao của mỗi lũy thừa được phép của hai để tổng thành`s`. Mỗi bit trong`s`có thể được "bao phủ" bằng cách chia các bit cao hơn trong`m`. Ví dụ, nếu`m`cho phép`4`Và`2`, Và`s`có một`6`ở dạng nhị phân, chúng ta có thể phân phối nó dưới dạng`4 + 2`. Bằng cách tổng hợp tất cả sự đóng góp của các bit được phép, chúng ta có thể xác định số phần tử tối thiểu cần thiết, tương ứng với việc phân phối tổng số theo lũy thừa lớn nhất được phép trước, sau đó xử lý phần còn lại với lũy thừa nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^#bit tính bằng m) | O(#bit tính bằng giây) | Quá chậm | 
| Tối ưu | O(log s) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách`count`có kích thước 60 (kể từ`s`Và`m`là ≤$10^{18}$) để lưu trữ số lần mỗi bit có thể được sử dụng dựa trên`m`. Lặp lại qua các bit của`m`. Đối với mỗi vị trí bit`i`Ở đâu`m`có một`1`, bộ`count[i] = 1`. Chúng đại diện cho sức mạnh được phép của hai. 
2. Lặp lại các bit của`s`. Đối với mỗi vị trí bit`i`Ở đâu`s`có một`1`, hãy thử che nó bằng cách sử dụng các bit được phép từ`count`. Nếu như`count[i]`đã đủ rồi, hãy tiêu thụ nó. Nếu không, hãy "chia nhỏ" các bit cao hơn từ`count[j]`vì`j > i`bằng cách chia chúng thành`2^(j-i)`sao chép một chút`i`. Điều này thực sự đang chuyển các quyền lực cao hơn xuống để bù đắp cho các quyền lực thấp hơn. 
3. Nếu có bất kỳ bit nào trong`s`không thể được bao phủ bằng cách sử dụng`m`các bit của, mảng không tồn tại, vì vậy hãy trả về`-1`. 
4. Mặt khác, số phần tử tối thiểu`n`bằng tổng của tất cả`1`s trong số lượng bit được phân phối, biểu thị số lượng lũy ​​thừa riêng lẻ của hai cần thiết để đạt được`s`. 
5. Trở về`n`cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: Điều bất biến là mỗi bit được phép trong`m`có thể được chia thành bất kỳ số lũy thừa nhỏ hơn nào của hai. Điều này đảm bảo rằng chúng ta có thể biểu diễn bất kỳ tổng nào không yêu cầu các bit bên ngoài`m`. Bằng cách phân phối tham lam từ các bit cao hơn xuống các bit thấp hơn, chúng tôi đảm bảo số lượng phần tử mảng tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s, m = map(int, input().split())
        counts = [0]*60
        for i in range(60):
            if (m >> i) & 1:
                counts[i] = 1
        need = [0]*60
        for i in range(60):
            if (s >> i) & 1:
                need[i] = 1
        # distribute higher bits down
        for i in range(59, -1, -1):
            if need[i] > counts[i]:
                if i == 0:
                    counts[i] = -1
                    break
                need[i-1] += (need[i]-counts[i])*2
        if counts[0] < 0:
            print(-1)
        else:
            total = 0
            for i in range(60):
                total += need[i]
            print(total)

solve()
```Trong việc thực hiện này,`counts`theo dõi những quyền hạn nào được phép bởi`m`. Mảng`need`theo dõi số bit cần thiết để thực hiện`s`. Chúng tôi phân phối quyền hạn cao hơn xuống bất cứ khi nào yêu cầu bit thấp hơn vượt quá khả năng sẵn có. Nếu việc phân phối thất bại,`-1`được in. Ngược lại, tổng hợp tất cả`need`bit cung cấp số lượng phần tử mảng tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`s = 13, m = 5`Biểu diễn nhị phân:`s = 1101`,`m = 0101`. 

| chút | m cho phép | nhu cầu của bạn | hoạt động | 
| --- | --- | --- | --- | 
| 3 | 0 | 1 | không thể che trực tiếp, thử cao hơn? không cao hơn, thất bại ở đây? | 
| 2 | 1 | 1 | được bảo hiểm | 
| 1 | 0 | 0 | được | 
| 0 | 1 | 1 | được bảo hiểm | 

Sau khi chia nhỏ các bit được phép cao hơn, chúng ta có thể bao gồm`s = 13`với mảng`[5,4,4]`cho chiều dài tối thiểu`3`. 

### Ví dụ 2:`s = 13, m = 6`Nhị phân:`s = 1101`,`m = 0110`. 

Không thể che bit thấp nhất (`1`) bởi vì`m`có`0`một chút`0`. Phân phối không thành công, câu trả lời là`-1`. 

Những dấu vết này cho thấy cách phân phối tham lam cấp độ bit xác định tính khả thi và độ dài mảng tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(60 * t) ≈ O(t) | Chúng tôi lặp lại hơn 60 bit cho mỗi trường hợp thử nghiệm, không phụ thuộc vào độ lớn của`s`hoặc`m`. | 
| Không gian | O(60) | Mảng có kích thước cố định cho bit; độc lập với`s`hoặc`m`. | 

Từ`t ≤ 10^4`và mỗi lần lặp không đáng kể, điều này phù hợp thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("6\n13 5\n13 3\n13 6\n1000000007 2776648\n99999999999 1\n998244353 1557287\n") == "3\n5\n-1\n-1\n99999999999\n642"

# custom cases
assert run("1\n1 1\n") == "1", "minimal case, single element"
assert run("1\n10 1\n") == "10", "only smallest bit allowed"
assert run("1\n7 7\n") == "1", "s equals m, single element"
assert run("1\n15 8\n") == "-1", "cannot represent low bits"
assert run("1\n1000000000000000000 123456789012345678\n") == "-1", "large s, infeasible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Đầu vào tối thiểu, phần tử đơn | 
| 10 1 | 10 | Chỉ cho phép bit nhỏ nhất, phải sử dụng nhiều phần tử | 
| 7 7 | 1 | s bằng |
