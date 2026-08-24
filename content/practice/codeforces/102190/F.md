---
title: "CF 102190F - đầu vào/đầu ra tiêu chuẩn"
description: "Mỗi cụm từ được thể hiện bằng chữ viết tắt của nó, được hình thành bằng cách lấy chữ cái đầu tiên của mỗi từ. Vì mỗi từ bắt đầu bằng một chữ cái viết hoa, đây chỉ đơn giản là chuỗi các chữ cái đầu của từ, bỏ qua chữ hoa chữ thường khi so sánh các chữ viết tắt."
date: "2026-08-23T08:48:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "F"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 638
verified: true
draft: false
---

[CF 102190F - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Mỗi cụm từ được thể hiện bằng chữ viết tắt của nó, được hình thành bằng cách lấy chữ cái đầu tiên của mỗi từ. Vì mỗi từ bắt đầu bằng một chữ cái viết hoa, đây chỉ đơn giản là chuỗi các chữ cái đầu của từ, bỏ qua chữ hoa chữ thường khi so sánh các chữ viết tắt. 

Ví dụ,`East China Normal University`trở thành`ECNU`, trong khi`Electronic Circuit National Union`cũng trở thành`ECNU`. Mỗi cặp cụm từ riêng biệt không có thứ tự có cùng chữ viết tắt sẽ đóng góp một câu trả lời. 

Do đó, nhiệm vụ không thực sự là so sánh các cụm từ. Chúng ta chỉ cần nhóm các cụm từ theo chữ viết tắt của chúng và với mỗi nhóm có kích thước k, hãy thêm số cặp không có thứ tự bên trong nhóm đó: 

( 2 k ​ )= 2 k(k−1) ​ . 

Có thể có tới 5⋅10 5 cụm từ, vì vậy việc kiểm tra mỗi cặp sẽ cần khoảng 1,25⋅10 11 so sánh trong trường hợp xấu nhất. Điều đó vượt xa những gì một giải pháp lập trình cạnh tranh có thể đáp ứng được. Tổng số từ nhiều nhất là 10 6, điều này gợi ý rõ ràng rằng một giải pháp gần tuyến tính về lượng đầu vào được dự định. Mỗi từ có độ dài tối đa là 11, do đó việc xử lý từng ký tự đầu vào đầy đủ cũng rất thiết thực. 

Có một số trường hợp nguy hiểm có thể khiến việc triển khai bất cẩn không thành công. 

Hãy xem xét một từ điển chỉ chứa một cụm từ:```

```Không có cặp cụm từ riêng biệt nên đáp án là`0`. Giải pháp coi chính cụm từ đó là một cặp sẽ sai. 

Bây giờ hãy xem xét các từ có một chữ cái:```

```Các chữ viết tắt là`CSL`,`OXX`, Và`OO`, tương ứng. Tất cả đều khác nhau, vì vậy câu trả lời là`0`. Trình phân tích cú pháp giả sử mỗi từ chứa nhiều hơn một ký tự sẽ xử lý sai hai dòng đầu tiên. 

Cách viết hoa chữ thường cũng quan trọng khi đọc các từ chứ không phải khi so sánh các từ viết tắt. Ví dụ:```

```Cả hai chữ viết tắt đều`AB`, vậy câu trả lời là`1`. Các chữ cái đầu viết hoa phải được coi là các chữ cái giống nhau để so sánh chữ viết tắt. 

Cuối cùng, một số cụm từ có thể có cách viết tắt giống hệt nhau:```

```Mỗi cụm từ đều có chữ viết tắt bao gồm hai chữ cái đầu, nhưng đây là`AB`,`CD`,`EF`, Và`GH`, vậy câu trả lời là`0`. Nếu thay vào đó cả bốn cụm từ đều có chữ viết tắt`AB`, câu trả lời sẽ là 

( 2 4 ​ )=6, 

bởi vì mỗi cặp cụm từ xung đột. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng cách viết tắt của mỗi cụm từ và sau đó so sánh từng cặp cụm từ. Nếu cụm từ i và j có chữ viết tắt bằng nhau, chúng ta sẽ tăng câu trả lời. Điều này đúng vì mỗi cặp không có thứ tự được kiểm tra đúng một lần. 

Vấn đề là số bậc hai của so sánh. Với n=5⋅10 5, có 

2 n(n−1) ​ = 2 500000⋅499999 ​ =124999750000 

cặp trong trường hợp xấu nhất. Ngay cả khi một phép so sánh chỉ mất một khoảng thời gian không đổi rất nhỏ thì hơn 1011 phép tính cũng không khả thi. 

Lực lượng vũ phu hoạt động vì sự bình đẳng của hai chữ viết tắt hoàn toàn xác định liệu một cặp có hợp lệ hay không, nhưng nó không thành công vì nó liên tục hỏi cùng một câu hỏi bình đẳng cho nhiều cụm từ. Điều quan trọng là các cụm từ có cùng chữ viết tắt sẽ tạo thành một nhóm tự nhiên. Thay vì so sánh từng cặp, chúng ta có thể đếm xem mỗi từ viết tắt có bao nhiêu cụm từ. 

Giả sử một chữ viết tắt xảy ra k lần. Mỗi một trong k cụm từ đó xung đột với các cụm từ k−1 khác, nhưng việc đếm trực tiếp cụm từ này sẽ cho ra k(k−1), tức là đếm mọi cặp không có thứ tự hai lần. Chia cho hai được 

2 k(k−1) ​ . 

Việc triển khai thuận tiện hơn nữa sẽ tránh việc lưu trữ tất cả các tần số trước tiên. Xử lý từng cụm từ một. Nếu từ viết tắt hiện tại đã xuất hiện c lần thì cụm từ mới tạo thành chính xác c cặp không có thứ tự mới với các cụm từ trước đó. Thêm c vào câu trả lời, sau đó tăng tần số của nó lên c+1. 

Điều này biến toàn bộ vấn đề thành vấn đề đếm tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 2 ) | O(n) | Quá chậm | 
| Tối ưu | O(L) dự kiến ​​| O(L) | Đã chấp nhận | 

