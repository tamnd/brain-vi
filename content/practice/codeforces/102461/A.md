---
title: "CF 102461A - Định dạng biểu thức"
description: "Đầu vào là một biểu thức số học hợp lệ được viết không có dấu cách. Toán hạng của nó là các chữ cái viết thường và biểu thức có thể chứa các toán tử nhị phân +, -, , / cùng với dấu ngoặc đơn."
date: "2026-08-09T03:06:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102461
codeforces_index: "A"
codeforces_contest_name: "Innopolis Open 2019-2020, qualification, contest 2"
rating: 0
weight: 102461
solve_time_s: 129
verified: true
draft: false
---

[CF 102461A - Định dạng biểu thức](https://codeforces.com/problemset/problem/102461/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào là một biểu thức số học hợp lệ được viết không có dấu cách. Toán hạng của nó là các chữ cái viết thường và biểu thức có thể chứa các toán tử nhị phân`+`,`-`,`*`,`/`cùng với dấu ngoặc đơn. Nhiệm vụ này hoàn toàn mang tính thẩm mỹ: giữ nguyên biểu thức, nhưng đặt một khoảng trắng ngay trước và ngay sau mỗi toán tử số học. 

Ví dụ,`a+b*(c-d)`trở thành`a + b * (c - d)`. Bản thân các dấu ngoặc đơn không bị thay đổi và không có dấu cách nào được thêm vào xung quanh chúng. 

Biểu thức có tối đa 200 ký tự, do đó, ngay cả thuật toán bậc hai cũng chỉ thực hiện một số ít thao tác đối với các ràng buộc chính thức. Không cần phân tích cú pháp, xử lý độ ưu tiên của toán tử hoặc đánh giá biểu thức. Đầu vào đã được đảm bảo đúng về mặt cú pháp, có nghĩa là mọi`+`,`-`,`*`, Và`/`mà chúng tôi gặp phải là một toán tử cần định dạng. 

Quét tuyến tính là giải pháp tự nhiên vì mỗi ký tự cần được kiểm tra một lần. Với tối đa 200 ký tự, điều này thoải mái nằm trong giới hạn một giây và sử dụng bộ nhớ không đáng kể. Thậm chí một`O(n^2)`việc xây dựng sẽ vượt qua những ràng buộc cụ thể này, nhưng không có lý do gì để sử dụng nó khi phép biến đổi cần thiết có thể được thực hiện trực tiếp trong`O(n)`. 

Trường hợp cạnh đầu tiên là một biểu thức không chứa toán tử. Đối với đầu vào`a`, đầu ra đúng là`a`. Việc triển khai bất cẩn luôn chèn dấu cách ở cuối biểu thức sẽ tạo ra kết quả như`a`, thay đổi các ký tự không được phép sửa đổi. 

Trường hợp cạnh thứ hai là toán tử bên cạnh dấu ngoặc đơn. Đối với đầu vào`(a)/(b)`, đầu ra đúng là`(a) / (b)`. Các không gian thuộc về xung quanh`/`, không ở xung quanh`(`hoặc`)`. Việc triển khai coi mọi ký tự không phải chữ cái là thứ gì đó yêu cầu khoảng trắng xung quanh sẽ sửa đổi dấu ngoặc đơn không chính xác. 

Trường hợp cạnh thứ ba là một biểu thức được lồng sâu. Đối với đầu vào`((a))-b+(c*(d))`, đầu ra đúng là`((a)) - b + (c * (d))`. Dấu ngoặc đơn có thể xuất hiện ngay trước hoặc sau toán tử, do đó việc triển khai phải quyết định dựa trên chính ký tự đó chứ không phải vị trí của nó trong biểu thức. 

Trường hợp cạnh thứ tư là một số toán tử được phân tách bằng các biến một chữ cái. Đối với đầu vào`a+b+c+d`, đầu ra đúng là`a + b + c + d`. Một giải pháp chỉ xử lý toán tử đầu tiên hoặc chèn dấu cách vào chuỗi gốc trong khi di chuyển chỉ mục về phía trước mà không tính đến các ký tự được chèn, có thể bỏ qua các toán tử sau. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đơn giản có thể liên tục tìm kiếm một toán tử và xây dựng một chuỗi mới với các khoảng trắng xung quanh nó. Nếu biểu thức hiện tại có độ dài`m`, chèn dấu cách bằng cách xây dựng lại toàn bộ chuỗi có thể sao chép`O(m)`nhân vật. Nếu có`k`các toán tử, độ dài sẽ tăng sau mỗi lần chèn, do đó tổng công việc là`O(nk)`, đó là`O(n^2)`trong trường hợp xấu nhất. Đối với biểu thức có độ dài thực tế tối đa ở đây, khoảng 200 ký tự, đây chỉ là thứ tự của hàng chục nghìn thao tác ký tự, vì vậy cách tiếp cận này thực sự đủ nhanh cho các ràng buộc nhất định. 

Cách tiếp cận bạo lực có hiệu quả vì mọi người vận hành đều có thể được xử lý độc lập. Điểm yếu của nó không phải là tính đúng đắn mà là việc lặp đi lặp lại không cần thiết. Sau khi xử lý một toán tử, nó có thể sao chép nhiều ký tự đã được xử lý chỉ để chuyển sang toán tử tiếp theo. 

Quan sát quan trọng là đầu ra mong muốn chỉ phụ thuộc vào ký tự đầu vào hiện tại. Nếu nhân vật là một trong`+`,`-`,`*`, hoặc`/`, chúng ta thêm một khoảng trắng, sau đó là toán tử, rồi một khoảng trắng khác. Mọi ký tự khác đều được sao chép không thay đổi. Không có sự tương tác giữa các toán tử khác nhau và việc chèn khoảng trắng không thay đổi cách diễn giải bất kỳ ký tự đầu vào nào sau này. 

Điều này cho phép chúng tôi xử lý biểu thức gốc từ trái sang phải chính xác một lần. Chúng tôi không bao giờ sửa đổi chuỗi đầu vào và không bao giờ cần điều chỉnh chỉ mục vì khoảng trắng được chèn vào. Kết quả được xây dựng riêng biệt nên mỗi ký tự đầu vào được xử lý theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^2)`|`O(n)`| Được chấp nhận cho`n <= 200`| 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu thức đầy đủ dưới dạng một chuỗi. Vì câu lệnh đảm bảo rằng đầu vào không chứa dấu cách nên chỉ cần đọc một dòng là đủ. 
2. Tạo một danh sách trống để lưu trữ các phần của biểu thức cuối cùng. Một danh sách rất tiện lợi vì việc thêm vào nó là thời gian không đổi và việc nối nó một lần vào cuối sẽ tránh việc liên tục tạo ra các chuỗi Python bất biến mới. 
3. Quét đầu vào từ ký tự đầu tiên đến ký tự cuối cùng. Đối với mỗi ký tự, hãy kiểm tra xem nó có phải là một trong bốn toán tử số học hay không. 
4. Nếu ký tự là toán tử, hãy thêm ba phần vào kết quả: một khoảng trắng, chính toán tử đó và một khoảng trắng khác. Điều này trực tiếp thực hiện quy tắc định dạng được yêu cầu. 
5. Nếu ký tự không phải là toán tử, hãy thêm nó không thay đổi. Đặc biệt, dấu ngoặc đơn và các biến phải giữ nguyên vị trí của chúng. 
6. Nối tất cả các phần kết quả thành một chuỗi và in nó. Vì đầu vào không chứa khoảng trắng hiện có nên điều này tạo ra chính xác một khoảng trắng ở mỗi bên của mỗi toán tử và không có khoảng trống nào khác. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của đầu vào, danh sách kết quả chứa chính xác phiên bản được định dạng chính xác của tiền tố đó. Một biến hoặc dấu ngoặc đơn được sao chép không thay đổi, trong khi mọi toán tử số học được thay thế bằng toán tử được bao quanh bởi đúng một khoảng trắng. Việc xử lý ký tự tiếp theo sẽ giữ nguyên thuộc tính này, do đó, bằng cảm ứng, toàn bộ biểu thức được định dạng chính xác khi quá trình quét kết thúc. Vì mọi quyết định chỉ dựa trên ký tự đầu vào hiện tại nên không toán tử nào có thể bị bỏ qua hoặc xử lý hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def format_expression(s):
    operators = {"+", "-", "*", "/"}
    result = []

    for ch in s:
        if ch in operators:
            result.append(" ")
            result.append(ch)
            result.append(" ")
        else:
            result.append(ch)

    return "".join(result)

def main():
    s = input().strip()
    print(format_expression(s))

if __name__ == "__main__":
    main()
```các`operators`tập hợp chứa chính xác bốn ký tự cần định dạng. Việc kiểm tra tư cách thành viên trong bộ này mất thời gian không đổi cho mỗi ký tự đầu vào. 

