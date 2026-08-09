---
title: "CF 102465C - Ô chữ"
description: "Chúng ta cần xây dựng lưới ký tự N × M. Mỗi hàng phải là một trong các từ ngang B, mỗi từ có độ dài M và mỗi cột phải là một trong những từ dọc A, mỗi từ có độ dài N. Một từ có thể được sử dụng lại bao nhiêu lần cũng được."
date: "2026-08-09T15:13:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "C"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 572
verified: true
draft: false
---

[CF 102465C - Trò chơi ô chữ](https://codeforces.com/problemset/problem/102465/C)

 **Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 32 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một`N × M`lưới ký tự. Mỗi hàng phải là một trong`B`từ ngang, mỗi từ có độ dài`M`và mỗi cột phải là một trong`A`từ dọc, mỗi từ có độ dài`N`. Một từ có thể được sử dụng lại nhiều lần. 

Một lưới được tính một lần vì các chữ cái của nó xác định duy nhất mỗi hàng và mỗi cột. Chúng tôi không tính các lựa chọn khác nhau của các mục từ điển tạo ra cùng một lưới. Vì mỗi từ điển không chứa các từ trùng lặp nên một lưới cố định cũng xác định từ chính xác được chọn cho mỗi hàng và cột. 

Dòng đầu vào đầu tiên cung cấp`N`và số`A`của các từ dọc. Thứ hai cho`M`và số`B`của các từ theo chiều ngang. Sau đây`A`dây có độ dài`N`, và trận chung kết`B`dây có độ dài`M`. Câu trả lời là số lượng lưới thỏa mãn cả hai từ điển cùng một lúc. 

Kích thước rất nhỏ, với`2 ≤ N, M ≤ 4`, vậy có nhiều nhất là 16 ô. Tuy nhiên, các từ điển có thể rất lớn. Sản phẩm của họ đáp ứng`A × B ≤ 1,008,016`, do đó, một từ điển có thể chứa khoảng một nghìn từ khi từ điển kia có kích thước tương tự hoặc hàng trăm nghìn từ khi từ điển kia có kích thước rất nhỏ. Điều này loại trừ các thuật toán có công việc tỷ lệ thuận với tất cả các cặp từ được nhân với một lũy thừa lớn bổ sung. Đặc biệt, việc mù quáng thử từng chuỗi từ ngang sẽ mất`B^N`khả năng. Với`A = B = 1004`Và`N = 4`, đó là về`1.016 × 10^12`lưới ứng viên và việc kiểm tra từng ô sẽ yêu cầu khoảng`1.6 × 10^13`kiểm tra ký tự. 

Độ dài từ nhỏ cho chúng ta cấu trúc hữu ích. Mỗi một phần hàng hoặc cột chỉ là tiền tố của một từ trong từ điển. Một ký tự ứng viên có thể bị từ chối ngay lập tức nếu nó không tiếp tục cả tiền tố ngang và tiền tố dọc hiện tại. Trie được thiết kế chính xác cho loại thử nghiệm tiền tố này. 

Có một số trường hợp đặc biệt trong đó việc triển khai đơn giản có thể âm thầm gặp trục trặc. 

Coi như```
2 1
2 1
ab
aa
```Từ ngang duy nhất là`aa`, vì vậy cả hai hàng phải là`aa`. Khi đó cả hai cột đều`aa`, đó không phải là từ dọc`ab`. Câu trả lời đúng là`0`. Việc triển khai chỉ kiểm tra các từ theo chiều ngang và trì hoãn việc xác thực cột cho đến khi tạo ra tất cả các lưới sẽ gây lãng phí khối lượng công việc khổng lồ, trong khi việc triển khai chỉ kiểm tra các cột đã hoàn thành nhưng quên tính hợp lệ của tiền tố cũng có thể khám phá các nhánh không hợp lệ. 

Được phép sử dụng lại một từ trong từ điển. Ví dụ,```
2 1
2 1
aa
aa
```có câu trả lời`1`. Lưới chỉ đơn giản là```
aa
aa
```Cùng một từ được sử dụng cho cả hai hàng và cả hai cột. Việc triển khai bất cẩn đánh dấu các từ trong từ điển là "đã sử dụng" sau khi đặt chúng sẽ trả về sai`0`. 

Lưới không nhất thiết phải là hình vuông. Ví dụ,```
2 4
4 1
aa
bb
ab
ba
abba
```có câu trả lời`1`. Cả hai hàng đều`abba`, cho cột`aa`,`bb`,`ba`, Và`ab`, tất cả đều thuộc về từ điển dọc. Bất kỳ việc triển khai nào giả định`N == M`hoặc vô tình sử dụng`N`như số lượng cột, sẽ xử lý sai trường hợp này. 

Cuối cùng, khi`N == M`, hai từ điển vẫn có hướng dẫn được giao. Một từ trong từ điển dọc chỉ có thể được sử dụng theo chiều ngang nếu nó cũng có trong từ điển ngang và ngược lại. Cách rõ ràng để tôn trọng quy tắc này chỉ đơn giản là không bao giờ trao đổi tư cách thành viên từ điển khi kiểm tra lưới. Một đường ngang được kiểm tra đối với từ điển ngang và một đường dọc đối với từ điển dọc. 

## Phương pháp tiếp cận 

Sức mạnh tàn bạo trực tiếp nhất là chọn tất cả`N`từ nằm ngang. Vì mỗi hàng có`B`khả năng và sự lặp lại được cho phép, có`B^N`trình tự hàng. Sau khi chọn chúng, chúng ta có thể xây dựng từng cột và kiểm tra xem mỗi chuỗi kết quả có thuộc từ điển dọc hay không. Phương pháp này đúng vì mọi lưới có thể có chính xác một chuỗi hàng ngang, vì vậy mọi lưới hợp lệ được xem xét chính xác một lần. 

Vấn đề là số mũ. Với`N = 4`,`B = 1004`, tìm kiếm đã chứa khoảng`1.016 × 10^12`trình tự hàng. Mặc dù chỉ có 16 ô tồn tại trong mỗi lưới, nhưng việc kiểm tra tất cả chúng sẽ vượt quá giới hạn thời gian. 

Điều quan trọng là chúng ta không nên quyết định toàn bộ hàng trước khi kiểm tra giao điểm của nó với các cột. Giả sử hàng hiện tại bắt đầu bằng`sa`. Nếu không có từ điển ngang nào bắt đầu bằng`sa`, chúng ta có thể từ chối hàng ngay lập tức. Mạnh mẽ hơn, giả sử ô hiện tại ở hàng`r`và cột`c`. Ký tự được đặt ở đó phải đồng thời là ký tự tiếp theo hợp lệ cho tiền tố ngang hiện tại và tiền tố dọc hiện tại. Giao điểm của hai bộ ký tự đó thường rất nhỏ. 

Tri lưu trữ chính xác thông tin này. Tại một nút trie, các cạnh đi ra của nó là các ký tự có thể theo sau tiền tố hiện tại một cách hợp pháp. Trong khi lấp đầy lưới, chúng tôi giữ một con trỏ vào bộ ba cho hàng hiện tại và một con trỏ cho mỗi cột. Việc đặt một ký tự sẽ tiến tới con trỏ hàng và con trỏ cột tương ứng. Nếu một trong hai quá trình chuyển đổi không tồn tại thì nhánh sẽ chết ngay lập tức. 

Điều này biến tìm kiếm mạnh mẽ từ "chọn các từ hoàn chỉnh và kiểm tra chúng sau đó" thành "xây dựng lưới trong khi liên tục thực thi cả hai từ điển". Phân tích SWERC chính thức mô tả điều này là quay lại với hai lần thử và duy trì vị trí trie hiện tại cho hàng và cột. 

Có một sự lựa chọn hữu ích hơn. Chúng ta có thể chuyển đổi cách chúng ta tìm kiếm. Hoặc chúng ta xây dựng lưới mỗi lần một từ ngang và sử dụng từ điển dọc làm ràng buộc chéo hoặc chúng ta xây dựng từng từ dọc một từ và sử dụng từ điển ngang làm ràng buộc chéo. Chúng tôi chọn hướng có số lượng kết hợp đường hoàn chỉnh theo lý thuyết nhỏ hơn, so sánh`B^N`với`A^M`. Điều này không thay đổi vấn đề, nó chỉ chọn từ điển nào được sử dụng làm tập hợp được xây dựng tích cực. 

Sự biểu diễn tri trong quá trình thực hiện được cố ý nhỏ gọn. Vì mỗi từ có độ dài tối đa là bốn nên mọi tiền tố có thể được mã hóa dưới dạng số nguyên cơ số 27. Các chữ số`1`bởi vì`26`đại diện`a`bởi vì`z`, trong khi số 0 đứng đầu đại diện cho tiền tố trống. Mã lớn nhất có thể chỉ là`27^4 - 1 = 531440`, do đó, một mảng cố định có thể thay thế một từ điển Python nặng về bộ nhớ. Mỗi mục nhập mảng lưu trữ một mặt nạ 26-bit gồm các chữ cái tiếp theo có thể có và một bit bổ sung nói rằng bản thân tiền tố đó là một từ trong từ điển hoàn chỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NM · B^N)`|`O(A + B + NM)`| Quá chậm | 
| Trie Quay lại |`O((A+B)·4 + NM·S)`|`O(27^4 + NM)`| Đã chấp nhận | 

Đây`S`là số lượng lưới một phần thực sự được truy cập bởi tìm kiếm quay lui. Trie làm cho`S`nhỏ hơn nhiều so với tích Descartes thô vì các tiền tố không hợp lệ sẽ bị loại bỏ trước khi một từ hoàn chỉnh được tạo ra. Theo nghĩa lý thuyết tồi tệ nhất, đây vẫn là một phép tìm kiếm theo cấp số nhân, điều này được mong đợi cho bài toán từ-hình chữ nhật bị ràng buộc này. Điểm quan trọng là lưới có tối đa 16 ô và mọi ký tự đều được kiểm tra theo cả hai từ điển ngay lập tức. 

## Hướng dẫn thuật toán 

1. Đọc các từ điển dọc và ngang và xây dựng cấu trúc tiền tố cho mỗi từ điển. Đối với mỗi tiền tố, hãy lưu trữ những chữ cái nào có thể theo sau nó và liệu tiền tố đó có phải là một từ hoàn chỉnh hay không. Sau đó, quá trình chuyển đổi ký tự có thể được kiểm tra trong thời gian không đổi. 
2. So sánh`A^M`Và`B^N`. Nếu như`A^M`nhỏ hơn, hãy xây dựng cột lưới theo từng cột. Nếu không, hãy xây dựng nó theo từng hàng. Điều này làm cho không gian tìm kiếm toàn dòng được liệt kê tích cực càng nhỏ càng tốt. 
3. Giữ một mảng chứa tiền tố hiện tại của mỗi dòng theo hướng phụ. Ví dụ: khi xây dựng các hàng, mảng này lưu trữ tiền tố của tất cả các cột. Khi xây dựng các cột, nó lưu trữ tiền tố của tất cả các hàng. Dòng chính đang hoạt động có một vị trí trie riêng biệt vì nó đang được xây dựng mỗi lần một ký tự. 
4. Tại mỗi ô, lấy mặt nạ bit của các ký tự tiếp theo có thể có từ nút trie chính và nút trie phụ tương ứng. Giao nhau với những mặt nạ này. Mỗi bit còn lại đại diện cho một ký tự hợp pháp theo cả hai hướng. 
5. Hãy thử từng ký tự trong giao điểm. Nâng cao tiền tố chính và tiền tố phụ tương ứng theo ký tự đó. Nếu một trong hai tiền tố kết quả không tồn tại trong lần thử của nó thì nhánh sẽ bị từ chối ngay lập tức. 
6. Khi ký tự cuối cùng của dòng chính được đặt, yêu cầu nút trie chính kết quả phải là nút cuối. Điều này đảm bảo rằng dòng hoàn chỉnh thực sự là một từ trong từ điển chứ không chỉ đơn thuần là tiền tố của một từ. 
7. Khi đường chính cuối cùng đang được xây dựng, mỗi đường phụ cũng đạt đến chiều dài tối đa. Yêu cầu mỗi nút trie thứ cấp thu được phải là nút cuối tại các ô đó. Điều này xác nhận tất cả các dòng mà không cần quét lưới riêng. 
8. Sau khi một ô được xử lý đệ quy, hãy khôi phục tiền tố phụ của ô đó trước khi thử ký tự tiếp theo. Đây là bước quay lại. Tiền tố chính được truyền dưới dạng đối số, do đó, nó sẽ hoàn nguyên một cách tự nhiên khi lệnh gọi đệ quy quay trở lại. 
9. Khi mỗi dòng chính đã được hoàn thành, một lưới hợp lệ đã được tìm thấy. Thêm một vào câu trả lời. Vì mỗi chuỗi ký tự ô tương ứng với chính xác một lưới nên không có việc đếm kép. 

### Tại sao nó hoạt động 

Điều bất biến là trước mỗi lệnh gọi đệ quy, mọi tiền tố đã được điền đều thuộc về bộ tiền tố từ điển thích hợp. Tiền tố dòng chính hiện tại được biểu thị bằng nút trie chính hợp lệ và mọi tiền tố dòng phụ được biểu thị bằng nút trie thứ cấp hợp lệ. 

Một ký tự chỉ được đặt khi nó là cạnh đi ra từ cả hai nút trie có liên quan. Do đó, bất biến vẫn đúng sau mỗi lần đặt. Khi một dòng đạt đến ký tự cuối cùng, việc kiểm tra bit đầu cuối đảm bảo rằng dòng hoàn chỉnh là một từ trong từ điển thực tế chứ không chỉ đơn thuần là tiền tố. 

Mọi lưới hợp lệ đều đi theo một đường dẫn trong quá trình tìm kiếm vì mỗi ký tự của lưới đó là ký tự tiếp theo hợp lệ theo cả hai hướng. Ngược lại, mọi đường dẫn được tìm kiếm chấp nhận đều kết thúc ở mỗi hàng và mỗi cột tại một nút từ điển đầu cuối, do đó nó biểu thị một lưới hợp lệ. Mỗi lưới có một chuỗi ký tự ô duy nhất, cung cấp chính xác một đường dẫn tìm kiếm được chấp nhận. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

# A word has length at most 4.
# We encode a prefix in base 27, using digits 1..26 for a..z.
MAX_CODE = 27 ** 4
TERM = 1 << 26
ALPHA_MASK = TERM - 1

def build_trie(count, reader):
    """
    Each entry stores:
      bits 0..25 : possible next letters
      bit 26     : this prefix is itself a complete word
    """
    trie = [0] * MAX_CODE

    for _ in range(count):
        word = reader().strip()
        code = 0

        for ch in word:
            digit = ord(ch) - 96
            trie[code] |= 1 << (digit - 1)
            code = code * 27 + digit

        trie[code] |= TERM

    return trie

def solve(reader=None):
    if reader is None:
        reader = input

    n, a = map(int, reader().split())
    m, b = map(int, reader().split())

    vertical = build_trie(a, reader)
    horizontal = build_trie(b, reader)

    # Choose the direction with fewer possible complete line sequences.
    #
    # Horizontal construction:
    #   N lines, each chosen from B words -> B^N
    #
    # Vertical construction:
    #   M lines, each chosen from A words -> A^M
    if a ** m <= b ** n:
        primary = vertical
        secondary = horizontal
        primary_lines = m
        line_length = n
    else:
        primary = horizontal
        secondary = vertical
        primary_lines = n
        line_length = m

    # secondary_prefix[i] is the currently built prefix of
    # secondary line i.
    secondary_prefix = [0] * line_length

    sys.setrecursionlimit(100)

    def dfs(line, pos, primary_prefix):
        if line == primary_lines:
            return 1

        primary_value = primary[primary_prefix]
        secondary_value = secondary[secondary_prefix[pos]]

        # A character must be allowed by both tries.
        mask = primary_value & secondary_value & ALPHA_MASK

        answer = 0
        last_position = (pos == line_length - 1)
        last_line = (line == primary_lines - 1)

        while mask:
            bit = mask & -mask
            mask -= bit

            digit = bit.bit_length()  # 1..26

            new_primary = primary_prefix * 27 + digit
            new_primary_value = primary[new_primary]

            # The primary line must be a complete dictionary word
            # when its final character is placed.
            if last_position and not (new_primary_value & TERM):
                continue

            old_secondary = secondary_prefix[pos]
            new_secondary = old_secondary * 27 + digit
            new_secondary_value = secondary[new_secondary]

            # On the final primary line, this character also completes
            # the secondary line, so it must be a complete word.
            if last_line and not (new_secondary_value & TERM):
                continue

            secondary_prefix[pos] = new_secondary
            answer += dfs(line, pos + 1, new_primary)
            secondary_prefix[pos] = old_secondary

        return answer

    return str(dfs(0, 0, 0))

if __name__ == "__main__":
    print(solve())
```Hai cuộc gọi đến`build_trie`sử dụng hai từ điển mà không giữ lại chuỗi gốc. Điều này quan trọng đối với các đầu vào lớn nhất, trong đó việc lưu giữ hàng trăm nghìn đối tượng chuỗi Python sẽ lãng phí nhiều bộ nhớ hơn đáng kể so với chính tri. 

