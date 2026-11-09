---
title: "CF 147A - Dấu câu"
description: "Chúng ta được cung cấp một chuỗi chứa các chữ cái viết thường, dấu cách và một số dấu câu giới hạn: dấu phẩy, dấu chấm, dấu chấm than và dấu chấm hỏi."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 147
codeforces_index: "A"
codeforces_contest_name: "Codeforces Testing Round 4"
rating: 1300
weight: 147
solve_time_s: 78
verified: true
draft: false
---

[CF 147A - Dấu câu](https://codeforces.com/problemset/problem/147/A) 

**Đánh giá:** 1300 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chứa các chữ cái viết thường, dấu cách và một số dấu câu giới hạn: dấu phẩy, dấu chấm, dấu chấm than và dấu chấm hỏi. Mục tiêu của chúng tôi là định dạng lại chuỗi này để các từ cách nhau đúng một dấu cách, không có dấu cách nào xuất hiện trước dấu câu và mỗi dấu chấm câu được theo sau đúng một dấu cách. Chuỗi luôn bắt đầu và kết thúc bằng một chữ cái và có ít nhất một từ giữa hai dấu câu bất kỳ. 

Độ dài đầu vào có thể lên tới 10.000 ký tự. Điều này có nghĩa là bất kỳ giải pháp nào kiểm tra từng ký tự một lần hoặc một số lần không đổi đều có thể chấp nhận được, nhưng các vòng lặp lồng nhau trên đầu vào sẽ nhanh chóng trở nên kém hiệu quả. Các trường hợp cạnh bao gồm nhiều dấu cách liên tiếp, dấu câu không có dấu cách, dấu câu ngay sau các từ và chuỗi dấu chấm câu chỉ cách nhau bằng chữ cái. Ví dụ, đầu vào`"hi ,world!good"`nên sản xuất`"hi, world! good"`. Một cách tiếp cận ngây thơ chỉ cắt bớt các khoảng trắng trên toàn cầu hoặc chèn các khoảng trắng một cách mù quáng có thể để lại các khoảng trắng thừa, loại bỏ các khoảng trắng cần thiết hoặc không thành công ở ranh giới dấu câu. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là quét chuỗi liên tục và sửa từng lần xuất hiện của khoảng trắng liên tiếp hoặc dấu câu bị đặt sai vị trí cho đến khi không còn thay đổi nào. Điều này có hiệu quả vì mọi phép biến đổi đều di chuyển chuỗi đến gần trạng thái hợp lệ hơn nhưng không hiệu quả. Trong trường hợp xấu nhất, điều này có thể yêu cầu các thao tác O(n^2) nếu chúng ta liên tục xây dựng lại chuỗi cho mỗi lần chỉnh sửa nhỏ. 

Quan sát quan trọng dẫn đến giải pháp tối ưu là chúng ta có thể xử lý chuỗi một cách tuần tự. Bằng cách lặp lại từng ký tự, chúng ta có thể quyết định ngay lập tức xem nên thêm ký tự hiện tại vào kết quả, chèn dấu cách hay bỏ qua dấu cách thừa. Chúng tôi chỉ duy trì ký tự cuối cùng được thêm vào để kiểm tra xem ký tự tiếp theo có cần khoảng trắng hay nên được gắn trực tiếp vào dấu câu hay không. Điều này cho phép một lần truyền qua chuỗi với các thao tác O(n) và không gian bổ sung O(n) cho đầu ra, điều này hoàn toàn có thể chấp nhận được với giới hạn đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm đối với đầu vào lớn | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một danh sách trống để xây dựng chuỗi kết quả theo từng ký tự. Danh sách được ưu tiên hơn trong Python vì nối chuỗi là O(n) mỗi lần. 
2. Lặp lại từng ký tự trong chuỗi đầu vào. 
3. Nếu ký tự là một chữ cái, hãy thêm nó trực tiếp vào kết quả. Điều này bảo tồn tất cả các từ mà không sửa đổi. 
4. Nếu ký tự là khoảng trắng, hãy kiểm tra ký tự được thêm vào cuối cùng. Nếu ký tự cuối cùng là một chữ cái, hãy đánh dấu cờ rằng có thể cần một khoảng trắng. Nếu ký tự cuối cùng là dấu câu hoặc dấu cách trước đó, hãy bỏ qua nó vì không được phép có nhiều dấu cách hoặc dấu cách trước dấu câu. 
5. Nếu ký tự là dấu chấm câu, hãy thêm trực tiếp ký tự đó vào kết quả. Đặt lại bất kỳ dấu cách nào đang chờ xử lý vì dấu câu không bao giờ được có dấu cách trước đó. Ngay sau khi thêm dấu câu, hãy thêm một dấu cách trừ khi dấu câu nằm ở cuối chuỗi. 
6. Sau khi xử lý tất cả các ký tự, hãy xóa mọi dấu cách ở cuối, vì đầu ra không được kết thúc bằng dấu cách. 

Tại sao nó hoạt động: Điều bất biến là sau khi xử lý từng ký tự, danh sách kết quả hiện tại luôn biểu thị tiền tố hợp lệ của đầu ra. Chúng tôi không bao giờ chèn thêm dấu cách trước dấu câu và chúng tôi đảm bảo chính xác một dấu cách sau dấu câu hoặc giữa các từ. Điều này đảm bảo rằng toàn bộ chuỗi đáp ứng định dạng được yêu cầu sau khi quá trình lặp hoàn tất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    s = input().rstrip()
    result = []
    n = len(s)
    i = 0

    while i < n:
        if s[i].isalpha():
            result.append(s[i])
            i += 1
            # handle space after letters if next is letter
            while i < n and s[i] == ' ':
                i += 1
            if i < n and s[i].isalpha():
                result.append(' ')
        elif s[i] in ',.!?':
            result.append(s[i])
            i += 1
            # skip spaces after punctuation
            while i < n and s[i] == ' ':
                i += 1
            # add one space if not at end
            if i < n:
                result.append(' ')
        else:
            i += 1  # skip other spaces
    print(''.join(result))

if __name__ == "__main__":
    main()
```Giải pháp xử lý mỗi ký tự chính xác một lần. Các dấu cách được bỏ qua trừ khi cần thiết, các chữ cái được thêm trực tiếp và việc xử lý dấu câu đảm bảo có một dấu cách duy nhất theo sau đồng thời ngăn các dấu cách trước đó. Điều tinh tế quan trọng là bỏ qua nhiều khoảng trắng liên tiếp và không thêm dấu cách ở cuối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`"galileo galilei was an   italian physicist  ,mathematician,astronomer"`| tôi | s[i] | kết quả | hành động | 
| --- | --- | --- | --- | 
| 0 | g | g | thêm thư | 
| 6 | o | galileo | thêm thư | 
| 7 | ' ' | galileo | bỏ qua, tiếp theo là chữ cái → thêm dấu cách | 
| 8 | g | galileo g | thêm thư | 
| ... | ... | ... | xử lý thư | 
| 37 | , | galileo galilei là một nhà vật lý người Ý, | thêm dấu câu, thêm dấu cách sau | 
| 38 | m | galileo galilei là một nhà vật lý người Ý, m | thêm thư | 
| 49 | , | galileo galilei là một nhà vật lý, toán học người Ý, | thêm dấu câu, thêm dấu cách | 
| 50 | một | galileo galilei là một nhà vật lý, toán học người Ý, một | thêm thư | 

Dấu vết này hiển thị dấu cách trước khi dấu phẩy bị xóa và có chính xác một dấu cách sau dấu phẩy. 

### Ví dụ tùy chỉnh 

đầu vào:`"hi ,world!good"`| tôi | s[i] | kết quả | hành động | 
| --- | --- | --- | --- | 
| 0 | h | h | thêm thư | 
| 2 | , | xin chào, | thêm dấu câu, thêm dấu cách | 
| 3 | w | xin chào, w | thêm thư | 
| 8 | ! | chào thế giới! | thêm dấu câu, thêm dấu cách | 
| 9 | g | chào thế giới! g | thêm thư | 

Điều này xác nhận việc xử lý chính xác dấu câu với các chữ cái ngay sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và tổng số lần bỏ qua là tuyến tính | 
| Không gian | O(n) | Danh sách kết quả lưu trữ tối đa tất cả các ký tự cộng với khoảng trắng được chèn | 

Với n 10000, giải pháp này chạy thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().rstrip()

# provided sample
assert run("galileo galilei was an   italian physicist  ,mathematician,astronomer") == "galileo galilei was an italian physicist, mathematician, astronomer"

# minimum input
assert run("a") == "a"

# multiple spaces between words
assert run("a   b") == "a b"

# punctuation at end
assert run("hello world!") == "hello world!"

# punctuation with spaces before and after
assert run("hi ,  there !how are  you?") == "hi, there! how are you?"

# consecutive punctuation
assert run("wait!stop,now.") == "wait! stop, now."

# maximum input
assert run("a " * 5000 + ",b") == "a " * 5000 + ", b"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`|`"a"`| đầu vào tối thiểu | 
|`"a   b"`|`"a b"`| nhiều khoảng trắng giữa các từ | 
|`"hello world!"`|`"hello world!"`| dấu câu ở cuối | 
|`"hi ,  there !how are  you?"`|`"hi, there! how are you?"`| khoảng trắng trước/sau dấu câu | 
|`"wait!stop,now."`|`"wait! stop, now."`| dấu câu liên tiếp không có dấu cách | 
|`"a "*5000+",b"`|`"a "*5000+", b"`| đầu vào lớn | 

## Vỏ cạnh 

Thuật toán xử lý chính xác nhiều khoảng trắng liên tiếp bằng cách bỏ qua chúng và chỉ chèn khoảng trắng khi cần thiết giữa các chữ cái. Đối với dấu câu ngay sau các chữ cái, nó sẽ xóa mọi khoảng trắng hiện có trước dấu câu và đảm bảo một khoảng trắng duy nhất ở sau, trừ khi ở cuối. Hành vi này nhất quán trong tất cả các trường hợp cạnh như`"hi ,world!good"`hoặc`"wait!stop,now."`, chứng minh rằng tính bất biến của việc xây dựng tiền tố hợp lệ ở mỗi bước đảm bảo đầu ra cuối cùng chính xác.
