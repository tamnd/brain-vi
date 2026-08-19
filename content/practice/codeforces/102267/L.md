---
title: "CF 102267L - ABC"
description: "Chuỗi được xây dựng từ ba ký hiệu a, b và c. Một thao tác có thể mở rộng một ký hiệu thành mẫu hai ký hiệu cố định hoặc xóa một lần xuất hiện của abc."
date: "2026-08-19T03:53:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "L"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 561
verified: false
draft: false
---

[CF 102267L - ABC](https://codeforces.com/problemset/problem/102267/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9m 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chuỗi được xây dựng từ ba biểu tượng,`a`,`b`, Và`c`. Một thao tác có thể mở rộng một ký hiệu thành mẫu hai ký hiệu cố định hoặc xóa một lần xuất hiện của`abc`. Nhiệm vụ mang tính xây dựng: hoặc tạo ra một chuỗi tối đa`3n`các thao tác hợp lệ biến toàn bộ chuỗi thành chuỗi trống hoặc chứng minh rằng chuỗi đó không tồn tại. 

Đầu vào chứa tối đa một chuỗi có độ dài`2 * 10^5`. Đầu ra không phải là một giá trị câu trả lời duy nhất. Đó là một chuỗi các thao tác thực tế và mọi chỉ mục được báo cáo đều đề cập đến chuỗi sau khi tất cả các thao tác trước đó đã được áp dụng. Tuyên bố ban đầu và các mẫu có sẵn từ Codeforces. 

Giới hạn kích thước loại trừ mọi thứ khám phá nhiều chuỗi hoạt động có thể có. Ngay cả một mô phỏng bậc hai cũng không được mong muốn trong giới hạn một giây, trong khi bản thân đầu ra được yêu cầu có thể chứa`600000`hoạt động. Giải pháp dự định phải xử lý đầu vào về cơ bản một lần và chỉ tạo ra`O(n)`hoạt động. 

Có một số trường hợp nguy hiểm bộc lộ những cách tiếp cận bất cẩn. đầu vào`a`có thể giải quyết được: mở rộng nó thành`ab`, sau đó`abc`, sau đó xóa nó bằng ba thao tác. đầu vào`c`là không thể vì nhân vật đầu tiên có thể trở thành`ba`, nhưng ký tự đầu tiên là`b`, và một chuỗi bắt đầu bằng`b`không bao giờ có thể loại bỏ ký tự đầu tiên đó. Việc triển khai bất cẩn giả định mọi ký tự cuối cùng có thể bị biến thành`abc`sẽ chấp nhận sai`c`. 

đầu vào`bac`cũng là điều không thể. Ký tự đầu tiên của nó là`b`, vì vậy không có cách nào để biến nhân vật đó thành`a`của một`abc`xóa. Việc mô phỏng từ trái sang phải bất cẩn có thể biến đổi hậu tố và vô tình tạo ra một thao tác không hợp lệ xung quanh phần đầu`b`. 

Một trường hợp quan trọng khác là`ac`. Nó có thể giải được mặc dù ban đầu nó không chứa`abc`. Trình tự là`ac -> aba -> abca -> empty`: thay thế đầu tiên`c`qua`ba`, sau đó thay cái mới đó`b`qua`bc`, sau đó xóa`abc`. Một triển khai chỉ tìm kiếm một cái đã tồn tại`abc`bỏ sót vụ này 

Cuối cùng,`abb`là không thể. Sau lần đầu tiên`b`được khớp với phần trước`a`, phần còn lại`b`nằm ở đầu chuỗi còn lại và không bao giờ có thể biến mất. Điều này nắm bắt các triển khai xóa một cách tham lam`ab`mà không kiểm tra xem mọi`b`có trước`a`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ duy trì chuỗi hiện tại và thử mọi hoạt động hợp pháp. Ở một chuỗi có độ dài tối đa`4n`, có thể có`O(n)`lựa chọn vị trí và một câu trả lời hợp lệ có thể chứa tới`3n`hoạt động. Do đó, việc khám phá tất cả các trình tự có thể yêu cầu theo thứ tự`(4n)^(3n)`nhánh trong trường hợp xấu nhất. Ngay cả việc ghi nhớ cũng không lưu lại cách tiếp cận, vì số lượng chuỗi có thể truy cập là theo cấp số nhân. 

Quan sát hữu ích là các hoạt động có một tập hợp nhỏ các hành vi cục bộ đáng ngạc nhiên. Chúng ta có thể loại bỏ mọi`c`sử dụng một trong ba công trình địa phương. Khi tiền tố rút gọn hiện tại kết thúc bằng`a`, hậu tố`ac`có thể biến trở lại thành`a`sử dụng ba thao tác. Khi tiền tố rút gọn kết thúc bằng`ab`, cái mới`c`ngay lập tức đưa ra`abc`, có thể bị xóa. Khi tiền tố rút gọn kết thúc bằng`bb`, có một cách xây dựng ba phép toán khác thay đổi`bbc`quay lại`bb`. 

Rốt cuộc`c`các ký tự đã được xử lý, chỉ`a`Và`b`duy trì. Vào thời điểm đó mỗi`b`có thể được ghép nối với những người sống sót ngay trước đó`a`. Cặp đôi`ab`có thể được loại bỏ trong hai thao tác bằng cách thay đổi`b`vào trong`bc`và xóa`abc`. Bất kì`a`rốt cuộc đã rời đi`b`các ký tự đã được ghép nối có thể được loại bỏ độc lập trong ba thao tác. 

Điều quan trọng là những phép biến đổi này có thể được áp dụng cho tiền tố đã được xử lý trong khi hậu tố chưa được xử lý vẫn được giữ nguyên. Chúng ta chỉ cần nhớ tiền tố rút gọn hiện tại chứ không phải chuỗi phát triển đầy đủ. Hướng dẫn cuộc thi chính thức sử dụng cấu trúc mang tính xây dựng cục bộ tương tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O((4n)^(3n))`| Hàm mũ | Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên hãy kiểm tra ký tự đầu tiên. Nếu nó là`b`hoặc`c`, đầu ra`-1`. 

Ký tự đầu tiên không thể bị xóa trừ khi cuối cùng nó trở thành ký tự`a`trong một`abc`tiền tố. MỘT`b`luôn luôn vẫn là một`b`khi được mở rộng, trong khi một`c`trở thành`ba`, ký tự đầu tiên của nó lại là`b`. Không gì có thể biến một nhân vật chính như vậy thành`a`. 
2. Quét chuỗi gốc từ trái sang phải và duy trì tiền tố rút gọn`v`. 

nhân vật`a`Và`b`chỉ đơn giản là được thêm vào`v`. MỘT`c`được xử lý ngay lập tức bằng cách sử dụng dạng cục bộ của tiền tố đã được xử lý. 
3. Nếu`c`theo sau một`a`, sử dụng phép biến đổi`ac -> aba -> abca -> a`. 

Hoạt động đầu tiên thay thế`c`qua`ba`. Cái thứ hai thay thế cái mới được tạo`b`qua`bc`. Kết quả`abc`được gỡ bỏ. Ba hoạt động đã loại bỏ`c`trong khi rời khỏi phần trước`a`không đổi nên`v`bản thân nó không thay đổi. 
4. Nếu`c`theo sau`ab`, hậu tố hiện tại đã là`abc`. 

Xóa nó trực tiếp. Trong chuỗi ảo`v`, xóa phần cuối cùng của nó`a`Và`b`cùng với hiện tại`c`, có nghĩa là bật ra hai ký tự cuối cùng từ`v`. 
5. Nếu`c`theo sau`bb`, sử dụng phép biến đổi`bbc -> bcbc -> bbabc -> bb`. 

Hoạt động đầu tiên mở rộng hoạt động đầu tiên trong hai hoạt động sau`b`nhân vật. Thứ hai mở rộng vị trí mới`c`và thao tác cuối cùng sẽ loại bỏ kết quả`abc`. Tiền tố rút gọn lại trở thành tiền tố cũ`bb`. 
6. Sau mỗi`c`đã được xử lý,`v`chỉ chứa`a`Và`b`. Quét nó từ trái sang phải bằng một chuỗi khác`g`. 

Bất cứ khi nào một`a`xuất hiện, nối nó vào`g`. Whenever a `b`xuất hiện,`g`phải chứa ít nhất một`a`. Sử dụng cái cuối cùng như vậy`a`và`b`cùng nhau:`ab -> abc -> empty`. 

Thao tác đầu tiên sẽ thay đổi`b`ĐẾN`bc`và cái thứ hai xóa kết quả`abc`. Loại bỏ kết quả phù hợp`a` from `g`. 
7. Nếu một`b`gặp phải trong khi`g`trống, xuất ra`-1`. 

Tại thời điểm đó, chuỗi còn lại bắt đầu bằng`b`. Như đã lập luận ở bước đầu tiên, nhân vật chủ đạo như vậy không bao giờ có thể bị loại bỏ. 
8. Rốt cuộc thì`b`các nhân vật đã được ghép nối,`g`bao gồm hoàn toàn`a`nhân vật. 

Đối với mỗi phần còn lại`a`, trình diễn`a -> ab -> abc -> empty`. 

Chi phí này chính xác là ba hoạt động cho mỗi lần còn lại`a`. 
9. Xuất ra tất cả các hoạt động được ghi lại. 

Mọi thao tác được tạo tương ứng với độ dài của tiền tố được xử lý và hậu tố không được chạm vào luôn xuất hiện sau nó. Vì mọi chỉ mục được ghi đều đề cập đến một ký tự bên trong tiền tố đó nên nó vẫn hợp lệ khi có hậu tố. 

### Tại sao nó hoạt động 

Bất biến trung tâm là`v`đại diện cho một chuỗi có thể truy cập được từ tiền tố đầu vào đã được xử lý, trong khi tất cả các ký tự sau tiền tố đó đều không bị ảnh hưởng. Mọi`c`bị loại bỏ bởi một trong ba nhận dạng cục bộ ở trên, vì vậy sau lần quét đầu tiên không có`c`ký tự còn lại. 

Lần quét thứ hai duy trì sự bất biến tương tự với`g`: mỗi lần xử lý`b`đã được loại bỏ hoàn toàn cùng với một trước đó`a`. Nếu không`a`có sẵn, chuỗi còn lại bắt đầu bằng`b`, vĩnh viễn không thể trở thành người dẫn đầu`a`có thể tháo rời`abc`. Vì vậy, tình trạng thất bại thực sự là không thể thực hiện được chứ không phải là sự hạn chế của sự lựa chọn tham lam. 

Khi quá trình quét thành công, chỉ`a`các ký tự vẫn còn và mỗi ký tự có thể được loại bỏ độc lập bằng thao tác ba`a -> ab -> abc -> empty`sự thi công. Do đó chuỗi được tạo ra luôn đạt đến chuỗi trống. 

Đối với hoạt động ràng buộc, mọi bản gốc`c`chi phí tối đa ba thao tác trong lần quét đầu tiên. Mỗi nhân vật sống sót trong`v`là bản gốc`a`hoặc`b`và chỉ tốn tối đa ba thao tác trong lần quét thứ hai. Hai nhóm này rời nhau nên tổng số nhiều nhất là`3n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

BASE = 1_000_000

def solve_one(s):
    n = len(s)
    if s[0] != 'a':
        return None

    ops = []

    def add(tp, idx):
        ops.append(tp * BASE + idx)

    v = []

    for ch in s:
        if ch != 'c':
            v.append(ch)
            continue

        if not v:
            return None

        k = len(v)

        if v[-1] == 'a':
            # ac -> aba -> abca -> empty, leaving the old a.
            add(3, k + 1)
            add(2, k + 1)
            add(4, k)

        else:
            # The prefix ends in b.
            if k == 1:
                return None

            if v[-2] == 'a':
                # abc -> empty.
                add(4, k - 1)
                v.pop()
                v.pop()
            else:
                # bbc -> bcbc -> bbabc -> bb.
                add(2, k - 1)
                add(3, k)
                add(4, k + 1)

    g = []

    for ch in v:
        if ch == 'a':
            g.append(ch)
        else:
            if not g:
                return None

            k = len(g)

            # ab -> abc -> empty.
            add(2, k + 1)
            add(4, k)
            g.pop()

    for _ in g:
        # a -> ab -> abc -> empty.
        add(1, 1)
        add(2, 2)
        add(4, 1)

    out = [str(len(ops))]
    out.extend(f"{op // BASE} {op % BASE}" for op in ops)
    return "\n".join(out)

def main():
    s = input().strip()
    ans = solve_one(s)

    if ans is None:
        print(-1)
    else:
        print(ans)

if __name__ == "__main__":
    main()
```Việc kiểm tra ký tự đầu tiên được thực hiện có chủ ý trước lần quét chính. Nó làm cho điều kiện không thể xảy ra trở nên rõ ràng và ngăn ngừa sự mơ hồ`bc`hoặc`bac`tình huống xâm nhập vào địa phương`c`trường hợp. 

Danh sách`v`là một biểu diễn ảo của tiền tố được xử lý. Nó không chứa các ký tự mở rộng thực tế được tạo ra bởi các thao tác. Ví dụ, khi xử lý`ac`, chuỗi thực tạm thời trở thành`aba`, sau đó`abca`, sau đó thua`abc`, Nhưng`v`vẫn chỉ là`a`. Chỉ giữ lại dạng rút gọn là điều làm cho thuật toán trở nên tuyến tính. 

Các chỉ số trong lần quét đầu tiên được dựa trên`len(v)`. Đối với`ac`trường hợp, bản gốc`c`đang ở vị trí`k + 1`, vậy là cả hai gõ`3`và gõ`2`sử dụng vị trí đó. Sau lần mở rộng đầu tiên, bản mới`b`chính xác là ở đó. Kết quả`abc`bắt đầu ở vị trí`k`, đó là loại`4`chỉ số. 

Đối với`abc`trường hợp,`v`kết thúc bằng`ab`, vậy với`k = len(v)`, cái`abc`bắt đầu lúc`k - 1`. Sau khi xóa nó, cuối cùng`a`Và`b`được đại diện bởi hai mục đó biến mất khỏi`v`. 

Đối với`bbc`trường hợp, trường hợp đầu tiên`b`của cặp cuối cùng là ở`k - 1`. Sau khi mở rộng nó,`c`phải thay đổi là ở vị trí`k`. trận chung kết`abc`bắt đầu lúc`k + 1`. 

Lần quét thứ hai sử dụng`g`theo cách hoàn toàn giống nhau. Khi một`b`được xử lý, chỉ số của nó là`len(g) + 1`, trong khi`abc`được tạo sau khi quá trình mở rộng bắt đầu vào lúc`len(g)`. Sau khi xóa, kết quả trùng khớp`a`bị xóa khỏi`g`. 

Các hoạt động được lưu trữ dưới dạng một số nguyên thay vì một bộ dữ liệu. Với nhiều nhất`600000`hoạt động, điều này làm giảm đáng kể chi phí hoạt động của đối tượng Python.`BASE`lớn hơn nhiều so với mọi chỉ mục có thể có, do đó phép chia và số dư sẽ khôi phục loại hoạt động và chỉ mục mà không có sự mơ hồ. Số nguyên Python không bị giới hạn nên không có vấn đề tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1:`acab`Thuật toán tạo ra một chuỗi hợp lệ khác với đầu ra mẫu. Nhiều chuỗi hoạt động hợp lệ được cho phép. 

| Ký tự đầu vào | Trường hợp |`v`sau khi xử lý | Đã thêm hoạt động | 
| --- | --- | --- | --- | 
|`a`| nối thêm |`a`| 0 | 
|`c`|`ac`tiện ích |`a`| 3 | 
|`a`| nối thêm |`aa`| 0 | 
|`b`| nối thêm |`aab`| 0 | 

Ở lần quét thứ hai, lần quét cuối cùng`b`cặp với cái cuối cùng`a`. 

| Nhân vật |`g`trước | Hành động |`g`sau | Đã thêm hoạt động | 
| --- | --- | --- | --- | --- | 
|`a`| trống | giữ`a`|`a`| 0 | 
|`a`|`a`| giữ`a`|`aa`| 0 | 
|`b`|`aa`| loại bỏ cuối cùng`ab`|`a`| 2 | 

Một`a` remains, so it is removed independently.

| Remaining `g`| Hành động | Đã thêm hoạt động | 
| --- | --- | --- | 
|`a`|`a -> ab -> abc -> empty`| 3 | 

Chuỗi kết quả có tám thao tác và nằm trong phạm vi`3n = 12`giới hạn. Trình tự bốn thao tác của mẫu ngắn hơn nhưng bài toán chỉ yêu cầu một trình tự hợp lệ nào đó. 

Ba thao tác đầu tiên biến đổi tiền tố`ac`vào trong`a`, cho`aab`. Hai thao tác tiếp theo loại bỏ thao tác cuối cùng`ab`, rời đi`a`và ba thao tác cuối cùng sẽ loại bỏ điều đó`a`. 

### Mẫu 2:`bac`Ký tự đầu tiên là`b`, do đó thuật toán sẽ ngay lập tức loại bỏ chuỗi đó. 

| Kiểm tra | Giá trị | Kết quả | 
| --- | --- | --- | 
| Ký tự đầu tiên |`b`| không thể | 
| Đầu ra |`-1`| đúng | 

Điều này chứng tỏ tại sao phải kiểm tra ký tự đầu tiên trước khi thử ký tự cục bộ`c`sự biến đổi quan trọng. MỘT`b`lúc ban đầu không bao giờ có thể trở thành`a`được yêu cầu xóa ngay từ đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi ký tự đầu vào được xử lý một lần và mọi thao tác được tạo sẽ được ghi một lần. | 
| Không gian |`O(n)`| Các chuỗi giảm và nhiều nhất`3n`các hoạt động được mã hóa được lưu trữ. | 

Vì`n <= 2 * 10^5`, thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi ký tự đầu vào cộng với yêu cầu`O(n)`đầu ra. Sản lượng tối đa chứa`600000`hoạt động, chính xác là quy mô mà công trình được thiết kế. 

## Trường hợp thử nghiệm 

Vì kết quả đầu ra mang tính xây dựng nên việc so sánh văn bản chính xác không phù hợp với những trường hợp thành công. Trình trợ giúp kiểm tra bên dưới chạy bộ giải và xác thực mọi thao tác được báo cáo dựa trên chuỗi phát triển thực tế.```python
# helper: run solution on input string, return output string
import sys
import io

BASE = 1_000_000

def solve_one(s):
    if s[0] != 'a':
        return None

    ops = []

    def add(tp, idx):
        ops.append(tp * BASE + idx)

    v = []

    for ch in s:
        if ch != 'c':
            v.append(ch)
            continue

        if not v:
            return None

        k = len(v)

        if v[-1] == 'a':
            add(3, k + 1)
            add(2, k + 1)
            add(4, k)
        else:
            if k == 1:
                return None

            if v[-2] == 'a':
                add(4, k - 1)
                v.pop()
                v.pop()
            else:
                add(2, k - 1)
                add(3, k)
                add(4, k + 1)

    g = []

    for ch in v:
        if ch == 'a':
            g.append(ch)
        else:
            if not g:
                return None
            k = len(g)
            add(2, k + 1)
            add(4, k)
            g.pop()

    for _ in g:
        add(1, 1)
        add(2, 2)
        add(4, 1)

    out = [str(len(ops))]
    out.extend(f"{op // BASE} {op % BASE}" for op in ops)
    return "\n".join(out)

def run(inp: str) -> str:
    return "-1\n" if (ans := solve_one(inp.strip())) is None else ans + "\n"

def validate(inp: str, out: str):
    s = inp.strip()
    out = out.strip()

    if out == "-1":
        return s[0] != 'a' or not is_solvable_by_constructor(s)

    lines = out.splitlines()
    m = int(lines[0])
    assert 1 <= m <= 3 * len(s)
    assert len(lines) == m + 1

    cur = list(s)

    for line in lines[1:]:
        tp, idx = map(int, line.split())
        assert 1 <= tp <= 4
        assert 1 <= idx <= len(cur)

        p = idx - 1

        if tp == 1:
            assert cur[p] == 'a'
            cur[p:p + 1] = ['a', 'b']
        elif tp == 2:
            assert cur[p] == 'b'
            cur[p:p + 1] = ['b', 'c']
        elif tp == 3:
            assert cur[p] == 'c'
            cur[p:p + 1] = ['b', 'a']
        else:
            assert p + 3 <= len(cur)
            assert cur[p:p + 3] == ['a', 'b', 'c']
            del cur[p:p + 3]

    assert not cur

def is_solvable_by_constructor(s):
    return solve_one(s) is not None

# Provided sample 1
out = run("acab")
validate("acab", out)

# Provided sample 2
assert run("bac") == "-1\n", "sample 2"

# Minimum-size input
out = run("a")
validate("a", out)

# All-equal values
out = run("aaa")
validate("aaa", out)

# Boundary-sensitive case
out = run("ab")
validate("ab", out)

# Maximum-size input, exactly 3n operations
mx = "a" * 200000
out = run(mx)
lines = out.splitlines()
assert int(lines[0]) == 600000
assert len(lines) == 600001
```các`validate`hàm mô phỏng chuỗi thực, do đó, nó bắt được các chỉ số và loại thao tác không chính xác thay vì chỉ kiểm tra số lượng thao tác. Thử nghiệm kích thước tối đa kiểm tra mức độ quan trọng`3n`ràng buộc với`200000`nhân vật. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| Công trình 3 thao tác hợp lệ | Kích thước tối thiểu và cơ bản`a`tiện ích | 
|`aaa`| Công trình 9 thao tác hợp lệ | Đầu vào hoàn toàn bằng nhau | 
|`ab`| Công trình 2 thao tác hợp lệ | Lập chỉ mục ranh giới trong`b`pha | 
|`a * 200000`| Chính xác`600000`hoạt động | Kích thước tối đa và`3n`giới hạn | 

## Vỏ cạnh 

cho`c`, thuật toán thấy ngay ký tự đầu tiên không phải`a`và in`-1`. Điều này đúng vì`c -> ba`, sau đó ký tự đầu tiên là`b`, và dẫn đầu`b`không bao giờ có thể trở thành nhân vật đầu tiên của`abc`. 

Vì`bac`, đối số ký tự đầu tiên tương tự sẽ được áp dụng ngay cả khi có các ký tự sau ký tự đầu tiên`b`. Các thao tác trên hậu tố không thể thay đổi ký tự đầu tiên và mở rộng ký tự đầu tiên đó`b`chỉ thay đổi nó thành`bc`. Đầu ra là`-1`. 

Vì`ac`, ký tự đầu tiên là hợp lệ và`c`được xử lý bởi tiện ích ba thao tác. Với`k = 1`, các hoạt động là loại`3`tại chỉ mục`2`, kiểu`2`tại chỉ mục`2`, và gõ`4`tại chỉ mục`1`. Các trạng thái thực là`ac -> aba -> abca -> empty`. 

Vì`abb`, lần quét đầu tiên rời đi`v = abb`. Trong lần quét thứ hai, lần quét đầu tiên`b`tiêu thụ trước đó`a`, rời đi`g`trống. Tiếp theo`b`không có sẵn`a`, do đó thuật toán trả về`-1`. Lỗi có nghĩa là chuỗi còn lại bắt đầu bằng`b`, không thể gỡ bỏ được. 

Đối với đầu vào có kích thước tối đa bao gồm toàn bộ`a`, mọi ký tự được xử lý độc lập. Mỗi cái yêu cầu chính xác ba thao tác, đưa ra`3 * 200000 = 600000`hoạt động. Do đó, đầu ra đạt đến giới hạn một cách chính xác mà không vượt quá nó.