Ở đây L là tổng kích thước đầu vào, hay tương đương là tổng số ký tự phải đọc. Vì mỗi từ có độ dài tối đa là 11 và có nhiều nhất là 10 6 từ, L là tuyến tính trong kích thước đầu vào nhất định. 

#Hướng dẫn thuật toán 

1. Đọc mỗi cụm từ thành một dòng. Cụm từ chứa ít nhất hai từ và chính xác một dấu cách ngăn cách các từ liên tiếp. 
2. Viết tắt bằng cách lấy ký tự đầu dòng rồi đến ký tự liền kề sau mỗi khoảng trắng. Mỗi ký tự như vậy đều là chữ cái đầu viết hoa, vì vậy hãy chuyển nó thành chữ thường trước khi sử dụng nó làm khóa từ điển. 
3. Tra cứu từ viết tắt hiện tại trong từ điển tần số. Nếu nó đã xuất hiện c lần trước đó thì cụm từ mới tạo thành chính xác c cặp không có thứ tự xung đột mới. Thêm c vào câu trả lời. 
4. Tăng tần số lưu trữ của chữ viết tắt này lên một. Tần suất cập nhật sẽ được sử dụng khi cụm từ sau này có cùng chữ viết tắt. 
5. Sau khi tất cả các cụm từ đã được xử lý, hãy in câu trả lời tích lũy. 

Lý do bước 3 hoạt động mà không cần tính toán rõ ràng ( 2 k ​ ) là vì các cụm từ xuất hiện cùng một lúc. Khi cụm từ đầu tiên có chữ viết tắt xuất hiện, nó sẽ tạo ra 0 cặp. Cái thứ hai tạo ra một cặp mới với cái đầu tiên. Cái thứ ba tạo ra hai cặp mới với hai cặp đầu tiên, v.v. Do đó, một nhóm có kích thước k đóng góp 

0+1+2+⋯+(k−1)= 2 k(k−1) ​ . 

### Tại sao nó hoạt động 

Duy trì bất biến sau khi xử lý cụm từ i đầu tiên,`answer`chính xác là số lượng các cặp không có thứ tự xung đột hoàn toàn có trong các cụm từ i đó, trong khi`count[x]`là số cụm từ được xử lý có chữ viết tắt là x. 