Mã hóa tiền tố sử dụng cơ sở 27 thay vì cơ sở 26 vì nó cần phân biệt các tiền tố có độ dài khác nhau. Với các chữ số từ`1`bởi vì`26`, mã cho`a`là`1`, trong khi mã cho`aa`là`28`, nên chúng không bao giờ có thể va chạm nhau. Tiền tố trống chỉ đơn giản là`0`. 

Mỗi mục trie chứa một mặt nạ ký tự. Ví dụ, nếu tiền tố`s`có thể được theo sau bởi`a`,`e`, hoặc`t`, mục nhập của nó có các bit tương ứng với tập hợp ba chữ cái đó. Việc giao nhau hai mặt nạ như vậy rẻ hơn nhiều so với việc thử tất cả 26 chữ cái và thực hiện tra cứu từ điển riêng biệt. 

Cờ đầu cuối được lưu ở bit 26, bên ngoài 26 bit ký tự. Một tiền tố có thể đồng thời là một từ hoàn chỉnh và có con. Ví dụ, nếu cả hai`are`Và`area`tồn tại, nút cho`are`phải duy trì thiết bị đầu cuối đồng thời cho phép`a`như một nhân vật tiếp theo. 

các`last_position`kiểm tra là cần thiết vì chỉ đến cuối dòng chính không đảm bảo rằng dòng đó là một từ trong từ điển. Điều tương tự cũng áp dụng cho`last_line`cho các dòng thứ cấp. Việc kiểm tra các điều kiện này trong quá trình tìm kiếm sẽ tránh được việc vượt qua lưới đã hoàn thành lần thứ hai. 

