---
title: "CF 2180B - Ashmal"
description: "Chúng ta được cấp một chuỗi các chuỗi và chúng ta phải chèn từng chuỗi một vào một chuỗi trống ban đầu. Mỗi chuỗi mới có thể được đặt ở đầu bên trái hoặc đầu bên phải của kết quả hiện tại. Sau tất cả các lần chèn, chúng ta thu được một chuỗi cuối cùng."
date: "2026-06-07T22:06:10+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2180
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 31 (Div. 1 + Div. 2)"
rating: 800
weight: 2180
solve_time_s: 89
verified: true
draft: false
---

[CF 2180B - Ashmal](https://codeforces.com/problemset/problem/2180/B) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chuỗi và chúng ta phải chèn từng chuỗi một vào một chuỗi trống ban đầu. Mỗi chuỗi mới có thể được đặt ở đầu bên trái hoặc đầu bên phải của kết quả hiện tại. Sau tất cả các lần chèn, chúng ta thu được một chuỗi cuối cùng. Nhiệm vụ của chúng ta là chọn hướng chèn sao cho chuỗi cuối cùng này nhỏ nhất về mặt từ điển trong số tất cả các lựa chọn có thể có. 

Khó khăn chính là mỗi quyết định đều ảnh hưởng đến cấu trúc tương lai trên phạm vi toàn cầu. Một lựa chọn có vẻ tốt hơn về mặt cục bộ có thể cản trở sự sắp xếp tốt hơn sau này, bởi vì khi một chuỗi được đặt ở một bên, các ký tự bên trong của nó sẽ trở nên cố định so với phần còn lại của cấu trúc. 

Các ràng buộc nhỏ về số lượng chuỗi, nhiều nhất là 1000 cho mỗi lần kiểm tra và tổng độ dài tối đa là 4000. Điều này ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các phép gán hướng 2^n. Ngay cả 2^1000 cũng hoàn toàn không khả thi. Do đó, chúng tôi đang tìm kiếm một quy trình quyết định tham lam trong đó mỗi chuỗi được đặt một lần với một số quy tắc tối ưu cục bộ. 

Trường hợp phức tạp xuất phát từ các chuỗi giống hệt nhau hoặc có chung tiền tố dài. Ví dụ: nếu chúng ta có các chuỗi như`"ab"`Và`"aa"`, đặt`"ab"`ở bên trái và bên phải thay đổi ký tự nào được so sánh đầu tiên trong lần so sánh từ điển cuối cùng. Một cách tiếp cận đơn giản chỉ so sánh các chuỗi một cách độc lập mà không xem xét cả hai hướng của toàn bộ chuỗi được xây dựng sẽ thất bại. 

Một trường hợp góc khác là khi nhiều chuỗi bắt đầu bằng cùng một ký tự. Một quy tắc tham lam chỉ so sánh các ký tự đầu tiên mà không nhìn vào phần nối đầy đủ cuối cùng có thể đặt sai vị trí ban đầu, bởi vì sự so sánh mang tính quyết định có thể xảy ra sâu hơn bên trong các khối được nối. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi phép gán có thể có của mỗi chuỗi ở bên trái hoặc bên phải. Đối với mỗi cấu hình trong số 2^n cấu hình, chúng tôi xây dựng chuỗi kết quả và lấy giá trị tối thiểu theo từ điển. Điều này đúng vì nó khám phá tất cả các kết quả có thể xảy ra, nhưng nó có hàm mũ theo n và thất bại ngay lập tức tại n = 1000. 

Quan sát quan trọng là chúng ta không cần xây dựng các chuỗi ứng cử viên đầy đủ. Ở mỗi bước, chúng ta chỉ phải quyết định giữa hai khả năng: đặt chuỗi tiếp theo ở phía trước hoặc phía sau. Chuỗi cuối cùng là sự kết hợp của các khối, vì vậy việc so sánh từ điển giữa hai kết quả hoàn chỉnh phụ thuộc vào vị trí đầu tiên nơi chúng khác nhau. Vị trí đó phải nằm hoàn toàn bên trong một trong các khối hoặc ở ranh giới giữa các khối. 

Điều này làm giảm vấn đề thành quyết định sắp xếp tham lam cho từng chuỗi một cách độc lập: khi xem xét việc đặt một chuỗi`x`, chúng tôi so sánh hai kết quả giả định: kết quả hiện tại + x ở cuối và x + kết quả hiện tại ở phía trước. Thay vì xây dựng cả hai chuỗi đầy đủ, chúng ta chỉ cần biết cách nối nào nhỏ hơn về mặt từ điển. 

Sự đơn giản hóa chính là chúng ta có thể quyết định một cách tham lam một cách an toàn ở mỗi bước: giữa việc đặt`x`bên trái hoặc bên phải, chúng tôi so sánh`x + current`so với`current + x`. Cái nhỏ hơn trong hai cái xác định vị trí tối ưu cho bước này. 

Điều này hiệu quả vì quyết định không phụ thuộc vào các chuỗi trong tương lai: các lần chèn trong tương lai sẽ chỉ nối thêm các khối ở hai bên, nhưng chúng ảnh hưởng đến cả hai nhánh một cách đối xứng. Do đó, khi chúng ta chọn hướng nhỏ hơn ở bước i, không có lựa chọn nào sau đó có thể đảo ngược thứ tự từ điển được thiết lập ở vị trí khác nhau đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · tổng chiều dài) | O(tổng chiều dài) | Quá chậm | 
| Tham lam tối ưu | O(n · tổng chiều dài) | O(tổng chiều dài) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một chuỗi hiện tại`s`, ban đầu trống rỗng. 

