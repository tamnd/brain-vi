---
title: "CF 105636A - \u7f16\u8f91\u5b57\u7b26\u4e32"
description: "Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau. Một số vị trí được đánh dấu là có thể di chuyển được và một số vị trí bị khóa. Một nước đi bao gồm việc hoán đổi hai ký tự liền kề, nhưng chỉ những ký tự được phép tham gia hoán đổi mới có thể được di chuyển."
date: "2026-06-25T06:10:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105636
codeforces_index: "A"
codeforces_contest_name: "NOIP 2024"
rating: 0
weight: 105636
solve_time_s: 62
verified: true
draft: false
---

[CF 105636A - \u7f16\u8f91\u5b57\u7b26\u4e32](https://codeforces.com/problemset/problem/105636/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau. Một số vị trí được đánh dấu là có thể di chuyển được và một số vị trí bị khóa. 

Một nước đi bao gồm việc hoán đổi hai ký tự liền kề, nhưng chỉ những ký tự được phép tham gia hoán đổi mới có thể được di chuyển. Vì các giao dịch hoán đổi là liền kề và có thể được lặp lại tùy ý nhiều lần nên mỗi phân đoạn vị thế liên tiếp tối đa đều được đánh dấu`1`trong mặt nạ hoạt động giống như một khối có thể hoán vị tự do. Nhân vật không thể vượt qua vị trí bị khóa. 

Sau khi thực hiện bất kỳ số lượng hoán đổi nào như vậy trên một trong hai chuỗi, chúng tôi muốn tối đa hóa số lượng vị trí trong đó hai chuỗi cuối cùng chứa cùng một ký tự. 

Độ dài của mỗi chuỗi có thể đạt tới$10^5$, và có nhiều trường hợp thử nghiệm. Bất kỳ giải pháp nào liên tục mô phỏng các giao dịch hoán đổi hoặc thử các cách sắp xếp lại khác nhau sẽ bị loại trừ ngay lập tức. Chúng tôi cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. MỘT$O(n^2)$cách tiếp cận sẽ yêu cầu khoảng$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. 

Điểm tinh tế chính là các ký tự có thể di chuyển không được tự do trên toàn cầu. Một nhân vật chỉ có thể di chuyển trong đoạn vị trí di chuyển tối đa của chính nó. Hai đoạn cách nhau bởi một vị trí khóa là hoàn toàn độc lập. 

Hãy xem xét một ví dụ đơn giản:```
s1 = 01
s2 = 10
t1 = 11
t2 = 00
```Chuỗi thứ hai đã được cố định. Chuỗi đầu tiên có thể sắp xếp lại hai ký tự của nó. Bằng cách di chuyển`1`đến vị trí đầu tiên chúng tôi có được hai trận đấu. Xử lý các vị trí di chuyển một cách độc lập sẽ bỏ lỡ khả năng này. 

Một lỗi dễ mắc khác là cho phép nhân vật vượt qua vị trí bị khóa:```
s1 = 101
t1 = 101
```Vị trí ở giữa bị khóa. Hai vị trí di chuyển được thuộc hai phân đoạn khác nhau nên`1`s không thể đổi chỗ qua trung tâm. 

## Phương pháp tiếp cận 

Ý tưởng Brute Force là tạo ra tất cả các cách sắp xếp có thể truy cập được của cả hai chuỗi một cách rõ ràng và kiểm tra số lượng vị trí bằng nhau tối đa. Bên trong một đoạn có chiều dài di động$k$, mọi hoán vị đều có thể truy cập được, do đó số lượng trạng thái tăng theo giai thừa. Ngay cả một đoạn có chiều dài 15 cũng đã có nhiều hơn$10^{12}$hoán vị. Cách tiếp cận này là vô vọng. 

Quan sát quan trọng là bên trong một phân đoạn có thể di chuyển được, chỉ có số lượng`0`Và`1`vấn đề. Thứ tự chính xác không liên quan vì bất kỳ thứ tự nào cũng có thể được tạo ra. 

Đầu tiên chúng ta phân tách từng chuỗi thành các thành phần. 

Vị trí bị khóa tạo thành một thành phần có kích thước có giá trị cố định mãi mãi. 

Chuỗi vị trí liên tiếp tối đa có giá trị mặt nạ`1`tạo thành một bộ phận có thể di chuyển được. Bên trong một thành phần như vậy chúng ta chỉ cần biết có bao nhiêu`0`s và bao nhiêu`1`s có sẵn. 

Bây giờ hãy nghĩ về vị trí mà một bên được cố định và bên kia thuộc về một bộ phận có thể di chuyển được. Nếu thành phần có thể di chuyển vẫn chứa chữ số được yêu cầu thì việc so khớp ở đây luôn có lợi. Một trận đấu đóng góp chính xác một cho câu trả lời và việc sử dụng một nhân vật bây giờ không thể làm giảm mức đóng góp tối đa có thể có trong tương lai vì mỗi nhân vật chỉ có thể đóng góp nhiều nhất một lần. 

Vì vậy, chúng tôi tham lam xử lý tất cả các vị trí có ít nhất một bên được cố định. Bất cứ khi nào một thành phần di động có thể cung cấp chữ số cần thiết, chúng tôi sử dụng một lần xuất hiện và đếm một kết quả trùng khớp. 

Sau đó, chỉ còn lại những vị trí mà cả hai bên đều có thể di chuyển được. Ở vị trí như vậy, nếu hai thành phần tương ứng vẫn chứa một`0`, chúng ta có thể khớp một`0`đây. Ngược lại, nếu cả hai vẫn chứa một`1`, chúng ta có thể khớp một`1`. Nếu cả hai bên đều không có chữ số nào thì vị trí này không bao giờ bằng nhau. 

Số lượng còn lại trong mỗi thành phần mô tả đầy đủ các ký tự không được sử dụng nên việc tham lam tạo ra sự trùng khớp bất cứ khi nào có thể là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam tối ưu với các thành phần | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1 

Xây dựng id thành phần cho cả hai chuỗi. 

Một vị trí bị khóa có thành phần riêng của nó. 

Một chuỗi vị trí di chuyển liên tiếp tối đa có chung một id thành phần. 

Với mỗi bộ phận có thể di chuyển được, hãy lưu trữ bao nhiêu`0`s và bao nhiêu`1`s nó hiện chứa. 

### Bước 2 

Xử lý mọi vị trí. 

Nếu cả hai vị trí đều bị khóa, giá trị của chúng sẽ được cố định vĩnh viễn. Thêm một vào câu trả lời nếu các chữ số đã bằng nhau. 

Nếu mặt thứ nhất bị khóa và mặt thứ hai có thể di chuyển được, hãy kiểm tra xem thành phần thứ hai có còn chứa chữ số được yêu cầu hay không. Nếu có, hãy sử dụng một lần xuất hiện và thêm một lần xuất hiện vào câu trả lời. 

Nếu mặt thứ hai bị khóa và mặt thứ nhất có thể di chuyển được thì thực hiện thao tác đối xứng. 

Lý do phải làm điều này trước tiên là vì các vị trí cố định chỉ có một giá trị khả dĩ. Nếu một thành phần có thể di chuyển được có thể đáp ứng giá trị đó thì việc lấy trận đấu ngay lập tức không bao giờ có hại. 

### Bước 3 

Xử lý lại mọi vị trí. 

Chỉ xem xét các vị trí mà cả hai bên đều có thể di chuyển được. 

Đặt các thành phần di chuyển tương ứng là$A$Và$B$. 

Nếu cả hai vẫn chứa một`0`, tiêu thụ một`0`từ mỗi và thêm một vào câu trả lời. 

Ngược lại, nếu cả hai vẫn chứa một`1`, tiêu thụ một`1`từ mỗi và thêm một vào câu trả lời. 

Nếu không thì không thể khớp được ở vị trí này. 

### Bước 4 

Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Mỗi thành phần di động được đặc trưng hoàn toàn bởi số lượng còn lại của`0`Và`1`. 

Khi một vị trí liên quan đến một ký tự cố định, mọi kết quả khớp thành công đều phải sử dụng một lần xuất hiện của chữ số cố định đó từ phía có thể di chuyển được. Việc trì hoãn trận đấu đó không mang lại lợi ích gì vì nhân vật được tiêu thụ chỉ có thể đóng góp tối đa một trận đấu ở bất kỳ đâu. 

Sau khi tất cả các nhu cầu về vị trí cố định được thỏa mãn nhiều nhất có thể, mọi vị trí còn lại đều có hai mặt có thể di chuyển được. Việc so khớp ở vị trí như vậy chỉ yêu cầu cả hai thành phần vẫn có chữ số giống nhau. Nếu tồn tại một chữ số chung, việc lấy kết quả khớp đó ngay lập tức sẽ tiêu tốn một ký tự từ mỗi bên và tăng câu trả lời lên một ký tự. Không có sự sắp xếp nào trong tương lai có thể trích xuất nhiều hơn một đóng góp từ hai ký tự được sử dụng đó. 

Do đó, mọi trận đấu tham lam đều tối ưu cục bộ và không bao giờ làm giảm số lượng trận đấu tối đa có thể đạt được trong tương lai. Việc lặp lại quá trình này sẽ tạo ra một câu trả lời tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())

    for _ in range(T):
        n = int(input())
        s1 = input().strip()
        s2 = input().strip()
        t1 = input().strip()
        t2 = input().strip()

        comp1 = [-1] * n
        comp2 = [-1] * n

        cnt1 = []
        cnt2 = []

        cid = 0
        i = 0
        while i < n:
            if t1[i] == '0':
                comp1[i] = -1
                i += 1
            else:
                cnt1.append([0, 0])
                j = i
                while j < n and t1[j] == '1':
                    comp1[j] = cid
                    cnt1[cid][int(s1[j])] += 1
                    j += 1
                cid += 1
                i = j

        cid = 0
        i = 0
        while i < n:
            if t2[i] == '0':
                comp2[i] = -1
                i += 1
            else:
                cnt2.append([0, 0])
                j = i
                while j < n and t2[j] == '1':
                    comp2[j] = cid
                    cnt2[cid][int(s2[j])] += 1
                    j += 1
                cid += 1
                i = j

        ans = 0

        for i in range(n):
            a_fixed = (t1[i] == '0')
            b_fixed = (t2[i] == '0')

            if a_fixed and b_fixed:
                if s1[i] == s2[i]:
                    ans += 1

            elif a_fixed and not b_fixed:
                need = int(s1[i])
                c = comp2[i]
                if cnt2[c][need] > 0:
                    cnt2[c][need] -= 1
                    ans += 1

            elif not a_fixed and b_fixed:
                need = int(s2[i])
                c = comp1[i]
                if cnt1[c][need] > 0:
                    cnt1[c][need] -= 1
                    ans += 1

        for i in range(n):
            if t1[i] == '1' and t2[i] == '1':
                c1 = comp1[i]
                c2 = comp2[i]

                if cnt1[c1][0] > 0 and cnt2[c2][0] > 0:
                    cnt1[c1][0] -= 1
                    cnt2[c2][0] -= 1
                    ans += 1
                elif cnt1[c1][1] > 0 and cnt2[c2][1] > 0:
                    cnt1[c1][1] -= 1
                    cnt2[c2][1] -= 1
                    ans += 1

        print(ans)