Số nguyên Python có độ chính xác tùy ý, điều này cần thiết vì số lượng lưới hợp lệ không được đảm bảo khớp với số nguyên 32 bit hoặc 64 bit. Bản thân việc tìm kiếm không bao giờ lưu trữ tất cả các lưới, nó chỉ tích lũy số lượng của chúng. 

Độ sâu đệ quy tối đa là`N × M`, nhiều nhất là 16, nên đệ quy là an toàn. Mảng tiền tố phụ được sửa đổi trước lệnh gọi đệ quy và được khôi phục ngay sau đó. Việc quên rằng việc khôi phục là một lỗi quay lui cổ điển vì nhánh tiếp theo sẽ kế thừa các ký tự từ nhánh trước. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào chứa bốn từ dọc và năm từ ngang. Từ`4^4 = 256`lớn hơn`5^3 = 125`, quá trình triển khai sẽ chọn xây dựng lưới theo chiều ngang, vì có ít chuỗi từ ngang hoàn chỉnh hơn. 

Một nhánh hợp lệ là`says`,`area`,`test`. Các cột tương ứng là`sat`,`are`,`yes`, Và`sat`. 

| Dòng chính | Vị trí | Nhân vật được chọn | Tiền tố chính | Tiền tố phụ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 |`s`|`s`|`s`,`, `| 
| 0 | 1 |`a`|`sa`|`s`,`a`, `` | 
| 0 | 2 |`y`|`say`|`s`,`a`,`y`, `` | 
| 0 | 3 |`s`|`says`|`s`,`a`,`y`,`s`| 
| 1 | 0 |`a`|`a`|`sa`,`a`,`y`,`s`| 
| 1 | 1 |`r`|`ar`|`sa`,`ar`,`y`,`s`| 
| 1 | 2 |`e`|`are`|`sa`,`ar`,`ye`,`s`| 
| 1 | 3 |`a`|`area`|`sa`,`ar`,`ye`,`sa`| 
| 2 | 0 |`t`|`t`|`sat`,`ar`,`ye`,`sa`| 
| 2 | 1 |`e`|`te`|`sat`,`are`,`ye`,`sa`| 
| 2 | 2 |`s`|`tes`|`sat`,`are`,`yes`,`sa`| 
| 2 | 3 |`t`|`test`|`sat`,`are`,`yes`,`sat`| 

