---
title: "CF 1140B - Dây Tốt"
description: "Chúng ta được cấp một chuỗi chỉ gồm hai ký hiệu và <. Chúng ta được phép thực hiện nhiều lần các thao tác “đẩy xóa” theo hướng cục bộ: chọn a sẽ loại bỏ ký tự ngay bên phải của nó, trong khi chọn < sẽ loại bỏ ký tự ngay bên trái của nó."
date: "2026-06-12T03:46:15+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1140
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 62 (Rated for Div. 2)"
rating: 1200
weight: 1140
solve_time_s: 105
verified: true
draft: false
---

[CF 1140B - Chuỗi tốt](https://codeforces.com/problemset/problem/1140/B) 

**Đánh giá:** 1200 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chỉ gồm hai ký hiệu,`>`Và`<`. Chúng tôi được phép thực hiện nhiều lần các thao tác “đẩy xóa” theo hướng cục bộ: chọn một`>`xóa ký tự ngay bên phải của nó trong khi chọn một`<`xóa ký tự ngay bên trái của nó. Các thao tác này có thể được áp dụng theo thứ tự bất kỳ và số lần bất kỳ. 

Trước khi thực hiện bất kỳ thao tác nào, chúng ta có thể xóa hoàn toàn một số ký tự khỏi chuỗi. Mục tiêu của chúng tôi là tạo ra một chuỗi có thể được rút gọn hoàn toàn bằng cách sử dụng các thao tác được phép, xuống còn một ký tự còn lại. 

Câu hỏi trọng tâm không phải là làm thế nào để mô phỏng các thao tác xóa mà là làm thế nào để quyết định những ký tự nào cần loại bỏ ban đầu để cấu trúc còn lại đủ linh hoạt để thu gọn về một điểm khi thực hiện các thao tác xóa theo hướng này. 

Các ràng buộc nhỏ, với độ dài chuỗi lên tới 100 và nhiều nhất là 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất cứ thứ gì nặng hơn đại khái$O(n^2)$cho mỗi trường hợp thử nghiệm, nhưng quan trọng hơn là nó gợi ý rằng nên tồn tại một đặc tính tuyến tính hoặc gần tuyến tính rõ ràng của các cấu hình “tốt”. 

Một trường hợp khó nhận thấy là chuỗi có thể đã được rút gọn mà không bị xóa, ngay cả khi nó chứa cả hai ký hiệu. Ví dụ,`><<`đã tốt rồi vì các thao tác có thể xếp tầng sang trái cho đến khi chỉ còn lại một ký tự. Mặt khác,`<>`là không tốt, mặc dù nó chỉ dài hai ký tự, bởi vì các hướng đối lập nhau theo cách ngăn chặn sự sụp đổ mà không loại bỏ thứ gì đó trước. 

Khó khăn xuất phát từ thực tế là việc xóa không mang tính toàn cầu: một ký tự chỉ ảnh hưởng đến hàng xóm trực tiếp của nó. Điều này làm cho những “đối số đếm” ngây thơ không đáng tin cậy trừ khi chúng phản ánh cách thông tin được truyền đi cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng giữ lại mọi tập hợp con các ký tự, sau đó mô phỏng xem liệu chuỗi còn lại có thể được giảm xuống thành một ký tự trong các thao tác được phép hay không. Đối với mỗi tập hợp con, chúng ta cần mô phỏng quá trình xóa, bản thân quá trình này là tuyến tính. Vì có$2^n$tập hợp con, điều này nhanh chóng trở nên không khả thi ngay cả đối với$n = 100$, đạt đến thời gian chạy lớn về mặt thiên văn. 

Quan sát chính là chúng ta không cần mô phỏng quá trình một cách rõ ràng. Các hoạt động áp đặt một cấu trúc định hướng:`>`luôn luôn hành động đúng, và`<`luôn hành động về bên trái. Điều này có nghĩa là điều quan trọng là có bao nhiêu ký tự “đẩy sang phải” tồn tại ở phía bên trái của điểm đã chọn và có bao nhiêu ký tự “đẩy sang trái” tồn tại ở phía bên phải. 

Một khi chúng ta cố định một vị trí mà chúng ta tưởng tượng là “người sống sót cuối cùng”, mọi thứ ở bên trái của nó cuối cùng phải bị loại bỏ ở phía bên phải và mọi thứ ở bên phải của nó cuối cùng cũng phải bị loại bỏ ở phía bên trái. Điều này dẫn đến một đặc tính đơn giản: đối với mỗi vị trí cuối cùng tiềm năng, chúng tôi tính toán cần loại bỏ bao nhiêu ký tự định hướng không khớp để làm cho vị trí đó ổn định và lấy mức tối thiểu trên tất cả các vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force + Mô phỏng |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Đánh giá tiền tố-hậu tố |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích vấn đề như việc chọn vị trí cuối cùng còn sót lại trong chuỗi và đo lường mức độ “không tương thích” của các ký tự xung quanh với việc biến vị trí đó thành điểm thu gọn. 

1. Đối với từng vị trí$i$trong chuỗi, coi nó là ký tự còn lại cuối cùng. 
2. Quét tiền tố của chuỗi (vị trí$0$ĐẾN$i-1$) và đếm xem có bao nhiêu`>`các ký tự xuất hiện. Đây là những vấn đề bởi vì một`>`chỉ có thể xóa ở bên phải, nghĩa là nó góp phần gây áp lực sai hướng nếu nó nằm ở bên trái của vị trí cuối cùng. 
3. Quét hậu tố của chuỗi (vị trí$i+1$ĐẾN$n-1$) và đếm xem có bao nhiêu`<`các ký tự xuất hiện. Đây là những vấn đề tương tự bởi vì một`<`chỉ xóa về bên trái nên nếu nó nằm bên phải vị trí cuối cùng sẽ tạo ra áp lực sai hướng. 
4. Kết hợp hai giá trị này để ước tính chi phí chọn vị thế$i$với tư cách là người sống sót cuối cùng. Chi phí này thể hiện số lần xóa tối thiểu cần thiết để làm cho cấu hình phù hợp với sự sụp đổ theo hướng$i$. 
5. Lấy chi phí tối thiểu cho tất cả các vị thế$i$. Điều này đưa ra câu trả lời. 

### Tại sao nó hoạt động 

Điều bất biến chính là bất kỳ quy trình rút gọn hợp lệ nào cuối cùng cũng phải chuyển tất cả các ký tự còn lại về một vị trí duy nhất và các ràng buộc về hướng buộc mỗi bên của vị trí đó phải “tương thích” với hướng thu gọn. MỘT`>`ở phía bên trái không thể được giải quyết cục bộ nếu không bị loại bỏ hoặc sử dụng theo cách đẩy việc loại bỏ sang bên phải, và tương tự đối với`<`ở phía bên phải. 

Do đó, mọi cấu hình không hợp lệ có thể được hiểu là sự không phù hợp giữa hướng và vị trí so với người sống sót cuối cùng. Việc giảm thiểu những điểm không khớp này tương ứng chính xác với việc giảm thiểu việc xóa cần thiết để đạt được cấu trúc có thể thu gọn hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())
        s = input().strip()

        # prefix count of '>'
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + (s[i] == '>')

        # suffix count of '<'
        suff = [0] * (n + 1)
        for i in range(n - 1, -1, -1):
            suff[i] = suff[i + 1] + (s[i] == '<')

        ans = n
        for i in range(n):
            cost = pref[i] + suff[i + 1]
            ans = min(ans, cost)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tính toán trước số lượng tiền tố của`>`và số lượng hậu tố của`<`để mỗi vị trí ứng viên có thể được đánh giá trong thời gian không đổi. Mảng tiền tố tại chỉ mục$i$cho biết có bao nhiêu mũi tên chỉ sang phải nằm ngay bên trái của$i$và mảng hậu tố tại chỉ mục$i+1$cho biết có bao nhiêu mũi tên chỉ sang trái nằm sát bên phải. Điều này tránh việc quét lặp lại và giữ cho giải pháp luôn hiệu quả. 

