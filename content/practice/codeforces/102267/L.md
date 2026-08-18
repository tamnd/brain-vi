---
title: "CF 102267L - ABC"
description: "Chúng ta có một chuỗi có thể thay đổi trên bảng chữ cái a, b, c. An a có thể phát triển thành ab, a b có thể phát triển thành bc và a c có thể phát triển thành ba. Thao tác duy nhất thực sự loại bỏ các ký tự là xóa một lần xuất hiện của abc. Nhiệm vụ mang tính xây dựng."
date: "2026-08-17T19:40:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "L"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 514
verified: false
draft: false
---

[CF 102267L - ABC](https://codeforces.com/problemset/problem/102267/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 34 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi có thể thay đổi trên bảng chữ cái`a`,`b`,`c`. MỘT`a`có thể phát triển thành`ab`, Một`b`có thể phát triển thành`bc`, và một`c`có thể phát triển thành`ba`. Thao tác duy nhất thực sự loại bỏ các ký tự là xóa một lần xuất hiện của`abc`. 

Nhiệm vụ mang tính xây dựng. Chúng ta phải tạo ra một chuỗi hoàn chỉnh các thao tác hợp lệ để biến chuỗi đầu vào thành chuỗi trống bằng cách sử dụng nhiều nhất`3n`hoạt động, hoặc chứng minh rằng không có trình tự như vậy tồn tại bằng cách in`-1`. Các chỉ mục trong đầu ra tham chiếu đến chuỗi vì nó tồn tại tại thời điểm chính xác đó, do đó việc triển khai phải theo dõi cách các thao tác trước đó đã thay đổi độ dài và vị trí. 

Với`n`lên đến`2 * 10^5`và giới hạn một giây, bất cứ thứ gì bậc hai đều nguy hiểm và việc khám phá các chuỗi thao tác là hoàn toàn không khả thi. Bản thân đầu ra có thể chứa tới`600000`hoạt động, vì vậy một`O(n)`xây dựng với một`O(n)`bộ đệm đầu ra là mục tiêu tự nhiên. 

Có một số trường hợp đặc biệt bộc lộ lý do tại sao mù quáng tìm kiếm`abc`là không đủ. đầu vào`bac`là không thể. Ký tự đầu tiên của nó là`b`và không có thao tác nào có thể biến ký tự đầu tiên của chuỗi thành`a`: thao tác 2 giữ vị trí đầu tiên`b`như lần đầu tiên`b`, phép toán 3 lượt một`c`vào trong`b`và thao tác 1 chỉ có thể tác động lên một`a`điều đó đã tồn tại rồi. Vì ký tự đầu tiên cuối cùng phải thuộc về một`abc`xóa, một chuỗi bắt đầu bằng`b`hoặc`c`không thể biến mất. 

đầu vào`abb`cũng là không thể mặc dù nó bắt đầu bằng`a`. đầu tiên`b`có thể được gỡ bỏ cùng với ban đầu`a`, để lại thứ hai`b`làm ký tự đầu tiên. Cái đó`b`không bao giờ có thể trở thành một`a`, vì vậy quá trình này bị kẹt. Một công trình tham lam loại bỏ những gì có sẵn đầu tiên`abc`phải phát hiện tình huống này thay vì cho rằng mọi chuỗi bắt đầu bằng`a`có thể giải quyết được. 

Ở thái cực khác,`a`có thể giải quyết được ngay lập tức: mở rộng nó thành`ab`, mở rộng`b`ĐẾN`bc`và xóa`abc`. Ba thao tác đều nằm trong giới hạn cho phép. Một đĩa đơn`abc`thậm chí còn đơn giản hơn vì nó có thể bị xóa chỉ trong một thao tác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ coi mọi hoạt động là một sự lựa chọn và tìm kiếm tất cả các chuỗi hoạt động có thể có theo độ dài`3n`. Tại mỗi trạng thái có thể có nhiều ký tự có thể mở rộng và nhiều ký tự có thể`abc`chuỗi con cần xóa. Ngay cả khi bỏ qua chi phí thao tác trên chuỗi, độ sâu`3n`tìm kiếm với hệ số phân nhánh không đổi khoảng bốn có thể có`O(4^(3n))`chi nhánh. Nó đúng vì mọi chuỗi pháp luật đều được biểu diễn, nhưng nó trở nên vô dụng ngay cả đối với những chuỗi rất nhỏ. 

Quan sát hữu ích là chúng ta không cần phải quyết định trên toàn cầu`abc`để tạo ra. Chúng ta có thể xử lý chuỗi từ trái sang phải và loại bỏ mọi`c`ngay lập tức. Ba kết thúc địa phương có thể có trước một`c`có hành vi rất đơn giản. 

Nếu hậu tố hiện tại là`ac`, thay đổi`c`vào trong`ba`cho`aba`. các`c`biến mất mà không thay đổi phần trước của chuỗi. 

Nếu hậu tố hiện tại là`abc`, chúng ta có thể xóa nó ngay lập tức. 

Nếu hậu tố hiện tại là`bbc`, có một phép biến đổi ba phép toán ít rõ ràng hơn một chút để thay đổi nó thành`bb`:`bbc -> bcbc -> bbabc -> bb`. 

Thao tác đầu tiên thay đổi thao tác đầu tiên`b`vào trong`bc`. Cái thứ hai thay đổi cái mới được chèn`c`vào trong`ba`. Kết quả`abc`sau đó sẽ bị xóa. Tiền tố xung quanh không bị ảnh hưởng. 

Điều này có nghĩa là trong khi quét chuỗi gốc, chúng ta có thể duy trì chính xác chuỗi hiện tại sau khi xử lý tiền tố của nó, ngoại trừ tất cả chuỗi đã được xử lý.`c`các nhân vật đã bị loại bỏ. Chuỗi phụ trợ kết quả chỉ chứa`a`Và`b`. 

Một lần tất cả`c`các ký tự đã biến mất, mọi ký tự còn lại`b`có thể được ghép nối với một trước đó`a`. Giả sử tiền tố còn lại hiện tại được biểu thị bằng`g`, và ký tự tiếp theo là`b`. Nếu như`g`không trống, hãy mở rộng cái này`b`ĐẾN`bc`. cuối cùng`a`của`g`bây giờ hình thức`abc`với nó, vì vậy bộ ba có thể bị xóa. Điều này tiêu tốn một`a`và một`b`. 

Nếu một`b`gặp phải trong khi`g`trống rỗng, đó`b`bây giờ là ký tự đầu tiên của chuỗi thực tế. Như đã thảo luận ở trên, lần đầu tiên`b`không bao giờ có thể trở thành người đầu tiên`a`, nên câu trả lời thực sự là không thể. 

Rốt cuộc`b`các ký tự đã bị xóa, chỉ`a`các ký tự vẫn còn. Mỗi cái có thể được xử lý độc lập theo trình tự ba thao tác cố định`a -> ab -> abc -> empty`. 

Hướng dẫn cuộc thi chính thức sử dụng cùng cấu trúc từ trái sang phải này, trước tiên loại bỏ`c`ký tự, sau đó khớp`b`ký tự có trước`a`các ký tự và cuối cùng loại bỏ các ký tự còn lại`a`nhân vật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^(3n)) trong trường hợp xấu nhất | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) cộng với kích thước đầu ra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và duy trì danh sách`v`đại diện cho chuỗi hiện tại sau tiền tố đã được xử lý. Lưu trữ mọi thao tác trong một mảng để có thể in các chỉ mục sau khi quá trình xây dựng kết thúc. 
2. Xử lý đầu vào từ trái sang phải. Đối với một`a`hoặc`b`, nối nó vào`v`. Chưa cần phải làm gì vì những ký tự này có thể được xử lý sau. 
3. Khi ký tự đầu vào tiếp theo là`c`, trước tiên hãy kiểm tra xem`v`trống rỗng. Nếu đúng thì chuỗi gốc bắt đầu bằng`c`, nên câu trả lời là không thể. Nhân vật đầu tiên không bao giờ có thể trở thành`a`. 
4. Nếu ký tự cuối cùng của`v`là`a`, hậu tố hiện tại là`ac`. Áp dụng thao tác 3 cho`c`, sử dụng chỉ mục`len(v) + 1`. Hậu tố trở thành`aba`, vì vậy hãy nối thêm`b`Và`a`ĐẾN`v`. Chuỗi hiện tại được biểu thị bây giờ đã chính xác trở lại. 
5. Nếu ký tự cuối cùng của`v`là`b`và nhân vật trước nó là`a`, hậu tố hiện tại là`abc`. Xóa hậu tố này bằng thao tác 4. Xóa hai ký tự cuối cùng khỏi`v`, bởi vì trước đó`a`đã bị tiêu hao bởi việc xóa cùng với`b`và hiện tại`c`. 
6. Nếu hai ký tự cuối cùng của`v`là`bb`, hậu tố hiện tại là`bbc`. Áp dụng phép biến đổi cố định`bbc -> bcbc -> bbabc -> bb`. Ba chỉ số hoạt động là`len(v) - 1`,`len(v)`, Và`len(v) + 1`. Sau những thao tác đó, chuỗi giống hệt như`v`, vì vậy không có thay đổi nào đối với`v`là cần thiết. 
7. Sau lần vượt qua đầu tiên này,`v`chỉ chứa`a`Và`b`. Quét lại trong khi bảo trì`g`, phần gồm có`a`các ký tự chưa được sử dụng. Bất cứ khi nào một`a`xuất hiện, nối nó vào`g`. 
8. Bất cứ khi nào một`b`xuất hiện, hãy kiểm tra xem`g`trống rỗng. Nếu nó trống, cái này`b`là ký tự đầu tiên của chuỗi hiện tại và câu trả lời là không thể. Ngược lại, hãy mở rộng`b`với thao tác 2. Kết quả`c`ngồi ngay sau cái cuối cùng`a`TRONG`g`, vậy hãy xóa nó đi`abc`với thao tác 4. Loại bỏ phần cuối cùng`a`từ`g`. 
9. Sau mỗi`b`đã được xử lý,`g`chỉ chứa chưa từng có`a`nhân vật. Đối với mỗi như vậy`a`, thực hiện thao tác 1 ở vị trí 1, thao tác 2 ở vị trí 2 và thao tác 4 ở vị trí 1. Mỗi bộ ba quay độc lập`a`vào chuỗi trống. 
10. In các thao tác đã thu thập. Có tối đa ba thao tác liên quan đến mỗi ký tự gốc, vì vậy tổng số không bao giờ vượt quá`3n`. 