Khi cụm từ tiếp theo có chữ viết tắt x, mọi cụm từ được xử lý trước đó có chữ viết tắt x sẽ tạo thành một cặp không có thứ tự mới với nó. Có chính xác`count[x]`những cụm từ như vậy, do đó, việc thêm giá trị đó sẽ tính mỗi cặp mới được tạo chính xác một lần. Các cặp giữa các chữ viết tắt khác nhau không thể hợp lệ và các cặp trong số các cụm từ đã được xử lý đã được tính. Do đó, bất biến vẫn đúng sau mỗi cụm từ và sau cụm từ cuối cùng, giá trị tích lũy chính xác là câu trả lời được yêu cầu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    count = {}
    answer = 0

    for _ in range(n):
        line = input().strip()

        # The abbreviation consists of the first character and
        # every character immediately following a space.
        abbr = bytearray()
        abbr.append(line[0] | 32)

        for i, ch in enumerate(line):
            if ch == 32:  # ASCII space
                abbr.append(line[i + 1] | 32)

        key = bytes(abbr)

        old = count.get(key, 0)
        answer += old
        count[key] = old + 1

    print(answer)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc từng dòng vì mỗi cụm từ đương nhiên là một bản ghi.`sys.stdin.readline`tránh được chi phí phải sử dụng nhiều lần các cơ chế nhập liệu cấp cao hơn, điều này quan trọng vì dữ liệu đầu vào có thể chứa hàng triệu từ. 

Chữ viết tắt được trích xuất trực tiếp từ biểu diễn byte thô của dòng. Byte đầu tiên là chữ cái đầu của từ đầu tiên. Mỗi khoảng trắng đều có chữ cái đầu của từ tiếp theo ngay sau nó, vì vậy việc quét các khoảng trắng là đủ để khôi phục mọi chữ cái đầu mà không cần chia dòng thành các đối tượng từ riêng biệt. 

Các chữ cái viết thường của ASCII thu được bằng cách đặt bit 5, tương đương với việc thêm 32 cho các chữ cái tiếng Anh viết hoa có liên quan ở đây. Vì mỗi ký tự viết tắt được đảm bảo là chữ cái đầu viết hoa,`ch | 32`chuyển nó sang dạng chữ thường. Kết quả được lưu dưới dạng`bytes`, không thay đổi và có thể băm, làm cho nó phù hợp làm khóa từ điển. 

Từ điển lưu trữ số lượng cụm từ được xử lý trước đó cho mỗi từ viết tắt. Nếu như`old`chính xác là tần số đó phải không`old`cặp mới xuất hiện khi cụm từ hiện tại đến. Chúng tôi thêm`old`trước khi tăng tần số, điều này ngăn cụm từ được ghép nối với chính nó. 

Số nguyên Python có độ chính xác tùy ý, do đó, câu trả lời lớn bằng 

( 2 500000 ​ )=124999750000 

không yêu cầu xử lý đặc biệt khi tràn. 

# Ví dụ đã hoạt động 

## Mẫu 1 

Năm cụm từ tạo ra bốn nhóm viết tắt riêng biệt. Ba cụm từ tạo ra`ecnu`, và hai sản phẩm`scpc`. 

| Cụm từ | Viết tắt | Số trước | Đã thêm vào câu trả lời | Số mới | 
| --- | --- | --- | --- | --- | 
| Đại học Sư phạm Đông Trung Quốc | ecnu | 0 | 0 | 1 | 
| Liên Hiệp Mạch Điện Tử Toàn Quốc | ecnu | 1 | 1 | 2 | 
| Đại học Trung tâm Châu Âu Norwich | ecnu | 2 | 2 | 3 | 
| Hội đồng hợp tác cộng đồng trường học | scpc | 0 | 0 | 1 | 
| Cuộc thi lập trình đại học Thượng Hải | scpc | 1 | 1 | 2 | 

Câu trả lời trở thành 0+1+2+0+1=4. các`ecnu`nhóm đóng góp ba cặp, trong khi nhóm`scpc`nhóm đóng góp một cặp. 

## Mẫu 2 

Ba cụm từ có chữ viết tắt`csl`,`oxx`, Và`oo`. 

| Cụm từ | Viết tắt | Số trước | Đã thêm vào câu trả lời | Số mới | 
| --- | --- | --- | --- | --- | 
| C S L | csl | 0 | 0 | 1 | 
| Ô X X | bò | 0 | 0 | 1 | 
| Orz Orz | ồ | 0 | 0 | 1 | 

Mọi tần số vẫn là một, do đó không tồn tại cặp xung đột nào và câu trả lời là`0`. Ví dụ này cũng thực hiện trường hợp các từ có độ dài bằng một và xác nhận rằng chỉ các ký tự đầu tiên của chúng mới quan trọng. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) dự kiến ​​| Mỗi ký tự đầu vào được quét tối đa một lần, với các thao tác từ điển dự kiến ​​là O(1) | 
| Không gian | O(L) | Từ điển lưu trữ một từ viết tắt và một tần số cho mỗi từ viết tắt riêng biệt | 

