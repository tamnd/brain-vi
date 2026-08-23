---
title: "CF 104282B - Emoji Master BSQ"
description: "Chúng tôi được cung cấp một từ điển cố định về thay thế từ. Mỗi quy tắc nêu rõ rằng một từ cụ thể phải luôn được thay thế bằng một từ cố định khác. Sau khi đọc tất cả các quy tắc, chúng ta sẽ được cung cấp một chuỗi các từ tạo thành câu do BSQ nói."
date: "2026-07-01T21:05:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104282
codeforces_index: "B"
codeforces_contest_name: "The 20th Hangzhou City University Programming Contest"
rating: 0
weight: 104282
solve_time_s: 55
verified: true
draft: false
---

[CF 104282B - Emoji Master BSQ](https://codeforces.com/problemset/problem/104282/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một từ điển cố định về thay thế từ. Mỗi quy tắc nêu rõ rằng một từ cụ thể phải luôn được thay thế bằng một từ cố định khác. Sau khi đọc tất cả các quy tắc, chúng ta sẽ được cung cấp một chuỗi các từ tạo thành câu do BSQ nói. Mỗi từ trong câu đó được đảm bảo xuất hiện ở phía bên trái theo đúng một quy tắc thay thế, do đó mỗi từ ánh xạ tới chính xác một từ đầu ra. 

Nhiệm vụ là chuyển đổi câu bằng cách thay thế từng từ một cách độc lập bằng cách sử dụng các quy tắc đã cho và in ra chuỗi kết quả. 

Các ràng buộc nhỏ, với cả số lượng quy tắc và số lượng từ trong câu lên tới 100 và độ dài mỗi từ lên tới 10. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào ít nhất là tuyến tính hoặc bậc hai trong kích thước đầu vào sẽ đủ nhanh. Ngay cả việc quét trực tiếp tất cả các quy tắc cho mỗi từ cũng chỉ thực hiện tối đa 10.000 so sánh chuỗi, điều này không đáng kể. 

Không có sự phức tạp về cấu trúc tiềm ẩn như biến đổi chuỗi hoặc viết lại nhiều bước; mỗi từ được thay thế chính xác một lần bằng cách sử dụng ánh xạ được cung cấp. 

Một số trường hợp đặc biệt vẫn cần được nêu rõ: 

Việc triển khai ngây thơ có thể cho rằng việc thay thế là lặp đi lặp lại một cách không chính xác. Ví dụ: nếu một quy tắc nói "a → b" và quy tắc khác nói "b → c", thì một giải pháp sai có thể áp dụng nhiều lần các quy tắc và biến "a" thành "c". Tuy nhiên, vấn đề không bao giờ yêu cầu đóng bắc cầu. Chỉ cần thay thế trực tiếp. 

Một cạm bẫy tiềm tàng khác là quên rằng các từ là các mã thông báo được phân tách bằng dấu cách. Nếu một người cố gắng xử lý dữ liệu đầu vào dưới dạng chuỗi thô và thay thế chuỗi con, thì việc trùng khớp một phần ngẫu nhiên có thể làm hỏng kết quả. Ví dụ: thay thế "wo" bên trong "wow" sẽ không chính xác theo cách tiếp cận dựa trên chuỗi con, mặc dù quá trình xử lý dựa trên mã thông báo sẽ tránh hoàn toàn điều này. 

## Phương pháp tiếp cận 

Cách đơn giản để giải quyết vấn đề là diễn giải các quy tắc như một quá trình tra cứu. Đối với mỗi từ trong câu, chúng tôi quét qua tất cả các quy tắc cho đến khi tìm thấy quy tắc có vế trái khớp với từ đó, sau đó xuất ra vế phải tương ứng của nó. 

Phương pháp brute-force này đúng vì các quy tắc tạo thành một danh sách liên kết đơn giản. Tuy nhiên, giá thành của nó tỷ lệ thuận với số từ trong câu nhân với số lượng quy tắc. Trong trường hợp xấu nhất, điều này dẫn đến so sánh chuỗi 100 × 100 = 10.000, vẫn có thể chấp nhận được ở đây nhưng sẽ trở nên lãng phí nếu các ràng buộc mở rộng quy mô. 

Quan sát quan trọng là chúng tôi liên tục tìm kiếm cùng một khóa trong tập dữ liệu tĩnh. Đây chính xác là mục đích mà bản đồ băm được thiết kế. Bằng cách tính toán trước một từ điển từ mỗi từ nguồn đến từ thay thế của nó, chúng tôi giảm thời gian tra cứu trung bình của mỗi truy vấn xuống O(1). Điều này biến vấn đề từ việc quét tuyến tính lặp đi lặp lại thành vấn đề ánh xạ trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(nm) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Tra cứu bản đồ băm | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta xây dựng một cấu trúc ánh xạ lưu trữ mọi quy tắc. Mỗi khóa là một từ nguồn và mỗi giá trị là từ thay thế của nó. 

1. Đọc các số nguyên n và m, xác định có bao nhiêu quy tắc ánh xạ và bao nhiêu từ xuất hiện trong câu. 
2. Khởi tạo một từ điển trống. 
3. Với mỗi quy tắc n, hãy đọc cặp (si, ti) và lưu nó vào từ điển dưới dạng si → ti. Bước này đảm bảo việc tra cứu liên tục sau này. 
4. Đọc m từ trong câu. 
5. Đối với mỗi từ trong câu, hãy truy xuất giá trị được ánh xạ của nó từ từ điển và thêm nó vào chuỗi đầu ra. 
6. In các từ đã chuyển theo thứ tự, cách nhau bằng dấu cách. 

Lý do chúng ta có thể dựa vào tra cứu trực tiếp một cách an toàn là vì vấn đề đảm bảo mọi từ đầu vào đều xuất hiện dưới dạng khóa trong ánh xạ, do đó không cần xử lý khóa bị thiếu. 

### Tại sao nó hoạt động

Thuật toán xây dựng một hàm f được xác định trên tập hợp các từ đầu vào, trong đó mỗi từ có chính xác một hình ảnh. Vì bài toán đảm bảo bao phủ toàn bộ và tính duy nhất của vế trái nên f là một ánh xạ được xác định rõ. Câu đầu ra chỉ đơn giản là ứng dụng theo từng điểm của f cho mỗi mã thông báo trong chuỗi đầu vào. Vì không có quy tắc nào phụ thuộc vào các lần thay thế trước đó nên mỗi phép chuyển đổi đều độc lập và duy trì trật tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    mp = {}

    for _ in range(n):
        s, t = input().split()
        mp[s] = t

    words = input().split()
    res = []

    for w in words:
        res.append(mp[w])

    print(" ".join(res))

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng việc xây dựng một từ điển`mp`lưu trữ tất cả các quy tắc thay thế. Mỗi cặp đầu vào được chèn trực tiếp, việc ghi đè không phải là vấn đề đáng lo ngại vì vấn đề đảm bảo tất cả`si`là khác biệt. 

Sau đó, câu này được đọc dưới dạng danh sách các mã thông báo bằng cách sử dụng`split()`, đảm bảo chúng tôi coi các từ là đơn vị nguyên tử chứ không phải là chuỗi con. Mỗi từ được dịch thông qua tra cứu từ điển và thêm vào danh sách kết quả. Cuối cùng, đầu ra được nối với khoảng trắng để xây dựng lại câu đã chuyển đổi. 

Một chi tiết tinh tế nhưng quan trọng là tránh nối chuỗi lặp lại trong quá trình xây dựng đầu ra. Sử dụng danh sách và`" ".join()`đảm bảo hành vi thời gian tuyến tính thay vì tăng trưởng bậc hai từ việc nối thêm chuỗi lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
wo i
qiu ball
ni you
wo qiu qiu ni
```| Bước | Lời | Tra cứu | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 1 | ôi | tôi | tôi | 
| 2 | qiu | bóng | tôi bóng | 
| 3 | qiu | bóng | tôi bóng bóng | 
| 4 | đấy | bạn | tôi bóng bóng bạn | 

Điều này cho thấy các từ lặp lại được xử lý độc lập, với mỗi lần xuất hiện sẽ kích hoạt cùng một thao tác tra cứu từ điển. 

Đầu ra:```
i ball ball you
```### Ví dụ 2 

đầu vào:```
2 5
a x
b y
a b a b a
```| Bước | Lời | Tra cứu | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 1 | một | x | x | 
| 2 | b | y | x y | 
| 3 | một | x | x y x | 
| 4 | b | y | x y x y | 
| 5 | một | x | x y x y x | 

Điều này thể hiện tính đúng đắn dưới các mẫu lặp đi lặp lại xen kẽ. 

Đầu ra:```
x y x y x
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi quy tắc được chèn một lần và mỗi từ được dịch một lần bằng cách sử dụng tra cứu từ điển trung bình O(1) | 
| Không gian | O(n) | Từ điển lưu trữ một ánh xạ cho mỗi quy tắc | 

Các ràng buộc cho phép tối đa 100 quy tắc và 100 từ, vì vậy giải pháp này nằm trong giới hạn thoải mái ngay cả với chi phí ban đầu. Cách tiếp cận bản đồ băm vẫn là cách giải thích rõ ràng nhất và có khả năng mở rộng nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    mp = {}

    for _ in range(n):
        s, t = input().split()
        mp[s] = t

    words = input().split()
    res = [mp[w] for w in words]
    return " ".join(res)

# provided sample
assert run("""3 4
wo i
qiu ball
ni you
wo qiu qiu ni
""") == "i ball ball you"

# single mapping
assert run("""1 3
a b
a a a
""") == "b b b"

# alternating pattern
assert run("""2 4
x y
y x
x y x y
""") == "y x y x"

# minimum size
assert run("""1 1
a z
a
""") == "z"

# all same mapping
assert run("""2 3
a b
c d
c a c
""") == "d b d"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| quy tắc duy nhất lặp đi lặp lại | b b b | tính nhất quán tra cứu lặp đi lặp lại | 
| ánh xạ xen kẽ | y x y x | xử lý mã thông báo độc lập | 
| trường hợp tối thiểu | z | độ đúng ranh giới | 
| quy tắc hỗn hợp | d b d | tính chính xác của nhiều ánh xạ | 

## Vỏ cạnh 

Một trường hợp tinh tế là sự xuất hiện lặp lại của cùng một từ trong câu. Ví dụ:```
2 3
a x
b y
a a b
```Thuật toán xử lý từng mã thông báo một cách độc lập. Đầu tiên "a" trở thành "x", thứ hai "a" lại trở thành "x" và "b" trở thành "y". Vì việc tra cứu từ điển không phụ thuộc vào vị trí hoặc lịch sử nên việc lặp lại không gây ra vấn đề gì. 

Một trường hợp khác là đảm bảo rằng chúng tôi không bao giờ thử thay thế một phần. Bởi vì chúng tôi chia câu bằng khoảng trắng nên mỗi mã thông báo được xử lý nguyên tử. Ngay cả khi các từ có chung tiền tố, chẳng hạn như "ab" và "a", chúng vẫn là những từ khóa riêng biệt trong từ điển và không bị nhầm lẫn. 

Cuối cùng, vì mọi từ đầu vào đều được đảm bảo tồn tại trong ánh xạ nên không cần xử lý dự phòng. Quyền truy cập từ điển trực tiếp luôn hợp lệ và không có trường hợp KeyError nào cần được xem xét trong các ràng buộc của vấn đề.