solve()
```Giai đoạn đầu tiên xây dựng các bộ phận có thể di chuyển được và ghi lại số lượng`0`cát`1`s mỗi thành phần chứa. 

Giai đoạn thứ hai xử lý mọi vị trí liên quan đến ít nhất một ký tự cố định. Những kết quả trùng khớp này bị hạn chế nhất vì phía cố định không thể thay đổi giá trị. 

Giai đoạn thứ ba xử lý các vị trí mà cả hai bên đều có thể di chuyển được. Tại thời điểm đó, tất cả số lượng còn lại tương ứng với các ký tự chưa được cam kết khớp với vị trí cố định. 

Việc triển khai sử dụng id thành phần để truy cập vào mảng đếm chính xác trong thời gian không đổi. Mỗi vị trí chỉ được truy cập một vài lần, tạo ra độ phức tạp tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 3
s1 = 010
s2 = 000
t1 = 111
t2 = 000
```Chuỗi đầu tiên có một thành phần di động chứa hai`0`s và một`1`. 

| Vị trí | Giá trị bên cố định | Có sẵn trong thành phần | Trận đấu | 
| --- | --- | --- | --- | 
| 1 | 0 | vâng | 1 | 
| 2 | 0 | vâng | 1 | 
| 3 | 0 | vâng | 1 | 

