---
title: "CF 102309I - Địa chỉ IPv6 của Orz Pandas"
description: "Mỗi trường hợp thử nghiệm cho một số nguyên không âm biểu thị một địa chỉ IPv6 128 bit hoàn chỉnh. Một địa chỉ IPv6 có chính xác tám phần 16 bit, vì vậy công việc đầu tiên là viết số nguyên có đúng 32 chữ số thập lục phân và chia các chữ số đó thành tám nhóm bốn chữ số."
date: "2026-08-13T23:49:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "I"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 99
verified: true
draft: false
---

[CF 102309I - Địa chỉ IPv6 của Orz Pandas](https://codeforces.com/problemset/problem/102309/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm cho một số nguyên không âm biểu thị một địa chỉ IPv6 128 bit hoàn chỉnh. Một địa chỉ IPv6 có chính xác tám phần 16 bit, vì vậy công việc đầu tiên là viết số nguyên có đúng 32 chữ số thập lục phân và chia các chữ số đó thành tám nhóm bốn chữ số. Mỗi nhóm sau đó được viết mà không có số 0 đứng đầu. 

Sau quá trình chuẩn hóa đó, một loạt các nhóm có giá trị 0 liên tiếp có thể được thay thế bằng`::`, nhưng chỉ khi lần chạy có ít nhất hai nhóm. Việc nén có thể được sử dụng nhiều nhất một lần. Trong số tất cả các cách biểu diễn pháp lý, trước tiên chúng ta muốn cái có ít phần rõ ràng nhất và sau đó là cách biểu diễn nhỏ nhất về mặt từ điển. Sự cố chính thức chỉ định tối đa đầu vào 128 bit, nhiều trường hợp thử nghiệm cho đến EOF và giới hạn 1 giây, 256 MB. 

Giới hạn 128-bit ở đây đặc biệt thân thiện vì một địa chỉ IPv6 luôn có chính xác tám nhóm. Không có mảng kích thước (n) phụ thuộc vào đầu vào và không cần cấu trúc dữ liệu phức tạp. Ngay cả một phương pháp kiểm tra mọi khoảng 0 có thể cũng chỉ có các khoảng (8 \cdot 9/2 = 36) để xem xét. Các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ các mối lo ngại về tràn có thể phát sinh trong các ngôn ngữ có loại số nguyên tiêu chuẩn nhỏ hơn 128 bit. 

Trường hợp tinh vi đầu tiên là một nhóm 0 duy nhất. Ví dụ, số nguyên`340282366920938463463374607431768145920`đại diện cho`ffff:ffff:ffff:ffff:ffff:ffff:ffff:0`. Đầu ra đúng là`ffff:ffff:ffff:ffff:ffff:ffff:ffff:0`. Việc triển khai bất cẩn có thể nén số 0 cuối cùng thành`::`, sản xuất`ffff:ffff:ffff:ffff:ffff:ffff:ffff::`, nhưng một phần bị bỏ qua sẽ bị cấm rõ ràng. 

Trường hợp tinh tế thứ hai là một địa chỉ không chứa nhóm nào khác 0. Đối với đầu vào`0`, tám nhóm chuẩn hóa đều`0`, và đầu ra đúng là`::`. Việc xử lý địa chỉ hoàn toàn bằng 0 giống như tiền tố và hậu tố thông thường có thể vô tình tạo ra một chuỗi trống hoặc một dấu hai chấm đơn không hợp lệ. 

Trường hợp khó phát hiện thứ ba là khi nhiều số 0 có cùng độ dài tối đa. Hãy xem xét tám nhóm`1:0:0:2:0:0:3:4`. Cả hai`1::2:0:0:3:4`Và`1:0:0:2::3:4`nén hai nhóm 0 và tốt như nhau đối với số phần còn lại. Câu trả lời đúng là`1:0:0:2::3:4`, bởi vì nó nhỏ hơn về mặt từ điển. Giải pháp chỉ chọn lần chạy dài nhất đầu tiên sẽ thất bại trong trường hợp này. 

Cuối cùng, số 0 ở đầu hoặc cuối cần có cấu trúc chuỗi đặc biệt. Vì`1`, các nhóm chuẩn hóa là`0:0:0:0:0:0:0:1`, vậy câu trả lời là`::1`. Vì`0`, cả hai phía của quá trình nén đều trống nên câu trả lời là chính xác`::`, không`:`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp là có thể thực hiện được vì chỉ có tám nhóm. Sau khi chuẩn hóa chúng, chúng ta có thể xem xét mọi khoảng thời gian`[l, r]`các nhóm của họ đều bằng không. Có nhiều nhất là 36 khoảng và các khoảng có độ dài bằng một sẽ bị loại bỏ. Đối với mỗi khoảng còn lại, chúng ta xây dựng địa chỉ thu được bằng cách thay thế khoảng đó bằng`::`, sau đó so sánh các ứng viên theo thứ tự yêu cầu. Nếu không có khoảng thời gian hợp lệ, địa chỉ chuẩn hóa không nén là câu trả lời. 

Lực lượng vũ phu này đã đủ nhanh cho vấn đề thực tế. Trong trường hợp xấu nhất, có 36 khoảng có thể xảy ra và việc xây dựng một ứng cử viên chạm vào tối đa 8 nhóm, do đó có nhiều nhất (36 \times 8 = 288) phép toán cấp nhóm cho mỗi trường hợp thử nghiệm, ngoại trừ các phép toán chuỗi ngắn. Không có kích thước đầu vào có ý nghĩa nào khiến cách tiếp cận này trở nên quá chậm theo biểu diễn tám nhóm cố định đã cho. Bản thân định dạng địa chỉ ngăn chặn vụ nổ (O(n^2)) thông thường được thấy trong các vấn đề có độ dài thay đổi. 

Chúng ta có thể đơn giản hóa việc tìm kiếm hơn nữa bằng cách quan sát ý nghĩa của tiêu chí tối ưu hóa đầu tiên. Việc nén một chuỗi có độ dài bằng 0 (k) sẽ loại bỏ chính xác (k) các nhóm rõ ràng và thay thế chúng bằng một nhóm`::`. Do đó, số phần còn lại được giảm thiểu một cách chính xác bằng cách chọn lần chạy 0 dài nhất. Một đường chạy có độ dài một không bao giờ đủ điều kiện, vì vậy chỉ những đường chạy có độ dài ít nhất hai mới quan trọng. 

Khi đã biết độ dài chạy tối đa, mọi ứng cử viên sẽ sử dụng cùng một số nhóm còn lại và cũng loại bỏ cùng một số ký tự. Tiêu chí duy nhất còn lại là thứ tự từ điển. Đối với các lần chạy 0 có độ dài bằng nhau, việc chọn lần chạy sau luôn nhỏ hơn về mặt từ điển. Tại vị trí đầu tiên nơi hai ứng viên khác nhau, ứng cử viên nén lần chạy trước đó chứa`:`, trong khi ứng cử viên rời khỏi nhóm 0 đó rõ ràng chứa`0`. Từ`0`về mặt từ điển nhỏ hơn`:`, lượt chạy sau sẽ thắng. Chúng ta có thể sử dụng thực tế này một cách trực tiếp hoặc mạnh mẽ hơn là xây dựng các ứng cử viên cho tất cả các lần chạy có độ dài tối đa và lấy Python`min`. 

Việc triển khai tối ưu sẽ quét tám nhóm một lần để tìm các lần chạy 0 dài nhất, chỉ xây dựng các ứng cử viên cho các lần chạy có độ dài tối đa đó và lấy nhóm nhỏ nhất về mặt từ điển. Độ phức tạp tiệm cận vẫn là (O(8)), đơn giản là (O(1)), nhưng lý do phản ánh trực tiếp các tiêu chí tối ưu hóa thay vì liệt kê các khoảng không liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(8²) | O(8) | Đã chấp nhận | 
| Tối ưu | O(8) | O(8) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi số nguyên đầu vào thành thập lục phân với chính xác 32 chữ số. Việc đệm là cần thiết vì các bit 0 đứng đầu vẫn là một phần của địa chỉ 128 bit. Chia kết quả thành tám nhóm gồm bốn chữ số thập lục phân và loại bỏ các số 0 đứng đầu khỏi mỗi nhóm, để lại`"0"`khi một nhóm hoàn toàn bằng không. 
2. Quét tám nhóm chuẩn hóa từ trái sang phải. Bất cứ khi nào tìm thấy nhóm 0, tiếp tục cho đến khi quá trình chạy số 0 kết thúc và ghi lại vị trí và chiều dài bắt đầu của nó. Giữ độ dài tối đa được tìm thấy, bỏ qua các phần chạy ngắn hơn hai nhóm vì nén kiểu RFC không thể biểu thị một phần bị bỏ qua. 
3. Nếu không tồn tại ít nhất hai chuỗi độ dài, hãy nối tám nhóm chuẩn hóa bằng dấu hai chấm và trả lại địa chỉ đó. Không có việc sử dụng hợp pháp`::`trong tình huống này. 
4. Đối với mỗi lần chạy 0 có độ dài bằng độ dài tối đa, hãy tạo một địa chỉ ứng cử viên bằng cách thay thế lần chạy hoàn chỉnh đó bằng`::`. Nhóm trước và sau khi chạy không thay đổi. Chỉ xem xét các lần chạy có độ dài tối đa là đủ vì mỗi lần chạy ngắn hơn sẽ để lại các phần rõ ràng hơn. 
5. So sánh các ứng viên theo từ điển và trả về giá trị nhỏ nhất. Bộ điều khiển này chạy ở phần đầu, phần giữa và phần cuối mà không cần logic ngắt ràng buộc riêng biệt. 
6. Nếu lần chạy đã chọn bao gồm tất cả tám nhóm, công trình sẽ tạo ra`::`. Nếu bắt đầu ở nhóm đầu tiên thì kết quả có dạng`::suffix`; nếu nó kết thúc ở nhóm cuối cùng, nó có dạng`prefix::`. Những trường hợp này đều được xử lý rõ ràng khi tham gia hai bên. 

Tại sao nó hoạt động: sau khi chuẩn hóa, mọi chữ viết tắt hợp pháp chỉ khác nhau ở chỗ lần chạy 0 liên tiếp được thay thế bằng`::`. Một chuỗi có độ dài (k) để lại (8-k) các phần rõ ràng, vì vậy mọi chữ viết tắt tối ưu phải sử dụng số 0 hợp pháp dài nhất. Trong số các lần chạy có độ dài tối đa bằng nhau, việc xây dựng tất cả các ứng cử viên và chọn cái nhỏ nhất về mặt từ điển sẽ thực hiện chính xác quy tắc tie-break cuối cùng. Mỗi ứng cử viên được xây dựng đại diện cho tám nhóm ban đầu giống nhau, bởi vì`::`viết tắt chính xác của nhóm 0 bị bỏ qua. Do đó, ứng cử viên được chọn đáp ứng cả ba yêu cầu: nén về mặt pháp lý, số phần còn lại tối thiểu có thể có và cách trình bày từ điển nhỏ nhất trong số các lựa chọn đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def make_candidate(groups, l, r):
    left = groups[:l]
    right = groups[r + 1:]

    # Two empty fields joined by ':' produce '::'.
    return ":".join(left + ["", ""] + right)

def abbreviate(x):
    # Exactly 32 hexadecimal digits correspond to eight 16-bit groups.
    h = f"{x:032x}"
    groups = []

    for i in range(0, 32, 4):
        part = h[i:i + 4]
        value = part.lstrip("0")
        groups.append(value if value else "0")

    best_len = 0
    runs = []

    i = 0
    while i < 8:
        if groups[i] != "0":
            i += 1
            continue

        j = i
        while j < 8 and groups[j] == "0":
            j += 1

        length = j - i

        if length >= 2:
            if length > best_len:
                best_len = length
                runs = [(i, j - 1)]
            elif length == best_len:
                runs.append((i, j - 1))

        i = j

    if best_len == 0:
        return ":".join(groups)

    answer = None

    for l, r in runs:
        candidate = make_candidate(groups, l, r)
        if answer is None or candidate < answer:
            answer = candidate

    return answer

def solve():
    out = []

    for line in sys.stdin:
        line = line.strip()
        if not line:
            continue

        x = int(line)
        out.append(abbreviate(x))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Sự chuyển đổi`f"{x:032x}"`là bước biểu diễn quan trọng. Chiều rộng của 32 không mang tính thẩm mỹ: một số nguyên như`1`phải trở thành`00000000000000000000000000000001`, nếu không thì tám ranh giới 16 bit sẽ bị mất. 

Mỗi đoạn bốn ký tự được chuyển đổi sang dạng thập lục phân chuẩn hóa của nó với`lstrip("0")`. Dự phòng cho`"0"`là cần thiết vì việc loại bỏ tất cả các số 0 khỏi`"0000"`tạo ra một chuỗi trống. 

Quét không chạy sử dụng`i`là nhóm chưa được xử lý đầu tiên và tiến bộ`j`cho đến khi quá trình chạy hiện tại kết thúc. Cài đặt`i = j`bỏ qua toàn bộ quá trình chạy, do đó mỗi nhóm trong số tám nhóm chỉ được kiểm tra với số lần không đổi. Những đoạn có độ dài một được cố tình bỏ qua. 

Trình tạo ứng cử viên sử dụng hai chuỗi trống thay vì cố gắng nối các tiền tố và hậu tố với các trường hợp đặc biệt. Ví dụ,`["1", "", "", "2"]`tham gia với`:`trở thành`1::2`, trong khi`["", "", "1"]`trở thành`::1`Và`["1", "", ""]`trở thành`1::`. Nếu tất cả các nhóm được nén,`["", ""]`trở thành`::`. 

Số nguyên Python có độ chính xác tùy ý, vì vậy việc đọc đầu vào thập phân bằng`int`xử lý trực tiếp toàn bộ phạm vi từ 0 đến (2^{128}-1). Không cần chuyển đổi thập phân sang nhị phân thủ công hoặc xử lý tràn. 

Việc so sánh ứng viên được ủy quyền có chủ ý theo thứ tự chuỗi thông thường của Python. Vì tất cả các ứng cử viên được xem xét tại thời điểm này đều có độ dài 0 lượt chạy tối ưu như nhau nên không ứng cử viên nào có thể giành chiến thắng vì tiêu chí tối ưu hóa đầu tiên ngắn hơn. Việc so sánh chuỗi chỉ giải quyết mối ràng buộc từ điển cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Số nguyên mẫu đầu tiên là`42540578239448488099419523072699400193`. Các nhóm thập lục phân được chuẩn hóa của nó là`2001:470:f003:0:0:0:0:1`. 

| tôi | Nhóm | Hiện tại không chạy | Độ dài tốt nhất | Chạy tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 |`2001`| không | 0 | không | 
| 1 |`470`| không | 0 | không | 
| 2 |`f003`| không | 0 | không | 
| 3 |`0`| 3..6, dài 4 | 4 | 3..6 | 
| 7 |`1`| không | 4 | 3..6 | 

Có một lần chạy tối đa, từ nhóm 3 đến nhóm 6. Thay thế bốn nhóm 0 đó bằng`::`cho`2001:470:f003::1`. 

Ví dụ này thể hiện sự tối ưu hóa chính một cách trực tiếp. Việc chạy bốn nhóm 0 chỉ để lại bốn nhóm rõ ràng, trong khi việc nén bất kỳ lần chạy ngắn hơn nào sẽ để lại nhiều phần hơn. 

### Mẫu 2 

Đối với đầu vào`1`, biểu diễn thập lục phân có chiều rộng cố định là`00000000000000000000000000000001`. 

| tôi | Nhóm | Hiện tại không chạy | Độ dài tốt nhất | Chạy tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 |`0`| 0..6, dài 7 | 7 | 0..6 | 
| 7 |`1`| không | 7 | 0..6 | 

Lần chạy số 0 đến đầu địa chỉ và kết thúc ngay trước lần chạy cuối cùng`1`. Người xây dựng ứng viên nhìn thấy phía bên trái trống và đưa ra`::1`. 

Dấu vết này xác nhận rằng các nhóm 0 đứng đầu phải vẫn là một phần của địa chỉ trong khi tìm kiếm khoảng thời gian nén. Chúng không bị loại bỏ dưới dạng các số 0 đứng đầu của toàn bộ số nguyên. 

### Mẫu 3 

Đối với đầu vào`0`, tất cả tám nhóm chuẩn hóa đều bằng không. 

| tôi | Nhóm | Hiện tại không chạy | Độ dài tốt nhất | Chạy tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 |`0`| 0..7, dài 8 | 8 | 0..7 | 

Lần chạy tối đa bao gồm toàn bộ địa chỉ, vì vậy ứng cử viên chỉ bao gồm`::`. 

Trường hợp này xác minh ranh giới nơi cả tiền tố và hậu tố đều trống. Đây là trường hợp mà địa chỉ IPv6 hoàn chỉnh được thể hiện bằng một dấu nén không có gì ở cả hai bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(8) = O(1) cho mỗi trường hợp thử nghiệm | Địa chỉ luôn chứa chính xác tám nhóm 16 bit và quá trình quét và xây dựng ứng cử viên chỉ kiểm tra các nhóm đó. | 
| Không gian | O(8) = O(1) cho mỗi trường hợp thử nghiệm | Chúng tôi lưu trữ tám nhóm chuẩn hóa và tối đa tám vị trí tranh cử của ứng cử viên. | 

Số nguyên đầu vào có tối đa 128 bit, do đó, ngay cả việc chuyển đổi sang hệ thập lục phân cũng là công việc có kích thước không đổi. Giới hạn một giây là đủ dễ dàng vì mọi trường hợp thử nghiệm chỉ thực hiện một số thao tác trên biểu diễn tám phần tử. Việc sử dụng bộ nhớ cũng không đáng kể so với giới hạn 256 MB mà vấn đề chỉ định. 

## Trường hợp thử nghiệm```python
import sys
import io

def make_candidate(groups, l, r):
    return ":".join(groups[:l] + ["", ""] + groups[r + 1:])

def abbreviate(x):
    h = f"{x:032x}"
    groups = []

    for i in range(0, 32, 4):
        part = h[i:i + 4]
        part = part.lstrip("0")
        groups.append(part if part else "0")

    best_len = 0
    runs = []

    i = 0
    while i < 8:
        if groups[i] != "0":
            i += 1
            continue

        j = i
        while j < 8 and groups[j] == "0":
            j += 1

        length = j - i

        if length >= 2:
            if length > best_len:
                best_len = length
                runs = [(i, j - 1)]
            elif length == best_len:
                runs.append((i, j - 1))

        i = j

    if best_len == 0:
        return ":".join(groups)

    return min(make_candidate(groups, l, r) for l, r in runs)

def solve():
    out = []
    for line in sys.stdin:
        line = line.strip()
        if line:
            out.append(abbreviate(int(line)))
    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve() + "\n"
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("42540578239448488099419523072699400193\n") == \
       "2001:470:f003::1\n", "sample 1"

assert run("1\n") == "::1\n", "sample 2"

assert run("0\n") == "::\n", "sample 3"

# Maximum 128-bit value, containing no zero groups.
assert run("340282366920938463463374607431768211455\n") == \
       "ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff\n", \
       "maximum value"

# A single zero group must not be compressed.
assert run("340282366920938463463374607431768145920\n") == \
       "ffff:ffff:ffff:ffff:ffff:ffff:ffff:0\n", \
       "single zero group"

# Two equally long zero runs. The later one is lexicographically smaller.
tie_value = (
    (1 << 112) +
    (2 << 64) +
    (3 << 16) +
    4
)
assert run(str(tie_value) + "\n") == \
       "1:0:0:2::3:4\n", \
       "lexicographic tie"

# No compressible zero run.
no_run_value = (
    (1 << 112) +
    (2 << 96) +
    (3 << 80) +
    (4 << 64) +
    (5 << 48) +
    (6 << 32) +
    (7 << 16) +
    8
)
assert run(str(no_run_value) + "\n") == \
       "1:2:3:4:5:6:7:8\n", \
       "no zero run"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`::`| Toàn bộ địa chỉ là một lần chạy bằng 0. | 
|`340282366920938463463374607431768211455`|`ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff`| Giá trị tối đa 128 bit và không có nhóm 0. | 
|`340282366920938463463374607431768145920`|`ffff:ffff:ffff:ffff:ffff:ffff:ffff:0`| Một phần số 0 không được phép nén. | 
|`1:0:0:2:0:0:3:4`được mã hóa bởi`tie_value`|`1:0:0:2::3:4`| Số lần chạy tối đa bằng nhau và sự ràng buộc về mặt từ điển. | 
|`1:2:3:4:5:6:7:8`được mã hóa bởi`no_run_value`|`1:2:3:4:5:6:7:8`| Không có khoảng thời gian nén hợp pháp. | 

## Vỏ cạnh 

Đối với địa chỉ hoàn toàn bằng 0, đầu vào là`0`. Chuyển đổi chiều rộng cố định tạo ra tám`0000`nhóm, chuẩn hóa thành tám`"0"`dây. Máy quét tìm thấy một lần chạy từ vị trí 0 đến 7 với độ dài 8, vì vậy nó hoàn toàn tốt hơn mọi cách nén có thể khác. Ứng viên không có nhóm ở hai bên và trở thành`::`. Thuật toán không bao giờ tạo ra`:`bởi vì trình tạo ứng viên sẽ chèn hai trường trống trước khi tham gia. 

Đối với một phần 0 duy nhất ở cuối, đầu vào`340282366920938463463374607431768145920`tương ứng với`ffff:ffff:ffff:ffff:ffff:ffff:ffff:0`. Máy quét nhìn thấy bảy nhóm khác 0, theo sau là một nhóm có độ dài bằng 0. Vì thời lượng chạy dưới hai nên nó bị từ chối hoàn toàn. Dự phòng là địa chỉ được chuẩn hóa, đưa ra`ffff:ffff:ffff:ffff:ffff:ffff:ffff:0`. Đây là ranh giới nắm bắt các triển khai nén mỗi lần chạy số 0 mà không kiểm tra độ dài của nó. 

Đối với các lần chạy tối đa bằng nhau, giá trị được sử dụng trong các thử nghiệm đại diện cho`1:0:0:2:0:0:3:4`. Máy quét ghi lại một chuỗi có độ dài hai ở các vị trí từ 1 đến 2 và một chuỗi có độ dài hai ở các vị trí từ 4 đến 5. Cả hai đều đáp ứng tiêu chí tối ưu hóa chính. Các ứng cử viên được tạo ra là`1::2:0:0:3:4`Và`1:0:0:2::3:4`. So sánh chúng đạt đến vị trí nén đầu tiên sau tiền tố chung`1:`. Ứng cử viên đầu tiên có`:`, trong khi cái thứ hai có`0`, Và`0`về mặt từ điển nhỏ hơn nên ứng viên thứ hai được chọn. 

Đối với một địa chỉ không có khả năng nén, giá trị kiểm tra đại diện cho`1:2:3:4:5:6:7:8`. Mọi nhóm đều khác 0 nên máy quét không bao giờ ghi lại lần chạy hợp lệ và`best_len`vẫn bằng không. Thuật toán bỏ qua việc xây dựng ứng viên và chỉ tham gia vào các nhóm chuẩn hóa, tạo ra`1:2:3:4:5:6:7:8`. Điều này cũng ngăn ngừa sự cố tình cờ`::`khỏi bị chèn vào khi không có phần nào cả. 

Đối với đầu vào`1`, địa chỉ chuẩn hóa chứa bảy nhóm 0 theo sau là`1`. Máy quét ghi lại các vị trí từ 0 đến 6 dưới dạng chuỗi có độ dài bảy, đây là mức tối ưu duy nhất. Phía bên trái của nén trống và phía bên phải là`1`, do đó chuỗi được xây dựng là`::1`. Điều này xác minh ranh giới chạy đầu và cũng xác nhận lý do tại sao số nguyên ban đầu trước tiên phải được đệm vào tất cả 32 chữ số thập lục phân.