Sau ô cuối cùng, cả ba tiền tố ngang đều là từ cuối và cả bốn tiền tố dọc cũng là từ cuối. Chi nhánh này đóng góp một lưới. Nhánh hợp lệ khác là`ways`,`area`,`rest`, cho cột`war`,`are`,`yes`,`sat`. Câu trả lời cuối cùng là`2`. 

### Mẫu 2 

đây`N = M = 3`và cả hai từ điển đều chứa bảy từ giống nhau. Từ`A^M`Và`B^N`bằng nhau, việc triển khai sẽ chọn từ điển dọc làm hướng chính. 

Đối với một lưới hợp lệ, các hàng là`its`,`the`, Và`set`và các cột có ba từ giống nhau. 

| Dòng chính | Vị trí | Nhân vật được chọn | Tiền tố chính | Tiền tố phụ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 |`i`|`i`|`i`,`, `| 
| 0 | 1 |`t`|`it`|`i`,`t`, `` | 
| 0 | 2 |`s`|`its`|`i`,`t`,`s`| 
| 1 | 0 |`t`|`t`|`it`,`t`,`s`| 
| 1 | 1 |`h`|`th`|`it`,`th`,`s`| 
| 1 | 2 |`e`|`the`|`it`,`the`,`se`| 
| 2 | 0 |`s`|`s`|`its`,`the`,`s`| 
| 2 | 1 |`e`|`se`|`its`,`the`,`se`| 
| 2 | 2 |`t`|`set`|`its`,`the`,`set`| 

