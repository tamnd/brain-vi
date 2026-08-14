---
title: "CF 102309B - Sức mạnh tàn bạo của gấu trúc Orz"
description: "Chương trình xây dựng lời giải đệ quy chuẩn cho bài toán Tháp Hà Nội. Đối với một tháp gồm n đĩa, trước tiên nó di chuyển n-1 đĩa trên cùng từ chốt nguồn sang chốt phụ, sau đó di chuyển đĩa n từ nguồn đến đích và cuối cùng di chuyển n-1 đĩa từ…"
date: "2026-08-13T23:42:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "B"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 77
verified: true
draft: false
---

[CF 102309B - Sức mạnh tàn bạo của gấu trúc Orz](https://codeforces.com/problemset/problem/102309/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chương trình xây dựng lời giải đệ quy chuẩn cho bài toán Tháp Hà Nội. Đối với một tòa tháp`n`đĩa, đầu tiên nó sẽ di chuyển phần trên cùng`n-1`đĩa từ chốt nguồn tới chốt phụ, sau đó di chuyển đĩa`n`từ nguồn đến đích và cuối cùng di chuyển`n-1`đĩa từ chốt phụ đến đích. 

Đối với mỗi trường hợp thử nghiệm,`n`là số lượng đĩa và`k`là một vị trí dựa trên đầu ra được tạo ra. Chúng ta cần xác định chính xác nước đi nào xuất hiện ở vị trí`k`, mà không tạo ra các bước di chuyển trước đó. Các vai trò ban đầu được cố định là nguồn`A`, điểm đến`B`, và phụ trợ`C`. Nếu dung dịch hoàn chỉnh Hà Nội chứa ít hơn`k`di chuyển, câu trả lời là`Orz`. 

Một tòa tháp với`n`đĩa tạo ra chính xác`2^n - 1`di chuyển. Vì cả hai`n`Và`k`có thể lớn như`10^18`, mô phỏng trực tiếp đệ quy là không thể. Thậm chí`n = 60`đã sản xuất rồi`2^60 - 1 = 1,152,921,504,606,846,975`di chuyển nhiều hơn mức tối đa có thể`k`. Thuật toán phải hoạt động mà không lặp qua tất cả các bước di chuyển và thậm chí nó không thể tạo ra một vòng lặp tỷ lệ với`n`khi`n`bản thân nó là`10^18`. 

Có một số trường hợp ranh giới có thể khiến việc triển khai dường như đúng không thành công. Với đầu vào`1 1`, hành động duy nhất là`move 1 from A to B`. Việc triển khai sử dụng lập chỉ mục dựa trên số 0 có thể vô tình từ chối bước đi đầu tiên này. Với đầu vào`1 2`, kết quả đúng là`Orz`, bởi vì tháp một đĩa chỉ có một đường đầu ra. Việc thực hiện bất cẩn để kiểm tra`k >= 2^n - 1`thay vì`k > 2^n - 1`sẽ từ chối không chính xác nước đi hợp lệ cuối cùng. Vì`n = 59`, tổng số lần di chuyển là`2^59 - 1`, Vì thế`59 576460752303423487`hợp lệ và yêu cầu nước đi cuối cùng, trong khi`59 576460752303423488`phải sản xuất`Orz`. Cuối cùng, đối với cực kỳ lớn`n`, chẳng hạn như`1000000000000000000 1`, câu trả lời là`move 1 from A to C`, bởi vì giải pháp Hà Nội cỡ chẵn bắt đầu bằng cách di chuyển đĩa nhỏ nhất về phía chốt phụ. Một vòng lặp chỉ đơn giản là giảm`n`từng cấp độ một sẽ không bao giờ kết thúc trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thực hiện thủ tục đệ quy đã cho và đếm xem có bao nhiêu dòng đã được tạo cho đến khi đạt đến dòng`k`. Điều này đúng vì chính chương trình đã xác định thứ tự đầu ra chính xác. Sự lặp lại của số lần di chuyển là`M(n) = 2M(n-1) + 1`với`M(0) = 0`, cho`M(n) = 2^n - 1`. Vì vậy trường hợp xấu nhất đòi hỏi`2^n - 1`các bước di chuyển được tạo ra cũng như số lượng các lệnh gọi đệ quy có thể so sánh được. Vì`n = 60`, đó là về rồi`1.15 * 10^18`di chuyển, vì vậy vũ lực là hoàn toàn không thể thực hiện được. 

Quan sát hữu ích là phép đệ quy chia đầu ra thành ba phần liên tiếp. Đối với một cuộc gọi`H(n, from, to, another)`, cái đầu tiên`2^(n-1)-1`dòng thuộc về`H(n-1, from, another, to)`. Dòng tiếp theo là lần di chuyển của đĩa`n`, và phần còn lại`2^(n-1)-1`dòng thuộc về`H(n-1, another, to, from)`. 

Điều này có nghĩa là chúng ta không bao giờ cần phải tạo ra một nước đi. Chúng ta chỉ cần quyết định vùng nào trong ba vùng này chứa vị trí`k`. Nếu như`k`nằm trong vùng đầu tiên, chúng ta bước vào bài toán con đệ quy đầu tiên. Nếu nó bằng đường ranh giới ngay sau vùng đó thì ta đã tìm được đáp án. Ngược lại, chúng tôi trừ toàn bộ vùng đầu tiên và vùng giữa khỏi`k`, sau đó nhập bài toán con thứ hai. 

Khó khăn còn lại đó là`n`bản thân nó có thể`10^18`. May mắn thay,`k`nhiều nhất là`10^18`. Một lần`n`lớn hơn`60`, khối đệ quy đầu tiên chứa`2^(n-1)-1`, chắc chắn là lớn hơn mọi khả năng có thể`k`. Chúng ta có thể bỏ qua tất cả các cấp độ đệ quy đầu tiên cùng một lúc. Mỗi cấp độ như vậy sẽ thay đổi vai trò của`to`Và`another`, vì vậy sau khi bỏ qua`t`cấp độ, chúng tôi hoán đổi hai vai trò đó một cách chính xác khi`t`thật kỳ quặc. Sau đó chúng ta có thể tiếp tục bình thường với`n = 60`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | Độ sâu đệ quy O(n) | Quá chậm | 
| Tối ưu | O(phút(n, 60)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên hãy xác định xem dòng được yêu cầu có thể tồn tại hay không. Khi`n < 60`, tổng số lần di chuyển là`2^n - 1`, vậy nếu`k`lớn hơn giá trị này, đầu ra`Orz`. Khi`n >= 60`, mọi thứ được phép`k <= 10^18`là hợp lệ bởi vì`2^60 - 1 > 10^18`. 
2. Nếu`n > 60`, bỏ qua phần đầu tiên`n - 60`mức độ đệ quy. Ở mọi cấp độ bị bỏ qua, vị trí mong muốn nhất thiết phải nằm trong lệnh gọi đệ quy đầu tiên, có đối số thay đổi từ`(from, to, another)`ĐẾN`(from, another, to)`. Do đó, chỉ có tính chẵn lẻ của số cấp độ bị bỏ qua mới quan trọng. Nếu như`n - 60`kỳ quặc, trao đổi`to`Và`another`, sau đó đặt`n = 60`. 
3. Đối với trạng thái hiện tại, hãy tính`half = 2^(n-1)`. Khối đệ quy đầu tiên chứa`half - 1`di chuyển, do đó di chuyển ở giữa xảy ra chính xác tại vị trí`half`. 
4. Nếu`k == half`, đường được yêu cầu là nước đi giữa của bài toán con Hà Nội hiện tại. Số đĩa của nó là`n`, và nó di chuyển từ`from`ĐẾN`to`, vậy hãy quay lại`move n from from to to`. 
5. Nếu`k < half`, câu trả lời nằm ở khối đệ quy đầu tiên. Thay thế vai trò chốt bằng`(from, another, to)`và giảm`n`bởi một. Giá trị của`k`không thay đổi vì vị trí đích vẫn được đo từ đầu bài toán con đó. 
6. Nếu`k > half`, câu trả lời nằm ở khối đệ quy thứ hai. Loại bỏ cái đầu tiên`half - 1`di chuyển và di chuyển giữa bằng cách thiết lập`k = k - half`. Khối thứ hai có đối số`(another, to, from)`, vì vậy hãy cập nhật ba vai trò chốt tương ứng và giảm`n`bởi một. 
7. Tiếp tục cho đến khi đạt đến vị trí chính giữa. Tối đa 60 cấp độ vẫn còn sau lớn-`n`phím tắt, vì vậy mọi trường hợp kiểm thử đều kết thúc sau một số lần lặp nhỏ. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi lần lặp, trạng thái hiện tại`(n, k, from, to, another)`mô tả chính xác cuộc gọi Hà Nội đệ quy có đầu ra chứa dòng được yêu cầu ban đầu. Đầu ra của cuộc gọi đó luôn bao gồm khối đầu tiên của`2^(n-1)-1`di chuyển, một di chuyển giữa tại vị trí`2^(n-1)`và khối thứ hai có cùng kích thước. Thuật toán chọn chính xác khối chứa`k`, thay đổi vai trò chốt để khớp với lệnh gọi đệ quy tương ứng. Khi`k`đến vị trí chính giữa, chương trình đệ quy sẽ in đĩa`n`từ`from`ĐẾN`to`, do đó đường được xây dựng chính xác là đường đầu ra được yêu cầu. Cái lớn-`n`phím tắt hợp lệ vì mọi cấp độ bị bỏ qua phải nhập khối đệ quy đầu tiên của nó và tác dụng duy nhất của việc làm như vậy nhiều lần là luân phiên`to`Và`another`chốt vai trò. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def kth_move(n, k):
    # For n < 60 we can check the exact number of moves.
    if n < 60 and k > (1 << n) - 1:
        return "Orz"

    # For n > 60, k <= 10^18 is always inside the first
    # recursive block for every skipped level.
    if n > 60:
        skipped = n - 60
        if skipped & 1:
            # Every first-recursion step swaps 'to' and 'another'.
            pass
        n = 60
    else:
        skipped = 0

    from_peg, to_peg, aux_peg = 'A', 'B', 'C'

    if skipped & 1:
        to_peg, aux_peg = aux_peg, to_peg

    while n > 0:
        half = 1 << (n - 1)

        if k == half:
            return f"move {n} from {from_peg} to {to_peg}"

        if k < half:
            # H(n-1, from, aux, to)
            to_peg, aux_peg = aux_peg, to_peg
        else:
            # Skip the first block and the middle move.
            k -= half

            # H(n-1, aux, to, from)
            from_peg, to_peg, aux_peg = aux_peg, to_peg, from_peg

        n -= 1

    return "Orz"

def solve():
    out = []

    for line in sys.stdin:
        if not line.strip():
            continue

        n, k = map(int, line.split())
        out.append(kth_move(n, k))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc kiểm tra tính hợp lệ ban đầu sử dụng`k > (1 << n) - 1`, không`>=`, bởi vì nước đi cuối cùng chính xác`2^n - 1`là hợp lệ. Việc kiểm tra chỉ được thực hiện đối với`n < 60`, vì đối với`n >= 60`tổng số lần di chuyển đã vượt quá số lần cho phép`k`. 

Vì`n > 60`, trước tiên mã tính toán số lượng cấp độ có thể bỏ qua. Ở mỗi cấp độ đó,`k`phải nằm trong lệnh gọi đệ quy đầu tiên. Cuộc gọi đó tiếp tục`from`không thay đổi khi trao đổi`to`Và`another`. Lặp lại phép biến đổi này với số lần chẵn sẽ khôi phục lại thứ tự ban đầu, trong khi số lần lẻ sẽ hoán đổi hai chốt. Mã nắm bắt chính xác điều đó với`skipped & 1`. 

Sau khi rút gọn, vòng lặp thực hiện trực tiếp ba vùng của đệ quy Hà Nội.`half`là vị trí di chuyển của đĩa hiện tại, vì lệnh gọi đệ quy đầu tiên góp phần`2^(n-1)-1`dòng. Bình đẳng xác định câu trả lời. Nhỏ hơn`k`tham gia cuộc gọi đệ quy đầu tiên mà không thay đổi`k`, trong khi lớn hơn`k`nhập cuộc gọi đệ quy thứ hai sau khi loại bỏ cuộc gọi đầu tiên`half`dòng. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn khi tính toán bằng hai. Việc thực hiện chỉ tính toán quyền hạn tối đa`2^59`bên trong vòng lặp chính và tất cả các cập nhật chốt xảy ra trước khi giảm dần`n`, khớp chính xác với lệnh gọi đệ quy. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`n = 5`Và`k = 10`. 

| n | k | một nửa | Quyết định | từ | đến | phụ trợ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 5 | 10 | 16 | khối đầu tiên | A | C | B | 
| 4 | 10 | 8 | khối thứ hai, k = 2 | B | C | A | 
| 3 | 2 | 4 | khối đầu tiên | B | A | C | 
| 2 | 2 | 2 | trả lời | B | A | C | 

Tại`n = 2`, vị trí ở giữa là`2`, vậy câu trả lời là`move 2 from B to A`, chính xác như trong mẫu. Dấu vết chứng tỏ tại sao các danh tính chốt phải được thực hiện trong suốt quá trình đệ quy thay vì giả định rằng mọi bài toán con vẫn di chuyển từ`A`ĐẾN`B`. 

Đối với mẫu thứ hai,`n = 5`Và`k = 100`. 

| n | k | tổng số bước di chuyển | Quyết định | 
| --- | --- | --- | --- | 
| 5 | 100 | 31 | không hợp lệ | 

Giải pháp Hà Nội năm đĩa chỉ chứa`31`di chuyển. Từ`100 > 31`, thuật toán trả về ngay`Orz`. Không cần phải duyệt qua đệ quy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(phút(n, 60)) | Tối đa 60 cấp độ đệ quy được xử lý sau khi bỏ qua các bước lớn`n`. | 
| Không gian | O(1) | Chỉ có số lượng đĩa, vị trí và ba nhãn chốt hiện tại được lưu trữ. | 

Giới hạn cố định của 60 đến trực tiếp từ`k <= 10^18`Và`2^60 > 10^18`. Ngay cả khi`n`lớn như`10^18`, thuật toán không bao giờ thực hiện quá 60 lần lặp. Nó sử dụng bộ nhớ không đổi và không xây dựng bất kỳ phần nào của sản lượng Hà Nội lớn theo cấp số nhân. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_text(inp: str) -> str:
    def kth_move(n, k):
        if n < 60 and k > (1 << n) - 1:
            return "Orz"

        skipped = 0
        if n > 60:
            skipped = n - 60
            n = 60

        from_peg, to_peg, aux_peg = 'A', 'B', 'C'

        if skipped & 1:
            to_peg, aux_peg = aux_peg, to_peg

        while n > 0:
            half = 1 << (n - 1)

            if k == half:
                return f"move {n} from {from_peg} to {to_peg}"

            if k < half:
                to_peg, aux_peg = aux_peg, to_peg
            else:
                k -= half
                from_peg, to_peg, aux_peg = aux_peg, to_peg, from_peg

            n -= 1

        return "Orz"

    out = []
    for line in inp.splitlines():
        if line.strip():
            n, k = map(int, line.split())
            out.append(kth_move(n, k))

    return "\n".join(out)

# Provided samples
assert solve_text("5 10\n") == "move 2 from B to A", "sample 1"
assert solve_text("5 100\n") == "Orz", "sample 2"

# Minimum-size inputs
assert solve_text("1 1\n") == "move 1 from A to B", "minimum valid case"
assert solve_text("1 2\n") == "Orz", "just beyond the minimum case"

# Equal values, n = k
assert solve_text("5 5\n") == "move 1 from C to A", "n equals k"

# Exact last valid position and first invalid position
assert solve_text("59 576460752303423487\n") == \
       "move 1 from A to B", "last valid move"
assert solve_text("59 576460752303423488\n") == \
       "Orz", "first invalid move"

# Large n, forcing the large-n shortcut
assert solve_text("1000000000000000000 1\n") == \
       "move 1 from A to C", "huge even n"

# Large odd n, checking the parity of skipped levels
assert solve_text("999999999999999999 1\n") == \
       "move 1 from A to B", "huge odd n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`move 1 from A to B`| Đầu vào hợp lệ tối thiểu và lập chỉ mục dựa trên một | 
|`1 2`|`Orz`| Vị trí đầu tiên vượt tổng sản lượng | 
|`5 5`|`move 1 from C to A`| gốc đệ quy chung với`n = k`| 
|`59 576460752303423487`|`move 1 from A to B`| Vị trí hợp lệ chính xác cuối cùng | 
|`59 576460752303423488`|`Orz`| Chính xác vị trí không hợp lệ đầu tiên | 
|`1000000000000000000 1`|`move 1 from A to C`| To lớn`n`và bỏ qua dựa trên tính chẵn lẻ | 
|`999999999999999999 1`|`move 1 from A to B`| Rất kỳ lạ`n`và sự chẵn lẻ ngược lại | 

## Vỏ cạnh 

cho`1 1`, thuật toán tính toán`half = 1`. Từ`k == half`, nó ngay lập tức quay trở lại`move 1 from A to B`. Đây là trường hợp cơ bản của phép đệ quy và xác nhận rằng vị trí là dựa trên một. 

Vì`1 2`, tổng số lần di chuyển là`2^1 - 1 = 1`. Kiểm tra tính hợp lệ ban đầu phát hiện`2 > 1`và trả về`Orz`trước khi vào vòng lặp. Điều này tránh việc dựa vào vòng lặp để phát hiện một vị trí không thể. 

Vì`59 576460752303423487`, vị trí được yêu cầu là chính xác`2^59 - 1`, bước đi cuối cùng của toàn bộ giải pháp. Kiểm tra tính hợp lệ chấp nhận nó vì điều kiện lớn hơn tổng. Quá trình đi xuống đệ quy cuối cùng đạt đến vị trí cuối cùng và quay trở lại`move 1 from A to B`. Thay thế séc bằng`k >= (1 << n) - 1`sẽ từ chối trường hợp này một cách không chính xác. 

Vì`59 576460752303423488`,`k`chính xác là một lớn hơn tổng số bước di chuyển. Kiểm tra tính hợp lệ trả về`Orz`ngay lập tức. Ranh giới này đặc biệt hữu ích vì việc triển khai có từng lỗi một có thể dễ dàng nhầm lẫn giữa vị trí hợp lệ cuối cùng với vị trí không hợp lệ đầu tiên. 

Vì`1000000000000000000 1`, thuật toán không thể giảm`n`một cấp độ tại một thời điểm Nó bỏ qua`999999999999999940`mức và chỉ giữ tính chẵn lẻ của số đó. Vì số lượng bị bỏ qua là số chẵn nên vai trò chốt vẫn được giữ nguyên`A, B, C`khi thuật toán đạt`n = 60`. Động thái đầu tiên cuối cùng là`move 1 from A to C`, phù hợp với hành vi của một giải pháp Hà Nội có quy mô đồng đều. 

Vì`999999999999999999 1`, số cấp độ bị bỏ qua là số lẻ. Thuật toán hoán đổi`B`Và`C`trước khi xử lý 60 cấp độ còn lại. Hoán vị chốt đó thay đổi bước đi đầu tiên thành`move 1 from A to B`. Trường hợp này xác nhận rằng việc bỏ qua một số lượng lớn các cấp độ đệ quy không thể đơn giản loại bỏ chính các cấp độ đó, bởi vì tính chẵn lẻ của chúng làm thay đổi nhận dạng của các chốt được sử dụng bởi bài toán con còn lại.
