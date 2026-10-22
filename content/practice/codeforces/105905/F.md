---
title: "CF 105905F - \u0422\u0443\u043d \u0442\u0443\u043d \u0442\u0443\u043d \u0442\u0443\u043d \u0421\u0430\u0445\u0443\u0440"
description: "Chúng tôi có hai chuỗi mô tả các sự kiện trống. Chuỗi đầu tiên p là chuỗi các lần đánh thực tế: mỗi ký tự là một lần đánh vào trống bên trái (L) hoặc trống bên phải (R). Chuỗi thứ hai s là những gì đã được nghe."
date: "2026-06-25T14:14:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105905
codeforces_index: "F"
codeforces_contest_name: "Ural championship 2025"
rating: 0
weight: 105905
solve_time_s: 35
verified: true
draft: false
---

[CF 105905F - \u0422\u0443\u043d \u0442\u0443\u043d \u0442\u0443\u043d \u0442\u0443\u043d \u0421\u0430\u0445\u0443\u0440](https://codeforces.com/problemset/problem/105905/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi mô tả các sự kiện trống. Chuỗi đầu tiên`p`là chuỗi lần đánh thực tế: mỗi ký tự là một lần đánh vào trống bên trái (`L`) hoặc trống bên phải (`R`). Chuỗi thứ hai`s`là những gì đã được nghe. Một lần nhấn không phải lúc nào cũng tạo ra một ký tự duy nhất trong bản ghi: một`L`lần truy cập có thể xuất hiện dưới dạng`L`hoặc`LL`và một`R`lần truy cập có thể xuất hiện dưới dạng`R`hoặc`RR`. 

Nhiệm vụ là quyết định xem chuỗi đã ghi có`s`có thể được tạo ra từ chuỗi hit`p`. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, độ dài của tất cả các chuỗi trên toàn bộ đầu vào đủ lớn để các giải pháp thực hiện tìm kiếm lặp lại hoặc quay lui sẽ không phù hợp. Với tổng chiều dài khoảng`2 * 10^5`, chúng ta cần một thuật toán tuyến tính hoặc gần tuyến tính. Bất cứ điều gì bậc hai sẽ thực hiện xung quanh`4 * 10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thời gian bình thường của Codeforce cho phép. 

Phần khó khăn là các ký tự liên tiếp bằng nhau có thể được kéo dài. Một giải pháp bất cẩn chỉ có thể kiểm tra xem liệu`p`là một dãy con của`s`, nhưng điều đó bỏ qua các quy tắc nhóm. Ví dụ, nếu`p = "LR"`Và`s = "LLLR"`, việc kiểm tra dãy con thành công vì chúng ta có thể tìm thấy`L`Và`R`, nhưng câu trả lời thực sự là`NO`. Cú đánh đầu tiên có thể tạo ra tối đa hai`L`nhân vật, vậy là ba`L`các ký tự trước`R`không thể đến từ cùng một bản hit gốc. 

Một trường hợp khác là khi số lượng ký tự đúng nhưng các nhóm sai. Vì`p = "LL"`Và`s = "L"`, đầu ra là`NO`. Mỗi nhân vật trong`p`đại diện cho một lần truy cập riêng biệt, vì vậy cả hai lần truy cập phải xuất hiện trong bản ghi. Một giải pháp chỉ so sánh số lượng chữ cái cuối cùng có thể chấp nhận điều này một cách không chính xác. 

Trường hợp ranh giới cuối cùng là một ký tự đơn. Vì`p = "R"`Và`s = "RR"`, đầu ra là`YES`bởi vì một cú đánh có thể vang lên hai lần. Vì`p = "RR"`Và`s = "R"`, đầu ra là`NO`bởi vì hai cú đánh không thể thu lại thành một âm thanh. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng tất cả các mở rộng có thể có của`p`. Mỗi ký tự có hai lựa chọn, do đó một chuỗi có độ dài`n`có thể sản xuất tới`2^n`các bản ghi âm có thể. Điều này đúng vì nó thử mọi phép biến đổi hợp lệ, nhưng nó trở nên vô dụng ngay cả với độ dài vừa phải. Với`n = 200000`, số lượng khả năng là không thể liệt kê được. 

Cấu trúc của bài toán cho chúng ta một cái nhìn nhỏ hơn nhiều. Các ký tự bằng nhau liên tiếp trong`p`phải tương ứng với một khối ký tự liên tiếp bằng nhau trong`s`. Nếu chúng ta chia cả hai chuỗi thành các khối tối đa thì mọi khối từ`p`có thể phát triển độc lập với hệ số một hoặc hai. Khối có chiều dài`x`TRONG`p`có thể trở thành một khối có chiều dài`x`hoặc`2x`TRONG`s`. 

Quan sát này loại bỏ sự cần thiết phải thử các lựa chọn. Chúng ta chỉ cần so sánh dạng nén của cả hai chuỗi. Nếu các chữ cái của các khối tương ứng khác nhau thì việc chuyển đổi là không thể. Nếu một khối từ`s`ngắn hơn khối phù hợp từ`p`, điều đó là không thể vì âm thanh không thể biến mất. Nếu nó dài hơn gấp đôi khối ban đầu thì không thể vì mỗi lần nhấn sẽ đóng góp tối đa hai bản sao. 

Lực lượng vũ phu hoạt động vì mọi câu trả lời hợp lệ đều là sự mở rộng của chuỗi gốc, nhưng không thành công vì nó khám phá nhiều khả năng tương đương. Việc so sánh khối hoạt động vì tất cả các ký tự bên trong một khối hoạt động giống hệt nhau, do đó thông tin duy nhất quan trọng là độ dài khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Nén khối | O( | p | + | 

## Hướng dẫn thuật toán 

1. Chia cả hai chuỗi thành các nhóm ký tự liên tiếp bằng nhau. Ví dụ,`"LLLRR"`trở thành khối`("L", 3)`Và`("R", 2)`. Các khối này đại diện cho các bản hit riêng lẻ của cùng loại trống xuất hiện cạnh nhau. 
2. So sánh số khối trong hai chuỗi. Nếu chúng khác nhau thì câu trả lời là ngay lập tức`NO`. Mỗi khối trong`p`phải khớp chính xác với một khối trong`s`, vì thay đổi duy nhất được phép là lặp lại cùng một ký tự. 
3. Đối với mỗi cặp khối giống nhau, hãy kiểm tra xem các ký tự của chúng có bằng nhau không. Cú đánh trái không thể biến thành âm phải, và cú đánh phải không thể biến thành âm trái. 
4. So sánh độ dài của từng cặp khối. Nếu độ dài khối được ghi nhỏ hơn độ dài khối ban đầu, bản ghi sẽ bị mất âm thanh và không hợp lệ. Nếu nó lớn hơn gấp đôi độ dài ban đầu thì có nghĩa là có quá nhiều âm thanh cho các bản hit có sẵn. Nếu không thì khối này hợp lệ. 
5. Nếu mọi khối đều vượt qua bước kiểm tra, hãy in`YES`. 

Lý do điều này có tác dụng là vì quá trình nén bảo toàn chính xác thông tin liên quan đến quá trình chuyển đổi. Bên trong một khối, mọi cú đánh ban đầu đều là cùng một kiểu đánh trống. Thay đổi duy nhất có thể xảy ra là thêm một bản sao bổ sung, do đó toàn bộ khối chỉ có thể chia tỷ lệ theo hệ số từ một đến hai. Các khối khác nhau không thể hợp nhất hoặc phân chia vì điều đó sẽ thay đổi thứ tự đánh trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compress(s):
    res = []
    i = 0
    n = len(s)
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        res.append((s[i], j - i))
        i = j
    return res

def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        p = input().strip()
        s = input().strip()

        a = compress(p)
        b = compress(s)

        if len(a) != len(b):
            ans.append("NO")
            continue

        ok = True
        for (c1, x), (c2, y) in zip(a, b):
            if c1 != c2 or y < x or y > 2 * x:
                ok = False
                break

        ans.append("YES" if ok else "NO")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`compress`hàm đi qua chuỗi một lần và xây dựng các khối liên tiếp. Nó sử dụng hai con trỏ:`i`đánh dấu sự bắt đầu của một khối và`j`di chuyển cho đến khi nhân vật thay đổi. Điều này tránh việc cắt lặp đi lặp lại và giữ cho tổng công việc được tuyến tính. 

Vòng lặp chính đầu tiên so sánh số khối. Điều này phát hiện các trường hợp thiếu ranh giới khối, chẳng hạn như bản gốc`"LR"`cố gắng để trở thành`"LRL"`. Sau đó, các khối được ghép nối sẽ được kiểm tra. điều kiện`y > 2 * x`là nơi phổ biến nhất xảy ra lỗi ngẫu nhiên vì một khối được phép nhân đôi, nhưng không được nhiều hơn. 

Số nguyên Python không tràn ở đây vì số học duy nhất liên quan đến độ dài chuỗi, nhiều nhất là vài trăm nghìn. 

## Ví dụ đã hoạt động 

cho`p = "LR"`Và`s = "LRR"`: 

| Bước | khối p | khối s | Kết quả | 
| --- | --- | --- | --- | 
| Nén chuỗi | (L,1), (R,1) | (L,1), (R,2) | tiếp tục | 
| Kiểm tra khối L | L, 1 ăn 1 | hợp lệ | tiếp tục | 
| Kiểm tra khối R | R, 1 đến 2 | hợp lệ | CÓ | 

Điều này cho thấy trường hợp bản ghi có chứa âm thanh bổ sung. Tiếng trống thứ hai được phép trở thành hai`R`nhân vật. 

Vì`p = "LR"`Và`s = "LLLR"`: 

| Bước | khối p | khối s | Kết quả | 
| --- | --- | --- | --- | 
| Nén chuỗi | (L,1), (R,1) | (L,3), (R,1) | tiếp tục | 
| Kiểm tra khối L | L, chiều dài ban đầu 1 | ghi dài 3 | không hợp lệ | 

Khối đầu tiên quá lớn. Một đĩa đơn`L`cú đánh chỉ có thể tạo ra một hoặc hai`L`các ký tự, vì vậy ba trong số chúng không thể được tạo ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | p | 
| Không gian | O( | p | 

Tổng chiều dài của tất cả các chuỗi được giới hạn bởi giới hạn đầu vào, do đó giải pháp tuyến tính dễ dàng phù hợp. Việc sử dụng bộ nhớ cũng tỷ lệ thuận với kích thước đầu vào và nằm trong giới hạn Codeforces thông thường. 

## Trường hợp thử nghiệm```python
import sys
import io

def compress(s):
    res = []
    i = 0
    while i < len(s):
        j = i
        while j < len(s) and s[j] == s[i]:
            j += 1
        res.append((s[i], j - i))
        i = j
    return res

def solve(inp):
    data = inp.strip().split()
    t = int(data[0])
    idx = 1
    out = []
    for _ in range(t):
        p = data[idx]
        s = data[idx + 1]
        idx += 2
        a = compress(p)
        b = compress(s)
        ok = len(a) == len(b)
        if ok:
            for (c1, x), (c2, y) in zip(a, b):
                if c1 != c2 or y < x or y > 2 * x:
                    ok = False
                    break
        out.append("YES" if ok else "NO")
    return "\n".join(out)

assert solve("""5
R
RR
LRLR
LRLR
LR
LLLR
LLLLLRL
LLLLRRLL
LLRLRLRRL
LLLRLRRLLRRRL
""") == """YES
YES
NO
NO
YES"""

assert solve("""2
L
L
RR
R
""") == """YES
NO"""

assert solve("""3
LL
LL
LL
LLLL
LL
L
""") == """YES
YES
NO"""

assert solve("""2
LR
LLRR
LR
LRR
""") == """YES
YES"""

assert solve("""1
LLLL
LLLLLLLL
""") == """YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn`R`trở thành`RR`| CÓ | Một khối có thể tăng gấp đôi kích thước. | 
| Hai`R`lượt truy cập trở thành một`R`| KHÔNG | Âm thanh không thể biến mất. | 
| Bốn lượt truy cập bằng nhau trở thành tám | CÓ | Việc mở rộng tối đa được xử lý. | 
| Khối hỗn hợp có mở rộng hợp lệ | CÓ | Các khối trống khác nhau luôn thẳng hàng. | 

## Vỏ cạnh 

cho`p = "LR"`Và`s = "LLLR"`, nén mang lại`(L,1),(R,1)`Và`(L,3),(R,1)`. Cặp đầu tiên không thành công vì độ dài được ghi lớn hơn gấp đôi độ dài ban đầu. Thuật toán trả về`NO`, tránh sai lầm khi điều trị`p`như một sự tiếp nối. 

Vì`p = "LL"`Và`s = "L"`, các dạng nén là`(L,2)`Và`(L,1)`. Dây thứ hai quá ngắn vì hai cú đánh riêng biệt cần tạo ra ít nhất hai âm thanh. Việc so sánh độ dài sẽ nắm bắt được điều này ngay lập tức. 

Vì`p = "R"`Và`s = "RR"`, các khối là`(R,1)`Và`(R,2)`. Kích thước được ghi chính xác là mức tối đa cho phép nên thuật toán chấp nhận nó. Điều này xác minh điều kiện biên trên. 

Vì`p = "LR"`Và`s = "LRL"`, dạng nén của chuỗi thứ hai có ba khối trong khi chuỗi thứ nhất có hai khối. Vì một khối ban đầu không thể tách thành một khối khác của trống khác nên thuật toán sẽ loại bỏ nó trước khi thực hiện bất kỳ kiểm tra độ dài nào.