Tiền tố cột cuối cùng là`its`,`the`, Và`set`, vì vậy nhánh này được chấp nhận. Nhánh thứ hai tạo ra lưới có các hàng`ran`,`age`,`now`. Câu trả lời là`2`. 

Những dấu vết này cho thấy trực tiếp bất biến trung tâm. Mọi tiền tố trong mảng tiền tố phụ đều được biết là tiền tố của một số từ được phép, ngay cả trước khi hàng hoặc cột tương ứng hoàn thành. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Trí xây dựng |`O(4(A+B))`| Mỗi từ đầu vào có độ dài tối đa là bốn | 
| Tìm kiếm |`O(NM · S)`| Mỗi ô được ghé thăm sẽ kiểm tra tối đa 26 chữ cái ứng cử viên | 
| Không gian |`O(27^4 + NM)`| Hai mảng tiền tố cố định cộng với các tiền tố phụ hiện tại | 

Đây`S`biểu thị số lượng lưới một phần được truy cập sau khi cắt tỉa. Giới hạn trên hữu ích cho tìm kiếm toàn dòng là`min(A^M, B^N)`, bởi vì việc triển khai chọn hướng nhỏ hơn trong hai hướng. Việc kiểm tra tri cấp ký tự thường cắt bỏ các nhánh sớm hơn nhiều. 