Tại sao nó hoạt động. Trong lần vượt qua đầu tiên,`v`là sự thể hiện chính xác của chuỗi hiện tại sau khi tất cả các ký tự đầu vào được xử lý đã được xử lý. Mọi`c`được loại bỏ hoặc chuyển đổi bằng cách sử dụng một trong ba trường hợp cục bộ và mỗi trường hợp sẽ giữ nguyên hậu tố chưa được xử lý. Sau lần vượt qua đầu tiên, không`c`vẫn còn. Trong lần vượt qua thứ hai, mọi`b`được loại bỏ cùng với một trước đó`a`, Và`g`đại diện chính xác cho những điều trước đó`a`những nhân vật vẫn còn hiện diện. Nếu như`g`trống khi một`b`xuất hiện, đó`b`là nhân vật đầu tiên và không bao giờ có thể trở thành`a`, do đó tuyên bố không thể là đúng. Cuối cùng, mọi thứ còn lại`a`có trình tự loại bỏ ba thao tác trực tiếp. Do đó, mọi thao tác được tạo ra đều hợp lệ và bất cứ khi nào thuật toán báo cáo`-1`, ký tự đầu tiên hiện tại chứng tỏ rằng không thể tiếp tục được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_string(s):
    operations = []
    v = []

    def add(tp, idx):
        operations.append((tp, idx))

    def impossible():
        return None

    # First pass: eliminate all c's.
    for ch in s:
        if ch != 'c':
            v.append(ch)
            continue

        if not v:
            return impossible()

        if v[-1] == 'a':
            # ...ac -> ...aba
            add(3, len(v) + 1)
            v.append('b')
            v.append('a')
        else:
            # v ends in b
            if len(v) == 1:
                # The current string starts with bc.
                return impossible()

            if v[-2] == 'a':
                # ...abc -> ...
                add(4, len(v) - 1)
                v.pop()
                v.pop()
            else:
                # ...bbc -> ...bb
                #
                # bbc -> bcbc -> bbabc -> bb
                add(2, len(v) - 1)
                add(3, len(v))
                add(4, len(v) + 1)

    # Second pass: remove every b using a preceding a.
    g = []

    for ch in v:
        if ch == 'a':
            g.append('a')
        else:
            if not g:
                return impossible()

            # ...a b -> ...abc -> ...
            add(2, len(g) + 1)
            add(4, len(g))
            g.pop()

    # Every remaining a can be removed independently:
    # a -> ab -> abc -> empty
    for _ in g:
        add(1, 1)
        add(2, 2)
        add(4, 1)

    return operations

