---
title: "CF 106125H - Chuỗi ẩn"
description: "Chúng ta được cấp ba chuỗi gồm các ký tự 1, 2 và 3. Đối tượng ẩn là chuỗi thực sự của những người chiến thắng trò chơi. Nếu người chơi 1 thắng ván thì người chơi 2 và 3 ghi 1, nhưng người chơi 1 lại quên ghi ván đó."
date: "2026-06-19T19:59:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106125
codeforces_index: "H"
codeforces_contest_name: "Delft Algorithm Programming Contest 2025 (DAPC 2025)"
rating: 0
weight: 106125
solve_time_s: 50
verified: true
draft: false
---

[CF 106125H - Chuỗi ẩn](https://codeforces.com/problemset/problem/106125/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp ba chuỗi bao gồm các ký tự`1`,`2`, Và`3`. 

Đối tượng ẩn là chuỗi thực sự của những người chiến thắng trò chơi. Nếu người chơi`1`thắng một trò chơi, sau đó người chơi`2`Và`3`viết ra`1`, nhưng người chơi`1`quên ghi lại trận đấu đó. Quy tắc tương tự được áp dụng đối xứng cho những người chơi khác. 

Kết quả là, mỗi chuỗi đầu vào là chuỗi chiến thắng đầy đủ với một ký tự cụ thể bị loại bỏ: 

-`s1`là chuỗi chiến thắng với tất cả`1`đã bị xóa. 
-`s2`là chuỗi chiến thắng với tất cả`2`đã bị xóa. 
-`s3`là chuỗi chiến thắng với tất cả`3`đã bị xóa. 

Nhiệm vụ là xây dựng lại chuỗi người chiến thắng ban đầu. Tuyên bố đảm bảo rằng câu trả lời tồn tại và là duy nhất. 

Các chuỗi có thể có độ dài lên tới`10^5`, do đó, bản thân dãy ẩn cũng có thể có độ dài trên thang đo đó. Bất kỳ thuật toán nào liên tục quét các chuỗi, thực hiện các thao tác chuỗi con đắt tiền hoặc quay lui nhiều sẽ quá chậm. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Một khía cạnh tinh tế là mỗi lần xuất hiện của người chiến thắng đều xuất hiện ở đúng hai trong ba chuỗi. Ví dụ: nếu người chiến thắng tiếp theo là`2`, thì đó`2`xuất hiện ở`s1`Và`s3`, nhưng không có trong`s2`. Khôi phục chuỗi có nghĩa là xác định ký tự nào phải đến tiếp theo dựa trên vị trí chưa đọc hiện tại của ba chuỗi. 

Hãy xem xét một ví dụ nhỏ:```
s1 = 2
s2 = 1
s3 = 21
```Câu trả lời là:```
21
```Người chiến thắng đầu tiên là`2`. Cái đó`2`xuất hiện ở phía trước của`s1`Và`s3`. Sau khi tiêu thụ nó từ hai chuỗi đó, các ký tự chưa đọc còn lại là`""`,`"1"`, Và`"1"`, vậy người chiến thắng tiếp theo phải là`1`. 

Một lỗi phổ biến là cố gắng khớp các ký tự một cách tham lam giữa các cặp chuỗi tùy ý mà không tôn trọng thứ tự. Ví dụ:```
s1 = 23
s2 = 13
s3 = 12
```Câu trả lời là:```
123
```Lúc đầu, cả ba chuỗi đều bắt đầu bằng các ký tự khác nhau. Chỉ nhìn vào tần số không mang lại manh mối gì. Quan sát quan trọng là có chính xác hai chuỗi phải thống nhất về người chiến thắng tiếp theo. 

Một cái bẫy dễ dàng khác xuất hiện khi cùng một người chiến thắng xảy ra nhiều lần liên tiếp:```
s1 = 2222
s2 = ""
s3 = 2222
```Điều này sẽ tương ứng với chuỗi ẩn`2222`. Bất kỳ phương pháp xây dựng lại nào cũng phải sử dụng chính xác các tiền tố trùng khớp nhiều lần liên tiếp thay vì cố gắng luân phiên giữa các tiền tố chiến thắng khác nhau. 

## Phương pháp tiếp cận 

Việc tái thiết bằng vũ lực sẽ cố gắng xác định người chiến thắng tiếp theo bằng cách kiểm tra mọi khả năng. 

Giả sử chúng ta có vị trí hiện tại trong ba chuỗi. Chúng ta có thể thử nối thêm`1`,`2`, hoặc`3`vào câu trả lời và kiểm tra xem lựa chọn có phù hợp với chuỗi không. Sau khi chọn người chiến thắng, hai chuỗi chứa người chiến thắng đó sẽ tiến lên một vị trí. 

Cách tiếp cận này đúng vì mỗi bước của chuỗi ẩn tương ứng với một trong ba người chiến thắng. Thật không may, việc khám phá các khả năng một cách đệ quy sẽ dẫn đến cây tìm kiếm theo cấp số nhân. Ngay cả khi cắt tỉa, các dây có chiều dài lên tới`10^5`làm cho một chiến lược như vậy là không thể. 

Quan sát quan trọng là người chiến thắng tiếp theo thực sự bị ép buộc. 

Lấy ký tự chưa được sử dụng đầu tiên của mỗi chuỗi. Nếu người chiến thắng tiếp theo là`1`, sau đó là ký tự chưa đọc tiếp theo của`s2`Và`s3`cả hai đều phải như vậy`1`. Tương tự, người chiến thắng`2`sẽ xuất hiện tiếp theo trong`s1`Và`s3`, và người chiến thắng`3`sẽ xuất hiện tiếp theo trong`s1`Và`s2`. 

Vì câu trả lời được đảm bảo là duy nhất nên ở mỗi bước có chính xác một ký tự xuất hiện dưới dạng ký tự hiện tại trong đúng hai chuỗi. Nhân vật đó phải là người chiến thắng tiếp theo. Sau khi xuất ra, chúng ta tiến con trỏ của hai chuỗi đó và lặp lại. 

Một cách khác để xem nó là từ đầu. Người chiến thắng trong trò chơi đầu tiên xuất hiện đầu tiên trong đúng hai danh sách. Sau khi xác định được người chiến thắng, chúng tôi sẽ xóa người đó khỏi hai danh sách đó. Lập luận tương tự sau đó áp dụng cho các hậu tố còn lại. Điều này mang lại một thuật toán tuyến tính dựa trên con trỏ đơn giản. Giải pháp chính thức sử dụng chính xác quan sát này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tái thiết ba con trỏ | O(n) | O(1) ngoài đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba chuỗi`s1`,`s2`, Và`s3`. 
2. Duy trì ba chỉ số`i1`,`i2`, Và`i3`, ban đầu bằng không. Mỗi chỉ mục trỏ đến ký tự chưa đọc đầu tiên trong chuỗi của nó. 
3. Trong khi ít nhất một chuỗi vẫn còn các ký tự chưa đọc, hãy xem ký tự hiện tại của mỗi chuỗi có con trỏ chưa đến cuối. 
4. Đếm ký tự nào trong số`1`,`2`, Và`3`xuất hiện ở phía trước của đúng hai chuỗi. 
5. Nhân vật đó là người chiến thắng tiếp theo trong dãy ẩn. Nối nó vào câu trả lời. 
6. Nâng cao con trỏ của chính xác hai chuỗi có ký tự hiện tại bằng chuỗi chiến thắng đã chọn. 
7. Lặp lại cho đến khi sử dụng hết tất cả ký tự từ tất cả các chuỗi. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện của người chiến thắng đều xuất hiện trong đúng hai danh sách và không có trong danh sách của chính người chiến thắng. 

Giả sử người chiến thắng ẩn tiếp theo là`x`. Sau đó, ký tự chưa đọc tiếp theo trong hai danh sách của những người chơi khác đều phải được`x`, bởi vì những danh sách đó giữ nguyên thứ tự ban đầu của những người chiến thắng. Danh sách thứ ba hoàn toàn không chứa trường hợp này. 

Vì vậy, bất cứ lúc nào, người chiến thắng tiếp theo chính là nhân vật xuất hiện ở vị trí hiện tại của đúng hai chuỗi. Việc tiêu thụ ký tự đó từ hai chuỗi đó sẽ loại bỏ một lần xuất hiện của trò chơi tiếp theo khỏi tất cả các biểu diễn. Các hậu tố còn lại tương ứng với các trò chơi còn lại, do đó, lập luận tương tự được áp dụng theo cách quy nạp. Thuật toán xây dựng lại mọi người chiến thắng theo đúng thứ tự và không thể chọn sai ký tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = [input().strip() for _ in range(3)]

    p = [0, 0, 0]
    ans = []

    while True:
        cur = []

        for k in range(3):
            if p[k] < len(s[k]):
                cur.append((k, s[k][p[k]]))

        if not cur:
            break

        cnt = {'1': 0, '2': 0, '3': 0}

        for _, ch in cur:
            cnt[ch] += 1

        winner = None
        for ch in "123":
            if cnt[ch] == 2:
                winner = ch
                break

        ans.append(winner)

        for k, ch in cur:
            if ch == winner:
                p[k] += 1

    print("".join(ans))

solve()
```Việc thực hiện phản ánh bằng chứng trực tiếp. 

Mảng`p`lưu trữ vị trí chưa đọc hiện tại trong mỗi chuỗi. Ở mỗi lần lặp, chúng tôi chỉ kiểm tra các ký tự hiện được trỏ đến, không bao giờ quét lùi hoặc tìm kiếm phía trước. 

Từ điển`cnt`ghi lại có bao nhiêu chuỗi hiện bắt đầu với mỗi chuỗi có thể thắng. Vì chỉ tồn tại ba ký hiệu nên công này là thời gian không đổi. 

Sau khi xác định được người chiến thắng, chúng tôi tiến tới chính xác các con trỏ có ký tự hiện tại bằng với người chiến thắng đó. Đây là bước quan trọng. Việc nâng cao bất kỳ con trỏ nào khác sẽ phá hủy thông tin thứ tự được lưu giữ trong danh sách. 

Mỗi con trỏ di chuyển về phía trước nhiều nhất là chiều dài của chuỗi của nó. Không có con trỏ nào di chuyển lùi lại, vì vậy tổng khối lượng công việc là tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1
21
```Sản lượng dự kiến:```
21
```| Bước | p1 | p2 | p3 | Ký tự phía trước | Người chiến thắng | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 2,1,2 | 2 | 2 | 
| 2 | 1 | 0 | 1 | -,1,1 | 1 | 21 | 

Ví dụ này cho thấy quy tắc cơ bản. Người chiến thắng tiếp theo luôn là nhân vật xuất hiện ở phía trước đúng hai chuỗi. 

### Ví dụ 2 

đầu vào:```
23
13
12
```Sản lượng dự kiến:```
123
```| Bước | p1 | p2 | p3 | Ký tự phía trước | Người chiến thắng | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 2,1,1 | 1 | 1 | 
| 2 | 0 | 1 | 1 | 2,3,2 | 2 | 12 | 
| 3 | 1 | 1 | 2 | 3,3,- | 3 | 123 | 

Dấu vết này chứng tỏ rằng ba chuỗi đều có thể bắt đầu bằng các ký hiệu khác nhau. Lựa chọn đúng vẫn được xác định duy nhất vì có đúng một biểu tượng xuất hiện hai lần trong số các vị trí hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi con trỏ chỉ tiến về phía trước và mỗi ký tự được xử lý một lần | 
| Không gian | O(n) | Chuỗi câu trả lời có độ dài n, bộ nhớ phụ là O(1) | 

Nếu dãy ẩn có độ dài`n`, thì ba chuỗi đầu vào cùng nhau cũng chỉ chứa`2n`nhân vật. Mỗi ký tự gây ra sự gia tăng một con trỏ chính xác một lần, do đó thuật toán dễ dàng xử lý các đầu vào có kích thước`10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = [input().strip() for _ in range(3)]
    p = [0, 0, 0]
    ans = []

    while True:
        cur = []

        for k in range(3):
            if p[k] < len(s[k]):
                cur.append((k, s[k][p[k]]))

        if not cur:
            break

        cnt = {'1': 0, '2': 0, '3': 0}

        for _, ch in cur:
            cnt[ch] += 1

        winner = None
        for ch in "123":
            if cnt[ch] == 2:
                winner = ch
                break

        ans.append(winner)

        for k, ch in cur:
            if ch == winner:
                p[k] += 1

    return "".join(ans)

# provided samples
assert run("2\n1\n21\n") == "21", "sample 1"
assert run("23\n13\n12\n") == "123", "sample 2"
assert run("23232323\n11331133\n12121212\n") == "121323121323", "sample 3"

# custom cases
assert run("2\n12\n1\n") == "12", "minimum nontrivial case"
assert run("22\n1\n221\n") == "221", "repeated winner"
assert run("33\n133\n1\n") == "133", "winner appears consecutively at end"
assert run("232\n13\n122\n") == "1232", "mixed ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 12 / 1`|`12`| Tái thiết nhỏ | 
|`22 / 1 / 221`|`221`| Người chiến thắng giống hệt liên tiếp | 
|`33 / 133 / 1`|`133`| Hậu tố dài của cùng một người chiến thắng | 
|`232 / 13 / 122`|`1232`| Nhiều lần chuyển đổi giữa những người chiến thắng | 

## Vỏ cạnh 

Hãy xem xét:```
2
1
21
```Ban đầu các mặt trận là`(2,1,2)`. nhân vật`2`xuất hiện hai lần, do đó thuật toán đưa ra`2`và nâng cao con trỏ thứ nhất và thứ ba. Các mặt trận còn lại trở thành`(end,1,1)`, buộc người chiến thắng`1`. Đầu ra là`21`, điều đó đúng. 

Bây giờ hãy xem xét những người chiến thắng lặp đi lặp lại:```
22
1
221
```Các mặt trận là`(2,1,2)`, vậy là người chiến thắng`2`được chọn. Sau khi tiến hai con trỏ, mặt trước lại`(2,1,2)`. Thuật toán chọn chính xác một thuật toán khác`2`. Chỉ sau đó mới làm`1`trở thành nhân vật xuất hiện hai lần. Kết quả là`221`. 

Một trường hợp thú vị hơn là:```
23
13
12
```Cả ba chuỗi đều bắt đầu bằng các ký hiệu khác nhau. Việc xây dựng lại toàn bộ chuỗi dựa trên tần số sẽ thất bại. Chỉ nhìn vào các vị trí chưa đọc hiện tại sẽ được tính`{1:2, 2:1, 3:0}`, buộc người chiến thắng`1`. Lặp đi lặp lại logic tương tự sẽ tái tạo lại`123`chính xác. 

Những ví dụ này minh họa tại sao bất biến cục bộ "xuất hiện ở chính xác hai vị trí hiện tại" lại đủ để khôi phục toàn bộ chuỗi một cách duy nhất.
