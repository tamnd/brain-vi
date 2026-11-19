---
title: "CF 2094A - Trippi Troppi"
description: "Mỗi trường hợp thử nghiệm mô tả một “tên quốc gia” được lưu trữ ở định dạng cũ hơn bao gồm ba từ riêng biệt. Cách biểu diễn hiện đại được hình thành bằng cách nén ba từ đó thành một từ viết tắt duy nhất: chúng ta lấy ký tự đầu tiên của từ đầu tiên, ký tự đầu tiên của…"
date: "2026-06-08T05:33:13+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 2094
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1017 (Div. 4)"
rating: 800
weight: 2094
solve_time_s: 87
verified: true
draft: false
---

[CF 2094A - Trippi Troppi](https://codeforces.com/problemset/problem/2094/A) 

**Đánh giá:** 800 
**Thẻ:** chuỗi 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một “tên quốc gia” được lưu trữ ở định dạng cũ hơn bao gồm ba từ riêng biệt. Cách biểu diễn hiện đại được hình thành bằng cách nén ba từ đó thành một chữ viết tắt duy nhất: chúng ta lấy ký tự đầu tiên của từ đầu tiên, ký tự đầu tiên của từ thứ hai và ký tự đầu tiên của từ thứ ba, sau đó ghép chúng lại theo thứ tự. 

Vì vậy, nhiệm vụ hoàn toàn là phân tích chuỗi. Đối với mỗi dòng, chúng tôi chia nó thành chính xác ba từ và trích xuất một ký tự từ mỗi từ. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 100 trường hợp thử nghiệm và mỗi từ có độ dài tối đa là 10. Điều này có nghĩa là tổng kích thước đầu vào rất nhỏ và bất kỳ phương pháp nào đọc và xử lý từng dòng một lần đều đã tối ưu. Ngay cả việc quét lặp đi lặp lại hoặc xây dựng lại các chuỗi cũng đủ nhanh, nhưng cấu trúc của vấn đề gợi ý rõ ràng về phương pháp trích xuất trực tiếp. 

Không có trường hợp cạnh số phức tạp nào, nhưng có một kiểu lỗi tinh vi xuất hiện khi triển khai không chính xác: xử lý toàn bộ dòng dưới dạng một chuỗi và lấy các vị trí cố định như`s[0], s[1], s[2]`. Điều đó bị phá vỡ ngay lập tức vì dấu cách ngăn cách các từ và chữ cái thứ hai và thứ ba của dòng đầy đủ thường là dấu cách chứ không phải ký tự. Ví dụ, ở đầu vào`oh my god`, chuỗi bắt đầu bằng`'o'`, sau đó là khoảng trắng, do đó việc lập chỉ mục một cách mù quáng sẽ cho kết quả sai. Hành vi đúng luôn phụ thuộc vào việc phân tách theo khoảng trắng trước tiên. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là lấy từng dòng và với mỗi ký tự, hãy cố gắng xác định xem đó có phải là ký tự đầu tiên của một từ hay không. Người ta có thể quét chuỗi và duy trì cờ boolean cho biết liệu chúng ta đang ở đầu một từ (ở chỉ số 0 hay sau dấu cách). Mỗi khi cờ đúng, chúng tôi sẽ thêm ký tự đó vào. Điều này có hiệu quả, nhưng nó thực hiện những công việc không cần thiết vì chúng ta đang lặp lại từng ký tự trong mỗi dòng, mặc dù chúng ta chỉ cần ba ký tự cho mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là cấu trúc được cố định. Mỗi dòng luôn chứa chính xác ba từ, đã được phân tách rõ ràng bằng dấu cách. Khi chúng tôi chia dòng thành các mã thông báo, câu trả lời sẽ được xác định theo thời gian không đổi cho mỗi trường hợp kiểm tra: chỉ cần truy cập ký tự đầu tiên của mỗi mã thông báo. Điều này làm giảm vấn đề thành việc phân tách và lập chỉ mục chuỗi đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhân vật quét lực lượng vũ phu | O(tổng ký tự) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tách + Truy cập trực tiếp | O(tổng số ký tự để phân tách) | O(1) | Đã chấp nhận | 

Phương pháp dựa trên phân tách hoàn toàn đơn giản hơn và tránh việc theo dõi trạng thái thủ công. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`, xác định số lượng dòng độc lập mà chúng tôi sẽ xử lý. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc toàn bộ dòng và chia nó thành ba từ bằng cách sử dụng khoảng trắng. Điều này đảm bảo chúng tôi tách biệt từng từ một cách rõ ràng bất kể khoảng cách. 
3. Trích xuất ký tự đầu tiên của mỗi từ trong số ba từ. Vì mỗi từ được đảm bảo không trống và bao gồm các chữ cái viết thường nên việc lập chỉ mục ở vị trí 0 luôn hợp lệ. 
4. Ghép 3 ký tự này theo thứ tự và xuất ra kết quả ngay. 

### Tại sao nó hoạt động 

Mỗi dòng đầu vào được cố định về mặt cấu trúc thành chính xác ba mã thông báo và tên hiện đại được xác định hoàn toàn là phép chiếu xác định của các mã thông báo đó. Thao tác lấy ký tự đầu tiên độc lập với từng từ nên không có sự tương tác giữa các vị trí. Miễn là mã thông báo chính xác, ánh xạ từ dòng đầu vào đến chuỗi đầu ra là không mất dữ liệu và duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = input().split()
    print(a[0] + b[0] + c[0])
```Giải pháp dựa trên tính năng tích hợp sẵn của Python`split()`tự động xử lý khoảng cách tùy ý và đảm bảo chúng tôi tách biệt chính xác ba từ. Sau đó, chúng tôi phân tách trực tiếp thành ba biến, điều này làm cho ý định trở nên rõ ràng và tránh các lỗi lập chỉ mục trên danh sách. 

Mỗi từ được đảm bảo tồn tại nên việc giải nén thành`a, b, c`là an toàn dưới các ràng buộc của vấn đề. 

Phép nối cuối cùng sử dụng phép cộng chuỗi đơn giản, hiệu quả với kích thước cố định là 3 ký tự. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào để xem quá trình chuyển đổi hoạt động như thế nào. 

### Ví dụ 1 

Dòng đầu vào:`binary indexed tree`| Bước | Mã thông báo 1 | Mã thông báo 2 | Mã thông báo 3 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Chia | nhị phân | được lập chỉ mục | cây | | 
| Những chữ cái đầu tiên | b | tôi | t | chút | 
| Cuối cùng | | | | chút | 

Điều này cho thấy mỗi từ độc lập đóng góp chính xác một ký tự mà không phụ thuộc vào vị trí hoặc độ dài. 

### Ví dụ 2 

Dòng đầu vào:`skibidi slay sigma`| Bước | Mã thông báo 1 | Mã thông báo 2 | Mã thông báo 3 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Chia | trượt tuyết | giết | sigma | | 
| Những chữ cái đầu tiên | s | s | s | sss | 
| Cuối cùng | | | | sss | 

Trường hợp này chứng tỏ rằng các chữ cái đầu lặp lại được xử lý một cách tự nhiên; không cần logic đặc biệt cho các chữ cái trùng lặp hoặc giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng kích thước đầu vào) | Mỗi dòng được chia một lần và chỉ có ba ký tự được truy cập | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được lưu trữ cho mỗi trường hợp thử nghiệm | 