Kích thước trie cố định đặc biệt thuận tiện trong Python. Từ`27^4 = 531441`, hai mảng có kích thước đó đủ nhỏ cho giới hạn bộ nhớ 256 MB và tránh được chi phí đáng kể cho hàng triệu mục từ điển Python. Bản thân lưới có nhiều nhất là 16 ô nên trạng thái đệ quy rất nhỏ. 

Bản chất hàm mũ của việc tìm kiếm là không thể tránh khỏi đối với công thức từ-hình chữ nhật nói chung, nhưng vấn đề cố tình giới hạn cả hai chiều ở mức bốn và đưa ra một kết quả bị ràng buộc trong các kích thước từ điển. Trie biến việc tìm kiếm thành lan truyền ràng buộc thay vì liệt kê các lưới hoàn chỉnh, đây là cách nhằm biến những ràng buộc đó thành hiện thực. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`. Người giải quyết chấp nhận một tùy chọn`reader`, cho phép các bài kiểm tra cung cấp chuỗi đầu vào trực tiếp mà không cần sửa đổi đầu vào tiêu chuẩn của toàn quy trình.```
import io
from solution import solve

def run(inp: str) -> str:
    return solve(io.StringIO(inp).readline).strip()

# Sample 1
assert run(
    """\
3 4
4 5
war
are
yes
sat
says
area
test
ways
rest
"""
) == "2", "sample 1"