các`result`list đại diện cho đầu ra đang được xây dựng. Khi tìm thấy một toán tử, mã sẽ nối thêm ba chuỗi riêng biệt thay vì sửa đổi chuỗi đầu vào. Đây là một chi tiết triển khai hữu ích trong Python vì các chuỗi là bất biến, do đó, việc chèn liên tục vào một chuỗi có thể yêu cầu sao chép một phần lớn chuỗi đó mỗi lần. 

Vòng lặp xử lý đầu vào ban đầu chứ không phải đầu ra đang tăng lên. Sự khác biệt đó giúp loại bỏ lỗi lập chỉ mục phổ biến nhất trong vấn đề này. Nếu chúng tôi chèn dấu cách vào chuỗi đang được quét, vị trí hiện tại sẽ thay đổi sau mỗi toán tử. Ở đây, đầu vào không bao giờ thay đổi, do đó chỉ số vòng lặp luôn đề cập đến ký tự gốc chính xác. 

trận chung kết`"".join(result)`thực hiện một cấu trúc của chuỗi đầu ra. Không có số học số nguyên, do đó việc tràn số nguyên là không liên quan và độ dài đầu vào nhỏ đến mức I/O Python tiêu chuẩn là quá đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`a+b`, quá trình quét hoạt động như sau. 

