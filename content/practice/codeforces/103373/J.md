---
title: "CF 103373J - JavaScript"
description: "Chúng ta có hai chuỗi ngắn, x và y, và chúng ta được yêu cầu đánh giá biểu thức x - y chính xác như JavaScript. Khó khăn chính là JavaScript không coi tất cả các chuỗi là chuỗi trong quá trình số học."
date: "2026-07-03T12:39:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103373
codeforces_index: "J"
codeforces_contest_name: "2021 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 103373
solve_time_s: 42
verified: true
draft: false
---

[CF 103373J - JavaScript](https://codeforces.com/problemset/problem/103373/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi ngắn,`x`Và`y`, và chúng tôi được yêu cầu đánh giá biểu thức`x - y`chính xác như JavaScript. Khó khăn chính là JavaScript không coi tất cả các chuỗi là chuỗi trong quá trình số học. Khi sử dụng toán tử trừ, cả hai toán hạng đều được chuyển đổi thành số. Nếu một chuỗi đại diện cho một số hợp lệ thì nó sẽ trở thành số đó. Nếu nó chứa bất kỳ ký tự không có chữ số nào, nó sẽ trở thành`NaN`. Một khi một trong hai toán hạng là`NaN`, kết quả cũng là`NaN`. 

Vì vậy, nhiệm vụ giảm xuống còn mô phỏng quy tắc ép buộc kiểu này: diễn giải mỗi chuỗi dưới dạng số nguyên hoặc dưới dạng giá trị số không hợp lệ, sau đó tính toán phép trừ nếu cả hai đều hợp lệ. 

Các ràng buộc cực kỳ nhỏ: mỗi chuỗi có độ dài nhỏ hơn 6 và chỉ bao gồm các chữ số và chữ cái tiếng Anh. Điều này ngay lập tức cho chúng tôi biết rằng chúng tôi không cần bất kỳ mối quan tâm tối ưu hóa nào như phân tích luồng hoặc xử lý số nguyên lớn. Quét trực tiếp từng chuỗi là đủ. 

Trường hợp cạnh tinh vi là sự lan truyền của các giá trị không hợp lệ. Ví dụ,`"a 2"`không hợp lệ vì`"a"`không thể chuyển đổi thành số, vì vậy kết quả là`NaN`mặc dù toán hạng thứ hai là hợp lệ. Một trường hợp khác là khi phép trừ tạo ra kết quả không nguyên, nhưng theo cách giải thích của bài toán, kết quả này vẫn được coi là số và được in không có số thập phân nếu nó là số nguyên, nếu không nó vẫn là kết quả số hợp lệ dưới dạng số thực, ngoại trừ trong mô tả bài toán này, tất cả các trường hợp hợp lệ vẫn là số nguyên do đầu vào giống số nguyên. 

Một chi tiết quan trọng cuối cùng là ngay cả một chữ cái ở bất kỳ đâu trong chuỗi cũng vô hiệu hóa nó hoàn toàn. 

## Phương pháp tiếp cận 

Việc giải thích brute-force theo nghĩa đen sẽ bắt chước hệ thống kiểu đầy đủ của JavaScript: triển khai một trình phân tích cú pháp chung cố gắng chuyển đổi chuỗi thành số dấu phẩy động, xử lý tất cả các trường hợp đặc biệt của ký hiệu khoa học, quy tắc khoảng trắng, dấu hiệu và định dạng không hợp lệ. Điều đó vượt xa những gì cần thiết ở đây và sẽ gây ra sự phức tạp không cần thiết. 

Quan sát quan trọng là định dạng đầu vào cực kỳ hạn chế. Một chuỗi hoàn toàn là các chữ số hoặc chứa ít nhất một chữ cái. Điều đó đưa ra một phân loại rất đơn giản: số nguyên hợp lệ hoặc số không hợp lệ. 

Vì vậy, thay vì mô phỏng JavaScript, chúng ta chỉ cần kiểm tra xem mỗi chuỗi có bao gồm toàn các chữ số hay không. Nếu có, chúng tôi phân tích nó dưới dạng số nguyên. Nếu không, chúng tôi coi nó như`NaN`. Khi chúng ta có sự phân loại này, phép trừ rất đơn giản. 

Cách tiếp cận bạo lực không thành công vì nó cố gắng mô phỏng thời gian chạy ngôn ngữ đầy đủ. Cách tiếp cận tối ưu hóa thành công vì vấn đề được chuyển thành bước xác thực và số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng JS đầy đủ | O(L log L) trở lên | O(L) | Quá chậm/không cần thiết | 
| Kiểm tra chữ số + phân tích | O(L) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định hàm trợ giúp xác định xem một chuỗi có phải là số hợp lệ hay không. 

1. Đọc chuỗi`x`Và`y`. Chúng đại diện cho các toán hạng của một biểu thức trừ. 
2. Với mỗi chuỗi, quét tất cả các ký tự. Nếu mỗi ký tự là một chữ số, hãy chuyển chuỗi thành giá trị số nguyên. 

Bước này hoạt động vì các chuỗi số hợp lệ trong đầu vào không chứa dấu hiệu, dấu cách hoặc biến chứng định dạng. 
3. Nếu bất kỳ ký tự nào không phải là chữ số, hãy đánh dấu giá trị tương ứng là không hợp lệ, được biểu thị bằng một ký tự đặc biệt`NaN`tình trạng. 
4. Nếu một trong hai giá trị là`NaN`, đầu ra`"NaN"`ngay lập tức. 
5. Ngược lại tính phép trừ số nguyên`value_x - value_y`. 
6. In kết quả dưới dạng số nguyên đơn giản. 

Lý do bước 4 đến trước phép trừ là vì`NaN`đang hấp thụ dưới các phép toán số học, vì vậy chúng tôi tránh tính toán không cần thiết một khi phát hiện thấy tính không hợp lệ. 

### Tại sao nó hoạt động 

Mỗi chuỗi xác định một cách độc lập liệu nó có thể được hiểu là một số hay không. Phép trừ chỉ quan trọng nếu cả hai toán hạng đều nằm trong miền số nguyên. Vì đầu vào đảm bảo rằng các chuỗi số hợp lệ biểu thị số nguyên, toàn bộ vấn đề giảm xuống việc kiểm tra tư cách thành viên trong tập hợp các chuỗi chỉ có chữ số. Kết quả cuối cùng chính xác là sự khác biệt số học của hai số nguyên này và bất kỳ sai lệch nào so với cấu trúc chỉ có chữ số sẽ buộc kết quả vào lớp không hợp lệ, khớp với các quy tắc cưỡng chế của JavaScript. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse(s: str):
    s = s.strip()
    if s.isdigit():
        return int(s)
    return None  # represents NaN

def solve():
    x, y = input().split()
    a = parse(x)
    b = parse(y)

    if a is None or b is None:
        print("NaN")
    else:
        print(a - b)

if __name__ == "__main__":
    solve()
```Bước phân tích cú pháp là cốt lõi của giải pháp. các`.isdigit()`kiểm tra chính xác khớp với ràng buộc rằng các số hợp lệ chỉ chứa các chữ số. Bất kỳ chữ cái nào ngay lập tức làm mất hiệu lực của chuỗi, tạo ra`None`. 

Chúng tôi sử dụng`None`thay vì một trọng điểm số vì nó phân tách rõ ràng các số nguyên hợp lệ khỏi các giá trị không hợp lệ mà không có nguy cơ xung đột. 

Phép trừ chỉ được thực hiện sau khi cả hai toán hạng được xác nhận hợp lệ, duy trì tính chính xác theo quy tắc JavaScript mà bất kỳ thao tác nào liên quan đến`NaN`sản lượng`NaN`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`22 2`| Bước | x | y | Đã phân tích cú pháp x | Đã phân tích y | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | "22" | "2" | 22 | 2 | cả hai đều hợp lệ | 
| 2 | - | - | 22 | 2 | trừ | 
| 3 | - | - | - | - | đầu ra 20 | 

Điều này xác nhận phép trừ số bình thường khi cả hai đầu vào đều là số nguyên hợp lệ. 

### Ví dụ 2 

đầu vào:`a 2`| Bước | x | y | Đã phân tích cú pháp x | Đã phân tích y | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | "một" | "2" | NaN | 2 | phát hiện x không hợp lệ | 
| 2 | - | - | - | - | ngắn mạch | 
| 3 | - | - | - | - | đầu ra NaN | 

Điều này cho thấy cách một toán hạng không hợp lệ lan truyền trực tiếp đến kết quả cuối cùng mà không cần bất kỳ phép tính nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) | Mỗi chuỗi được quét một lần để xác minh cấu trúc chỉ có chữ số | 
| Không gian | O(1) | Chỉ có hai số nguyên hoặc không có giá trị nào được lưu trữ | 

Cho rằng mỗi chuỗi có độ dài tối đa là 5, thời gian chạy thực sự không đổi và phù hợp một cách tầm thường trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def parse(s: str):
        if s.isdigit():
            return int(s)
        return None

    x, y = input().split()
    a = parse(x)
    b = parse(y)

    if a is None or b is None:
        return "NaN"
    return str(a - b)

# provided samples
assert run("22 2\n") == "20"
assert run("a 2\n") == "NaN"

# custom cases
assert run("123 0\n") == "123"
assert run("0 123\n") == "-123"
assert run("12a 3\n") == "NaN"
assert run("99999 1\n") == "99998"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 123 0 | 123 | hành vi trừ 0 | 
| 0 123 | -123 | xử lý kết quả âm tính | 
| 12a 3 | NaN | phát hiện chữ số không hợp lệ | 
| 99999 1 | 99998 | độ chính xác của phép trừ giới hạn trên | 

## Vỏ cạnh 

Trường hợp cạnh chính là các chuỗi chữ và số hỗn hợp trong đó chỉ có một toán hạng không hợp lệ. Đối với đầu vào như`12a 3`, trình phân tích cú pháp cho chuỗi đầu tiên ngay lập tức không xác thực được chữ số, do đó giá trị trở thành`NaN`. Thuật toán không tiến hành phép trừ, khớp với hành vi của JavaScript khi có bất kỳ`NaN`toán hạng làm ô nhiễm kết quả. 

Một trường hợp cạnh khác là các giá trị 0 thuần túy như`0 0`. Cả hai chuỗi đều vượt qua`.isdigit()`, do đó chúng được chuyển đổi thành số nguyên và phép trừ tạo ra`0`, in dưới dạng số nguyên bình thường mà không gặp vấn đề về định dạng. 

Trường hợp cạnh cuối cùng là các chuỗi số có độ dài tối đa, chẳng hạn như`99999 1`. Cả hai đều là số nguyên hợp lệ và phép trừ rất đơn giản vì Python xử lý các số nguyên nhỏ mà không lo tràn, tạo ra kết quả đầu ra chính xác một cách trực tiếp.