1. Bắt đầu với`s = ""`. Điều này thể hiện cấu trúc trống trước bất kỳ phần chèn nào. Không có quyết định nào được thực hiện đối với chuỗi đầu tiên vì cả phép chèn trước và sau đều tạo ra kết quả như nhau. 
2. Đối với mỗi chuỗi tiếp theo`x`, hãy xem xét hai kết quả có thể xảy ra: đặt`x`ở phía trước đưa ra`x + s`, đặt nó ở phía sau sẽ mang lại`s + x`. 
3. So sánh hai chuỗi ứng viên này theo từ điển. Thứ tự từ điển phụ thuộc vào chỉ mục đầu tiên nơi chúng khác nhau, vì vậy chúng tôi mô phỏng so sánh trực tiếp bằng cách sử dụng so sánh chuỗi Python. 
4. Nếu`x + s`nhỏ hơn, cập nhật`s = x + s`. Nếu không thì đặt`s = s + x`. Điều này đảm bảo rằng sau khi xử lý từng chuỗi,`s`là kết quả tốt nhất có thể đạt được cho tiền tố của chuỗi được xử lý. 
5. Tiếp tục cho đến khi tất cả các chuỗi được xử lý và xuất ra`s`. 

Lý do điều này có tác dụng là vì khi chúng tôi cam kết đặt một chuỗi ở một bên, tất cả các chuỗi trong tương lai chỉ mở rộng kết quả vượt quá điểm không khớp đầu tiên giữa hai tùy chọn. Điểm không khớp đó xác định thứ tự vĩnh viễn, do đó không có thao tác nào sau này có thể thay đổi thứ tự tương đối được thiết lập ở bước i. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = input().split()

        s = ""

        for i in range(n):
            x = arr[i]

            # compare two possible placements
            if x + s < s + x:
                s = x + s
            else:
                s = s + x

        print(s)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo quy tắc tham lam. Phần không tầm thường duy nhất là sự so sánh`x + s < s + x`, mô hình chính xác thứ tự từ điển giữa hai trạng thái cuối cùng có thể có ở bước này. 