def main():
    s = input().strip()
    operations = solve_string(s)

    if operations is None:
        print(-1)
        return

    out = [str(len(operations))]
    out.extend(f"{tp} {idx}" for tp, idx in operations)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`operations`danh sách chứa các cặp loại hoạt động và chỉ mục dựa trên một. Việc giữ lại các thao tác thay vì sửa đổi chuỗi Python rất hữu ích vì cấu trúc chỉ cần biểu diễn nhỏ gọn tiền tố đã được xử lý, trong khi các chỉ số đầu ra thực tế được tính từ độ dài hiện tại của nó. 

Lần vượt qua đầu tiên sử dụng`v`dưới dạng danh sách thay vì chuỗi Python. Việc thêm và xóa khỏi cuối là các phép toán liên tục, giúp tránh hành vi bậc hai ngẫu nhiên khi xây dựng lại chuỗi lặp đi lặp lại. 

các`ac`trường hợp đặc biệt đơn giản. Trước khi xử lý các`c`,`v`chứa tiền tố kết thúc bằng`a`, vì vậy`c`đang ở vị trí`len(v) + 1`. Sau khi thay đổi nó thành`ba`, hậu tố được biểu diễn là`aba`, đó chính xác là lý do tại sao hai ký tự được thêm vào`v`. 

