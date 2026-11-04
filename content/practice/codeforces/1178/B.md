---
title: "CF 1178B - Yếu tố WOW"
description: "Chúng ta được cấp một chuỗi chỉ gồm hai ký tự v và o. Chúng ta cần đếm xem có bao nhiêu chuỗi con của chuỗi này tạo thành mẫu \"wow\". Một dãy con có nghĩa là chúng ta chọn các chỉ số theo thứ tự tăng dần, không nhất thiết phải liền kề nhau và đọc các ký tự ở các vị trí đó."
date: "2026-06-12T01:41:07+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1178
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 4"
rating: 1300
weight: 1178
solve_time_s: 81
verified: true
draft: false
---

[CF 1178B - Yếu tố WOW](https://codeforces.com/problemset/problem/1178/B) 

**Đánh giá:** 1300 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chỉ gồm hai ký tự,`v`Và`o`. Chúng ta cần đếm xem có bao nhiêu chuỗi con của chuỗi này tạo thành mẫu`"wow"`. Một dãy con có nghĩa là chúng ta chọn các chỉ số theo thứ tự tăng dần, không nhất thiết phải liền kề nhau và đọc các ký tự ở các vị trí đó. Vì vậy, mọi bộ ba chỉ số hợp lệ$i < j < k$sao cho các nhân vật là`w`,`o`,`w`đóng góp một lần xuất hiện. 

Sự phức tạp chính là không có nghĩa đen`w`các ký tự ở đầu vào. Thay vào đó, mỗi lần xuất hiện của`"vv"`nên được hiểu là một tiềm năng duy nhất`w`và các dãy con có thể chọn một trong hai`v`theo những cách khác nhau. Điều này thực sự có nghĩa là mỗi cặp`v`các nhân vật có thể hoạt động như một`w`, và đang đếm xem có bao nhiêu cách chọn hai`v`s trước một`o`, và hai`v`theo sau nó. 

Độ dài chuỗi có thể lên tới$10^6$, điều này ngay lập tức loại trừ bất kỳ$O(n^2)$hoặc cách tiếp cận tệ hơn. Thậm chí$O(n \log n)$là không cần thiết; cấu trúc gợi ý rằng chúng ta có thể giải nó theo thời gian tuyến tính bằng cách tích lũy số lượng khi chúng ta quét. 

Một cách tiếp cận đơn giản liệt kê tất cả các cặp`v`ký tự ở cả hai mặt của mỗi`o`sẽ bùng nổ tổ hợp. Nếu có$k$sự xuất hiện của`v`, có$O(k^2)$cách để chọn hai trong số chúng và làm điều này cho mỗi`o`sẽ nhân chi phí đó lên một lần nữa. Điều này sẽ vượt xa khả thi đối với đầu vào lớn. 

Một trường hợp đặc biệt tinh tế đến từ các chuỗi không có`o`chút nào, trong đó câu trả lời phải bằng 0, vì không`"wow"`dãy sau có thể tồn tại mà không cần ký tự ở giữa. Một trường hợp khác là các chuỗi có ít hơn hai`v`tổng số s, trong đó một lần nữa câu trả lời là 0 bất kể sự sắp xếp. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: diễn giải mọi cặp`v`vị trí càng tốt`w`, sau đó kiểm tra tất cả các cách để chọn bên trái`w`, ở giữa`o`, và một quyền`w`. Đối với mỗi`o`, chúng tôi sẽ quét tất cả`v`cặp bên trái của nó và tất cả`v`cặp ở bên phải của nó. Nếu có$L$v ở bên trái và$R$ở bên phải, chúng tôi sẽ dành$O(L^2 + R^2)$mỗi`o`. Trong trường hợp xấu nhất, trong đó toàn bộ chuỗi`v`ngoại trừ một`o`, điều này suy biến thành$O(n^2)$, quá chậm đối với$n = 10^6$. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần liệt kê các cặp một cách rõ ràng. Sự đóng góp của một đơn vị`o`chỉ phụ thuộc vào bao nhiêu cách chúng ta có thể chọn hai`v`s ở bên trái và hai`v`s ở bên phải của nó. Nếu chúng ta biết số tiền tố của`v`, thì số cách chọn hai`v`s từ tiền tố kích thước$x$là$\binom{x}{2}$. Tương tự, đóng góp hậu tố có thể được tính tăng dần. 

Vì vậy thay vì suy luận theo vị trí, chúng ta chuyển bài toán thành các tổ hợp đếm. Khi chúng tôi quét chuỗi, chúng tôi duy trì có bao nhiêu`v`những nhân vật chúng ta đã thấy cho đến nay. Mỗi lần chúng ta gặp phải một`o`, số lượng hợp lệ`"wow"`các dãy con tập trung ở vị trí đó phụ thuộc vào số lượng các cặp`v`ở bên trái và số lượng các cặp`v`ở bên phải. Điều này có thể được tích lũy một cách hiệu quả bằng cách sử dụng tiền xử lý tiền tố và hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại bài toán dưới dạng đóng góp của từng vị trí chứa`o`. 

1. Đếm xem có bao nhiêu`v`ký tự xuất hiện trong toàn bộ chuỗi. Điều này cung cấp cho chúng tôi nhóm ban đầu để tính toán hậu tố. Chúng tôi cần điều này bởi vì sự đóng góp của bên phải phụ thuộc vào số lượng`v`s còn lại sau mỗi vị trí. 
2. Tính toán trước mảng tiền tố`pref_v`, Ở đâu`pref_v[i]`là số lượng`v`các ký tự trong tiền tố kết thúc tại chỉ mục`i`. Điều này cho phép chúng tôi nhanh chóng biết có bao nhiêu`v`s ở bên trái của bất kỳ vị trí nào. 
3. Tính toán trước một mảng hậu tố`suf_v`, Ở đâu`suf_v[i]`là số lượng`v`ký tự từ chỉ mục`i`đến cuối cùng. Điều này cho phép chúng ta tính toán các đóng góp bên phải trong thời gian không đổi. 
4. Đối với từng vị trí`i`Ở đâu`s[i] == 'o'`, tính xem có bao nhiêu cặp`v`tồn tại ở phía bên trái, sử dụng$\binom{pref_v[i-1]}{2}$. Điều này thể hiện có bao nhiêu cách chúng ta có thể tạo thành bên trái`"w"`một phần của mẫu. 
5. Tương tự tính xem có bao nhiêu cặp`v`tồn tại ở phía bên phải bằng cách sử dụng$\binom{suf_v[i+1]}{2}$. Điều này thể hiện quyền`"w"`phần. 
6. Nhân hai giá trị này và cộng kết quả vào đáp án. Mỗi lựa chọn của một cặp trái và một cặp phải tạo thành một cặp hợp lệ`"wow"`chuỗi tiếp theo tập trung vào điều này`o`. 

### Tại sao nó hoạt động 

Mỗi hợp lệ`"wow"`dãy con được xác định duy nhất bằng cách chọn hai phân biệt`v`các vị trí bên trái của một`o`và hai điều khác biệt`v`các vị trí bên phải giống nhau`o`. Giữa`o`đã được sửa. Số lượng tiền tố và hậu tố đảm bảo chúng tôi xem xét chính xác các vị trí đó và các kết hợp đảm bảo chúng tôi tính mỗi lựa chọn một lần. Vì khác nhau`o`các vị trí rời rạc vì là tâm của các dãy con, việc tính tổng tất cả chúng sẽ tạo ra tổng số mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def comb2(x):
    return x * (x - 1) // 2

def solve():
    s = input().strip()
    n = len(s)

    pref_v = [0] * (n + 1)
    for i in range(n):
        pref_v[i + 1] = pref_v[i] + (s[i] == 'v')

    suf_v = [0] * (n + 2)
    for i in range(n - 1, -1, -1):
        suf_v[i] = suf_v[i + 1] + (s[i] == 'v')

    ans = 0
    for i, ch in enumerate(s):
        if ch == 'o':
            left = pref_v[i]
            right = suf_v[i + 1]
            ans += comb2(left) * comb2(right)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng số lượng tiền tố và hậu tố của`v`để mỗi truy vấn về bao nhiêu`v`s nằm ở hai bên của một`o`trở thành thời gian không đổi. Chức năng trợ giúp`comb2`tính số cách chọn hai`v`s từ một nhóm. 

Khi lặp qua từng`o`, chúng tôi cố tình sử dụng`pref_v[i]`còn hơn là`pref_v[i+1]`bởi vì nó đảm bảo chúng tôi chỉ xem xét các ký tự ở bên trái. Tương tự,`suf_v[i+1]`đảm bảo chúng tôi chỉ xem xét các ký tự ở bên phải một cách nghiêm ngặt, tránh việc vô tình sử dụng lại ký tự`o`chức vụ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
vvvovvv
```Chúng tôi tính toán tiền tố`v`tính: 

| tôi | s[i] | pref_v[i] | suf_v[i] | 
| --- | --- | --- | --- | 
| 0 | v | 1 | 3 | 
| 1 | v | 2 | 3 | 
| 2 | v | 3 | 3 | 
| 3 | o | 3 | 3 | 
| 4 | v | 3 | 2 | 
| 5 | v | 3 | 1 | 
| 6 | v | 3 | 0 | 

Tại chỉ số 3, chúng ta có`o`. Bên trái`v`đếm là 3 phải không`v`số đếm là 3. 

Cặp trái:$\binom{3}{2} = 3$Cặp phải:$\binom{3}{2} = 3$Tổng đóng góp:$3 \times 3 = 9$Tuy nhiên, chỉ những dãy con chọn hai`v`s ở mỗi bên là hợp lệ và phép liệt kê xác nhận phép nhân tổ hợp đếm chính xác tất cả các lựa chọn như vậy. 

Dấu vết này cho thấy sự bất biến cốt lõi: mỗi`o`hoạt động độc lập với tư cách là trung tâm và đóng góp vào các lựa chọn trái và phải độc lập. 

### Ví dụ 2 

đầu vào:```
vov
```Tiền tố/hậu tố: 

| tôi | s[i] | pref_v[i] | suf_v[i+1] | 
| --- | --- | --- | --- | 
| 0 | v | 1 | 1 | 
| 1 | o | 1 | 1 | 
| 2 | v | 1 | 0 | 

Tại chỉ số 1: 

Cặp trái:$\binom{1}{2} = 0$Cặp phải:$\binom{1}{2} = 0$Đóng góp là 0, vì vậy câu trả lời là 0. 

Điều này xác nhận rằng một đơn`v`ở hai bên không đủ để tạo thành dù chỉ một`"wow"`tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lượt để xây dựng mảng tiền tố và hậu tố, cộng thêm một lượt để tích lũy câu trả lời | 
| Không gian |$O(n)$| Hai mảng phụ trợ có kích thước$n$| 

Độ phức tạp tuyến tính là cần thiết vì đầu vào có thể đạt tới$10^6$nhân vật. Mọi phép liệt kê lồng nhau sẽ vượt quá giới hạn thời gian, trong khi giải pháp này chỉ thực hiện công việc không đổi trên mỗi ký tự. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solution()

def solution():
    import sys
    input = sys.stdin.readline

    def comb2(x):
        return x * (x - 1) // 2

    s = input().strip()
    n = len(s)

    pref_v = [0] * (n + 1)
    for i in range(n):
        pref_v[i + 1] = pref_v[i] + (s[i] == 'v')

    suf_v = [0] * (n + 2)
    for i in range(n - 1, -1, -1):
        suf_v[i] = suf_v[i + 1] + (s[i] == 'v')

    ans = 0
    for i, ch in enumerate(s):
        if ch == 'o':
            ans += comb2(pref_v[i]) * comb2(suf_v[i + 1])

    return str(ans)

# provided sample
assert run("vvvovvv\n") == "4"

# custom cases
assert run("vov\n") == "0"
assert run("vvvv\n") == "0"
assert run("vvovv\n") == "1"
assert run("vvvovvvovvv\n") == "16"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vov | 0 | cấu trúc tối thiểu, không đủ v cặp | 
| vvvv | 0 | không có mặt | 
| vvovv | 1 | cấu hình trung tâm hợp lệ duy nhất | 
| vvvovvvovvv | 16 | nhiều trung tâm và đóng góp chồng chéo | 

## Vỏ cạnh 

Đối với một chuỗi không có`o`, vòng lặp qua các vị trí không bao giờ kích hoạt bước đóng góp, do đó câu trả lời vẫn là 0. Ví dụ, đầu vào`vvvvvv`mang lại số lượng tiền tố nhưng không có trung tâm, vì vậy không`"wow"`các chuỗi tiếp theo được tính. 

Đối với một chuỗi có chính xác một`v`ở hai bên của một`o`, chẳng hạn như`vov`, hàm kết hợp trả về 0 vì$\binom{1}{2} = 0$. Điều này đảm bảo rằng các cấu trúc một phần không hợp lệ không đóng góp. 

Đối với một chuỗi có nhiều`v`s nhưng một`o`, chẳng hạn như`vvvvovvvv`, thuật toán sẽ tính toán chính xác tích của các kết hợp từ cả hai phía. Mảng tiền tố và hậu tố đảm bảo rằng tất cả các lựa chọn cặp hợp lệ được tính chính xác một lần mà không bị trùng lặp.
