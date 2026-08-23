---
title: "CF 104264A - Nguyên âm"
description: "Chúng ta được cung cấp một chuỗi duy nhất chỉ bao gồm các chữ cái tiếng Anh viết thường. Nhiệm vụ là tính một số nguyên dựa trên chuỗi này và in nó. Từ các mẫu, chúng tôi nhận thấy rằng chỉ một số chữ cái nhất định đóng góp vào câu trả lời, trong khi tất cả những chữ cái khác không đóng góp gì."
date: "2026-07-01T21:31:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104264
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #9 (Fool-Forces)"
rating: 0
weight: 104264
solve_time_s: 66
verified: true
draft: false
---

[CF 104264A - Nguyên âm](https://codeforces.com/problemset/problem/104264/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất chỉ bao gồm các chữ cái tiếng Anh viết thường. Nhiệm vụ là tính một số nguyên dựa trên chuỗi này và in nó. Từ các mẫu, chúng tôi nhận thấy rằng chỉ một số chữ cái nhất định đóng góp vào câu trả lời, trong khi tất cả những chữ cái khác không đóng góp gì. 

Các mẫu làm cho quy tắc rõ ràng. TRONG`"abue"`đầu ra là`3`, TRONG`"amir"`nó là`2`, và trong`"qwsdr"`nó là`0`. Cách giải thích nhất quán duy nhất là chúng ta đang đếm xem có bao nhiêu nguyên âm xuất hiện trong chuỗi, trong đó nguyên âm là các chữ cái`a`,`e`,`i`,`o`, Và`u`. 

Độ dài chuỗi tối đa là 100. Độ dài này đủ nhỏ để chỉ cần quét trực tiếp mọi ký tự với một điều kiện đơn giản là đủ. Giới hạn trên có nghĩa là tổng số thao tác trên mỗi trường hợp thử nghiệm tối đa là 100 phép so sánh, không đáng kể trong giới hạn 1 giây. Bất kỳ giải pháp tuyến tính nào cũng an toàn một cách tầm thường. 

Không có ràng buộc cấu trúc ẩn như thứ tự hoặc nhóm. Trường hợp cạnh có ý nghĩa duy nhất là khi chuỗi không chứa nguyên âm nào cả, trong trường hợp đó câu trả lời là 0. Một trường hợp cạnh khác là khi chuỗi bao gồm toàn bộ nguyên âm, trong trường hợp đó câu trả lời bằng độ dài chuỗi. Đầu vào một ký tự cũng hoạt động tự nhiên theo quy tắc tương tự. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải quyết vấn đề này là xem xét từng ký tự và quyết định xem đó có phải là nguyên âm hay không bằng cách kiểm tra nó dựa trên tất cả các chữ cái nguyên âm. Đối với mỗi nhân vật, chúng tôi so sánh nó với`a`,`e`,`i`,`o`, Và`u`. Nếu bất kỳ sự trùng khớp nào xảy ra, chúng tôi sẽ tăng một bộ đếm. 

Điều này hoạt động chính xác vì nó trực tiếp thực hiện định nghĩa của tác vụ: đếm các ký tự nguyên âm. Chi phí được giới hạn bởi 5 so sánh cho mỗi ký tự, do đó, đối với một chuỗi có độ dài n, độ phức tạp là O(5n), đơn giản hóa thành O(n). Với n ≤ 100, tốc độ này cực kỳ nhanh. 

Không thực sự cần phải tối ưu hóa thêm ngoài việc nhận ra rằng việc kiểm tra tư cách thành viên có thể được đơn giản hóa bằng cách sử dụng một bộ cho rõ ràng, nhưng ngay cả điều đó cũng là tùy chọn. Cấu trúc của bài toán hoàn toàn là quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra trực tiếp) | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (đặt tư cách thành viên / kiểm tra trực tiếp) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. Chúng ta cần toàn bộ chuỗi vì mỗi ký tự đóng góp độc lập vào kết quả. 
2. Khởi tạo bộ đếm về 0. Biến này tích lũy số lượng nguyên âm gặp phải cho đến nay. 
3. Xác định bộ nguyên âm`{a, e, i, o, u}`. Điều này cho phép kiểm tra tư cách thành viên liên tục cho từng ký tự. 
4. Lặp lại từng ký tự trong chuỗi. Mỗi ký tự được đánh giá độc lập vì sự đóng góp của một chữ cái không phụ thuộc vào chữ cái khác. 
5. Nếu ký tự hiện tại thuộc bộ nguyên âm, hãy tăng bộ đếm. Nếu không, không làm gì và tiếp tục. Điều kiện này là cốt lõi của việc xác định vấn đề. 
6. Sau khi xử lý tất cả các ký tự, xuất ra giá trị cuối cùng của bộ đếm. 

### Tại sao nó hoạt động 