| Bước | Ký tự đầu vào | Hành động | Phần kết quả | 
| --- | --- | --- | --- | 
| 1 |`a`| Sao chép không thay đổi |`a`| 
| 2 |`+`| Thêm không gian,`+`, không gian |`a`,` `,`+`,` `| 
| 3 |`b`| Sao chép không thay đổi |`a`,` `,`+`,` `,`b`| 

Việc nối các mảnh mang lại`a + b`. Dấu vết thể hiện tính bất biến cốt lõi: các ký tự thông thường được sao chép chính xác, trong khi toán tử đóng góp chính xác hai khoảng trắng cần thiết. 

### Mẫu 2 

Đối với đầu vào`((a))-b+(c*(d))`, quá trình quét sẽ dài hơn nhưng tuân theo chính xác quy tắc tương tự. 

| Bước | Ký tự đầu vào | Hành động | Tiền tố kết quả | 
| --- | --- | --- | --- | 
| 1 |`(`| Sao chép không thay đổi |`(`| 
| 2 |`(`| Sao chép không thay đổi |`((`| 
| 3 |`a`| Sao chép không thay đổi |`((a`| 
| 4 |`)`| Sao chép không thay đổi |`((a)`| 
| 5 |`)`| Sao chép không thay đổi |`((a))`| 
| 6 |`-`| Thêm dấu cách xung quanh toán tử |`((a)) - `| 
| 7 |`b`| Sao chép không thay đổi |`((a)) - b`| 
| 8 |`+`| Thêm dấu cách xung quanh toán tử |`((a)) - b + `| 
| 9 |`(`| Sao chép không thay đổi |`((a)) - b + (`| 
| 10 |`c`| Sao chép không thay đổi |`((a)) - b + (c`| 
| 11 |`*`| Thêm dấu cách xung quanh toán tử |`((a)) - b + (c * `| 
| 12 |`(`| Sao chép không thay đổi |`((a)) - b + (c * (`| 
| 13 |`d`| Sao chép không thay đổi |`((a)) - b + (c * (d`| 
| 14 |`)`| Sao chép không thay đổi |`((a)) - b + (c * (d)`| 
| 15 |`)`| Sao chép không thay đổi |`((a)) - b + (c * (d))`| 

Đầu ra cuối cùng là`((a)) - b + (c * (d))`. Ví dụ này xác nhận cụ thể rằng dấu ngoặc đơn không bao giờ được sửa đổi, ngay cả khi chúng ở gần một toán tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi ký tự đầu vào được kiểm tra một lần và đóng góp một lượng công việc không đổi. | 
| Không gian |`O(n)`| Kết quả chứa các ký tự gốc cộng với tối đa hai khoảng trắng cho mỗi toán tử. | 

