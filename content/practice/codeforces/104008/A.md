---
title: "CF 104008A - Hoa Huệ"
description: "Chúng ta được cung cấp một dải ô một chiều. Mỗi ô trống hoặc chứa hoa huệ, được biểu thị bằng một chuỗi ký tự trong đó L có nghĩa là hoa huệ đã được trồng và . có nghĩa là ô là đất trống."
date: "2026-07-02T05:28:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104008
codeforces_index: "A"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Guilin Site"
rating: 0
weight: 104008
solve_time_s: 43
verified: true
draft: false
---

[CF 104008A - Lily](https://codeforces.com/problemset/problem/104008/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dải ô một chiều. Mỗi ô trống hoặc chứa hoa huệ, được biểu thị bằng một chuỗi ký tự trong đó`L`có nghĩa là hoa huệ đã được trồng và`.`có nghĩa là ô là đất trống. 

Chúng ta được phép đặt thức ăn cho mèo lên một số ô trống, đánh dấu chúng là`C`. Tuy nhiên, việc đặt thức ăn cho mèo ở vị trí`i`áp đặt một ràng buộc an toàn nghiêm ngặt: chính ô đó và các ô lân cận của nó`i-1`Và`i+1`không được chứa hoa huệ. Nói cách khác, vị trí đặt thức ăn cho mèo sẽ “chặn” bán kính 1 xung quanh nó để không cho hoa huệ chứa. 

Nhiệm vụ là chọn vị trí để thức ăn cho mèo sao cho có bao nhiêu`C`các ô chúng tôi đặt trong khi tôn trọng ràng buộc này. Bất kỳ cấu hình hợp lệ nào đạt được số lượng vị trí đặt thức ăn cho mèo tối đa đều được chấp nhận. 

Ràng buộc`n ≤ 1000`có nghĩa là ngay cả các giải pháp bậc hai hoặc bậc ba vừa phải cũng được, nhưng cấu trúc của bài toán gợi ý rõ ràng rằng quét tham lam hoặc quét tuyến tính là đủ. Vì mỗi quyết định chỉ ảnh hưởng đến các ô lân cận nên việc tìm kiếm toàn cục hoặc DP là không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi hoa loa kèn rất rậm rạp. Ví dụ, nếu chuỗi là`LLL`, không được phép đặt thức ăn cho mèo ở bất cứ đâu, bởi vì mọi ô trống cạnh hoa huệ sẽ vi phạm ràng buộc ngay lập tức. Một trường hợp cạnh khác là các mẫu xen kẽ như`L.L.L`, nơi vị trí tham lam ngây thơ có thể cố gắng đặt không chính xác`C`quá gần một bông huệ mà không kiểm tra cả hai hàng xóm. 

Một cách tiếp cận bất cẩn thường thất bại khi đặt thức ăn cho mèo bất cứ khi nào một ô liền kề an toàn, thay vì xác minh cả hai bên. Ví dụ, trong`..L..`, việc đặt thức ăn cho mèo ở vị trí 2 sẽ ngăn không cho vị trí 3 được sử dụng và vị trí từ trái sang phải tham lam phải tính đến sự lan truyền đó. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử mọi tập hợp con của các ô trống và kiểm tra tính hợp lệ. Đối với mỗi tập hợp con, chúng tôi sẽ quét chuỗi và xác minh rằng không có chuỗi nào được chọn`C`vi phạm quy tắc với hoa huệ gần đó. Có tới`n`vị trí, vì vậy số lượng tập hợp con là`2^n`, và mỗi chi phí kiểm tra`O(n)`, dẫn đến`O(n·2^n)`hoạt động. Điều này trở nên không thể ngay cả đối với nhỏ`n`. 

Quan sát quan trọng là mỗi vị trí đặt thức ăn cho mèo chỉ giới hạn một vùng lân cận cục bộ có kích thước 3. Vị trí này có nghĩa là chúng ta không bao giờ cần phải xem xét lại các quyết định trước đó nếu chúng ta quét từ trái sang phải. Một khi chúng tôi đặt một`C`ở vị trí`i`, vị trí`i-1`Và`i+1`thực sự bị cấm nếu chúng có hoa huệ và chúng ta đương nhiên bỏ qua chúng trong một cách xây dựng tham lam. 

Cách xây dựng tối ưu là đi ngang qua sợi dây và đặt một`C`bất cứ khi nào vị trí hiện tại trống và không có hàng xóm nào của nó có hoa huệ vi phạm quy tắc. Sau khi đặt một`C`, chúng tôi bỏ qua ô tiếp theo để tránh xung đột ngẫu nhiên với các vị trí lân cận ở các vị trí trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Quét tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải và xây dựng câu trả lời tăng dần. 

1. Bắt đầu từ chỉ mục 0 và duy trì một bản sao có thể thay đổi của lưới. 

Chúng ta cần khả năng thay đổi vì việc đặt một`C`thay đổi những vị trí trong tương lai được phép. 
2. Tại mỗi vị trí`i`, kiểm tra xem ô có trống không. 

Nếu nó đã chứa`L`, chúng ta không thể đặt bất cứ thứ gì ở đó nên chúng ta tiến về phía trước. 
3. Nếu ô`.`, chúng tôi kiểm tra xem việc đặt một`C`ở đây sẽ vi phạm quy tắc. 

Điều này đòi hỏi phải đảm bảo rằng không`i-1`,`i`, cũng không`i+1`chứa`L`. 
4. Nếu điều kiện được thỏa mãn, chúng ta đặt một`C`ở vị trí`i`. 

Khi thực hiện việc này, chúng tôi đánh dấu nó ngay lập tức để các quyết định trong tương lai thấy được hiệu quả của nó. 
5. Sau khi đặt`C`, chúng ta bỏ qua vị trí tiếp theo bằng cách tăng dần`i`bằng 2 thay vì 1. 

Điều này ngăn chặn việc bố trí ngẫu nhiên có thể tạo ra xung đột chồng chéo trong các vùng lân cận bị hạn chế. 
6. Nếu chúng ta không đặt`C`, chúng ta chỉ cần chuyển sang chỉ mục tiếp theo. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mỗi khi chúng ta đặt một`C`, tất cả các tương tác bị cấm hoàn toàn mang tính cục bộ và không bao giờ lan truyền vượt quá khoảng cách 1. Điều này có nghĩa là các quyết định được đưa ra trước đó không thể bị vô hiệu bởi các lựa chọn sau này. Vì chúng tôi chỉ đặt một`C`khi vùng lân cận của nó sạch sẽ, không có vị trí nào sau này có thể gây ra xung đột liên quan đến ô đó, bởi vì bất kỳ vị trí nào trong tương lai`C`cách đó ít nhất hai bước. Do đó, lựa chọn tham lam là an toàn và tối ưu cục bộ, còn tối ưu cục bộ hàm ý tối ưu toàn cục vì mỗi vị trí tiêu thụ chính xác vùng cấm tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = list(input().strip())
    n = len(s)

    i = 0
    while i < n:
        if s[i] == '.':
            left_ok = (i == 0 or s[i - 1] != 'L')
            right_ok = (i == n - 1 or s[i + 1] != 'L')

            if left_ok and right_ok:
                s[i] = 'C'
                i += 2
                continue
        i += 1

    print(''.join(s))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh quá trình quét tham lam. Chúng tôi chuyển đổi chuỗi thành danh sách để cho phép cập nhật tại chỗ vì chuỗi Python không thể thay đổi. Hai kiểm tra ranh giới xử lý các ô cạnh một cách rõ ràng mà không có lỗi chỉ mục. Sau khi đặt một`C`, bỏ qua chỉ mục tiếp theo là một cách tối ưu hóa thực tế cũng ngăn cản việc đánh giá lại vị trí có vùng lân cận vừa được “sử dụng”. 

Một cạm bẫy phổ biến là quên kiểm tra cả hai hàng xóm trước khi đặt`C`. Một nguyên nhân khác là không xử lý đúng ranh giới, điều này có thể gây ra lỗi từ chối hoặc lập chỉ mục không chính xác tại`i = 0`hoặc`i = n - 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
..L..
```Chúng tôi quét từ trái sang phải. 

| tôi | s[i] | trái_ok | đúng_ok | hành động | tiểu bang | 
| --- | --- | --- | --- | --- | --- | 
| 0 | . | đúng | đúng | nơi C | C.L.. | 
| 2 | L | - | - | bỏ qua | C.L.. | 
| 4 | . | đúng | đúng | nơi C | C.L.C | 

Điều này xác nhận rằng các vị trí được tối đa hóa trong khi vẫn tôn trọng ràng buộc hoa huệ và không có hai vị trí nào cản trở. 

### Ví dụ 2 

đầu vào:```
L.L.L
```| tôi | s[i] | trái_ok | đúng_ok | hành động | tiểu bang | 
| --- | --- | --- | --- | --- | --- | 
| 0 | L | - | - | bỏ qua | L.L.L | 
| 2 | L | - | - | bỏ qua | L.L.L | 
| 4 | L | - | - | bỏ qua | L.L.L | 

Không có vị trí nào có thể thực hiện được và thuật toán tránh được các vị trí bất hợp pháp một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét một lần từ trái sang phải với kiểm tra liên tục | 
| Không gian | O(1) | việc sửa đổi được thực hiện tại chỗ | 

Giới hạn kích thước đầu vào của`n ≤ 1000`được xử lý thoải mái bằng cách quét tuyến tính. Ngay cả với chi phí hoạt động từ chuỗi Python, giải pháp vẫn chạy tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = list(input().strip())
    n = len(s)

    i = 0
    while i < n:
        if s[i] == '.':
            left_ok = (i == 0 or s[i - 1] != 'L')
            right_ok = (i == n - 1 or s[i + 1] != 'L')
            if left_ok and right_ok:
                s[i] = 'C'
                i += 2
                continue
        i += 1

    return ''.join(s)

# provided samples
assert run("..L..\n") == "C.L.C"

# custom cases
assert run("L") == "L", "single lily"
assert run(".") == "C", "single empty cell"
assert run("LLL") == "LLL", "no valid placement"
assert run("L.L.L") == "L.L.L", "blocked by lilies"
assert run(".....") == "C.C.C", "maximum spacing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`L`|`L`| xử lý ranh giới đơn | 
|`.`|`C`| vị trí tầm thường | 
|`LLL`|`LLL`| cấu hình bị chặn hoàn toàn | 
|`L.L.L`|`L.L.L`| ràng buộc xen kẽ | 
|`.....`|`C.C.C`| đóng gói tham lam tối đa | 

## Vỏ cạnh 

Đối với một chuỗi ô đơn như`.`thuật toán ngay lập tức đặt một`C`bởi vì cả hai lần kiểm tra hàng xóm đều trôi qua. Không có rủi ro biên giới vì`i == 0`Và`i == n-1`cả hai đều đúng. 

Đối với một chuỗi bị chặn hoàn toàn như`LLL`, mọi vị trí đều bị bỏ qua vì lần kiểm tra đầu tiên`s[i] == '.'`không thành công ở mọi nơi nên không có vị trí không hợp lệ nào được thử. 

Đối với các mẫu nặng về ranh giới như`.L`hoặc`L.`, các bước kiểm tra cạnh sẽ xử lý chính xác các hàng xóm bị thiếu là an toàn. Ví dụ ở`.L`, chỉ số 0 trống nhưng hàng xóm bên phải của nó là`L`, do đó không có vị trí nào xảy ra, tạo ra`.L`.