Mỗi ký tự trong chuỗi được phân thành đúng một trong hai loại: nguyên âm hoặc không nguyên âm. Thuật toán tăng bộ đếm chính xác một lần cho mỗi nguyên âm và không bao giờ tăng nó cho các nguyên âm không phải nguyên âm. Vì mỗi ký tự được xử lý chính xác một lần và đóng góp độc lập nên số đếm cuối cùng bằng tổng số nguyên âm trong chuỗi. Không có sự sắp xếp lại hoặc tương tác giữa các ký tự ảnh hưởng đến kết quả, do đó, một lần chuyển tuyến tính duy nhất là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

vowels = set("aeiou")

cnt = 0
for ch in s:
    if ch in vowels:
        cnt += 1

print(cnt)
```Giải pháp đọc chuỗi đầu vào và xóa khoảng trắng ở cuối bằng cách sử dụng`strip`. Bộ nguyên âm được lưu trữ trong Python`set`vì vậy việc kiểm tra tư cách thành viên diễn ra trong thời gian trung bình là O(1). 

Vòng lặp xử lý mỗi ký tự chính xác một lần. Đối với mỗi ký tự, chúng tôi thực hiện kiểm tra tư cách thành viên đối với bộ nguyên âm. Nếu thành công thì bộ đếm sẽ tăng lên. Kết quả cuối cùng được in trực tiếp. 

Một lỗi triển khai phổ biến là quên loại bỏ ký tự dòng mới khỏi đầu vào, điều này vô hại ở đây nhưng có thể dẫn đến nhầm lẫn trong các vấn đề khác. Một lỗi tiềm ẩn khác là sử dụng danh sách thay vì một tập hợp để kiểm tra tư cách thành viên, cách này vẫn có tác dụng nhưng làm cho mục đích không rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`"abue"`Chúng tôi theo dõi bộ đếm khi chúng tôi quét chuỗi. 

| Nhân vật | Là nguyên âm | Quầy | 
| --- | --- | --- | 
| một | vâng | 1 | 
| b | không | 1 | 
| bạn | vâng | 2 | 
| e | vâng | 3 | 

Đầu ra cuối cùng là 3. 

Điều này xác nhận rằng nhiều nguyên âm nằm rải rác trong chuỗi được tính độc lập và tích lũy chính xác. 

### Ví dụ 2:`"amir"`| Nhân vật | Là nguyên âm | Quầy | 
| --- | --- | --- | 
| một | vâng | 1 | 
| m | không | 1 | 
| tôi | vâng | 2 | 
| r | không | 2 | 

Đầu ra cuối cùng là 2. 

Điều này cho thấy các ký tự không phải nguyên âm không ảnh hưởng đến số đếm và được bỏ qua một cách an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra một lần dựa trên bộ nguyên âm có kích thước không đổi | 
| Không gian | O(1) | Chỉ một bộ nguyên âm cố định và bộ đếm được lưu trữ | 

Kích thước đầu vào tối đa là 100, do đó quét tuyến tính thấp hơn nhiều so với mọi giới hạn tính toán. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    s = input().strip()
    vowels = set("aeiou")
    cnt = 0
    for ch in s:
        if ch in vowels:
            cnt += 1
    return str(cnt)

# provided samples
assert run("abue\n") == "3", "sample 1"
assert run("amir\n") == "2", "sample 2"
assert run("qwsdr\n") == "0", "sample 3"

# custom cases
assert run("a\n") == "1", "single vowel"
assert run("bcd\n") == "0", "no vowels"
assert run("aeiou\n") == "5", "all vowels"
assert run("abcdeiouxyz\n") == "6", "mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`| 1 | đầu vào kích thước tối thiểu | 
|`"bcd"`| 0 | không có trường hợp nguyên âm | 
|`"aeiou"`| 5 | trường hợp tất cả các nguyên âm | 
|`"abcdeiouxyz"`| 6 | độ chính xác phân phối hỗn hợp | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`"a"`được xử lý một cách tự nhiên. Vòng lặp chạy một lần, kiểm tra nguyên âm thành công và bộ đếm trở thành 1. Không cần logic trường hợp đặc biệt. 

Một chuỗi không có nguyên âm như`"bcd"`kết quả là không có sự gia tăng trong quá trình lặp lại. Mỗi ký tự được kiểm tra, không vượt qua bài kiểm tra tư cách thành viên và bộ đếm vẫn giữ nguyên số 0 xuyên suốt, tạo ra kết quả đầu ra chính xác. 

Một chuỗi nguyên âm đầy đủ như`"aeiou"`tăng bộ đếm ở mỗi bước. Mỗi lần lặp lại đạt được điều kiện thành viên, do đó số đếm cuối cùng bằng độ dài của chuỗi. Điều này xác nhận rằng thuật toán tích lũy chính xác các kết quả trùng khớp tích cực lặp lại mà không bị nhiễu.