Đây`n`là độ dài của biểu thức đầu vào, với`n <= 200`. Một đường truyền tuyến tính duy nhất thấp hơn nhiều so với giới hạn thời gian có sẵn và bản thân đầu ra chỉ`O(n)`ký tự nên việc sử dụng bộ nhớ cũng không đáng kể so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def format_expression(s):
    operators = {"+", "-", "*", "/"}
    result = []

    for ch in s:
        if ch in operators:
            result.append(" ")
            result.append(ch)
            result.append(" ")
        else:
            result.append(ch)

    return "".join(result)

def run(inp: str) -> str:
    return format_expression(inp.strip())

# Provided samples
assert run("a+b") == "a + b", "sample 1"
assert run("((a))-b+(c*(d))") == "((a)) - b + (c * (d))", "sample 2"
assert run("(a)/(b-b)+((d)+((c)))") == "(a) / (b - b) + ((d) + ((c)))", "sample 3"

# Minimum-size input: a single variable, no operators.
assert run("a") == "a", "single variable"

# All variables are equal and many operators occur.
assert run("a+a+a+a+a") == "a + a + a + a + a", "repeated equal variables"

# Every supported operator occurs.
assert run("a-b*c/d+e") == "a - b * c / d + e", "all operators"

# Operators adjacent to parentheses.
assert run("(a)/(b)+(c)*(d)") == "(a) / (b) + (c) * (d)", "parentheses boundaries"

# Maximum-length valid expression for this grammar.
# 100 variables and 99 plus operators give length 199.
max_expr = "+".join(["a"] * 100)
expected_max = " + ".join(["a"] * 100)
assert len(max_expr) == 199
assert run(max_expr) == expected_max, "maximum length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Đầu vào có kích thước tối thiểu và sự vắng mặt của các toán tử | 
|`a+a+a+a+a`|`a + a + a + a + a`| Toán tử lặp lại và toán hạng bằng nhau | 
|`a-b*c/d+e`|`a - b * c / d + e`| Tất cả bốn toán tử được hỗ trợ | 
|`(a)/(b)+(c)*(d)`|`(a) / (b) + (c) * (d)`| Toán tử bên cạnh dấu ngoặc đơn | 
| 100`a`các biến được nối bởi`+`| Biểu thức tương tự với khoảng trắng xung quanh mỗi`+`| Độ dài đầu vào tối đa và xử lý lặp lại | 

## Vỏ cạnh 

Đối với biểu thức`a`, thuật toán chỉ nhìn thấy một ký tự. Từ`a`không có trong tập toán tử, nó được sao chép trực tiếp vào kết quả. Đầu ra cuối cùng là`a`, do đó giải pháp không tạo ra các khoảng trắng không mong muốn khi không cần định dạng. 

Vì`(a)/(b)`, ba ký tự đầu tiên là`(`,`a`, Và`)`, tất cả đều được sao chép không thay đổi. Khi`/`đạt được, thuật toán sẽ nối thêm`/`như một phép toán logic. Các ký tự còn lại được sao chép không thay đổi, tạo ra`(a) / (b)`. Điều này xử lý ranh giới giữa toán tử và dấu ngoặc đơn mà không coi chính dấu ngoặc đơn là toán tử. 

Vì`((a))-b+(c*(d))`, mọi dấu ngoặc đơn mở và đóng đều được sao chép chính xác như khi nó xuất hiện. Khi`-`,`+`, Và`*`gặp thì mỗi người nhận được một khoảng trống ở cả hai bên. Kết quả là`((a)) - b + (c * (d))`. Cấu trúc lồng nhau không ảnh hưởng đến thuật toán vì tác vụ không yêu cầu phân tích biểu thức. 

Vì`a+a+a+a+a`, mỗi`+`được xử lý độc lập. Sau toán tử đầu tiên, kết quả chứa`a + `, sau giây nó chứa`a + a + `, vân vân. Đầu ra cuối cùng là`a + a + a + a + a`. Điều này xác nhận rằng quá trình quét tiếp tục trên mọi ký tự gốc và không bao giờ mất vị trí do khoảng trắng được thêm vào kết quả riêng biệt. 

Đối với biểu thức có độ dài tối đa bao gồm 100`a`các biến được nối bởi 99`+`toán tử, quá trình quét sẽ thực hiện một quyết định liên tục theo thời gian cho mỗi ký tự trong số 199 ký tự đầu vào. Mỗi toán tử trong số 99 toán tử tạo ra chính xác hai khoảng trắng, do đó kết quả đầu ra có 397 ký tự. Thuật toán vẫn tuyến tính ở kích thước đầu vào và xử lý biểu thức hợp lệ lớn nhất một cách thoải mái.