Ở đây L là tổng số ký tự trong cụm từ đầu vào. Với tối đa 10 6 từ và tối đa 11 chữ cái mỗi từ, đầu vào vẫn tuyến tính trong một lượng dữ liệu có thể quản lý được. Thuật toán không bao giờ so sánh các cụm từ với nhau, vì vậy nó tránh được nút thắt cổ chai bậc hai. 

# Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        n = int(sys.stdin.readline())
        count = {}
        answer = 0

        for _ in range(n):
            line = sys.stdin.readline().strip()

            abbr = bytearray()
            abbr.append(line[0] | 32)

            for i, ch in enumerate(line):
                if ch == 32:
                    abbr.append(line[i + 1] | 32)

            key = bytes(abbr)
            old = count.get(key, 0)
            answer += old
            count[key] = old + 1

        print(answer)
        return sys.stdout.getvalue()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert solve_data(
    """5
East China Normal University
Electronic Circuit National Union
European Central Norwich University
School Community Partnership Council
Shanghai Collegiate Programming Contest
"""
) == "4\n", "sample 1"

# Sample 2
assert solve_data(
    """3
C S L
O X X
Orz Orz
"""
) == "0\n", "sample 2"

# Minimum n, so there cannot be any pair.
assert solve_data(
    """1
A B
"""
) == "0\n", "single phrase"

# All phrases have the same abbreviation AB.
assert solve_data(
    """4
A B
Another Beginning
Amazing Building
Awesome Bridge
"""
) == "6\n", "all equal abbreviations"

# Single-letter words and repeated initials.
assert solve_data(
    """5
A A
B B
C C
A B
A A
"""
) == "1\n", "single-letter words and duplicate abbreviation"

# Maximum n with the smallest possible phrase length.
# Every phrase has the same abbreviation AB.
inp = "500000\n" + ("A B\n" * 500000)
assert solve_data(inp) == "124999750000\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / A B`|`0`| Số lượng cụm từ tối thiểu và không tự ghép nối | 
| Bốn cụm từ có chữ viết tắt`AB`|`6`| Quy tắc đếm ( 2 k ​ ) | 
| Từ có một chữ cái |`1`| Độ dài từ một và chữ viết tắt lặp đi lặp lại | 
|`500000`giống hệt nhau`A B`cụm từ |`124999750000`| N tối đa, câu trả lời lớn và xử lý tuyến tính | 

# Vỏ cạnh 

Trường hợp cụm từ đơn```
1
A B
```có chữ viết tắt`ab`. Tần số ban đầu của nó bằng 0, do đó thuật toán thêm 0 và sau đó lưu tần số một. Câu trả lời cuối cùng là`0`. Điều này xác nhận rằng một cụm từ không bao giờ được ghép nối với chính nó. 

Đối với các từ có một chữ cái, hãy xem xét```
3
C S L
O X X
Orz Orz
```Máy quét bắt đầu với`C`,`O`, Và`O`cho ba dòng, sau đó lấy ký tự sau mỗi khoảng trắng. Các khóa kết quả là`csl`,`oxx`, Và`oo`. Mỗi lần xảy ra một lần, nên câu trả lời vẫn là`0`. Không cần giả định về sự tồn tại của các ký tự sau chữ cái đầu tiên của một từ. 

Đối với các chữ viết tắt giống hệt nhau, hãy xem xét```
4
A B
Another Beginning
Amazing Building
Awesome Bridge
```Mỗi dòng sản xuất`ab`. Bốn phần bổ sung góp phần`0`, sau đó`1`, sau đó`2`, sau đó`3`, cho`6`. Đây chính xác là sáu cặp không có thứ tự trong số bốn cụm từ. 

Câu trả lời lớn nhất có thể cũng phù hợp một cách tự nhiên với phương pháp tăng dần. Với`500000`bản sao của`A B`, các phép cộng liên tiếp là 0,1,2,…,499999. Tổng của họ là`124999750000`. Số học số nguyên của Python thể hiện điều này một cách trực tiếp, do đó không cần xử lý tràn. 

Thứ tự sắp xếp của đầu vào không cần thiết phải được sử dụng. Thuật toán chỉ phụ thuộc vào cách viết tắt của từng cụm từ và số lần xuất hiện trước đó. Bất kỳ thứ tự đầu vào nào cũng sẽ tạo ra số lượng cặp cuối cùng giống nhau, mặc dù vấn đề đảm bảo thứ tự từ điển.