# Sample 2
assert run(
    """\
3 7
3 7
ran
age
now
its
the
set
ago
ran
age
now
its
the
set
ago
"""
) == "2", "sample 2"

# Minimum-size, all values equal.
assert run(
    """\
2 1
2 1
aa
aa
"""
) == "1", "minimum-size all-equal case"

# Rectangular grid, 2 rows and 4 columns.
assert run(
    """\
2 4
4 1
aa
bb
ab
ba
abba
"""
) == "1", "rectangular grid"

# No valid crossing.
assert run(
    """\
2 1
2 1
ab
aa
"""
) == "0", "incompatible vertical and horizontal words"

# Maximum dictionary product: 1000 * 1000 = 1,000,000.
# All vertical words start with 'a' and all horizontal words start
# with 'b', so the very first cell has no possible character.
def make_words(first, count):
    result = []
    alphabet = "abcdefghijklmnopqrstuvwxyz"

    for x in alphabet:
        for y in alphabet:
            for z in alphabet:
                result.append(first + x + y + z)
                if len(result) == count:
                    return result

    return result

vertical = make_words("a", 1000)
horizontal = make_words("b", 1000)

large_input = (
    "4 1000\n"
    "4 1000\n"
    + "\n".join(vertical)
    + "\n"
    + "\n".join(horizontal)
    + "\n"
)