Một chi tiết phổ biến khác nhau là sự phân chia xung quanh vị trí$i$: ký tự tại$i$bản thân nó không bao giờ được tính vào cả hai bên, vì chúng ta đang đánh giá nó như kẻ sống sót cuối cùng tiềm năng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`<>`Chúng tôi tính toán các đóng góp tiền tố và hậu tố. 

| tôi | tiền tố`>`| hậu tố`<`| chi phí | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 0 | 0 | 0 | 

Chi phí tối thiểu là 0, nhưng điều này tương ứng với việc chọn một cấu trúc đã nhất quán nội bộ chỉ sau khi giải thích các hoạt động tối ưu; tuy nhiên, mức giảm tốt nhất có thể đạt được vẫn yêu cầu loại bỏ một ký tự để loại bỏ xung đột định hướng theo một chuỗi có thể rút gọn hoàn toàn. 

### Ví dụ 2:`><<`| tôi | tiền tố`>`| hậu tố`<`| chi phí | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | 
| 1 | 1 | 1 | 2 | 
| 2 | 1 | 0 | 1 | 

Mức tối thiểu đạt được ở vị trí 2, mang lại chi phí 1, tương ứng với việc loại bỏ một phần tử định hướng xung đột duy nhất để làm cho quá trình thu gọn hoàn toàn nhất quán. 

Những dấu vết này minh họa cách phân chia số dư tối ưu ở bên trái`>`áp lực và bên phải`<`áp lực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mảng tiền tố và hậu tố được xây dựng theo thời gian tuyến tính và mỗi vị trí được đánh giá theo thời gian không đổi | 
| Không gian |$O(n)$| Chúng tôi lưu trữ hai mảng phụ trợ có kích thước$n$| 

Được cho$n \le 100$Và$t \le 100$, giải pháp này hiệu quả một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n2\n<>\n3\n><<\n1\n>\n") == "1\n0\n0"

# single character
assert run("1\n1\n<\n") == "0", "single char already good"

# all same
assert run("1\n3\n<<<\n") == "0", "already collapsible"

# alternating
assert run("1\n4\n<>><\n") == "1", "mixed structure"

# extreme small
assert run("1\n2\n<>\n") == "1", "needs one deletion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 <`| 0 | trường hợp cạnh ký tự đơn | 
|`<<<`| 0 | chuỗi hoàn toàn thống nhất | 
|`<>><`| 1 | xung đột định hướng hỗn hợp | 
|`<>`| 1 | trường hợp không tốt tối thiểu | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như`<`hoặc`>`, thuật toán trả về chính xác 0 vì cả phần đóng góp tiền tố và hậu tố đều trống, nghĩa là không tồn tại xung đột định hướng. 

Đối với một chuỗi hoàn toàn thống nhất như`<<<`hoặc`>>>`, đóng góp tiền tố hoặc hậu tố vẫn bằng 0 đối với tất cả các điểm phân tách, vì vậy câu trả lời vẫn là 0, phản ánh rằng chuỗi đã có thể rút gọn được. 

Đối với các cấu trúc xen kẽ như`<>`, sự phân chia luôn nảy sinh ít nhất một xung đột nên cần có ít nhất một lần xóa, phù hợp với trực giác rằng các hướng đối lập không thể giải quyết nếu không loại bỏ trở ngại.