Giới hạn đầu vào rất nhỏ nên ngay cả việc quét từng ký tự cũng sẽ an toàn. Cách tiếp cận được lựa chọn là tối ưu và thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = sys.stdin.readline().split()
        out.append(a[0] + b[0] + c[0])
    return "\n".join(out)

# provided sample
assert run("""7
united states america
oh my god
i cant lie
binary indexed tree
believe in yourself
skibidi slay sigma
god bless america
""") == """usa
omg
icl
bit
biy
sss
gba"""

# custom 1: minimal repetition
assert run("""1
a b c
""") == "abc"

# custom 2: identical words
assert run("""1
same same same
""") == "sss"

# custom 3: mixed words
assert run("""1
code forces round
""") == "cfr"

# custom 4: multiple cases
assert run("""2
hello world again
alpha beta gamma
""") == """hwa
abg"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a b c`|`abc`| trường hợp tối thiểu | 
|`same same same`|`sss`| tên viết tắt lặp đi lặp lại | 
|`code forces round`|`cfr`| từ hỗn hợp thông thường | 
| hai dòng |`hwa`,`abg`| tính đúng đắn của nhiều bài kiểm tra | 

## Vỏ cạnh 

Một cạm bẫy tiềm ẩn là giả sử các vị trí ký tự cố định trong chuỗi thô thay vì mã hóa. Ví dụ, đầu vào`oh my god`không thể được xử lý bằng cách lấy`s[0]`,`s[1]`,`s[2]`, bởi vì`s[1]`là một không gian. Việc xử lý đúng sẽ phân chia trước: 

đầu vào:`oh my god`Sau khi chia tách:`["oh", "my", "god"]`Sau đó: 

- từ đầu tiên cho`'o'`- từ thứ hai cho`'m'`- từ thứ ba cho`'g'`Kết quả:`omg`Một trường hợp đặc biệt khác là các chữ cái được lặp lại trong các từ. đầu vào`skibidi slay sigma`sản xuất`sss`, điều này xác nhận rằng chúng tôi không cần bất kỳ sự trùng lặp hoặc xử lý đặc biệt nào.
