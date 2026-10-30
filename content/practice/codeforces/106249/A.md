---
title: "CF 106249A - MIT và TIM"
description: "Chúng ta có một chuỗi chỉ được tạo từ các ký tự M, I và T. Một thao tác có thể thay đổi chuỗi con MIT thành TIM hoặc ngược lại. Mục tiêu là thực hiện bất kỳ số lượng hoạt động nào và làm cho số lần xuất hiện liền kề của MITIT càng lớn càng tốt."
date: "2026-06-25T07:19:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106249
codeforces_index: "A"
codeforces_contest_name: "MITIT Winter 2025-26 Advanced Individual Round"
rating: 0
weight: 106249
solve_time_s: 65
verified: true
draft: false
---

[CF 106249A – MIT và TIM](https://codeforces.com/problemset/problem/106249/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chỉ được tạo từ các ký tự`M`,`I`, Và`T`. Một thao tác có thể thay đổi một chuỗi con`MIT`vào trong`TIM`hoặc ngược lại. Mục tiêu là thực hiện bất kỳ số lượng thao tác nào và tạo ra số lần xuất hiện liền kề của`MITIT`càng lớn càng tốt. 

Quan sát quan trọng là một hoạt động chỉ hoán đổi một`M`và một`T`xung quanh một cố định`I`. Các vị trí chứa`I`không bao giờ thay đổi. Kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là$10^5$, vì vậy một$O(n^2)$mô phỏng các hoạt động hoặc thử nhiều chuỗi cuối cùng là không thể. Chúng ta cần xử lý mỗi ký tự một số lần không đổi. 

Một sai lầm phổ biến là nghĩ rằng các ký tự có thể được sắp xếp lại một cách tự do vì thao tác thay đổi thứ tự của các ký tự.`M`Và`T`. Họ không thể. Ví dụ, trong`MIMT`, cái`I`vị trí vẫn cố định, vì vậy đầu tiên`I`sẽ luôn ở giữa hai điểm đầu tiên không`I`các vị trí. Một giải pháp chỉ đếm tổng số`M`,`I`, Và`T`sẽ thất bại. 

Hãy xem xét chuỗi`MIMTI`. Nó có hai`I`ký tự, nhưng nó không bao giờ có thể tạo`MITIT`, bởi vì trận chung kết`T`sau lần thứ hai`I`không tồn tại. Câu trả lời đúng là`0`. Một giải pháp bất cẩn chỉ dựa trên số lượng có thể dự đoán không chính xác một lần xảy ra vì có một lần xảy ra.`M`, hai`I`, và hai`T`nhân vật. 

Một trường hợp khác là khi các ký tự được yêu cầu tồn tại nhưng`I`những vị trí ngăn cản họ gặp nhau. Vì`MMITT`, số lượng là đủ cho`MITIT`, nhưng không có hai`I`ký tự ở khoảng cách cần thiết. Câu trả lời là`0`, vì các thao tác không thể di chuyển`I`nhân vật. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng hiểu tất cả các chuỗi có thể truy cập được thông qua các hoạt động. Vì mỗi thao tác hoán đổi hai ký tự nên chúng ta có thể thử các hoán đổi khác nhau và tìm được kết quả tốt nhất. Điều này đúng vì mọi cách sắp xếp cuối cùng hợp lệ đều có thể đạt được bằng một số chuỗi thao tác, nhưng số lượng cách sắp xếp có thể tăng theo cấp số nhân, do đó nó không thể sử dụng được đối với các chuỗi có độ dài$10^5$. 

Cấu trúc quan trọng đến từ việc xem xét thực sự một thao tác đơn lẻ thực hiện những gì. Một sự biến đổi`MIT <-> TIM`chỉ hoán đổi hai không`I`các nhân vật xung quanh một`I`. Điều này có nghĩa là`I`các vị trí được cố định mãi mãi. Việc không`I`các vị trí tạo thành các nhóm nơi các nhân vật có thể di chuyển xung quanh. 

Nếu chúng ta kết nối hai không`I`vị trí bất cứ khi nào có chính xác một`I`giữa chúng, mọi thao tác chỉ là sự hoán đổi bên trong một thành phần được kết nối. Bên trong một thành phần, tất cả`M`Và`T`các ký tự có thể được sắp xếp lại tùy ý. 

MỘT`MITIT`sự xuất hiện sử dụng ba không`I`các vị trí. Các vị trí đó cách nhau hai`I`các ký tự, vì vậy chúng luôn thuộc về cùng một thành phần. Điều này có nghĩa là mọi thành phần đều có thể được giải quyết một cách độc lập. 

Bên trong một thành phần, giả sử chiều dài của nó là`len`và nó chứa`m`nhân vật`M`Và`t`nhân vật`T`. Chúng ta cần tối đa hóa số lượng độ dài ba mẫu`MTT`. Mỗi mẫu như vậy cần một`M`và hai`T`các ký tự và các lần xuất hiện khác nhau không thể có cùng vị trí bắt đầu. Tối đa là:$$\min(m,\lfloor t/2\rfloor,\lfloor len/3\rfloor)$$Giá trị này luôn có thể đạt được bằng cách đặt mỗi`M`theo sau là hai`T`các ký tự và sử dụng các vị trí còn lại cho các chữ cái không sử dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi và xây dựng các thành phần được kết nối của không`I`các vị trí. Hai không`I`các vị trí thuộc về cùng một thành phần nếu có chính xác một`I`giữa họ. Điều này tương đương với việc nhóm liên tiếp không`I`các vị trí có cùng tính chẵn lẻ được kết nối thông qua`I`các vị trí. 
2. Đối với mỗi thành phần, hãy đếm kích thước và số lượng của nó.`M`nhân vật và`T`các ký tự bên trong nó. Sự sắp xếp hiện tại không liên quan vì các thao tác cho phép chúng ta sắp xếp lại các ký tự này bên trong thành phần. 
3. Tính mức đóng góp của thành phần đó bằng số lượng tối thiểu có sẵn`M`ký tự, một nửa số có sẵn`T`ký tự và một phần ba chiều dài thành phần. Mỗi giá trị đại diện cho một tài nguyên cần thiết cho một`MITIT`mẫu. 
4. Cộng phần đóng góp của tất cả các thành phần. Vì không có lần xuất hiện nào có thể sử dụng các ký tự từ hai thành phần khác nhau nên tổng là giá trị tối đa toàn cục. 

Tại sao nó hoạt động: Điều bất biến là mọi thao tác đều giữ tất cả`I`vị trí cố định và chỉ hoán đổi`M`Và`T`bên trong thành phần được kết nối của họ. Một khả năng`MITIT`sự xuất hiện cần ba không`I`vị trí được kết nối bởi hai`I`vị trí, vì vậy nó phải ở bên trong một thành phần duy nhất. Vì mỗi thành phần có thể được sắp xếp lại một cách tự do nên hạn chế duy nhất là số lượng`M`,`T`và chiều dài có sẵn. Công thức nắm bắt chính xác các giới hạn này nên tổng được tính là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(s):
    n = len(s)
    seen = [False] * n
    ans = 0

    for i in range(n):
        if s[i] == 'I' or seen[i]:
            continue

        stack = [i]
        seen[i] = True
        length = 0
        m = 0
        t = 0

        while stack:
            x = stack.pop()
            length += 1
            if s[x] == 'M':
                m += 1
            else:
                t += 1

            for y in (x - 2, x + 2):
                if 0 <= y < n and not seen[y] and s[(x + y) // 2] == 'I' and s[y] != 'I':
                    seen[y] = True
                    stack.append(y)

        ans += min(m, t // 2, length // 3)

    return ans

def main():
    data = sys.stdin.read().split()
    if not data:
        return
    t = int(data[0])
    out = []
    for i in range(1, t + 1):
        out.append(str(solve_case(data[i])))
    print("\n".join(out))

main()
```Giải pháp sử dụng tìm kiếm theo chiều sâu đầu tiên trên biểu đồ ẩn của không`I`các vị trí. Một cạnh chỉ tồn tại khi hai vị trí có`I`giữa chúng, đó là lý do tại sao việc kiểm tra hàng xóm sử dụng khoảng cách hai. 

DFS thu thập kích thước của một thành phần và số lượng của mỗi chữ cái. Thứ tự thực tế của các ký tự bên trong thành phần là không cần thiết vì việc hoán đổi cho phép chúng ta sắp xếp lại chúng. Tính toán tối thiểu cuối cùng áp dụng trực tiếp các giới hạn tài nguyên cho việc xây dựng`MTT`ba lần, tương ứng với`MITIT`các chuỗi con. 

Việc kiểm tra ranh giới trong vòng lặp lân cận ngăn chặn việc truy cập bên ngoài chuỗi. Chỉ số trung gian`(x + y) // 2`là an toàn bởi vì`y`luôn cách nhau đúng hai vị trí`x`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
TITIMMIT
```Các thành phần được xử lý như sau: 

| Thành phần | Chiều dài | Đếm M | Đếm T | Đóng góp | 
| --- | --- | --- | --- | --- | 
| vị trí phi I nối với nhau | 5 | 2 | 3 | 1 | 

Thành phần này có đủ ký tự để tạo thành một`MTT`, vậy câu trả lời là`1`. Điều này cho thấy thuật toán sử dụng chính xác sự sắp xếp lại bên trong một thành phần. 

Đối với một thành phần mà các ký tự không thể cung cấp đủ`T`ký tự: 

| Thành phần | Chiều dài | Đếm M | Đếm T | Đóng góp | 
| --- | --- | --- | --- | --- | 
| một thành phần | 7 | 3 | 1 | 0 | 

Mặc dù có`M`ký tự có sẵn, mỗi lần xuất hiện cần có hai ký tự`T`các ký tự sau`M`. Câu trả lời vẫn là con số không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được truy cập một lần trong quá trình truyền tải thành phần | 
| Không gian | O(n) | Mảng đã truy cập và ngăn xếp DFS lưu trữ ở hầu hết các vị trí | 

Tổng chiều dài của tất cả các chuỗi là$10^5$, do đó nghiệm tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve_case(s):
    n = len(s)
    seen = [False] * n
    ans = 0
    for i in range(n):
        if s[i] == 'I' or seen[i]:
            continue
        stack = [i]
        seen[i] = True
        length = m = t = 0
        while stack:
            x = stack.pop()
            length += 1
            if s[x] == 'M':
                m += 1
            else:
                t += 1
            for y in (x - 2, x + 2):
                if 0 <= y < n and not seen[y] and s[(x + y) // 2] == 'I' and s[y] != 'I':
                    seen[y] = True
                    stack.append(y)
        ans += min(m, t // 2, length // 3)
    return ans

def run(inp: str) -> str:
    data = inp.split()
    out = []
    for s in data[1:]:
        out.append(str(solve_case(s)))
    return "\n".join(out)

assert run("6 TITIMMIT TITITIMITIMTMTMTMMITMI MIMTITMTIMTITMTITITMTI ITMTTMITMITMTMTITITMIM MMITMITTTIMTITITTTITIT MITITIMIMIMITITITITIMIMIMIMIMIMITITITIT") == "1\n2\n0\n0\n0\n5"

assert run("1 MIMTI") == "0"
assert run("1 MMITT") == "0"
assert run("1 MITIT") == "1"
assert run("1 MMMMMIIIIITTTTT") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`MIMTI`|`0`| Kiểm tra xem chỉ số ký tự thôi là chưa đủ | 
|`MMITT`|`0`| Đã sửa lỗi kiểm tra`I`vị trí | 
|`MITIT`|`1`| Mẫu thành công trực tiếp tối thiểu | 
|`MMMMMIIIIITTTTT`|`0`| Kiểm tra các thành phần riêng biệt và sắp xếp lại không thể | 

## Vỏ cạnh 

cho`MIMTI`, thuật toán tạo ra các thành phần từ không`I`các vị trí. Thành phần chứa`M`Và`T`vị trí không có đủ không gian và`T`các ký tự cho hoàn chỉnh`MTT`mẫu, vì vậy sự đóng góp bằng không. 

Vì`MMITT`, hai`I`ký tự không ở vị trí cần thiết cho`MITIT`. Vì các thành phần được xây dựng xung quanh cố định`I`vị trí, thuật toán không bao giờ kết hợp sai các ký tự này và trả về 0. 

Đối với một chuỗi đã bằng`MITIT`, cả ba không`I`vị trí thuộc về một thành phần. Độ dài thành phần là ba, nó có một`M`và hai`T`nhân vật, đưa ra:$$\min(1, 2/2, 3/3)=1$$vậy câu trả lời là một. Điều này xác nhận rằng phương pháp xử lý sự xuất hiện đơn giản nhất có thể.