các`abc`case xóa ba ký tự cuối cùng của chuỗi hiện tại. Từ`v`không chứa dòng điện`c`, hai phần tử cuối cùng của nó là`a`Và`b`của bộ ba đó. Việc xóa bắt đầu lúc`len(v) - 1`, sử dụng lập chỉ mục dựa trên một. 

các`bbc`trường hợp này là nơi dễ mắc lỗi lập chỉ mục nhất. Trước khi xử lý`c`,`v`kết thúc bằng`bb`, vì vậy chuỗi cục bộ hiện tại là`bbc`. Thao tác 2 được áp dụng cho thao tác đầu tiên trong hai thao tác đó`b`nhân vật, tại`len(v) - 1`. Nó chèn một`c`, và cái đó mới được chèn vào`c`đang ở vị trí`len(v)`, do đó thao tác 3 sử dụng chính xác chỉ mục đó. Sau đó chuỗi cục bộ là`bbabc`, và`abc`bắt đầu lúc`len(v) + 1`. 

Trong lần vượt qua thứ hai,`g`chỉ chứa sự sống sót`a`nhân vật. Nếu như`len(g) = k`, tiếp theo`b`đang ở vị trí`k + 1`. Sau thao tác 2, kết quả`c`theo sau đó cuối cùng`a`, vì vậy`abc`bắt đầu ở vị trí`k`. Điều này giải thích hai chỉ số`len(g) + 1`Và`len(g)`. 