Câu trả lời trở thành 3. 

Ví dụ này cho thấy lý do tại sao một phân đoạn có thể di chuyển phải được coi là một tập hợp các chữ số có sẵn thay vì theo từng vị trí. 

### Ví dụ 2```
n = 4
s1 = 0011
s2 = 1100
t1 = 1111
t2 = 1111
```Cả hai chuỗi đều bao gồm một thành phần có thể di chuyển được. 

Sau khi tính số lần xây dựng: 

| Thành phần | Số không | Những cái | 
| --- | --- | --- | 
| s1 | 2 | 2 | 
| s2 | 2 | 2 | 

Xử lý các vị trí di chuyển-di chuyển: 

| Vị trí | Chữ số phổ biến được sử dụng | Còn lại s1 | Còn lại s2 | Trận đấu | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | (1,2) | (1,2) | 1 | 
| 2 | 0 | (0,2) | (0,2) | 1 | 
| 3 | 1 | (0,1) | (0,1) | 1 | 
| 4 | 1 | (0,0) | (0,0) | 1 | 

Câu trả lời cuối cùng là 4. 

Điều này chứng tỏ rằng chỉ có chữ số mới quan trọng bên trong một bộ phận có thể di chuyển được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một số lần không đổi | 
| Không gian | O(n) | Id thành phần và mảng đếm | 

Tổng công việc tăng tuyến tính theo chiều dài chuỗi. Với$n \le 10^5$, điều này dễ dàng phù hợp với giới hạn chương trình cạnh tranh điển hình. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = input_data
    sys.stdout = output_data

    try:
        import sys
        input = sys.stdin.readline

        T = int(input())
        for _ in range(T):
            n = int(input())
            s1 = input().strip()
            s2 = input().strip()
            t1 = input().strip()
            t2 = input().strip()

            comp1 = [-1] * n
            comp2 = [-1] * n
            cnt1 = []
            cnt2 = []

            cid = 0
            i = 0
            while i < n:
                if t1[i] == '0':
                    i += 1
                else:
                    cnt1.append([0, 0])
                    j = i
                    while j < n and t1[j] == '1':
                        comp1[j] = cid
                        cnt1[cid][int(s1[j])] += 1
                        j += 1
                    cid += 1
                    i = j

            cid = 0
            i = 0
            while i < n:
                if t2[i] == '0':
                    i += 1
                else:
                    cnt2.append([0, 0])
                    j = i
                    while j < n and t2[j] == '1':
                        comp2[j] = cid
                        cnt2[cid][int(s2[j])] += 1
                        j += 1
                    cid += 1
                    i = j

            ans = 0

            for i in range(n):
                a_fixed = t1[i] == '0'
                b_fixed = t2[i] == '0'

                if a_fixed and b_fixed:
                    ans += (s1[i] == s2[i])
                elif a_fixed:
                    d = int(s1[i])
                    c = comp2[i]
                    if cnt2[c][d]:
                        cnt2[c][d] -= 1
                        ans += 1
                elif b_fixed:
                    d = int(s2[i])
                    c = comp1[i]
                    if cnt1[c][d]:
                        cnt1[c][d] -= 1
                        ans += 1

            for i in range(n):
                if t1[i] == '1' and t2[i] == '1':
                    c1 = comp1[i]
                    c2 = comp2[i]
                    if cnt1[c1][0] and cnt2[c2][0]:
                        cnt1[c1][0] -= 1
                        cnt2[c2][0] -= 1
                        ans += 1
                    elif cnt1[c1][1] and cnt2[c2][1]:
                        cnt1[c1][1] -= 1
                        cnt2[c2][1] -= 1
                        ans += 1

            print(ans)

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output_data.getvalue()

# provided sample
assert run(
"""1
6
011101
111010
111010
101101
"""
) == "4\n"

# minimum size
assert run(
"""1
1
0
0
0
0
"""
) == "1\n"

# all movable
assert run(
"""1
4
0011
1100
1111
1111
"""
) == "4\n"

# all fixed
assert run(
"""1
4
0101
0011
0000
0000
"""
) == "2\n"

# locked barrier separates segments
assert run(
"""1
3
101
111
101
111
"""
) == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ký tự cố định bằng nhau duy nhất | 1 | Kích thước tối thiểu | 
| Tất cả các vị trí có thể di chuyển | 4 | Sắp xếp lại toàn bộ các thành phần bên trong | 
| Tất cả các vị trí cố định | 2 | Không được phép di chuyển | 
| Rào cản ở giữa | 2 | Nhân vật không thể vượt qua các vị trí bị khóa | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
101
111
101
111
```Vị trí giữa của chuỗi đầu tiên bị khóa. Hai vị trí di chuyển thuộc về các thành phần khác nhau. 