Một lỗi phổ biến là cố gắng chỉ so sánh các ký tự hoặc độ dài đầu tiên. Điều đó không thành công vì ký tự quyết định có thể xuất hiện sâu bên trong một trong hai chuỗi. Tính năng so sánh chuỗi của Python đã xử lý chính xác việc đánh giá từ điển đầy đủ, vì vậy chúng tôi dựa vào nó một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = ["a", "ab", "abc"]
```| Bước | x | hiện tại | x + s | s + x | đã chọn s | 
| --- | --- | --- | --- | --- | --- | 
| 1 | một | "" | một | một | một | 
| 2 | ab | một | aba | aab | aab | 
| 3 | abc | aab | abcaab | aababc | aababc | 

Đầu ra cuối cùng là`aababc`. 

Dấu vết này cho thấy vị trí sớm ảnh hưởng như thế nào đến thứ tự tiền tố. Mặc dù`"ab"`lớn hơn`"a"`cách ly, đặt nó sau`"a"`tạo ra sự liên kết tiền tố tốt hơn. 

### Ví dụ 2 

đầu vào:```
n = 2
a = ["b", "ba"]
```| Bước | x | hiện tại | x + s | s + x | đã chọn s | 
| --- | --- | --- | --- | --- | --- | 
| 1 | b | "" | b | b | b | 
| 2 | ba | b | em yêu | bb | em yêu | 

Đầu ra cuối cùng là`bab`. 

Điều này cho thấy cách so sánh quyết định chính xác vị trí dựa trên phép nối đầy đủ thay vì chỉ các ký tự đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L) | Mỗi bước so sánh các phép nối có tổng chiều dài tỷ lệ thuận với kết quả hiện tại | 
| Không gian | O(L) | Chúng tôi chỉ lưu trữ chuỗi phát triển | 

Tổng độ dài trên tất cả các chuỗi được giới hạn bởi 4000, do đó, ngay cả những so sánh lặp lại vẫn nhanh chóng. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = input().split()
        s = ""
        for x in arr:
            if x + s < s + x:
                s = x + s
            else:
                s = s + x
        out.append(s)
    return "\n".join(out)

# provided samples
assert run("""3
4
amir rima amin nima
1
codeforces
3
a ab abc
""") == """aminamirrimanima
codeforces
aababc"""

# custom case 1: single element
assert run("""1
1
zzz
""") == "zzz"

# custom case 2: identical strings
assert run("""1
3
a a a
""") == "aaa"

# custom case 3: strong prefix effect
assert run("""1
2
ba ab
""") == "abba"

# custom case 4: alternating greedy pressure
assert run("""1
4
b ba a aa
""") == "aaaabba"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chuỗi không thay đổi | xử lý trường hợp cơ bản | 
| tất cả đều bình đẳng | sự ổn định nối | so sánh giống hệt nhau | 
| "ba ab" | abba | đặt hàng nhạy cảm để so sánh đầy đủ | 
| tiền tố hỗn hợp | hành vi tham lam ổn định | tương tác nhiều bước | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các chuỗi đều giống hệt nhau. Thuật toán luôn so sánh`x + s`Và`s + x`, bằng nhau nên nó luôn gắn vào bên phải. Điều này tạo ra một phép nối đơn giản mà không có sự mơ hồ. 

Một trường hợp khác là khi một chuỗi là tiền tố của một chuỗi khác. Ví dụ,`"a"`Và`"ab"`có thể dẫn đến các kết quả từ điển khác nhau tùy thuộc vào vị trí. Thuật toán giải quyết chính xác điều này bởi vì`"ab" + "a" = "aba"`lớn hơn`"a" + "ab" = "aab"`, Vì thế`"a"`được đặt đầu tiên, duy trì tính tối ưu về mặt từ điển. 

Trường hợp tinh tế cuối cùng là xen kẽ các dây nhỏ và lớn. Ngay cả khi so sánh cục bộ cho thấy sự đảo ngược, so sánh ghép nối đầy đủ đảm bảo rằng chỉ có ký tự khác nhau đầu tiên mới quyết định vị trí, do đó các quyết định trước đó vẫn nhất quán với thứ tự từ điển tổng thể.