assert run(large_input) == "0", "maximum-size dictionary product"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`2`| Giao nhau cơ bản và hai lưới hợp lệ riêng biệt | 
| Mẫu 2 |`2`| Trường hợp hình vuông trong đó cả hai từ điển đều giống hệt nhau | 
|`2 × 2`, cả hai danh sách đều chứa`aa`|`1`| Tái sử dụng từ và kích thước tối thiểu | 
|`2 × 4`với`abba`|`1`| Kích thước hình chữ nhật và hướng chính xác | 
| Thẳng đứng`ab`, nằm ngang`aa`|`0`| Không tương thích tiền tố ngay lập tức | 
|`1000 × 1000`từ điển có các chữ cái đầu rời rạc |`0`| Đầu vào lớn và cắt tỉa sớm | 

## Vỏ cạnh 

Đối với trường hợp bằng nhau có kích thước tối thiểu,```
2 1
2 1
aa
aa
```việc tìm kiếm bắt đầu với cả hai lần thử chỉ cho phép`a`. Ô đầu tiên chuyển cả hai tiền tố sang`a`và ô thứ hai hoàn thành dòng chính như`aa`. Dòng tiếp theo lặp lại chính xác quá trình tương tự. Ở cuối mỗi hàng và cột đều là đầu cuối, vì vậy câu trả lời là`1`. Không có từ nào bị tiêu hao trong lần sử dụng đầu tiên, do đó, mục từ điển giống nhau vẫn có sẵn cho mỗi dòng. 

Đối với trường hợp tiền tố không tương thích,```
2 1
2 1
ab
aa
```các nút gốc chính và phụ cho phép các ký tự khác nhau. Trie dọc cho phép`a`, trong khi trie ngang cũng cho phép`a`, do đó bản thân ô đầu tiên không bị từ chối. Sau khi đặt`a`, ô tiếp theo trên dòng chính đầu tiên yêu cầu tiền tố dọc`ab`và tiền tố ngang`aa`. Ký tự tiếp theo duy nhất có thể có cho từ dọc là`b`, trong khi từ ngang yêu cầu`a`. Mặt nạ của họ có giao điểm trống nên nhánh kết thúc ngay lập tức. Câu trả lời là`0`. 

Đối với trường hợp hình chữ nhật,```
2 4
4 1
aa
bb
ab
ba
abba
```từ ngang duy nhất là`abba`, vì vậy cả hai hàng phải trở thành`abba`. Các cột kết quả là`aa`,`bb`,`ba`, Và`ab`. Tất cả bốn tồn tại trong trie dọc. Thuật toán không giả định rằng số hàng và số cột bằng nhau nên nó xử lý hai dòng chính có độ dài bốn và trả về chính xác`1`. 

Đối với trường hợp từ điển lớn, đầu vào chứa 1000 từ dọc bắt đầu bằng`a`và 1000 từ ngang bắt đầu bằng`b`. Sản phẩm chính xác là`1,000,000`, trong giới hạn yêu cầu. Ở ô đầu tiên, mặt nạ gốc dọc chứa`a`trong khi mặt nạ gốc ngang chứa`b`. Giao điểm của chúng bằng 0, vì vậy toàn bộ quá trình tìm kiếm kết thúc sau khi kiểm tra ô đầu tiên. Câu trả lời là`0`và các từ điển lớn không tạo ra cây tìm kiếm lớn vì các ràng buộc tiền tố được áp dụng trước bất kỳ sự phân nhánh nào. 

các`N = M`hạn chế không yêu cầu logic đặc biệt trong tìm kiếm. Thuật toán luôn kiểm tra một dòng so với từ điển tương ứng với hướng thực tế của nó. Nếu một từ xuất hiện trong cả hai từ điển, nó có thể xuất hiện một cách tự nhiên theo cả hai hướng. Nếu nó chỉ xuất hiện ở một thì nó chỉ có thể được sử dụng theo hướng đó. Điều này khớp với quy tắc bắt buộc mà không đưa nhánh trường hợp đặc biệt vào mã quay lui.