Thành phần đầu tiên chứa một`1`, thành phần thứ hai cũng chứa một`1`. Họ không thể hợp nhất thành một nhóm duy nhất. Thuật toán chỉ định các id thành phần riêng biệt và duy trì số lượng riêng biệt, do đó không bao giờ sử dụng chuyển động xuyên rào cản bất hợp pháp. 

Bây giờ hãy xem xét:```
1
2
01
10
11
00
```Chuỗi thứ hai đã được cố định hoàn toàn. Thành phần di động của chuỗi đầu tiên chứa một`0`và một`1`. 

Tại vị trí 1 giá trị cố định là`1`, do đó thuật toán tiêu thụ có sẵn`1`. 

Tại vị trí 2 giá trị cố định là`0`, do đó thuật toán tiêu thụ có sẵn`0`. 

Cả hai vị trí đều khớp nhau và câu trả lời là 2. Xử lý các vị trí một cách độc lập sẽ bỏ lỡ sự sắp xếp lại này. 

Cuối cùng, hãy xem xét:```
1
4
0000
1111
1111
1111
```Mọi vị trí đều có thể di chuyển được ở cả hai bên, nhưng các thành phần chỉ chứa các chữ số đối diện nhau. Không có chữ số chung nào ở bất kỳ bước nào, vì vậy thuật toán trả về chính xác 0.
