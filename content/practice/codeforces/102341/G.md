---
title: "CF 102341G - Gurdurr"
description: "Mỗi lớp chỉ có thể có một trong bốn cấu hình ổn định quan trọng đối với trò chơi. Lớp hoàn toàn nguyên vẹn là lớp III. Một lớp có hai khối là II. hoặc .II và hai trường hợp này có hành vi trò chơi giống hệt nhau, vì vậy chúng ta có thể coi chúng là một trạng thái."
date: "2026-08-14T05:08:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "G"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 237
verified: true
draft: false
---

[CF 102341G - Gurdurr](https://codeforces.com/problemset/problem/102341/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi lớp chỉ có thể có một trong bốn cấu hình ổn định quan trọng đối với trò chơi. Một lớp hoàn toàn nguyên vẹn là`III`. Một lớp có hai khối là`II.`hoặc`.II`và hai trường hợp này có hành vi trò chơi giống hệt nhau, vì vậy chúng ta có thể coi chúng là một trạng thái. Hai cấu hình một khối là`I.I`Và`.I.`. 

Sự khác biệt cơ bản giữa hai trạng thái cuối cùng là cả hai đều hoàn toàn bất động, nhưng`.I.`ảnh hưởng đến hàng xóm của nó bởi vì một hàng xóm khác`.I.`không thể ở gần nó. MỘT`I.I`lớp không áp đặt hạn chế đó. 

Một động thái từ`III`có thể loại bỏ một khối bên ngoài và biến nó thành`II.`hoặc`.II`, hoặc loại bỏ khối ở giữa và biến nó thành`I.I`. MỘT`II.`hoặc`.II`lớp có thể loại bỏ khối bên ngoài còn lại của nó và trở thành`.I.`, nhưng chỉ khi điều đó không tạo ra hai lớp đơn lân cận. Khi một lớp trở nên`I.I`hoặc`.I.`, nó không bao giờ thay đổi nữa. 

Đầu vào cho tới 30.000 tháp độc lập. Đối với mỗi tòa tháp,`n`tối đa là 20, tiếp theo là cấu hình ba ký tự của mỗi lớp từ trên xuống dưới. Chúng ta chỉ cần quyết định xem vị trí ban đầu là người chơi thứ nhất thắng hay người chơi thứ nhất thua. 

Giá trị nhỏ`n <= 20`là manh mối chính. Tìm kiếm trực tiếp ở trạng thái trò chơi sẽ có khoảng bốn khả năng cho mỗi lớp, đưa ra giới hạn trên của`4^20 = 1,099,511,627,776`mã hóa trạng thái ổn định thậm chí trước khi xem xét hạn chế về độ ổn định giữa các lớp đơn lân cận. Ngay cả việc ghi nhớ cũng không thể làm cho điều đó trở nên khả thi. Chúng ta cần khai thác thực tế rằng sự tương tác giữa các lớp chỉ mang tính cục bộ và các lớp bất biến sẽ phân chia trò chơi. 

Có một số trường hợp khó khăn mà giải pháp phải xử lý chính xác. Một đĩa đơn`I.I`lớp không có động thái hợp pháp vì việc loại bỏ một trong hai khối bên ngoài sẽ để lại một khối bên ngoài, vì vậy câu trả lời của nó là`Second`. Một đĩa đơn`.I.`lớp cũng không thể di chuyển được, vì vậy`1 / .I.`là`Second`. Một đĩa đơn`III`lớp có hai loại di chuyển và đang chiến thắng, vì vậy`1 / III`là`First`. 

Một trường hợp tinh tế khác là một lớp đầy đủ bên cạnh`.I.`. Ví dụ,```
2.I.III
```là`First`. các`III`lớp có hai bước di chuyển hợp pháp: loại bỏ khối giữa của nó sẽ tạo ra`I.I`, trong khi loại bỏ một khối bên ngoài sẽ tạo ra`II.`hoặc`.II`. Ở cả hai vị trí kết quả, lớp đã thay đổi không thể di chuyển thêm vì nó nằm cạnh`.I.`. Do đó, toàn bộ thành phần cục bộ có giá trị Grundy 1. Xử lý mọi lớp đầy đủ như một lớp độc lập thông thường`III`lớp sẽ cho sự phân hủy sai. 

Trường hợp ranh giới thứ hai là hai lớp đơn. Một đầu vào như```
2.I..I.
```hoàn toàn không được phép, vì tháp ban đầu sẽ không ổn định. Việc triển khai bất cẩn không duy trì được điều kiện ổn định đã hứa có thể cố gắng xử lý nó và vô tình cho phép di chuyển bất hợp pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thể hiện mọi lớp theo cấu hình hiện tại của nó, liệt kê mọi khối có thể bị loại bỏ, loại bỏ các chuyển động làm cho tòa tháp không ổn định và giải quyết đệ quy các vị trí kết quả. Vì mỗi nước đi sẽ loại bỏ một khối nên quá trình đệ quy sẽ kết thúc và phép lặp lại trò chơi khách quan thông thường sẽ xác định chính xác vị trí thắng và thua. Việc ghi nhớ sẽ tránh giải quyết cùng một vị trí nhiều lần. 

Vấn đề là số lượng vị trí. Ngay cả khi mỗi lớp được phép có bốn trạng thái độc lập thì vẫn có`4^20`, Về`1.1 * 10^12`, mã hóa có thể. Việc tìm kiếm được ghi nhớ trên không gian trạng thái đó vượt xa giới hạn. Nếu không ghi nhớ, cây đệ quy thậm chí còn lớn hơn vì có thể đạt được các cấu hình giống nhau thông qua các thứ tự loại bỏ khác nhau. 

Quan sát quan trọng là`I.I`Và`.I.`bản thân không thể di chuyển. Lớp như vậy vĩnh viễn tách trò chơi thành các phần độc lập. Sau khi cố định dấu phân cách, các lớp duy nhất vẫn cần được biểu diễn là`III`và trạng thái hai khối`II.`hoặc`.II`. 

Có một hiệu ứng ranh giới. MỘT`.I.`dấu phân cách ngăn chặn các hàng xóm ngay lập tức của nó trở thành các lớp đơn. Nếu một người hàng xóm như vậy đã là một lớp hai khối, thì nó sẽ trở nên hoàn toàn bất động. Nếu nó là`III`, nó có chính xác một nước đi hiệu quả và giá trị Grundy của nó là 1. Điều này cho phép chúng tôi loại bỏ các lớp ranh giới đó khỏi phần phức tạp của trò chơi. 

Những gì còn lại là một phân khúc chỉ bao gồm`III`và các lớp hai khối. Mã hóa`III`bởi bit 1 và`II.`hoặc`.II`bằng bit 0. Một đoạn có độ dài`m`hiện được mô tả bằng một bitmask với`m`bit. chỉ có`2^m`những chiếc mặt nạ như vậy. 

Đối với mỗi mặt nạ, chúng ta có thể tính giá trị Grundy của nó trực tiếp từ tất cả các nước đi hợp pháp. Khi một`III`vị trí được chọn, việc loại bỏ khối bên ngoài sẽ xóa bit của nó và giữ cho phân đoạn được kết nối. Loại bỏ khối giữa sẽ tạo ra`I.I`, là một dấu phân cách cố định, do đó tiền tố và hậu tố còn lại trở thành các trò chơi độc lập và các giá trị Grundy của chúng là XORed. 

Khi vị trí hai khối được chọn, động thái hợp pháp duy nhất của nó là trở thành`.I.`. Singleton mới đó chặn hai hàng xóm của nó. Do đó, những người hàng xóm đó sẽ biến mất khỏi phân khúc đang hoạt động. Nếu một trong những người hàng xóm đó là`III`, nó đóng góp một giá trị Grundy độc lập là 1 vì nó có thể thực hiện đúng một bước nữa trước khi trở thành bất động. 

Mỗi phân đoạn kết quả đều ngắn hơn phân đoạn hiện tại hoặc có cùng độ dài nhưng mặt nạ nhỏ hơn, do đó các trạng thái có thể được tính toán trước theo chiều dài tăng dần và thứ tự mặt nạ. 

Độ phức tạp thu được chỉ theo cấp số nhân trong`n`, không phải ở số lượng cấu hình tháp hoàn chỉnh. Với`n <= 20`, tổng số trạng thái phân đoạn là`2^1 + 2^2 + ... + 2^20 = 2^21 - 2`, 

và mỗi bang kiểm tra tối đa 20 vị trí. Đây là dự định`O(n 2^n)`tiền xử lý, tiếp theo là xử lý tuyến tính của mọi tháp đầu vào. Giống nhau`O(2^n n + tn)`sự phức tạp được mô tả bằng các phân tích cuộc thi độc lập của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với khả năng ghi nhớ |`O(n 4^n)`trong trường hợp xấu nhất |`O(4^n)`| Quá chậm | 
| Tiền xử lý SG tối ưu |`O(n 2^n + tn)`|`O(2^n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng lớp và phân loại nó thành`F`,`D`,`T`, hoặc`S`, Ở đâu`F = III`,`D = II.`hoặc`.II`,`T = I.I`, Và`S = .I.`. Hai hướng của trạng thái hai khối là tương đương nhau vì cả hai đều có chính xác một khả năng loại bỏ và trở thành đơn vị ở giữa. 
2. Đánh dấu mọi`T`Và`S`như một dấu phân cách. Đồng thời đánh dấu cả hai hàng xóm của mỗi`S`. Hàng xóm của một singleton không thể thực hiện động tác tạo ra một singleton khác bên cạnh nó. 
3. Mỗi lần đánh dấu`F`bên cạnh một`S`đóng góp giá trị Grundy 1. Lớp như vậy có hai loại bỏ hợp pháp, một tạo ra`T`và sản xuất khác`D`; cả hai trạng thái kết quả đều không thể di chuyển được vì lớp vẫn ở bên cạnh`S`. Do đó, tất cả các tùy chọn của nó đều có giá trị Grundy 0, mang lại`mex{0} = 1`. 
4. Quét các lớp chưa được đánh dấu và chia chúng thành các phân đoạn tối đa. Mỗi lớp trong một phân đoạn như vậy hoặc`F`hoặc`D`, do đó mã hóa`F`như bit 1 và`D`dưới dạng bit 0. XOR giá trị Grundy được tính toán trước của mọi phân đoạn vào câu trả lời. 
5. Để tính toán trước một đoạn có độ dài`m`, cho phép`sg[m][mask]`là giá trị Grundy của nó. Đối với mọi vị trí`k`, kiểm tra bit của nó. 
6. Nếu bit`k`là 1, lớp là`F`. Việc loại bỏ một khối bên ngoài sẽ thay đổi nó thành`D`, đưa ra trạng thái với bit`k`đã xóa. Loại bỏ khối giữa sẽ thay đổi nó thành`T`, chia các vị trí còn lại thành một đoạn bên trái độc lập và một đoạn bên phải độc lập. Thêm cả hai giá trị Grundy thu được vào bộ mex. 
7. Nếu bit`k`là 0, lớp là`D`. Động thái duy nhất của nó thay đổi nó thành`S`. Người độc thân đó tạo nên vị trí`k-1`Và`k+1`không thể tiếp tục chuyển sang trạng thái đơn lẻ, vì vậy họ sẽ bị loại khỏi trò chơi đang hoạt động. Một người hàng xóm`F`có đúng một nước đi còn lại và đóng góp 1 nước đi, trong khi nước đi lân cận`D`không đóng góp gì cả. Các phần nằm ngoài những vùng lân cận đó vẫn là các phân đoạn độc lập. 
8. Lấy tổng của tất cả các giá trị Grundy thu được từ các nước đi hợp pháp. Đó là`sg[m][mask]`. 
9. Toàn bộ tòa tháp là tổng hợp rời rạc của các thành phần được tìm thấy trong quá trình quét. Theo định lý Sprague-Grundy, người chơi đầu tiên thắng chính xác khi XOR của tất cả các giá trị Grundy thành phần khác 0. 

### Tại sao nó hoạt động 

Điều bất biến là mọi phân đoạn hoạt động chỉ chứa`III`và các lớp hai khối, trong khi mọi lớp bên ngoài các phân đoạn đã không thể di chuyển được hoặc là hiệu ứng ranh giới ngay lập tức gây ra bởi`.I.`. MỘT`III`di chuyển hoặc vẫn ở trong cùng một phân khúc hoặc tạo`I.I`và chia tách nó. Một bước di chuyển hai khối tạo ra`.I.`và loại bỏ hai hàng xóm của nó khỏi sự tương tác trong tương lai. Do đó, mọi bước chuyển hợp lệ từ một phân đoạn đều tương ứng chính xác với một trong các chuyển đổi được sử dụng bởi phép lặp và mọi chuyển đổi được tạo ra bởi phép lặp đều tương ứng với một bước chuyển đổi hợp lệ. 

Từ`I.I`Và`.I.`không thể di chuyển, các thành phần khác nhau bị chúng ngăn cách sẽ không bao giờ tương tác nữa. Do đó, các giá trị Grundy của chúng được kết hợp bằng XOR. Việc tính toán trước`sg`do đó, giá trị là giá trị Grundy chính xác của mọi thành phần và XOR cuối cùng bằng 0 chính xác là điều kiện cho vị trí thua. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def build_sg(max_n):    # sg[m][mask] = Grundy value of a segment of length m.    # A bit 1 means III, a bit 0 means II. or .II.    sg = [bytearray(1 << m) for m in range(max_n + 1)]
    # Empty segment.    sg[0][0] = 0
    for m in range(1, max_n + 1):        cur = sg[m]
        for mask in range(1 << m):            seen = 0
            for k in range(m):                bit = 1 << k
                if mask & bit:                    # Move 1: III -> II. / .II.                    g = cur[mask ^ bit]                    seen |= 1 << g
                    # Move 2: III -> I.I.                    # The new I.I. is immovable and separates                    # the prefix and suffix.                    left_len = k                    left_mask = mask & (bit - 1)
                    right_len = m - k - 1                    right_mask = mask >> (k + 1)
                    g = sg[left_len][left_mask] ^ sg[right_len][right_mask]                    seen |= 1 << g
                else:                    # Move: II. / .II -> .I.                    # The new singleton blocks its immediate neighbors.                    g = 0
                    # Active part strictly to the left of k-1.                    if k >= 2:                        left_len = k - 1                        left_mask = mask & ((1 << (k - 1)) - 1)                        g ^= sg[left_len][left_mask]
                    # If k-1 exists and is III, it has exactly one                    # remaining move after k becomes a singleton.                    if k >= 1 and (mask & (1 << (k - 1))):                        g ^= 1
                    # Active part strictly to the right of k+1.                    if k + 2 < m:                        right_len = m - k - 2                        right_mask = mask >> (k + 2)                        g ^= sg[right_len][right_mask]
                    # Symmetric boundary contribution.                    if k + 1 < m and (mask & (1 << (k + 1))):                        g ^= 1
                    seen |= 1 << g
            # mex(seen)            g = 0            while seen & (1 << g):                g += 1
            cur[mask] = g
    return sg

def solve(data):    it = iter(data.split())    t = int(next(it))
    tests = []    max_n = 0
    for _ in range(t):        n = int(next(it))        layers = [next(it).decode() for _ in range(n)]        tests.append((n, layers))        max_n = max(max_n, n)
    sg = build_sg(max_n)
    out = []
    for n, layers in tests:        # 0 = singleton .I.        # 1 = I.I        # 2 = II. or .II        # 3 = III        a = [0] * n
        for i, s in enumerate(layers):            if s == b"III":                a[i] = 3            elif s == b".I.":                a[i] = 0            elif s == b"I.I":                a[i] = 1            else:                a[i] = 2
        # Mark layers that cannot belong to an ordinary active segment.        blocked = [False] * n
        for i in range(n):            if a[i] == 0:                blocked[i] = True                if i > 0:                    blocked[i - 1] = True                if i + 1 < n:                    blocked[i + 1] = True            elif a[i] == 1:                blocked[i] = True
        answer = 0        mask = 0        length = 0
        for i in range(n):            # A full layer adjacent to .I. is an independent SG-1 game.            if blocked[i] and a[i] == 3:                answer ^= 1
            if not blocked[i]:                # III -> 1, II. / .II -> 0                mask = (mask << 1) | (a[i] - 2)                length += 1            else:                if length:                    answer ^= sg[length][mask]                    length = 0                    mask = 0
        if length:            answer ^= sg[length][mask]
        out.append("First\n" if answer else "Second\n")
    return "".join(out)

def main():    data = sys.stdin.buffer.read().splitlines()    sys.stdout.write(solve(b"\n".join(data)))

if __name__ == "__main__":    main()
```Quá trình tiền xử lý lưu trữ một`bytearray`cho mỗi chiều dài đoạn. Việc sử dụng mảng byte rất quan trọng trong Python vì có khoảng`2^21`tổng số trạng thái được tính toán trước và mọi giá trị Grundy đều nhỏ. Một đối tượng số nguyên Python bình thường cho mỗi mục trong bảng sẽ tiêu tốn nhiều bộ nhớ hơn đáng kể. 

Sự tái phát diễn ra sau hai loại di chuyển thực tế. Đối với một chút thiết lập,`mask ^ bit`đại diện cho việc loại bỏ một khối bên ngoài khỏi`III`, trong khi các mặt nạ tiền tố và hậu tố thể hiện việc loại bỏ khối ở giữa và tạo ra`I.I`. Đối với bit 0, lớp hai khối được chọn sẽ trở thành`.I.`, do đó các lân cận trực tiếp sẽ bị loại khỏi các phân đoạn hoạt động còn lại. 

Mặt nạ bên trái và bên phải sử dụng các vị trí dựa trên số 0. Sau khi chọn vị trí`k`, phần hoạt động bên trái kết thúc tại`k - 2`, trong khi phần hoạt động bên phải bắt đầu tại`k + 2`. Đó là lý do tại sao quá trình chuyển đổi hai khối sử dụng`k - 1`Và`k + 2`khi tính độ dài đoạn còn lại. Đây là những nơi có nhiều khả năng xảy ra lỗi riêng lẻ nhất. 

Đầu vào được đọc qua`sys.stdin.buffer`, điều này rất hữu ích ở đây vì có thể có 30.000 trường hợp thử nghiệm. Quá trình tiền xử lý chỉ được thực hiện ở mức lớn nhất`n`xuất hiện trong đầu vào, do đó các tệp thử nghiệm nhỏ sẽ không trả tiền cho các trạng thái không cần thiết. 

các`seen`biến là một bit số nguyên. Nếu một vị trí có thể tiếp cận có giá trị Grundy`g`, chút`g`được thiết lập. Việc tính toán mex sau đó chỉ yêu cầu tìm bit chưa được đặt đầu tiên. Điều này tránh việc phân bổ một bộ Python tạm thời cho mỗi một trong khoảng hai triệu trạng thái. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét trường hợp thứ nhất và thứ năm của mẫu đầu tiên. 

Trong trường hợp đầu tiên chỉ có một`III`lớp. Đoạn một bit tương ứng có mặt nạ`1`. 

| Độ dài đoạn | Mặt nạ | Ý nghĩa | Giá trị Grundy có thể tiếp cận | SG | 
| --- | --- | --- | --- | --- | 
| 1 | 0 |`II.`|`{0}`| 1 | 
| 1 | 1 |`III`|`{1, 0}`| 2 | 

các`III`lớp có thể trở thành lớp hai khối, có SG là 1 hoặc có thể trở thành`I.I`, có SG bằng 0. Do đó SG của nó là`mex{0,1} = 2`, khác 0, nên đầu ra là`First`. 

Đối với trường hợp thứ năm, tháp có hai lớp đầy đủ. 

| Độ dài đoạn | Mặt nạ | Ý nghĩa | SG | 
| --- | --- | --- | --- | 
| 1 | 1 |`III`| 2 | 
| 2 | 3 |`III / III`| 1 | 

Đối với trạng thái hai lớp, việc loại bỏ khối bên ngoài sẽ để lại trạng thái SG 0, ​​trong khi loại bỏ khối ở giữa sẽ chia tháp thành hai lớp một lớp.`III`trò chơi với SG`2 xor 2 = 0`. Các giá trị SG có thể truy cập bao gồm 0 chứ không phải 1, cho ra SG 1. XOR cuối cùng khác 0, vì vậy câu trả lời là`First`. 

Những dấu vết này cho thấy tại sao việc điều trị`III`vì chỉ đơn giản là "còn lại hai nước đi" là không đủ. Hai động thái của nó dẫn đến các vị trí có tương tác khác nhau trong tương lai, do đó cần phải có sự tái diễn đầy đủ của Grundy. 

### Mẫu 2 

Mẫu thứ hai là```
3II..IIIII
```Cả hai lớp hai khối được mã hóa thành 0 và lớp đầy đủ là một, tạo ra mặt nạ`100`nếu đọc từ trên xuống dưới trong cấu trúc nhị phân. 

Ba động thái có thể dễ hiểu hơn một cách trực tiếp. 

| Lớp được chọn | Hiện trạng | Kết quả trò chơi | Kết quả SG | 
| --- | --- | --- | --- | 
| 1 |`II.`|`.I.`chặn lớp 2, để lại một lớp`III`thành phần | 2 | 
| 2 |`.II`|`.I.`khối lớp 1 và 3, để lại một lớp đầy đủ biệt lập | 1 | 
| 3, ngoại thất |`III`| ba lớp hai khối | 1 | 
| 3, giữa |`III`|`I.I`ngăn cách hai lớp hai khối | 1 | 

Tập hợp các giá trị Grundy có thể truy cập là`{1, 2}`, do đó vị trí hiện tại có giá trị SG 0. Do đó, người chơi đầu tiên thua, đưa ra`Second`. 

Ví dụ này thực hiện quá trình chuyển đổi tinh tế nhất trong phép truy toán: khi một lớp hai khối trở thành`.I.`, các hàng xóm trực tiếp của nó sau đó không thể trở thành các lớp đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian tiền xử lý |`O(n 2^n)`| có`2^m`mặt nạ cho mọi người`m <= n`, và mỗi mặt nạ kiểm tra nhiều nhất`m`vị trí | 
| Mỗi trường hợp thử nghiệm |`O(n)`| Tháp được phân loại và quét một lần | 
| Tổng thời gian |`O(n 2^n + tn)`| Quá trình tiền xử lý được chia sẻ bởi tất cả các trường hợp thử nghiệm | 
| Không gian |`O(2^n)`| Bảng SG chứa`2^(n+1) - 1`các mục byte lên đến các hệ số không đổi | 

Vì`n = 20`, quá trình tiền xử lý có khoảng hai triệu trạng thái phân đoạn và nhiều nhất là 20 lần chuyển đổi cho mỗi trạng thái. Việc sử dụng các mảng byte nhỏ gọn giúp chiếm ít dung lượng bộ nhớ, trong khi công việc tuyến tính trên mỗi testcase không đáng kể so với quá trình tiền xử lý chung. 

## Trường hợp thử nghiệm```python
Pythonimport ioimport sys

def run(inp: str) -> str:    return solve(inp.encode()).strip()

# The solve() and build_sg() functions from the submitted solution# are assumed to be defined above.

sample1 = """\51III1I.I1.I.1.II2IIIIII"""
assert run(sample1) == """\FirstSecondSecondFirstFirst""".strip(), "sample 1"
sample2 = """\13II..IIIII"""
assert run(sample2) == "Second", "sample 2"

# Minimum-size positions.assert run("""\41III1II.1I.I1.I.""") == """\FirstFirstSecondSecond""".strip(), "single-layer states"

# Two full layers have SG 1, so the position is winning.assert run("""\12IIIIII""") == "First", "two full layers"

# Two full layers separated by singleton layers become two# independent SG-1 components, so their XOR is zero.assert run("""\13III.I.III""") == "Second", "singleton boundary decomposition"

# Maximum n, all layers are immovable I.I.assert run(    "1\n20\n" + "\n".join(["I.I"] * 20) + "\n") == "Second", "maximum-size all-equal terminal tower"

# Boundary case: a full layer immediately next to .I. has SG 1.assert run("""\12.I.III""") == "First", "full layer next to singleton"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / III`,`1 / II.`,`1 / I.I`,`1 / .I.`|`First, First, Second, Second`| Kích thước tối thiểu và tất cả bốn trạng thái lớp cơ bản | 
|`III / III`|`First`| Tương tác giữa hai lớp đầy đủ đang hoạt động | 
|`III / .I. / III`|`Second`| Các thành phần độc lập được phân tách bằng một dấu đơn | 
| Hai mươi`I.I`lớp |`Second`| Tối đa`n`, tất cả các lớp đầu cuối bằng nhau và tiền xử lý an toàn bộ nhớ | 
|`.I. / III`|`First`| Xử lý đặc biệt toàn bộ lớp liền kề với ranh giới đơn lẻ | 

## Vỏ cạnh 

Một đĩa đơn`I.I`lớp là thiết bị đầu cuối. Các khối duy nhất còn lại là hai khối bên ngoài và việc loại bỏ một trong hai khối sẽ để lại một khối bên ngoài duy nhất, vi phạm quy tắc lớp. Thuật toán phân loại nó thành một dấu phân cách, không tạo ra phân đoạn hoạt động nào và để XOR ở mức 0.```
1I.I
```Việc thực thi đánh dấu lớp là bị chặn, không tìm thấy lớp nào đang hoạt động và xuất ra`Second`. 

Một đĩa đơn`.I.`lớp cũng là thiết bị đầu cuối. Không có khối nào có thể bị xóa trong khi để lại một lớp không trống hợp lệ, do đó thuật toán lại không tạo ra phân đoạn hoạt động.```
1.I.
```Câu trả lời là`Second`. 

Một lớp đầy đủ bên cạnh`.I.`yêu cầu xử lý đặc biệt. Coi như```
2.I.III
```Singleton đánh dấu chính nó và hàng xóm của nó là bị chặn. các`III`do đó lớp không được đưa vào phân đoạn DP thông thường. Thay vào đó, nó đóng góp giá trị XOR 1. Hai bước di chuyển có thể có của nó đều tạo ra trạng thái cố định, do đó giá trị SG của nó thực sự là 1. XOR cuối cùng là khác 0 và thuật toán xuất ra`First`. 

Cuối cùng, hãy xem xét cấu hình sample-2:```
3II..IIIII
```Ban đầu không có singleton nào tồn tại nên cả ba lớp đều thuộc về một phân đoạn hoạt động. Mặt nạ là`100`. Việc chọn một trong hai bit 0 đầu tiên sẽ tạo ra`.I.`và chặn các hàng xóm của nó, trong khi chọn lớp đầy đủ cuối cùng sẽ thay đổi nó thành lớp hai khối hoặc tạo`I.I`và chia đoạn. Các giá trị Grundy thu được là 2, 1 và 1, cho ra mex 0. Do đó, thuật toán tạo ra`Second`, đúng như yêu cầu.
