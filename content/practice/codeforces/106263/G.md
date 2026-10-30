---
title: "CF 106263G - \u5c0f\u6a58\u7684\u7cd6\u679c\u6e38\u620f"
description: "Chúng tôi được cung cấp một trò chơi theo lượt chơi trên một đống kẹo. Hai người chơi luân phiên nhau di chuyển, người đầu tiên luôn đi trước. Trong mỗi lần di chuyển, người chơi hiện tại phải loại bỏ chính xác 1, 3 hoặc 4 viên kẹo khỏi đống."
date: "2026-06-18T23:20:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106263
codeforces_index: "G"
codeforces_contest_name: "2025 \u534e\u5357\u5e08\u8303\u5927\u5b66\u201c\u5353\u8d8a\u6559\u80b2\u676f\u201d\u7b97\u6cd5\u4e0e\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u65b0\u751f\u8d5b\uff09"
rating: 0
weight: 106263
solve_time_s: 64
verified: true
draft: false
---

[CF 106263G - \u5c0f\u6a58\u7684\u7cd6\u679c\u6e38\u620f](https://codeforces.com/problemset/problem/106263/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một trò chơi theo lượt chơi trên một đống kẹo. Hai người chơi luân phiên nhau di chuyển, người đầu tiên luôn đi trước. Trong mỗi lần di chuyển, người chơi hiện tại phải loại bỏ chính xác 1, 3 hoặc 4 viên kẹo khỏi đống. Nếu một người chơi bắt đầu lượt của mình và không thể thực hiện một nước đi hợp lệ, nghĩa là cọc trống hoặc không có nước đi nào được phép phù hợp với số đếm còn lại, người chơi đó sẽ thua ngay lập tức. 

Mỗi trường hợp thử nghiệm đưa ra số lượng kẹo ban đầu và chúng tôi cần xác định người chơi nào thắng trong cách chơi tối ưu. Đầu ra chỉ đơn giản là tên của người chơi chiến thắng: “Orange” cho người đi đầu tiên và “ice” cho người đi thứ hai. 

Các ràng buộc cho phép tối đa 100000 trường hợp thử nghiệm và các giá trị n lên tới 1e9. Điều này ngay lập tức loại trừ mọi lập trình động cho mỗi lần kiểm tra trên toàn bộ phạm vi lên đến n. Ngay cả DP tuyến tính lên tới 1e9 cũng không thể thực hiện được và thậm chí việc tính toán trước lên tới 1e9 cũng không khả thi cả về thời gian và bộ nhớ. Giải pháp phải giảm từng truy vấn về thời gian không đổi sau một giai đoạn tiền xử lý nhỏ hoặc rút ra công thức trực tiếp. 

Có hai trường hợp phức tạp thường phá vỡ lối suy luận ngây thơ. 

Đầu tiên, n = 0. Người chơi đầu tiên không di chuyển và thua ngay lập tức nên đáp án phải là “băng”. Bất kỳ DP nào cho rằng vị trí bắt đầu đang thắng sẽ đánh dấu sai đây là thắng trừ khi được xử lý rõ ràng. 

Thứ hai, những giá trị nhỏ như n = 2 bộc lộ lối suy nghĩ tham lam không đúng đắn. Từ 2 viên kẹo, nước đi duy nhất là lấy 1, để lại 1 cho đối phương và vị trí đó là phần thắng dành cho người chơi tiếp theo. Vì vậy, 2 là thua đối với người chơi hiện tại, mặc dù nó có thể trông cân đối hoặc “đủ nhỏ để thắng”. 

Trường hợp cạnh cấu trúc thứ ba là các bước di chuyển được phép không đối xứng như {1,2}. Sự hiện diện của 3 và 4 tạo ra một cấu trúc tuần hoàn thay vì một mẫu hình chẵn lẻ đơn giản. Mọi nỗ lực nhằm giảm trò chơi thành “thắng lẻ, chẵn thua” đều thất bại ngay lập tức, ví dụ n = 3 là thắng trực tiếp vì bạn có thể lấy hết kẹo. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để phân tích trò chơi này là trò chơi DP khách quan tiêu chuẩn. Chúng ta hãy xác định trạng thái dp[n] đúng nếu người chơi hiện tại có thể giành chiến thắng với n viên kẹo còn lại. Từ vị trí n, người chơi thử tất cả các bước di chuyển hợp lệ: loại bỏ 1, 3 hoặc 4 viên kẹo và chuyển sang dp[n-1], dp[n-3] và dp[n-4] khi các trạng thái đó tồn tại. Một thế cờ được coi là thắng nếu có ít nhất một nước đi dẫn đến thế thua cho đối phương. 

Phép truy hồi này đúng, nhưng việc tính toán nó đến n = 1e9 là không thể. Ngay cả khi chúng ta chỉ tính toán ở giới hạn vừa phải như 1e7 thì số lượng ca kiểm thử và yêu cầu bộ nhớ vẫn trở nên quá lớn. 

Điều quan trọng cần lưu ý là đây là một trò chơi trừ với một bước di chuyển cố định. Những trò chơi như vậy thường trở thành định kỳ sau một tiền tố ngắn vì trạng thái chỉ phụ thuộc vào một số lượng nhỏ các giá trị trước đó. Ở đây dp[n] chỉ phụ thuộc vào dp[n-1], dp[n-3] và dp[n-4], do đó không gian trạng thái bị giới hạn và cuối cùng trình tự lặp lại. 

Bằng cách tính toán dp cho vài chục giá trị đầu tiên, chúng ta quan sát thấy một mô hình lặp lại. Trên thực tế, các vị trí bị mất lặp lại ở giai đoạn 7. Khi chu kỳ này được xác định, mọi truy vấn sẽ giảm xuống tính toán n modulo 7 và kiểm tra bảng tra cứu cố định. 

Điều này biến vấn đề thành thời gian không đổi cho mỗi truy vấn sau quá trình tiền xử lý O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute DP lên đến n | O(n) mỗi lần kiểm tra hoặc tổng O(max n) | O(n) | Quá chậm | 
| DP định kỳ + modulo | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tính trạng thái thắng và trạng thái thua cho các giá trị nhỏ của n cho đến khi một chu kỳ trở nên rõ ràng. Chúng tôi xác định một mảng boolean trong đó sai có nghĩa là thua và đúng có nghĩa là thắng.

Chúng tôi khởi tạo dp[0] là thua vì người chơi không có kẹo không thể di chuyển. Từ đó, chúng ta tính toán lặp lại dp[i] bằng cách sử dụng các chuyển đổi được phép từ i − 1, i − 3 và i − 4 bất cứ khi nào các chỉ số đó hợp lệ. 

Khi mở rộng bảng, chúng tôi tìm kiếm sự lặp lại trong mẫu giá trị dp. Khi chúng tôi thấy rằng trình tự ổn định thành chu kỳ lặp lại có độ dài 7, chúng tôi dừng tiền xử lý và ghi lại khoảng thời gian. 

Tại thời điểm này, chúng tôi xây dựng một bảng tra cứu có kích thước 7 mã hóa xem n mod 7 tương ứng với vị trí thắng hay thua. 

Đối với mỗi truy vấn n, chúng tôi tính toán r = n mod 7. Nếu dp_pattern[r] chỉ ra trạng thái chiến thắng, chúng tôi xuất ra “Orange”, nếu không thì chúng tôi xuất ra “ice”. 

### Tại sao nó hoạt động 

Trò chơi này là một trò chơi trừ hữu hạn với một tập hợp các nước đi có giới hạn. Trạng thái của mỗi vị trí chỉ phụ thuộc vào một số không đổi các trạng thái trước đó, do đó chuỗi các giá trị dp tạo thành một phép truy hồi tuyến tính trên một bảng chữ cái nhị phân hữu hạn. Những chuỗi như vậy cuối cùng phải trở thành tuần hoàn. Khi đạt đến vùng tuần hoàn, giá trị của dp[n] chỉ phụ thuộc vào n modulo độ dài chu kỳ, do đó việc giảm n modulo chu kỳ được phát hiện sẽ duy trì tính chính xác cho tất cả các giá trị lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute DP until we see a cycle
moves = [1, 3, 4]
N = 50  # enough to observe periodicity

dp = [False] * (N + 1)
dp[0] = False

for i in range(1, N + 1):
    win = False
    for m in moves:
        if i - m >= 0 and not dp[i - m]:
            win = True
    dp[i] = win

# Observed cycle for this problem: period 7 starting early
# We extract pattern for mod 7
pattern = [dp[i] for i in range(7)]

t = int(input())
out = []

for _ in range(t):
    n = int(input())
    if pattern[n % 7]:
        out.append("Orange")
    else:
        out.append("ice")

print("\n".join(out))
```Đầu tiên, đoạn mã này xây dựng một bảng DP nhỏ đủ để tiết lộ cấu trúc của trò chơi. Quá trình chuyển đổi kiểm tra xem bất kỳ nước đi hợp lệ nào có dẫn đến trạng thái thua cuộc hay không; nếu vậy thì trạng thái hiện tại đang thắng thế. 

Sau khi tính toán đủ các giá trị, chúng tôi trực tiếp đọc hành vi của dp[i] modulo 7. Mảng mẫu có kích thước không đổi lưu trữ xem mỗi lớp dư lượng có chiến thắng hay không. 

Sau đó, mỗi truy vấn sẽ giảm xuống thành một thao tác modulo duy nhất và tra cứu bảng. Điều này tránh mọi sự phụ thuộc vào n. 

Một điểm tinh tế là xử lý dp[0] một cách chính xác. Nó phải được khởi tạo là thua, vì người chơi hiện tại không có nước đi hợp lệ. Tất cả các trạng thái tiếp theo đều phụ thuộc vào trường hợp cơ bản này. 

## Ví dụ đã hoạt động 

Xét n = 5, n = 6 và n = 7. Chúng ta theo dõi các quyết định DP. 

### Dấu vết ví dụ 

| tôi | i-1 | i-3 | i-4 | quyết định dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | - | - | - | thua | 
| 1 | 0 | - | - | thắng (ăn 1) | 
| 2 | 1 | - | - | thua | 
| 3 | 2 | 0 | - | thắng (ăn 3) | 
| 4 | 3 | 1 | 0 | thắng | 
| 5 | 4 | 2 | 1 | thắng | 

Với n = 2 thì mỗi nước đi đều dẫn đến trạng thái thắng cho đối phương nên thua. Với n = 4 thì lấy 4 liền là thắng, tức là thắng. 

Bây giờ hãy xem xét các giá trị lớn hơn để hiển thị tính tuần hoàn: 

| n | n mod 7 | kết quả | 
| --- | --- | --- | 
| 6 | 6 | thua | 
| 7 | 0 | thua | 
| 8 | 1 | chiến thắng | 

Điều này xác nhận rằng hành vi ổn định thành một mô hình lặp lại dựa trên lớp dư lượng. 

Các dấu vết cho thấy trò chơi không chỉ bị chi phối bởi tính chẵn lẻ. Sự sẵn có của 3 và 4 nước đi tạo ra những bước nhảy chiến thắng phá vỡ các mô hình đơn giản nhưng vẫn tạo ra một chu kỳ giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | mỗi trường hợp thử nghiệm sử dụng một modulo và tra cứu | 
| Không gian | O(1) | chỉ một mẫu DP có kích thước không đổi được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì T có thể lớn tới 100000 và mỗi truy vấn được trả lời trong thời gian không đổi với chi phí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    moves = [1, 3, 4]
    N = 50

    dp = [False] * (N + 1)
    dp[0] = False

    for i in range(1, N + 1):
        win = False
        for m in moves:
            if i - m >= 0 and not dp[i - m]:
                win = True
        dp[i] = win

    pattern = [dp[i] for i in range(7)]

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        res.append("Orange" if pattern[n % 7] else "ice")

    return "\n".join(res)

# provided sample (illustrative; exact sample not fully specified)
# assert run("3\n0\n1\n2\n") == "ice\nOrange\nice"

# custom cases
assert run("1\n0\n") == "ice"
assert run("1\n3\n") == "Orange"
assert run("1\n2\n") == "ice"
assert run("1\n7\n") == "ice"
assert run("1\n8\n") == "Orange"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 0 | băng | trường hợp cọc rỗng mất đế | 
| 1, 3 | Cam | tồn tại nước đi thắng ngay lập tức | 
| 1, 2 | băng | trường hợp mất mát cưỡng bức | 
| 1, 7 | băng | cặn thất thoát định kỳ | 
| 1, 8 | Cam | sự đúng đắn bao quanh sau khoảng thời gian | 

## Vỏ cạnh 

Với n = 0, thuật toán gán trực tiếp băng vì dp_pattern[0] là sai. Điều này phù hợp với thực tế là không có nước đi nào tồn tại nên người chơi đầu tiên sẽ thua ngay lập tức. 

Với n = 2, lớp dư dẫn đến trạng thái thua. Truy tìm DP xác nhận rằng cả hai kết quả có thể xảy ra (ăn 1) đều để lại thế thắng cho đối thủ nên kết quả đúng là băng. 

Với n = 7, chúng tôi tính r = 0. Mẫu đánh dấu đây là thua, vì vậy băng thắng. Mặc dù 7 cho phép di chuyển nhiều lần, nhưng mỗi tùy chọn đều dẫn đến một trạng thái có thể chống lại một cách tối ưu. 

Với n = 8, r = 1, tức là thắng. Thuật toán tạo ra màu Cam, phù hợp với thực tế là người chơi đầu tiên có thể buộc phải thắng bằng cách chuyển sang lớp còn lại đang thua cuộc.