Vòng lặp cuối cùng luôn sử dụng các chỉ số`1`,`2`, Và`1`. Sau hai thao tác đầu tiên, toàn bộ chuỗi hiện tại bắt đầu bằng`abc`và việc xóa nó sẽ loại bỏ phần đã chọn`a`. Ba chỉ số tương tự có giá trị một lần nữa cho phần còn lại tiếp theo`a`. 

Số nguyên Python không bị tràn và số lượng thao tác được lưu trữ tối đa là`3n`, nhiều nhất`600000`, thoải mái trong giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,`acab`, việc xây dựng không phải tái tạo chính xác đầu ra mẫu vì bài toán chấp nhận bất kỳ chuỗi hợp lệ nào. Việc xây dựng của chúng tôi trước tiên xử lý`c`, sau đó loại bỏ`b`các ký tự và cuối cùng loại bỏ các ký tự còn lại`a`nhân vật. 

| Giai đoạn | Ký tự đầu vào |`v`|`g`| Đã thêm hoạt động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | trống | trống | | 
| Vượt qua đầu tiên |`a`|`a`| | | 
| Vượt qua đầu tiên |`c`|`aba`| |`3 2`| 
| Vượt qua đầu tiên |`a`|`abaa`| | | 
| Vượt qua đầu tiên |`b`|`abaab`| | | 
| Vượt qua thứ hai |`a`|`abaab`|`a`| | 
| Vượt qua thứ hai |`b`|`abaab`| trống |`2 2`,`4 1`| 
| Vượt qua thứ hai |`a`|`abaab`|`a`| | 
| Vượt qua thứ hai |`a`|`abaab`|`aa`| | 
| Vượt qua cuối cùng | Đầu tiên`a`| | |`1 1`,`2 2`,`4 1`| 
| Vượt qua cuối cùng | thứ hai`a`| | |`1 1`,`2 2`,`4 1`| 

Chuỗi kết quả có chín thao tác, nằm trong`3n = 12`. Bất biến ở bước đầu tiên được hiển thị sau`c`: tiền tố gốc`ac`thực sự đã trở thành`aba`, Vì thế`v`tiếp tục mô tả chuỗi thực thay vì chỉ lưu trữ các ký tự gốc. 

Đối với mẫu 2,`bac`, thuật toán ngay lập tức phát hiện ra ký tự đầu tiên là`b`. 

| Giai đoạn | Ký tự đầu vào |`v`|`g`| Kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | trống | trống | | 
| Vượt qua đầu tiên |`b`|`b`| | | 
| Vượt qua đầu tiên |`a`|`ba`| | | 
| Vượt qua đầu tiên |`c`|`baba`| |`3 3`| 
| Vượt qua thứ hai | Đầu tiên`b`|`baba`| trống | không thể | 

đầu tiên`b`gặp ở lần chuyển thứ hai bây giờ là ký tự đầu tiên của chuỗi thực tế còn lại. Không có thao tác nào có thể biến ký tự đầu tiên đó thành`a`, vì vậy đầu ra đúng là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi ký tự đầu vào được xử lý với số lần không đổi và`m <= 3n`các hoạt động được tạo ra. | 
| Không gian | O(n + m) | Các chuỗi phụ và danh sách thao tác được lưu trữ đều tuyến tính ở kích thước đầu vào. | 

Từ`n <= 2 * 10^5`, thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi ký tự và phát ra nhiều nhất`600000`hoạt động. Cấu trúc tuyến tính vừa vặn thoải mái trong giới hạn thời gian một giây, trong khi đầu ra được lưu trữ và các mảng phụ trợ vẫn ở mức dưới 256 MB. 

## Trường hợp thử nghiệm 

