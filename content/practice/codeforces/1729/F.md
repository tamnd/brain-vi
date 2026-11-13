---
title: "CF 1729F - Kirei và hàm tuyến tính"
description: "Chúng tôi được cung cấp một chuỗi chữ số. Đối với mỗi truy vấn, chúng ta xem xét một chuỗi con của chuỗi gốc, cụ thể là s[l..r], và tính giá trị số modulo 9 của nó. Riêng biệt, chúng ta có thể chọn bất kỳ hai chuỗi con khác nhau nào có độ dài cố định w. Đặt vị trí bắt đầu của họ là L1 và L2."
date: "2026-06-09T18:47:58+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "math"]
categories: ["algorithms"]
codeforces_contest: 1729
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 820 (Div. 3)"
rating: 1900
weight: 1729
solve_time_s: 108
verified: true
draft: false
---

[CF 1729F - Kirei và Hàm tuyến tính](https://codeforces.com/problemset/problem/1729/F) 

**Xếp hạng:** 1900 
**Tags:** băm, toán học 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chữ số. Đối với mỗi truy vấn, chúng ta xem xét một chuỗi con của chuỗi gốc, cụ thể là`s[l..r]`và tính giá trị số của nó theo modulo`9`. 

Riêng biệt, chúng ta có thể chọn bất kỳ hai chuỗi con khác nhau có độ dài cố định`w`. Hãy để vị trí bắt đầu của họ là`L1`Và`L2`. Truy vấn yêu cầu các vị trí thỏa mãn$$v(L_1,L_1+w-1)\cdot v(l,r)+v(L_2,L_2+w-1)\equiv k \pmod 9$$Trong số tất cả các cặp hợp lệ, chúng ta phải giảm thiểu`L1`, và trong số đó, giảm thiểu`L2`. 

Điều nổi bật đầu tiên là con số thực tế có thể cực kỳ lớn. Một chuỗi con có thể chứa tới`2·10^5`các chữ số, do đó việc xây dựng số một cách trực tiếp là không thể. May mắn thay, điều kiện chỉ quan tâm đến modulo còn lại`9`. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, và tổng số truy vấn cũng nhiều nhất`2·10^5`. Bất kỳ thuật toán nào kiểm tra tất cả các cặp độ dài-`w`chuỗi con cho mọi truy vấn là vô vọng. Ngay cả một trường hợp thử nghiệm đơn lẻ cũng có thể chứa khoảng`2·10^5`các chuỗi con ứng cử viên, vì vậy công thức bậc hai sẽ xoay quanh`4·10^{10}`hoạt động. 

Giải pháp phải xử lý trước tất cả các chiều dài-`w`chuỗi con một lần và sau đó trả lời từng truy vấn trong thời gian gần như không đổi. 

Một điểm tinh tế là câu trả lời không chỉ là bất kỳ cặp hợp lệ nào. Vấn đề giảm thiểu từ điển học. Hãy xem xét một tình huống trong đó lớp dư lượng`3`chứa vị trí bắt đầu`[2,7,20]`và lớp dư lượng`5`chứa`[4,8]`. Nếu cả hai`(2,8)`Và`(7,4)`thỏa mãn phương trình mô đun, câu trả lời đúng là`(2,8)`bởi vì`L1`nhỏ hơn. Việc triển khai bất cẩn dừng lại ở sự kết hợp dư lượng hợp lệ đầu tiên có thể trả về cặp sai. 

Một sai lầm dễ dàng khác là quên điều đó`L1`Và`L2`phải có những vị trí khác nhau. Giả sử mọi chiều dài-`w`chuỗi con có cùng phần dư và chỉ có một lần xuất hiện phần dư đó. Sau đó sử dụng cùng một vị trí hai lần đều bị cấm. 

Ví dụ:```
s = "111"
w = 2
```Chuỗi con có độ dài 2 duy nhất bắt đầu tại các vị trí`1`Và`2`. Bất kỳ logic nào cho phép sử dụng lại một lần xuất hiện của dư lượng có thể tạo ra sai sót`(1,1)`. 

Số 0 đứng đầu cũng có liên quan. Chuỗi con`"000"`đại diện cho số`0`, không phải là giá trị không hợp lệ. Vì chúng ta chỉ cần modulo`9`, tổng chữ số xử lý việc này một cách tự nhiên. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi truy vấn, hãy liệt kê mọi độ dài có thể-`w`chuỗi con là lựa chọn đầu tiên và mọi độ dài có thể-`w`chuỗi con làm lựa chọn thứ hai. Kiểm tra xem$$(a\cdot x+b)\bmod 9=k$$Ở đâu`a`là giá trị chuỗi con truy vấn modulo`9`, Và`x`,`b`là phần dư của độ dài đã chọn-`w`các chuỗi con. 

Có khoảng`n-w+1`ứng viên. Trong trường hợp xấu nhất thì đây là về`2·10^5`, vì vậy việc kiểm tra tất cả các cặp tốn khoảng$$(2\cdot10^5)^2 = 4\cdot10^{10}$$hoạt động cho mỗi truy vấn. Điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là modulo`9`thu gọn không gian tìm kiếm một cách đáng kể. 

Đối với bất kỳ số thập phân nào,$$value \bmod 9 = (\text{sum of digits}) \bmod 9.$$Mỗi chiều dài-`w`chuỗi con thuộc về một trong chín lớp duy nhất,`0`bởi vì`8`. 

Thay vì ghi nhớ từng giá trị chuỗi con, chúng ta chỉ quan tâm đến modulo dư của nó`9`. Đối với mỗi dư lượng`r`, chúng tôi lưu trữ các vị trí bắt đầu của tất cả chiều dài-`w`chuỗi con có giá trị modulo`9`bằng`r`. 

Bây giờ hãy xem xét một truy vấn. Cho phép$$a = v(l,r)\bmod 9.$$Chúng tôi cần dư lượng`x`Và`y`như vậy$$(a\cdot x+y)\bmod 9 = k.$$chỉ có`9×9=81`cặp dư lượng để kiểm tra. 

Đối với mỗi cặp dư lượng ứng viên, chúng ta có thể xác định ngay lập tức các vị trí hợp lệ nhỏ nhất về mặt từ điển từ danh sách được lưu trữ. Vì 81 là hằng số nên mỗi truy vấn sẽ trở thành`O(81)`. 

Nhiệm vụ còn lại là tính toán dư lượng chuỗi con một cách hiệu quả. Sử dụng tổng tiền tố của các chữ số, tổng chữ số của bất kỳ chuỗi con nào đều có sẵn ở dạng`O(1)`, do đó modulo của nó`9`giá trị cũng có sẵn trong`O(1)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m·n²) | O(n) | Quá chậm | 
| Tối ưu | O(n + 81m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố của các giá trị chữ số. 

Cho phép`pref[i]`là tổng của số đầu tiên`i`chữ số. Khi đó tổng các chữ số của chuỗi con bất kỳ là$$pref[r]-pref[l-1].$$2. Liệt kê mọi chiều dài-`w`chuỗi con. 

Đối với mỗi vị trí xuất phát`i`, tính tổng các chữ số của`s[i..i+w-1]`sử dụng tổng tiền tố và lấy nó theo modulo`9`. 
3. Nhóm các vị trí bắt đầu theo dư lượng. 

Tạo chín danh sách. Nếu một chiều dài-`w`chuỗi con có dư lượng`r`, nối vị trí bắt đầu của nó vào danh sách`pos[r]`. 

Vì chúng tôi quét từ trái sang phải nên các vị trí trong mỗi danh sách sẽ được sắp xếp tự động. 
4. Xử lý từng truy vấn. 

Tính toán$$a=v(l,r)\bmod 9.$$Một lần nữa, tổng các chữ số cho thấy điều này`O(1)`. 
5. Liệt kê tất cả các cặp dư`(x,y)`. 

Kiểm tra xem$$(a\cdot x+y)\bmod 9 = k.$$Chỉ những cặp như vậy mới có thể đóng góp vào câu trả lời. 
6. Đối với cặp dư lượng hợp lệ, hãy xác định vị trí có thể sử dụng nhỏ nhất. 

Nếu như`x != y`, sự lựa chọn tốt nhất là phần tử đầu tiên của`pos[x]`và phần tử đầu tiên của`pos[y]`. 

Nếu như`x == y`, chúng ta cần hai chuỗi con riêng biệt, do đó lớp dư lượng phải chứa ít nhất hai vị trí. Sự lựa chọn tốt nhất là hai vị trí đầu tiên trong danh sách đó. 
7. Trong số tất cả các câu trả lời của ứng viên, hãy giữ cặp câu trả lời nhỏ nhất về mặt từ điển. 

So sánh đầu tiên bằng`L1`, sau đó bởi`L2`. 
8. Xuất cặp đã lưu, hoặc`-1 -1`nếu không có ứng cử viên nào tồn tại. 

### Tại sao nó hoạt động 

Mỗi chiều dài-`w`chuỗi con chỉ được biểu diễn bằng modulo dư của nó`9`. Điều kiện truy vấn chỉ phụ thuộc vào các dư lượng sau:$$(v_1\cdot a+v_2)\bmod 9 = ((v_1\bmod9)\cdot(a\bmod9)+(v_2\bmod9))\bmod9.$$Vì vậy, việc thay thế mỗi chuỗi con bằng phần dư của nó sẽ không làm mất thông tin. 

Đối với cặp dư lượng cố định`(x,y)`, các vị trí có thể sử dụng nhỏ nhất về mặt từ điển luôn thu được từ các lần xuất hiện sớm nhất được lưu trữ trong danh sách tương ứng. Bất kỳ sự xuất hiện sau đó sẽ chỉ tăng lên`L1`hoặc`L2`. 

Thuật toán kiểm tra mọi cặp dư lượng có thể. Vì mỗi chuỗi con thuộc về chính xác một lớp dư lượng nên mọi câu trả lời hợp lệ đều tương ứng với một trong các cặp được kiểm tra. Trong số tất cả các ứng cử viên khả thi, cái nhỏ nhất về mặt từ điển sẽ được chọn, phù hợp với yêu cầu của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        s = input().strip()
        n = len(s)

        w, m = map(int, input().split())

        pref = [0] * (n + 1)
        for i, ch in enumerate(s, 1):
            pref[i] = pref[i - 1] + int(ch)

        pos = [[] for _ in range(9)]

        for start in range(1, n - w + 2):
            end = start + w - 1
            rem = (pref[end] - pref[start - 1]) % 9
            pos[rem].append(start)

        for _ in range(m):
            l, r, k = map(int, input().split())

            a = (pref[r] - pref[l - 1]) % 9

            best = None

            for x in range(9):
                if not pos[x]:
                    continue

                for y in range(9):
                    if (a * x + y) % 9 != k:
                        continue

                    if x != y:
                        if not pos[y]:
                            continue
                        cand = (pos[x][0], pos[y][0])
                    else:
                        if len(pos[x]) < 2:
                            continue
                        cand = (pos[x][0], pos[x][1])

                    if best is None or cand < best:
                        best = cand

            if best is None:
                print(-1, -1)
            else:
                print(best[0], best[1])

solve()
```Mảng tổng tiền tố lưu trữ tổng chữ số thay vì giá trị số. Modulo`9`, tổng chữ số và số tương đương nhau, điều này tránh được việc xử lý các số nguyên khổng lồ. 

Giai đoạn tiền xử lý tính toán phần dư của mỗi chiều dài-`w`chuỗi con. Mỗi vị trí bắt đầu được chèn vào một trong chín nhóm. Vì các vị trí được xử lý theo thứ tự tăng dần nên phần tử đầu tiên của nhóm tự động là vị trí bắt đầu nhỏ nhất có phần dư đó. 

Trong quá trình xử lý truy vấn, phần dư của`s[l..r]`được lấy từ cùng một tổng tiền tố. Vòng lặp kép kết thúc`x`Và`y`kiểm tra tất cả 81 sự kết hợp dư lượng có thể. Bất cứ khi nào một cặp thỏa mãn phương trình mô đun, các vị trí hợp lệ sớm nhất sẽ được trích xuất từ ​​các nhóm tương ứng. 

Trường hợp đặc biệt`x == y`đòi hỏi sự chăm sóc. Chúng ta cần hai chuỗi con khác nhau, vì vậy không thể sử dụng một nhóm chỉ có một vị trí. Hai vị trí đầu tiên cung cấp cặp hợp lệ nhỏ nhất về mặt từ điển. 

So sánh bộ dữ liệu Python thực hiện thuận tiện thứ tự được yêu cầu:`(a,b) < (c,d)`chính xác có nghĩa là nhỏ hơn`L1`, và nếu bằng thì nhỏ hơn`L2`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = 1003004
w = 4
query = (1,2,1)
```Chuỗi con có độ dài 4: 

| Bắt đầu | Chuỗi con | Tổng chữ số | Dư lượng | 
| --- | --- | --- | --- | 
| 1 | 1003 | 4 | 4 | 
| 2 | 0030 | 3 | 3 | 
| 3 | 0300 | 3 | 3 | 
| 4 | 3004 | 7 | 7 | 

Danh sách được lưu trữ: 

| Dư lượng | Vị trí | 
| --- | --- | 
| 3 | [2, 3] | 
| 4 | [1] | 
| 7 | [4] | 

Chuỗi con truy vấn: 

| tôi | r | Giá trị | Dư lượng | 
| --- | --- | --- | --- | 
| 1 | 2 | 10 | 1 | 

Chúng tôi cần:$$(1\cdot x+y)\bmod9=1$$Một cặp dư lượng hợp lệ là`(3,7)`bởi vì$$3+7=10\equiv1\pmod9.$$Điều này mang lại vị trí`(2,4)`. 

| x | y | Ứng viên | 
| --- | --- | --- | 
| 3 | 7 | (2,4) | 

Câu trả lời là:```
2 4
```Ví dụ này minh họa cách các giá trị chuỗi con thực sự biến mất khỏi bài toán. Chỉ có dư lượng quan trọng. 

### Ví dụ 2 

đầu vào:```
s = 111
w = 2
query = (2,2,6)
```Chuỗi con có độ dài-2: 

| Bắt đầu | Chuỗi con | Tổng chữ số | Dư lượng | 
| --- | --- | --- | --- | 
| 1 | 11 | 2 | 2 | 
| 2 | 11 | 2 | 2 | 

Danh sách được lưu trữ: 

| Dư lượng | Vị trí | 
| --- | --- | 
| 2 | [1,2] | 

Truy vấn dư lượng chuỗi con: 

| tôi | r | Dư lượng | 
| --- | --- | --- | 
| 2 | 2 | 1 | 

phương trình cần thiết:$$(1\cdot x+y)\bmod9=6$$Lựa chọn`x=y=2`cho$$2+2=4$$cái đó không hoạt động. 

Kiểm tra tất cả 81 cặp dư lượng không tìm thấy giải pháp. 

Đầu ra:```
-1 -1
```Dấu vết này cho thấy thuật toán loại bỏ chính xác các kết hợp dư lượng ngay cả khi tồn tại đủ số lần xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 81m) | Tiền xử lý là tuyến tính, mỗi truy vấn kiểm tra 81 cặp dư | 
| Không gian | O(n) | Lưu trữ tất cả chiều dài-`w`vị trí xuất phát | 

Vì tổng cộng`n`và tổng cộng`m`trên tất cả các trường hợp thử nghiệm đều có nhiều nhất`2·10^5`, độ phức tạp là tuyến tính một cách hiệu quả. Hệ số không đổi của 81 cặp cặn là rất nhỏ, khiến dung dịch dễ dàng nằm gọn trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup = sys.stdout
    sys.stdout = out

    def solve():
        input = sys.stdin.readline

        t = int(input())
        for _ in range(t):
            s = input().strip()
            n = len(s)

            w, m = map(int, input().split())

            pref = [0] * (n + 1)
            for i, ch in enumerate(s, 1):
                pref[i] = pref[i - 1] + int(ch)

            pos = [[] for _ in range(9)]

            for st in range(1, n - w + 2):
                rem = (pref[st + w - 1] - pref[st - 1]) % 9
                pos[rem].append(st)

            for _ in range(m):
                l, r, k = map(int, input().split())

                a = (pref[r] - pref[l - 1]) % 9
                best = None

                for x in range(9):
                    if not pos[x]:
                        continue

                    for y in range(9):
                        if (a * x + y) % 9 != k:
                            continue

                        if x != y:
                            if not pos[y]:
                                continue
                            cand = (pos[x][0], pos[y][0])
                        else:
                            if len(pos[x]) < 2:
                                continue
                            cand = (pos[x][0], pos[x][1])

                        if best is None or cand < best:
                            best = cand

                if best is None:
                    print("-1 -1")
                else:
                    print(best[0], best[1])

    solve()

    sys.stdout = backup
    return out.getvalue()

# sample
assert run("""1
1003004
4 1
1 2 1
""") == "2 4\n"

# minimum size
assert run("""1
10
1 1
1 1 1
""") == "1 2\n"

# all zeros
assert run("""1
0000
1 1
1 4 0
""") == "1 2\n"

# no solution
assert run("""1
111
2 1
2 2 6
""") == "-1 -1\n"

# same residue requires two occurrences
assert run("""1
000
1 1
1 3 0
""") == "1 2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10`,`w=1`|`1 2`| Trường hợp pháp lý nhỏ nhất | 
|`0000`,`w=1`|`1 2`| Số 0 đứng đầu và dư lượng 0 | 
|`111`,`w=2`|`-1 -1`| Sửa lỗi bác bỏ khi phương trình không thể thực hiện được | 
|`000`,`w=1`|`1 2`| Cần có các vị trí riêng biệt cho cùng một dư lượng | 

## Vỏ cạnh 

Hãy xem xét:```
1
0000
1 1
1 4 0
```Mỗi chuỗi con có độ dài 1 đều có phần dư`0`. Cái xô là:```
pos[0] = [1,2,3,4]
```Chuỗi con truy vấn cũng có dư lượng`0`, do đó phương trình trở thành:$$0\cdot x+y\equiv0\pmod9.$$Lựa chọn`x=y=0`là hợp lệ. Vì số dư bằng nhau nên thuật toán yêu cầu hai vị trí riêng biệt và trả về hai lần xuất hiện đầu tiên,`(1,2)`. Điều này xử lý chính xác các số 0 đứng đầu. 

Bây giờ hãy xem xét:```
1
12
1 1
1 1 5
```Số thùng còn lại là:```
pos[1] = [1]
pos[2] = [2]
```Thuật toán kiểm tra tất cả 81 cặp dư lượng. Nếu không có phương trình nào thỏa mãn thì`best`còn lại`None`Và`-1 -1`được in. Không có câu trả lời ngẫu nhiên nào được tạo ra. 

Cuối cùng, hãy xem xét trường hợp cùng một dư lượng phải được sử dụng hai lần:```
1
000
1 1
1 3 0
```Đây:```
pos[0] = [1,2,3]
```Cặp dư lượng`(0,0)`thỏa mãn phương trình. Thuật toán không sử dụng`(1,1)`. Thay vào đó, nó chiếm hai vị trí đầu tiên một cách rõ ràng, tạo ra`(1,2)`, tôn trọng yêu cầu các chuỗi con được chọn phải khác nhau.
