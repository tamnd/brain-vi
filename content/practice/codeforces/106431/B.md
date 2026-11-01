---
title: "CF 106431B - Bigint"
description: "Nhiệm vụ là thực hiện số học trên các số nguyên quá lớn để phù hợp với các kiểu số nguyên thông thường có sẵn trong hầu hết các ngôn ngữ lập trình. Đầu vào chứa hai số nguyên không âm được viết dưới dạng chuỗi thập phân và đầu ra là tổng của chúng được viết dưới dạng thập phân."
date: "2026-06-25T09:38:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106431
codeforces_index: "B"
codeforces_contest_name: "Entrenamiento OIE Nivel Experto - Semana 12"
rating: 0
weight: 106431
solve_time_s: 36
verified: true
draft: false
---

[CF 106431B - Bigint](https://codeforces.com/problemset/problem/106431/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là thực hiện số học trên các số nguyên quá lớn để phù hợp với các kiểu số nguyên thông thường có sẵn trong hầu hết các ngôn ngữ lập trình. Đầu vào chứa hai số nguyên không âm được viết dưới dạng chuỗi thập phân và đầu ra là tổng của chúng được viết dưới dạng thập phân. 

Các chuỗi có thể chứa nhiều chữ số, vì vậy việc chuyển đổi chúng thành các kiểu số nguyên tích hợp không phải là một cách tiếp cận hợp lệ. Một số nguyên 64 bit chỉ có thể lưu trữ các giá trị tối đa khoảng 19 chữ số thập phân, trong khi một bài toán số nguyên lớn được thiết kế sao cho số chữ số có thể lớn hơn nhiều. 

Kích thước của đầu vào xác định thuật toán. Nếu các số chứa`n`Và`m`chữ số, đọc chúng đã mất phí rồi`O(n + m)`thời gian, vì vậy mọi giải pháp được chấp nhận đều phải hoạt động theo cùng một mức độ. Một phương pháp cố gắng chuyển đổi toàn bộ số nhiều lần hoặc thực hiện các phép tính cho từng cặp chữ số sẽ trở nên quá chậm khi số lượng chữ số tăng lên. 

Các trường hợp khó khăn chính xuất phát từ cách hoạt động của phép cộng thủ công. Một sai lầm phổ biến là quên mang theo cuối cùng. Ví dụ, đầu vào```
9
1
```sản xuất```
10
```vì việc cộng các chữ số cuối cùng sẽ tạo ra một chữ số mới. 

Một sai lầm khác là giả sử cả hai số có cùng độ dài. Ví dụ,```
999
1
```phải sản xuất```
1000
```Số ngắn hơn thiếu các chữ số đầu hoạt động giống như số 0 và số mang tiếp tục vượt quá phần cuối của chuỗi ngắn hơn. 

Việc triển khai bất cẩn cũng có thể xử lý sai các giá trị 0. Ví dụ,```
0
0
```nên xuất ra```
0
```Câu trả lời không được chứa các số 0 ở đầu không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là dựa vào kiểu số nguyên của ngôn ngữ và phân tích chuỗi đầu vào thành số. Điều này chỉ hoạt động khi các giá trị đủ nhỏ, vì các kiểu số nguyên thông thường có số bit cố định. Khi đầu vào chứa nhiều chữ số hơn loại có thể lưu trữ, quá trình chuyển đổi sẽ bị tràn hoặc không thành công trước khi phép cộng thực sự diễn ra. 

Một mô phỏng thủ công là cần thiết. Phép cộng thập phân có một thuộc tính cục bộ hữu ích: chữ số ở một vị trí chỉ phụ thuộc vào hai chữ số ở vị trí đó và số mang đến từ vị trí trước đó. Điều này có nghĩa là toàn bộ số không cần phải được biểu diễn dưới dạng một số nguyên máy. 

Ý tưởng là quá trình tương tự được sử dụng khi thêm số trên giấy. Bắt đầu từ các chữ số có ý nghĩa nhỏ nhất, thêm các chữ số tương ứng và số mang hiện tại, lưu trữ chữ số kết quả và chuyển số mang mới sang vị trí tiếp theo. Khi một số kết thúc, các chữ số bị thiếu được coi là 0. Sau khi tất cả các vị trí được xử lý, mọi số hạng còn lại sẽ trở thành chữ số hàng đầu mới. 

Lực lượng vũ phu và các phương pháp tối ưu thực sự giống nhau về mặt công việc toán học, nhưng phương pháp tối ưu tránh được bước chuyển đổi không thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force sử dụng số nguyên tích hợp | O(n) | O(n) | Thất bại khi số vượt quá giới hạn số nguyên | 
| Cộng chữ số thủ công | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cả hai số dưới dạng chuỗi thay vì số nguyên. Các chuỗi bảo toàn mọi chữ số và tránh tràn. 
2. Đặt con trỏ ở ký tự cuối cùng của cả hai chuỗi và khởi tạo giá trị mang về 0. Phép cộng bắt đầu từ bên phải vì số thập phân di chuyển từ vị trí thấp hơn đến vị trí cao hơn. 
3. Trong khi một trong hai con trỏ vẫn ở trong chuỗi của nó hoặc vẫn còn một ký tự mang, hãy lấy chữ số hiện tại từ mỗi số nếu nó tồn tại, nếu không thì sử dụng số 0. Thêm hai chữ số và mang. 
4. Lưu trữ chữ số cuối cùng của tổng làm chữ số kết quả hiện tại và cập nhật số mang bằng cách chia số nguyên cho mười. Điều này tách chữ số thuộc vị trí hiện tại khỏi giá trị được chuyển sang vị trí tiếp theo. 
5. Di chuyển cả hai con trỏ sang trái và tiếp tục cho đến khi mọi chữ số đều được xử lý. 
6. Các chữ số được tạo theo thứ tự ngược lại vì phép cộng bắt đầu từ cuối các số. Đảo ngược kết quả trước khi in. 

Tại sao nó hoạt động: 

Bất biến được duy trì trong suốt thuật toán là sau khi xử lý một vị trí, kết quả được lưu trữ chứa chính xác các chữ số chính xác cho tất cả các vị trí đã được xử lý và số mang chứa chính xác số tiền phải được thêm vào vị trí tiếp theo. Mỗi vị trí thập phân độc lập sau khi biết số mang đến của nó, do đó việc xử lý các vị trí từ phải sang trái sẽ tái tạo quy trình cộng thông thường. Sau vị trí cuối cùng, phần đóng góp còn thiếu duy nhất có thể là phần mang cuối cùng mà vòng lặp cũng xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def add_bigints(a, b):
    i = len(a) - 1
    j = len(b) - 1
    carry = 0
    result = []

    while i >= 0 or j >= 0 or carry:
        digit_a = ord(a[i]) - ord('0') if i >= 0 else 0
        digit_b = ord(b[j]) - ord('0') if j >= 0 else 0

        total = digit_a + digit_b + carry
        result.append(chr(ord('0') + total % 10))
        carry = total // 10

        i -= 1
        j -= 1

    result.reverse()
    answer = ''.join(result)

    answer = answer.lstrip('0')
    return answer if answer else "0"

def solve():
    data = sys.stdin.read().split()
    if len(data) < 2:
        return

    print(add_bigints(data[0], data[1]))

if __name__ == "__main__":
    solve()
```chức năng`add_bigints`thực hiện mô phỏng toàn bộ chữ số bằng chữ số. Các con trỏ`i`Và`j`đại diện cho chữ số hiện tại được thêm vào trong mỗi số đầu vào. Chúng di chuyển từ chữ số có nghĩa nhỏ nhất đến chữ số có nghĩa nhất, phù hợp với cách thức lan truyền tự nhiên. 

Việc trích xuất chữ số có điều kiện rất quan trọng vì các số có thể có độ dài khác nhau. Khi con trỏ di chuyển trước phần đầu của chuỗi, chữ số bị thiếu sẽ được coi là 0. 

Kết quả được xây dựng ngược vì chữ số được tính đầu tiên là chữ số hàng đơn vị. Đảo ngược ở cuối sẽ khôi phục thứ tự thập phân bình thường. Việc loại bỏ cuối cùng các số 0 đứng đầu sẽ xử lý các trường hợp trong đó biểu diễn đầu vào chứa các số 0 không cần thiết trong khi vẫn giữ được câu trả lời`0`có hiệu lực. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
123
456
```Việc bổ sung tiến hành như sau: 

| Vị trí từ phải | Chữ số từ số đầu tiên | Chữ số từ số thứ hai | Mang vào | Chữ số kết quả | Thực hiện | 
| --- | --- | --- | --- | --- | --- | 
| Những cái | 3 | 6 | 0 | 9 | 0 | 
| Hàng chục | 2 | 5 | 0 | 7 | 0 | 
| Hàng trăm | 1 | 4 | 0 | 5 | 0 | 

Các chữ số được tạo ra được tạo từ phải sang trái như`975`, sau đó đảo ngược để có được:```
579
```Điều này xác nhận rằng mọi vị trí đều được giải quyết chỉ bằng cách sử dụng hai chữ số của nó và số mang trước đó. 

### Ví dụ 2 

đầu vào:```
999
1
```| Vị trí từ phải | Chữ số từ số đầu tiên | Chữ số từ số thứ hai | Mang vào | Chữ số kết quả | Thực hiện | 
| --- | --- | --- | --- | --- | --- | 
| Những cái | 9 | 1 | 0 | 0 | 1 | 
| Hàng chục | 9 | 0 | 1 | 0 | 1 | 
| Hàng trăm | 9 | 0 | 1 | 0 | 1 | 
| Mang thêm | 0 | 0 | 1 | 1 | 0 | 

Các chữ số được tạo ra là`0001`theo thứ tự ngược lại, cho:```
1000
```Điều này chứng tỏ tại sao vòng lặp phải tiếp tục trong khi tồn tại một chuỗi nhớ, thậm chí sau khi cả hai chuỗi đầu vào đã cạn kiệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi chữ số của cả hai số đều được truy cập một lần | 
| Không gian | O(n + m) | Kết quả lưu trữ một chữ số cho mỗi vị trí được xử lý | 

Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi chữ số, do đó, nó chia tỷ lệ tuyến tính với kích thước đầu vào và hoạt động với các số lớn tùy ý chỉ bị giới hạn bởi bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def add_bigints(a, b):
    i = len(a) - 1
    j = len(b) - 1
    carry = 0
    result = []

    while i >= 0 or j >= 0 or carry:
        x = ord(a[i]) - ord('0') if i >= 0 else 0
        y = ord(b[j]) - ord('0') if j >= 0 else 0
        s = x + y + carry
        result.append(str(s % 10))
        carry = s // 10
        i -= 1
        j -= 1

    result.reverse()
    ans = ''.join(result).lstrip('0')
    return ans if ans else "0"

def run(inp: str) -> str:
    data = inp.split()
    return add_bigints(data[0], data[1]) + "\n"

assert run("123 456") == "579\n", "basic addition"
assert run("999 1") == "1000\n", "final carry"
assert run("0 0") == "0\n", "zero handling"
assert run("1 999999999999999999999999") == "1000000000000000000000000\n", "different lengths"
assert run("500000000000000000000 500000000000000000000") == "1000000000000000000000\n", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`123 456`|`579`| Bổ sung bình thường | 
|`999 1`|`1000`| Truyền bá qua tất cả các chữ số | 
|`0 0`|`0`| Xử lý số 0 và số 0 đứng đầu | 
|`1 999999999999999999999999`|`1000000000000000000000000`| Độ dài số khác nhau | 
|`500000000000000000000 500000000000000000000`|`1000000000000000000000`| Số lượng rất lớn | 

## Vỏ cạnh 

cho`9 + 1`, thuật toán xử lý các chữ số duy nhất trước tiên. Tổng là 10 nên nó lưu số 0 và mang một. Cả hai chuỗi đều đã hoàn thành, nhưng phần mang vẫn giữ cho vòng lặp chạy và tạo ra chữ số đứng đầu. Đầu ra trở thành`10`. 

Đối với các số có độ dài khác nhau như:```
999
1
```số thứ hai hết chữ số sau vị trí đầu tiên của nó. Thuật toán thay thế các chữ số bị thiếu bằng 0, do đó các phép cộng còn lại trở thành`9 + 0 + carry`. Điều này tiếp tục cho đến khi vật mang biến mất, tạo ra`1000`. 

Đối với giá trị bằng 0:```
0
0
```thuật toán tạo ra một chữ số kết quả bằng 0. Việc loại bỏ các số 0 đứng đầu sẽ tạo ra một chuỗi trống, vì vậy điều kiện cuối cùng sẽ thay thế nó bằng`"0"`. Điều này giữ cho sự đại diện hợp lệ.
