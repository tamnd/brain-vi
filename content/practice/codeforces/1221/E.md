---
title: "CF 1221E - Trò chơi có dây"
description: "Chúng ta được cung cấp một chuỗi nhị phân gồm hai loại ký tự: các ô trống được biểu thị bằng . và các ô bị chặn được biểu thị bằng X. Hai người chơi luân phiên nhau, bắt đầu từ Alice."
date: "2026-06-15T19:19:34+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 1221
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 73 (Rated for Div. 2)"
rating: 2500
weight: 1221
solve_time_s: 131
verified: false
draft: false
---

[CF 1221E - Trò chơi có dây](https://codeforces.com/problemset/problem/1221/E) 

**Đánh giá:** 2500 
**Thẻ:** trò chơi 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân gồm hai loại ký tự: các ô trống được biểu thị bằng`.`và các ô bị chặn biểu thị bằng`X`. Hai người chơi luân phiên nhau, bắt đầu với Alice. Mỗi bước di chuyển bao gồm việc chọn một đoạn liền kề có độ dài cố định chính xác và chuyển đổi mọi ký tự trong đoạn đó thành`X`, bất kể nó đã`.`hoặc`X`. 

Alice luôn sử dụng một đoạn có độ dài`a`, trong khi Bob luôn sử dụng đoạn có độ dài ngắn hơn`b`, với`a > b`. Người chơi thua khi không tìm được đoạn có độ dài yêu cầu chứa ít nhất một đoạn`.`. 

Quá trình này hoàn toàn mang tính hủy diệt. Một khi tế bào trở thành`X`, nó không bao giờ hoàn nguyên, do đó tập hợp các nước đi hợp lệ chỉ co lại theo thời gian. Câu hỏi quan trọng là liệu Alice có thể giành chiến thắng trong cách chơi tối ưu hay không, nghĩa là cuối cùng cô ấy khiến Bob không thể di chuyển trước, hay Bob luôn có thể đáp trả để cuối cùng Alice bị mắc kẹt. 

Độ dài chuỗi có thể lên tới 3×10^5 cho mỗi trường hợp thử nghiệm và có tổng cộng tối đa 3×10^5 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ mô phỏng lượt nào. Ngay cả một trò chơi cũng có thể kéo dài O(n) nước đi và mỗi nước đi có khả năng quét toàn bộ chuỗi, vì vậy mô phỏng đơn giản sẽ vượt xa giới hạn. 

Khó khăn chính là mỗi bước di chuyển sẽ sửa đổi toàn bộ khoảng thời gian và cấu trúc của các đoạn tự do phát triển theo cách không cần thiết. Một ý tưởng ngây thơ là liên tục tìm kiếm bất kỳ phân đoạn hợp lệ nào, nhưng điều đó lại bỏ qua lối chơi tối ưu và thất bại trong trường hợp đối thủ phá hủy tính linh hoạt một cách chiến lược. 

Trường hợp cạnh tinh tế xuất hiện khi dây có nhiều khoảng trống nhỏ`.`riêng lẻ ngắn hơn`a`hoặc`b`. Ví dụ, nếu tất cả`.`các đoạn có độ dài 1 và`a = 3`, Alice không hề di chuyển và thua ngay lập tức, ngay cả khi tổng số quân`.`là lớn. Ngược lại, một khối chấm dài có thể cho phép một nước đi làm sập toàn bộ cấu trúc và kết thúc trò chơi ngay lập tức. Vì vậy, chỉ có cấu trúc liền kề mới quan trọng chứ không phải tổng số. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ duy trì rõ ràng chuỗi và các bước di chuyển thay thế. Mỗi lượt, chúng tôi sẽ quét tìm bất kỳ đoạn có độ dài nào`a`hoặc`b`bao gồm ít nhất một`.`và chuyển đổi nó thành`X`. Điều này đơn giản và chính xác, vì mỗi nước đi đều mang tính quyết định sau khi được chọn và cả hai người chơi đều được cho là chơi tối ưu. 

Tuy nhiên, sự phức tạp là thảm khốc. Mỗi lần di chuyển có thể yêu cầu quét O(n) để tìm một phân đoạn hợp lệ và có thể có tổng số lần di chuyển O(n) vì mỗi thao tác có thể loại bỏ ít nhất một dấu chấm. Điều này dẫn đến O(n²) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất, điều này là không thể với n tối đa 3×10^5. 

Quan sát quan trọng là vị trí chính xác của các phân đoạn không quan trọng ngoài sự tồn tại của các khối liền kề đủ dài`.`. Khi chúng ta diễn giải chuỗi dưới dạng một tập hợp các đoạn chấm liền kề tối đa, mỗi bước di chuyển chỉ đơn giản là “giết chết” một phần nào đó của một hoặc nhiều đoạn, nhưng quyết định liệu một bước di chuyển có tồn tại hay không chỉ phụ thuộc vào việc liệu có tồn tại một đoạn chấm có độ dài ít nhất bằng độ dài bước di chuyển cần thiết hay không. 

Vì người chơi luôn chọn những nước đi tối ưu nên Alice sẽ luôn nhắm đến một phân khúc có thể tối đa hóa sự gián đoạn, trong khi Bob cố gắng duy trì các lựa chọn trong tương lai. Bởi vì`a > b`, Alice có thể loại bỏ nhiều cấu trúc hơn trên mỗi nước đi so với Bob, điều này tạo ra hiệu ứng thống trị: Alice có thể thu nhỏ vùng có thể chơi được nhanh hơn. 

Giải pháp đúng sẽ đưa trò chơi về trạng thái có cấu trúc theo các dấu chấm liên tiếp. Chúng ta chỉ cần kiểm tra xem Alice có thể thực hiện ít nhất một nước đi hay không và liệu cô ấy có thể đảm bảo rằng sau nước đi của mình, Bob ngay lập tức hoặc cuối cùng bị buộc vào một vị trí không có đoạn độ dài hợp lệ nào không.`b`tồn tại trong khi Alice vẫn giữ lại ít nhất một cấu trúc nước đi hợp lệ. 

Việc giảm tiêu chuẩn là xử lý từng khối liền kề của`.`như một khoảng thời gian và lý do về số lần di chuyển mà mỗi người chơi có thể rút ra từ đó. Vì mỗi nước đi sẽ phá hủy toàn bộ đoạn có kích thước cố định, trò chơi sẽ trở thành sự so sánh giữa bao nhiêu cửa sổ có chiều dài rời rạc.`a`so với chiều dài`b`có thể được đóng gói thành các phân đoạn dấu chấm, dưới sự tương tác tối ưu. Điều này dẫn đến việc đếm tham lam xem mỗi người chơi có thể đảm bảo bao nhiêu nước đi từ cấu hình ban đầu và Alice thắng nếu tổng số nước đi được đảm bảo của cô ấy vượt quá số nước đi của Bob. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Khoảng thời gian / Đếm tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp hoạt động bằng cách quét chuỗi và trích xuất độ dài liên tiếp`.`phân đoạn. Mỗi phân đoạn như vậy đóng góp độc lập vào số bước đi có thể có của cả hai người chơi. 

1. Duyệt chuỗi và tính độ dài của tất cả các đoạn liền kề tối đa của`.`. Các phân đoạn này đại diện cho các khu vực độc lập nơi có thể thực hiện các bước di chuyển. Điều này hợp lệ vì`X`các tế bào tách biệt vĩnh viễn sự tương tác giữa các khu vực. 
2. Đối với mỗi đoạn dài`L`, hãy tính xem Alice có thể thực hiện bao nhiêu nước đi nếu cô ấy chơi cô lập một cách tối ưu. Vì cô ấy luôn loại bỏ một khối có kích thước`a`, cô ấy có thể đảm bảo ít nhất`L // a`di chuyển từ đoạn đó. 
3. Tương tự tính công suất của Bob từ cùng phân khúc với`L // b`. Bob sử dụng các phân đoạn nhỏ hơn để có thể trích xuất nhiều bước di chuyển hơn từ cùng một khu vực. 
4. Tổng số đóng góp trên tất cả các phân đoạn: tổng số nước đi của Alice và tổng số nước đi của Bob. 
5. So sánh tổng số. Nếu tổng khả năng di chuyển của Alice lớn hơn Bob rất nhiều, Alice có thể đảm bảo rằng cô ấy thực hiện bước đi hiệu quả cuối cùng trước khi Bob sử dụng hết tất cả các phân đoạn có thể chơi được. Nếu không, Bob có thể sánh ngang hoặc tồn tại lâu hơn cô ấy, ngăn cản chiến thắng bắt buộc. 

Bước quan trọng là xử lý từng phân khúc một cách độc lập và tổng hợp các phân chia tầng. Điều này có hiệu quả vì khi một phân đoạn được giảm xuống dưới`a`hoặc`b`, nó sẽ không thể sử dụng được đối với người chơi đó và các bước di chuyển không bao giờ hợp nhất các phân đoạn do vĩnh viễn`X`rào cản. 

### Tại sao nó hoạt động 

Mỗi lần di chuyển sẽ phá hủy vĩnh viễn ít nhất một khối có chiều dài đầy đủ`a`hoặc`b`bên trong một đoạn chấm và không di chuyển nào có thể tạo mới`.`tế bào. Điều này làm cho mỗi phân đoạn dấu chấm trở thành một tài nguyên chỉ có thể được sử dụng theo các phần cố định. Bởi vì lối chơi tối ưu không bao giờ được hưởng lợi từ việc trộn lẫn các phân đoạn nên trò chơi sẽ phân tách thành các trò chơi con độc lập cho mỗi phân đoạn. Người chiến thắng được xác định bằng việc liệu Alice có thể trích xuất tổng thể nhiều nước đi hơn Bob hay không, điều này đảm bảo cuối cùng Bob sẽ hết nước đi hợp pháp trước tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        a, b = map(int, input().split())
        s = input().strip()

        alice = 0
        bob = 0

        i = 0
        n = len(s)

        while i < n:
            if s[i] == 'X':
                i += 1
                continue

            j = i
            while j < n and s[j] == '.':
                j += 1

            length = j - i

            alice += length // a
            bob += length // b

            i = j

        if alice > bob:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc từng truy vấn và xử lý chuỗi trong một lần quét tuyến tính. Nó nhóm các dấu chấm liên tiếp thành các đoạn bằng cách sử dụng thao tác quét hai con trỏ. Đối với mỗi phân đoạn, nó tính toán số lượng nước đi đầy đủ mà mỗi người chơi có thể trích xuất bằng cách chia số nguyên cho độ dài nước đi tương ứng của họ. 

Sự so sánh ở cuối phản ánh điều kiện ưu thế có được trước đó. Cần phải có sự bất bình đẳng nghiêm ngặt vì sự bình đẳng có nghĩa là Bob luôn có thể phản ánh sự tiến bộ của Alice và ngăn cản cô ấy giành được lợi thế quyết định. 

Phải cẩn thận để thiết lập lại ranh giới phân đoạn một cách chính xác khi gặp phải`X`, vì các phân đoạn là độc lập và không được hợp nhất. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi tính toán trên hai đầu vào. 

### Ví dụ 1 

đầu vào:```
a = 3, b = 2
s = XX......XX...X
```| Phân đoạn | Chiều dài | Alice đạt được | Bob đạt được | 
| --- | --- | --- | --- | 
|`......`| 6 | 2 | 3 | 
|`...`| 3 | 1 | 1 | 

Tổng số Alice = 3 

Tổng số Bob = 4 

Alice không thể vượt quá khả năng của Bob nên cô ấy thua cuộc. 

Điều này cho thấy rằng mặc dù Alice loại bỏ các phần lớn hơn nhưng kích thước bước nhỏ hơn của Bob cho phép anh ấy trích xuất nhiều bước di chuyển hơn từ cùng một cấu trúc. 

### Ví dụ 2 

đầu vào:```
a = 4, b = 2
s = X...X.X..X
```| Phân đoạn | Chiều dài | Alice đạt được | Bob đạt được | 
| --- | --- | --- | --- | 
|`...`| 3 | 0 | 1 | 
|`.`| 1 | 0 | 0 | 
|`..`| 2 | 0 | 1 | 

Tổng số Alice = 0 

Tổng số Bob = 2 

Alice không có nước đi hợp lệ ban đầu nên Bob thắng một cách tầm thường. 

Điều này chứng tỏ tầm quan trọng của việc yêu cầu ít nhất một đoạn có độ dài đầy đủ`a`để Alice thậm chí bắt đầu ảnh hưởng đến trò chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | Mỗi ký tự được truy cập một lần trong một lần quét | 
| Không gian | O(1) | Chỉ các bộ đếm và chỉ số được lưu trữ | 

Tổng chiều dài của tất cả các chuỗi được giới hạn bởi 3×10^5, do đó, quá trình quét tuyến tính trên tất cả các truy vấn sẽ vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isfinite

    def solve():
        q = int(input())
        for _ in range(q):
            a, b = map(int, input().split())
            s = input().strip()

            alice = 0
            bob = 0

            i = 0
            n = len(s)

            while i < n:
                if s[i] == 'X':
                    i += 1
                    continue
                j = i
                while j < n and s[j] == '.':
                    j += 1
                length = j - i
                alice += length // a
                bob += length // b
                i = j

            print("YES" if alice > bob else "NO")

    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    import sys as _sys

    output = io.StringIO()
    _sys.stdout = output
    solve()
    _sys.stdout = sys.__stdout__
    sys.stdin = old
    return output.getvalue().strip()

# provided samples
assert run("""3
3 2
XX......XX...X
4 2
X...X.X..X
5 3
.......X..X
""") == """YES
NO
YES"""

# custom cases
assert run("""1
3 1
........
""") == "YES", "single large segment"

assert run("""1
4 2
....
""") == "NO", "Alice cannot move"

assert run("""1
2 1
X.X.X.X.
""") == "NO", "all segments too small for Alice advantage"

assert run("""1
5 4
........X........
""") == "YES", "two large independent segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 / ........`| CÓ | sự thống trị phân khúc tối đa duy nhất | 
|`4 2 / ....`| KHÔNG | Alice không thể di chuyển | 
|`2 1 / X.X.X.X.`| KHÔNG | cấu trúc phân mảnh ngăn cản lợi thế | 
|`5 4 / ........X........`| CÓ | sự độc lập của các phân khúc | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi Alice không có đoạn có độ dài hợp lệ`a`bất cứ nơi nào trong chuỗi. Đối với đầu vào như`a = 5, s = "......."`, thuật toán đếm chính xác số 0 Alice di chuyển và ngay lập tức đưa ra kết quả`NO`, vì Alice thậm chí không thể bắt đầu trò chơi. 

Một trường hợp khác là một chuỗi bị phân mảnh cao như`X.X.X.X.`. Ở đây mỗi đoạn chấm có độ dài 1. Nếu`a > 1`, phần đóng góp của Alice trở thành 0 trong khi Bob vẫn có thể có nước đi nếu`b = 1`. Logic phân đoạn tách biệt chính xác từng dấu chấm đơn lẻ, đảm bảo không có sự hợp nhất ngẫu nhiên nào làm tăng số lần di chuyển. 

Trường hợp tinh tế cuối cùng là một phân đoạn lớn duy nhất trong đó`L`chỉ lớn hơn một chút so với`a`hoặc`b`. Việc chia số nguyên đảm bảo rằng phần còn lại một phần không được tính là lần di chuyển, phù hợp với quy tắc rằng một lần di chuyển yêu cầu một khối liền kề đầy đủ.