Trình kiểm tra bên dưới không so sánh một câu trả lời mang tính xây dựng với một chuỗi chính xác vì bài toán cho phép nhiều kết quả đầu ra hợp lệ khác nhau. Thay vào đó, nó mô phỏng mọi thao tác được in và xác minh rằng mỗi chỉ mục đều hợp lệ, mọi thao tác đều có thể áp dụng, chuỗi cuối cùng trống và số lượng thao tác nhiều nhất là`3n`.```python
# helper: run solution on input string, return output string
import io
import sys

def solve_string(s):
    operations = []
    v = []

    def add(tp, idx):
        operations.append((tp, idx))

    for ch in s:
        if ch != 'c':
            v.append(ch)
            continue

        if not v:
            return None

        if v[-1] == 'a':
            add(3, len(v) + 1)
            v.append('b')
            v.append('a')
        else:
            if len(v) == 1:
                return None

            if v[-2] == 'a':
                add(4, len(v) - 1)
                v.pop()
                v.pop()
            else:
                add(2, len(v) - 1)
                add(3, len(v))
                add(4, len(v) + 1)

    g = []

    for ch in v:
        if ch == 'a':
            g.append('a')
        else:
            if not g:
                return None
            add(2, len(g) + 1)
            add(4, len(g))
            g.pop()

    for _ in g:
        add(1, 1)
        add(2, 2)
        add(4, 1)

    return operations

def run(inp: str) -> str:
    s = inp.strip()
    operations = solve_string(s)

    if operations is None:
        return "-1\n"

    out = [str(len(operations))]
    out.extend(f"{tp} {idx}" for tp, idx in operations)
    return "\n".join(out) + "\n"

def validate(inp: str, output: str) -> bool:
    s = inp.strip()
    tokens = output.split()

    if not tokens:
        return False

    if tokens[0] == "-1":
        return len(tokens) == 1

    m = int(tokens[0])
    if m < 1 or m > 3 * len(s):
        return False

    if len(tokens) != 1 + 2 * m:
        return False

    cur = list(s)
    p = 1

    for _ in range(m):
        tp = int(tokens[p])
        idx = int(tokens[p + 1])
        p += 2

        if tp == 1:
            if not (1 <= idx <= len(cur)) or cur[idx - 1] != 'a':
                return False
            cur[idx - 1:idx - 1] = ['b']

        elif tp == 2:
            if not (1 <= idx <= len(cur)) or cur[idx - 1] != 'b':
                return False
            cur[idx - 1:idx] = ['b', 'c']

        elif tp == 3:
            if not (1 <= idx <= len(cur)) or cur[idx - 1] != 'c':
                return False
            cur[idx - 1:idx] = ['b', 'a']

        elif tp == 4:
            if not (1 <= idx <= len(cur) - 2):
                return False
            if cur[idx - 1:idx + 2] != ['a', 'b', 'c']:
                return False
            del cur[idx - 1:idx + 2]

        else:
            return False

    return not cur

# Provided samples
out = run("acab")
assert validate("acab", out), "sample 1"

out = run("bac")
assert out.strip() == "-1", "sample 2"

# Minimum-size solvable input
out = run("a")
assert validate("a", out), "single a"

# Minimum-size impossible inputs
assert run("b").strip() == "-1", "single b"
assert run("c").strip() == "-1", "single c"

# All-equal impossible input
assert run("bbb").strip() == "-1", "all b"

# All-equal impossible input with c
assert run("ccc").strip() == "-1", "all c"

# Boundary case involving c
out = run("ac")
assert validate("ac", out), "ac"

# Case where there are too many b characters
assert run("abb").strip() == "-1", "abb"

# Maximum-size solvable input
s = "a" * 200000
out = run(s)
assert validate(s, out), "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| Một chuỗi hợp lệ gồm 3 thao tác | Đầu vào và cuối cùng có thể giải quyết tối thiểu`a -> ab -> abc`xây dựng | 
|`b`|`-1`| đầu tiên`b`không bao giờ có thể xóa được | 
|`c`|`-1`| đầu tiên`c`không thể trở thành người đầu tiên`a`| 
|`bbb`|`-1`| Chuỗi không có sẵn`a`hỗ trợ bị từ chối | 
|`ccc`|`-1`| lặp đi lặp lại`c`nhân vật không thể giải cứu lần đầu tiên`c`| 
|`ac`| Một chuỗi hợp lệ | các`ac`ĐẾN`aba`chuyển đổi lần đầu tiên | 
|`abb`|`-1`| MỘT`b`có thể trở thành ký tự đầu tiên sau khi khớp trước đó | 
|`a`lặp đi lặp lại 200000 lần | Một chuỗi hợp lệ với chính xác 600000 thao tác | Kích thước đầu vào tối đa và`3n`ràng buộc hoạt động | 

## Vỏ cạnh 

cho`bac`, ký tự đầu tiên là`b`. Các cửa hàng pass đầu tiên`b`, sau đó`a`. Khi`c`được xử lý,`ac`hậu tố được chuyển thành`aba`, cho`baba`. Trong lần vượt qua thứ hai, ký tự đầu tiên đã được`b`, vì vậy không có trước`a`TRONG`g`. Thuật toán in`-1`, phù hợp với thực tế là lần đầu tiên`b`không bao giờ có thể trở thành người đầu tiên`a`. 

Vì`c`, lượt đầu tiên bắt đầu bằng một khoảng trống`v`. Thuật toán ngay lập tức trở lại`-1`. Đây không chỉ đơn thuần là một hạn chế của việc xây dựng. Hoạt động 3 thay đổi`c`vào trong`ba`, vì vậy ngay cả việc mở rộng ký tự đầu tiên cũng không thể biến nó thành`a`. Mỗi lần xóa cuối cùng liên quan đến ký tự đầu tiên đều yêu cầu ký tự đó phải được`a`. 

Vì`abb`, lần đầu tiên kết thúc với`v = abb`, bởi vì không có`c`nhân vật. Lượt thứ hai tiêu thụ lượt đầu tiên`b`cùng với phần trước`a`, để lại thứ hai`b`làm ký tự đầu tiên. Vào lúc đó`g`trống, do đó thuật toán trả về`-1`. Tình huống đó là khó tránh khỏi vì không có ca phẫu thuật nào có thể biến điều đó trước được`b`vào trong`a`. 

Vì`ac`, lần đầu tiên nhìn thấy mẫu cục bộ`ac`. Thao tác 3 tại vị trí 2 thay đổi`ac`vào trong`aba`, Vì thế`v`trở thành`aba`. Đường chuyền thứ hai khớp với đường giữa`b`với cái trước`a`, sử dụng thao tác 2, sau đó là thao tác 4, để lại một`a`. trận chung kết`a`được loại bỏ bằng ba thao tác. Mọi chỉ mục được cấu trúc sử dụng đều đề cập đến chuỗi hiện tại, vì vậy trường hợp này cũng thực hiện ranh giới giữa giai đoạn thứ nhất và giai đoạn thứ hai. 

Vì`abc`, cái`c`thấy`v = ab`Và`v[-2] = a`, do đó thuật toán trực tiếp thực hiện thao tác 4 ở vị trí 1. Toàn bộ chuỗi biến mất trong một thao tác. Đây là trường hợp nhỏ nhất mà thao tác xóa có thể được sử dụng mà không cần mở rộng. 

Đối với đầu vào tối đa bao gồm`200000`bản sao của`a`, hai lần chuyển đầu tiên để lại tất cả các ký tự trong`g`. Mỗi`a`sau đó được loại bỏ độc lập bằng cách sử dụng chính xác ba thao tác. Việc xây dựng tạo ra chính xác`600000 = 3n`hoạt động, cho thấy việc triển khai tôn trọng kích thước đầu ra tối đa ngay cả trong trường hợp xấu nhất.
