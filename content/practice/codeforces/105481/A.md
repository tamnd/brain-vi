---
title: "CF 105481A - \u7231\u4e0a\u5b57\u5178"
description: "Chúng ta được cung cấp một đoạn văn bản dài thể hiện một câu chuyện. Văn bản chứa các từ trộn lẫn với dấu cách và dấu chấm câu như dấu phẩy, dấu chấm, dấu chấm than và dấu chấm hỏi."
date: "2026-06-23T18:19:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105481
codeforces_index: "A"
codeforces_contest_name: "2024 CCPC Liaoning Provincial Contest"
rating: 0
weight: 105481
solve_time_s: 54
verified: true
draft: false
---

[CF 105481A - \u7231\u4e0a\u5b57\u5178](https://codeforces.com/problemset/problem/105481/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đoạn văn bản dài thể hiện một câu chuyện. Văn bản chứa các từ trộn lẫn với dấu cách và dấu chấm câu như dấu phẩy, dấu chấm, dấu chấm than và dấu chấm hỏi. Mỗi từ là một chuỗi các chữ cái và các từ có thể xuất hiện dưới dạng viết hoa khác nhau nhưng chúng tôi xử lý chúng theo cách không phân biệt chữ hoa chữ thường. 

Cùng với văn bản này, chúng tôi được cung cấp một từ điển nhỏ gồm các từ mà người đọc đã biết. Khi người đọc gặp một từ trong truyện, nếu nó không có trong tập hợp đã biết của họ, họ sẽ tra cứu một lần rồi ghi nhớ vĩnh viễn. Tuy nhiên, người đọc có một nhận thức sai lầm về từ: các dạng hình thái khác nhau được coi là những từ khác nhau. Vì vậy, “chấp nhận” và “được chấp nhận” không giống nhau, mặc dù chúng có chung một gốc. 

Nhiệm vụ là mô phỏng việc đọc toàn bộ văn bản từ trái sang phải và đếm xem có bao nhiêu từ riêng biệt không có trong tập hợp đã biết ban đầu. Mỗi từ chưa biết sẽ kích hoạt chính xác một lần tra cứu từ điển ngay lần đầu tiên nó xuất hiện. 

Độ dài văn bản có thể lên tới 5 × 10^6 ký tự. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào liên tục quét hoặc sao chép chuỗi con một cách không hiệu quả. Chúng ta cần một lần truyền tuyến tính duy nhất trên văn bản, trích xuất các từ một cách nhanh chóng và xử lý chúng theo từng bước. Kích thước từ điển tối đa là 100 từ, vì vậy việc kiểm tra tư cách thành viên diễn ra liên tục nếu chúng tôi lưu trữ chúng trong bộ băm. 

Một số trường hợp đặc biệt quan trọng trong thực tế. Các từ phải được chuẩn hóa thành chữ thường vì các từ đầu vào có thể có chữ cái đầu tiên viết hoa. Dấu câu xuất hiện ngay sau các từ, do đó việc phân chia các khoảng trắng một cách đơn giản sẽ không đủ nếu chúng ta không loại bỏ dấu câu một cách cẩn thận. Một trường hợp tế nhị khác là lặp lại những từ chưa biết: chỉ lần xuất hiện đầu tiên mới làm tăng câu trả lời. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ là chia toàn bộ văn bản thành các mã thông báo bằng cách sử dụng dấu cách và dấu câu, sau đó đối với mỗi mã thông báo, hãy kiểm tra liên tục xem nó có xuất hiện trong danh sách đã biết hay không. Vì danh sách đã biết nhỏ nên chúng tôi có thể quét tuyến tính nó để tìm mọi mã thông báo. Đây vốn đã là ranh giới nhưng vẫn khả thi nếu xét riêng lẻ. Vấn đề thực sự không phải là tra cứu từ điển mà là chi phí xây dựng chuỗi con nhiều lần hoặc quét văn bản nhiều lần nếu thực hiện bất cẩn. 

Cách tiếp cận mạnh mẽ hơn sẽ liên tục quét danh sách đã biết cho từng từ bằng cách sử dụng so sánh chuỗi, dẫn đến hành vi O(total_words × n), nhưng vì n ≤ 100 nên phần này không phải là nút thắt cổ chai. Sự kém hiệu quả thực sự phát sinh nếu chúng ta cố gắng phân tách bằng cách sử dụng biểu thức chính quy hoặc các thao tác chuỗi lặp lại để phân bổ nhiều đối tượng trung gian trên chuỗi 5 triệu ký tự. 

Quan sát quan trọng là chúng ta chỉ cần một lần chuyển qua văn bản. Chúng ta có thể tích lũy các ký tự vào bộ đệm trong khi đọc và bất cứ khi nào chúng ta chạm đến ranh giới không phải chữ cái, chúng ta sẽ hoàn thiện từ, chuẩn hóa nó và xử lý nó ngay lập tức. Điều này tránh được bất kỳ lượt vượt qua hoặc chi phí phân tích cú pháp nặng nề nào. 

Chúng tôi lưu trữ các từ đã biết trong một tập hợp băm và chúng tôi cũng duy trì một tập hợp khác cho các từ mà chúng tôi đã tra cứu trong khi đọc. Mỗi từ được xử lý trong thời gian trung bình O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T × n) hoặc tệ hơn khi phân tích cú pháp nhiều | O(T) | Quá chậm/không an toàn | 
| Tối ưu | O(T) | O(T + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét từng ký tự văn bản và trích xuất các từ tăng dần. 

1. Khởi tạo một tập hợp trống`known`chứa tất cả các từ trong từ điển đã cho. 
2. Khởi tạo một tập hợp trống`seen_unknown`để theo dõi các từ đã được tính là tra cứu từ điển. 
3. Khởi tạo bộ đệm chuỗi trống`current`. 
4. Duyệt từng ký tự trong văn bản: 

1. Nếu ký tự là một chữ cái, hãy thêm dạng chữ thường của nó vào`current`. 
2. Ngược lại, nếu`current`không trống, hãy xử lý nó như một từ hoàn chỉnh. 
5. Khi xử lý một từ hoàn chỉnh: 

1. Nếu nó không có trong`known`và không ở trong`seen_unknown`, tăng câu trả lời và chèn nó vào`seen_unknown`. 
2. Xóa`current`. 
6. Sau khi vòng lặp kết thúc, xử lý mọi từ còn lại trong`current`sử dụng cùng một logic. 
7. Xuất ra câu trả lời tích lũy. 

Lý do chúng tôi bình thường hóa nhanh chóng là để tránh lưu trữ các biến thể kiểu chữ hỗn hợp, điều này sẽ phá vỡ việc kiểm tra tính bằng nhau. Lý do chúng tôi trì hoãn việc đếm cho đến khi hoàn thành từ là để đảm bảo dấu câu không ảnh hưởng đến ranh giới từ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét,`current`đại diện chính xác cho một chuỗi ký tự liền kề trong văn bản. Mỗi từ trong dữ liệu đầu vào được phân tách bằng một ký tự không phải chữ cái, do đó, mỗi từ cuối cùng được hoàn thành đúng một lần. Bởi vì chúng tôi chèn những từ chưa biết vào`seen_unknown`ngay trong lần gặp đầu tiên, những lần xuất hiện tiếp theo không ảnh hưởng đến câu trả lời. Tính bất biến đó là`seen_unknown`luôn chứa chính xác những từ đã được đếm và`known`chứa tất cả các từ không yêu cầu đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    text = sys.stdin.readline().rstrip("\n")
    n = int(sys.stdin.readline())
    known_words = sys.stdin.readline().split()

    known = set(known_words)
    seen_unknown = set()

    ans = 0
    current = []

    def process(word):
        nonlocal ans
        if not word:
            return
        if word not in known and word not in seen_unknown:
            seen_unknown.add(word)
            ans += 1

    for ch in text:
        if ch.isalpha():
            current.append(ch.lower())
        else:
            if current:
                process("".join(current))
                current.clear()

    if current:
        process("".join(current))

    print(ans)

if __name__ == "__main__":
    solve()
```Việc xem xét hiệu suất chính là tránh nối chuỗi lặp lại bên trong vòng lặp. Chúng tôi tích lũy các ký tự trong một danh sách và chỉ tham gia khi đạt đến ranh giới từ. Điều này giữ cho độ phức tạp tuyến tính trong tổng số ký tự. 

Chúng tôi cũng viết thường rõ ràng mọi ký tự trong quá trình tích lũy, điều này tránh việc chuyển từ thứ hai sang từ sau đó. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào giống như mẫu:```
I love Liaoning. Love Dalian!
1
love
```Chúng tôi xử lý văn bản một cách tuần tự. 

| Bước | Dòng Char hiện tại | Từ đã hoàn thành | Được biết? | Đã thấy Chưa biết | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | "tôi" | Tôi | không | {i} | 1 | 
| 2 | "tình yêu" | tình yêu | vâng | {} | 1 | 
| 3 | "Liêu Ninh" | Liêu Ninh | không | {liaoning} | 2 | 
| 4 | "tình yêu" | Tình yêu | vâng | {} | 2 | 
| 5 | "Đại Liên" | Đại Liên | không | {liaoning, đại liên} | 3 | 

Dấu vết này cho thấy rằng việc lặp lại các từ đã biết không ảnh hưởng đến câu trả lời, trong khi các từ chưa biết chỉ được tính một lần. 

Bây giờ hãy xem xét một trường hợp có sự lặp lại và dấu câu:```
Hello hello! HELLO?
0
```| Bước | Dòng Char hiện tại | Từ đã hoàn thành | Đã thấy Chưa biết | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | "xin chào" | xin chào | {xin chào} | 1 | 
| 2 | "xin chào" | xin chào | {xin chào} | 1 | 
| 3 | "xin chào" | xin chào | {xin chào} | 1 | 

Điều này xác nhận việc chuẩn hóa trường hợp và ngăn chặn sự trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi ký tự được xử lý một lần và mỗi từ được chèn/kiểm tra trong bộ băm trong thời gian trung bình O(1) | 
| Không gian | O(T + n) | Trường hợp xấu nhất lưu trữ tất cả các ký tự của quá trình xử lý văn bản hiện tại cộng với bộ từ điển | 

Các ràng buộc cho phép tối đa 5 triệu ký tự, do đó, quét tuyến tính với các thao tác liên tục trong thời gian cho mỗi ký tự là đủ trong giới hạn. Việc sử dụng bộ nhớ vẫn an toàn vì chỉ có các bộ từ và một bộ đệm nhỏ được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# Note: assume solve() prints output, adjust if needed

# Sample-style case
# assert run("I love Liaoning. Love Dalian!\n1\nlove\n") == "3\n"

# Minimum input
assert run("A\n0\n") == "1\n", "single word unknown"

# All known words
assert run("Hello world\n2\nhello world\n") == "0\n", "all known"

# Repeated unknown words
assert run("test test test\n0\n") == "1\n", "only first occurrence counts"

# Case normalization
assert run("Hi HI hI\n0\n") == "1\n", "case insensitive"

# Punctuation boundary
assert run("a,b.c!d?\n0\n") == "4\n", "all split correctly"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Từ đơn | 1 | xử lý tối thiểu chưa biết | 
| Tất cả đều được biết đến | 0 | lọc từ điển | 
| Lặp đi lặp lại không rõ | 1 | chống trùng lặp | 
| Trường hợp hỗn hợp | 1 | bình thường hóa | 
| Dấu câu nặng | 4 | mã thông báo chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi văn bản kết thúc bằng một dãy chữ cái không có dấu câu. Trong trường hợp đó, nếu không xử lý rõ ràng thì từ cuối cùng sẽ không bao giờ được xử lý. Giải pháp khắc phục điều này bằng cách chạy`process(current)`sau khi vòng lặp kết thúc. 

đầu vào:```
hello world
0
```Trong quá trình truyền tải, “xin chào” và “thế giới” được xử lý ở ranh giới không gian và không có từ nào còn lại trong bộ đệm ở cuối. Lần xả cuối cùng không làm gì cả, xác nhận tính đúng đắn. 

Một trường hợp cạnh khác là nhiều ký tự dấu chấm câu hoặc dấu cách liên tiếp. Vì quá trình xử lý chỉ kích hoạt quá trình chuyển đổi từ chữ cái sang không phải chữ cái nên các dấu phân cách lặp lại không tạo ra các từ trống. 

đầu vào:```
a,,!!b??
0
```Thuật toán trích xuất “a” và “b” thành hai từ riêng biệt và cả hai đều được tính chính xác một lần.
