---
title: "CF 102416A - Palindrome"
description: "Chúng ta cần kiểm tra một tập hợp các số nguyên dương và đếm xem có bao nhiêu số có cùng dãy chữ số khi đọc từ trái sang phải và từ phải sang trái."
date: "2026-08-14T14:43:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102416
codeforces_index: "A"
codeforces_contest_name: "Edinburgh Competition 2019"
rating: 0
weight: 102416
solve_time_s: 87
verified: false
draft: false
---

[CF 102416A - Palindrome](https://codeforces.com/problemset/problem/102416/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần kiểm tra một tập hợp các số nguyên dương và đếm xem có bao nhiêu số có cùng dãy chữ số khi đọc từ trái sang phải và từ phải sang trái. Ví dụ,`1221`đủ điều kiện vì hai nửa của nó phản chiếu lẫn nhau, trong khi`1234`không phải vì chữ số đầu tiên và cuối cùng khác nhau. 

Giá trị đầu vào đầu tiên cho biết số lượng số nguyên,`n`. Đầu vào sau đây chứa những`n`số nguyên. Câu trả lời chỉ là một phép đếm: có bao nhiêu số nguyên trong số đó là các số palindromes. 

Số lượng giá trị nhiều nhất là 100 và mỗi giá trị có thể chứa tối đa 51 chữ số thập phân vì giới hạn trên là`10^50`. Các số có thể lớn hơn nhiều so với phạm vi số nguyên 64 bit thông thường, vì vậy việc coi chúng dưới dạng chuỗi là cách biểu diễn tự nhiên. Ngay cả việc quét trực tiếp từng chữ số ở đây cũng rất nhỏ. Trên tất cả các đầu vào có nhiều nhất`100 * 51 = 5100`các chữ số, do đó, mọi giải pháp tuyến tính theo số chữ số đều kết thúc một cách thoải mái trong giới hạn một giây. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bị sai một cách tinh tế. Một chữ số duy nhất luôn là một bảng màu. Ví dụ:```
1
7
```Đầu ra đúng là`1`. Việc triển khai bắt đầu bằng cách so sánh các cặp mà không xử lý chính xác phần giữa của số có thể vô tình từ chối trường hợp này. 

Giá trị lớn nhất có thể cũng đáng được chú ý:```
1
100000000000000000000000000000000000000000000000000
```Giá trị này là`10^50`, có 51 chữ số và không phải là một bảng màu, vì vậy kết quả đầu ra là`0`. Giải pháp lưu trữ giá trị theo kiểu số nguyên có chiều rộng cố định có thể tràn trước khi nó kiểm tra các chữ số. 

Một palindrome có thể có độ dài lẻ hoặc chẵn. Ví dụ:```
2
12321
1221
```Cả hai số đều là palindromes, vì vậy kết quả đầu ra là`2`. Một vòng lặp chỉ kiểm tra các cặp có tối đa một nửa cố định mà không xử lý chính xác cả hai trường hợp chẵn lẻ có thể gây ra lỗi riêng lẻ. 

Mô tả đầu vào trình bày các số sau`n`và mẫu đặt chúng trên các dòng riêng biệt. Đọc các mã thông báo được phân tách bằng khoảng trắng thay vì giả sử tất cả các số nằm trên chính xác một dòng sẽ xử lý cả hai bố cục một cách tự nhiên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra các chữ số từ cả hai đầu về phía giữa. Đối với một số như`74647`, so sánh chữ số đầu tiên và chữ số cuối cùng, sau đó là chữ số thứ hai và thứ tư. Nếu mọi cặp tương ứng đều khớp thì số đó là một bảng màu. Nếu có cặp nào khác nhau thì không phải vậy. Vì có nhiều nhất 51 chữ số trong một số nên điều này đòi hỏi nhiều nhất`ceil(51 / 2) = 26`so sánh chữ số trên mỗi số hoặc nhiều nhất là 2600 so sánh trên tất cả 100 số. Không có điểm nào mà phương pháp bạo lực này trở nên quá chậm dưới những ràng buộc thực tế. 

Việc triển khai đơn giản hơn sử dụng ý tưởng tương tự thông qua các thao tác chuỗi. Chuyển đổi mỗi số đầu vào thành một chuỗi`s`, xây dựng dạng đảo ngược của nó`s[::-1]`, và so sánh hai chuỗi. Một chuỗi bằng số nghịch đảo của nó một cách chính xác khi các chữ số của nó đọc giống hệt nhau theo cả hai hướng. Hoạt động cắt lát xử lý mỗi chữ số một lần, do đó độ phức tạp tiệm cận vẫn tuyến tính theo số chữ số. 

Điều quan trọng là bài toán không yêu cầu tính toán trên giá trị của một số. Chúng tôi chỉ quan tâm đến thứ tự các chữ số thập phân của nó. Vì một giá trị có thể chứa 51 chữ số nên việc biểu diễn nó dưới dạng chuỗi sẽ tránh tràn và cho phép truy cập trực tiếp vào thuộc tính mà chúng ta cần kiểm tra. 

Cách tiếp cận bạo lực và cách tiếp cận dây có cùng độ phức tạp tiệm cận. Ở đây, phiên bản chuỗi thích hợp hơn vì Python đã cung cấp thao tác đảo ngược ngắn gọn và đáng tin cậy, loại bỏ việc quản lý chỉ mục thủ công và giảm khả năng xảy ra lỗi từng cái một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(D) | O(1) thêm cho mỗi số | Đã chấp nhận | 
| Tối ưu | O(D) | O(D) mỗi số | Đã chấp nhận | 

Đây,`D`là tổng số chữ số thập phân trong tất cả các số đầu vào. Từ`D <= 5100`, cả hai cách tiếp cận đều đủ nhanh. 

## Hướng dẫn thuật toán 

1. Đọc`n`, số lượng các số nguyên cần kiểm tra. Các mã thông báo được phân tách bằng khoảng trắng còn lại là số thực tế nên việc sắp xếp dòng của chúng không quan trọng. 
2. Khởi tạo bộ đếm về 0. Bộ đếm này sẽ biểu thị có bao nhiêu số đầu vào đã được xác nhận là palindromes. 
3. Đối với mỗi số đầu vào, hãy giữ nó dưới dạng chuỗi thay vì chuyển đổi thành số nguyên. Điều này tránh mọi lo ngại về tràn và bảo toàn chuỗi chữ số chính xác. 
4. Đảo ngược chuỗi bằng cách sử dụng`s[::-1]`. Chuỗi kết quả chứa các chữ số giống nhau theo thứ tự ngược lại. 
5. So sánh chuỗi gốc với chuỗi đảo ngược của nó. Nếu chúng bằng nhau thì mỗi chữ số đều có cùng một chữ số ở phía đối diện, do đó hãy tăng bộ đếm palindrome. 
6. Suy cho cùng`n`số đã được xử lý, in bộ đếm. 

### Tại sao nó hoạt động 

Đối với bất kỳ chuỗi nào`s`, mặt sau của nó chứa các chữ số của`s`theo đúng thứ tự ngược lại. Sự bình đẳng`s == s[::-1]`giữ chính xác khi mọi vị trí có cùng chữ số với vị trí được phản chiếu của nó. Đó chính xác là định nghĩa của bảng màu số. Vì mọi số đầu vào đều được kiểm tra độc lập và bộ đếm được tăng chính xác cho những số thỏa mãn đặc tính này nên bộ đếm cuối cùng là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    tokens = sys.stdin.read().split()

    if not tokens:
        return

    n = int(tokens[0])
    numbers = tokens[1:1 + n]

    answer = 0

    for s in numbers:
        if s == s[::-1]:
            answer += 1

    print(answer)

if __name__ == "__main__":
    solve()
```Giải pháp này đọc tất cả các mã thông báo đầu vào được phân tách bằng khoảng trắng, giúp giải pháp này độc lập với việc các số xuất hiện trên một dòng hay nhiều dòng. Mã thông báo đầu tiên được chuyển đổi thành số nguyên vì nó kiểm soát số lượng số sẽ được xử lý. 

Mỗi số vẫn là một chuỗi. Đây là cố ý vì`10^50`nằm ngoài phạm vi của số nguyên có dấu 64 bit thông thường, trong khi chuỗi Python có thể biểu thị trực tiếp số nguyên đó mà không cần bất kỳ chuyển đổi số nào. 

biểu thức`s[::-1]`tạo ra chuỗi chữ số đảo ngược. Python xử lý các ranh giới lập chỉ mục nội bộ, do đó không cần tính toán điểm giữa thủ công và không cần xử lý riêng cho độ dài lẻ và chẵn. 

Điều kiện cuối cùng so sánh chuỗi gốc hoàn chỉnh và chuỗi đảo ngược. Chuỗi một chữ số sẽ tự động so sánh bằng với chuỗi đảo ngược của nó, trong khi chuỗi không phải palindrome sẽ không thành công ngay khi các chuỗi kết quả khác nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào chứa bốn số. Chúng tôi xử lý từng cái một cách độc lập. 

| Số | Đảo ngược | Palindrom | Trả lời sau khi xử lý | 
| --- | --- | --- | --- | 
|`3`|`3`| Có | 1 | 
|`546`|`645`| Không | 1 | 
|`74647`|`74647`| Có | 2 | 
|`74565`|`56547`| Không | 2 | 

Chữ số duy nhất`3`bằng với số nghịch đảo của nó, nên nó đóng góp một phần vào câu trả lời.`546`không thành công vì chữ số đầu tiên và chữ số cuối cùng của nó khác nhau.`74647`không thay đổi khi đảo ngược, vì vậy nó đóng góp một số đếm khác. Cuối cùng,`74565`trở thành`56547`, nên nó bị từ chối. Câu trả lời cuối cùng là`2`. 

### Ví dụ tùy chỉnh 

Hãy xem xét:```
5
1
1221
12321
1234
100000000000000000000000000000000000000000000000000
```Dấu vết là: 

| Số | Đảo ngược | Palindrom | Trả lời sau khi xử lý | 
| --- | --- | --- | --- | 
|`1`|`1`| Có | 1 | 
|`1221`|`1221`| Có | 2 | 
|`12321`|`12321`| Có | 3 | 
|`1234`|`4321`| Không | 3 | 
|`100000000000000000000000000000000000000000000000000`|`000000000000000000000000000000000000000000000000001`| Không | 3 | 

Ví dụ này bao gồm một số có một chữ số, một palindrome có độ dài chẵn, một palindrome có độ dài lẻ, một palindrome không phải palindrome thông thường và giá trị kích thước tối đa. Thuật toán xử lý tất cả chúng thông qua cùng một bài kiểm tra đẳng thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(D) | Mỗi chữ số đầu vào được xử lý một số lần không đổi trong khi đảo ngược và so sánh các chuỗi. | 
| Không gian | O(L) | Đảo ngược một số sẽ tạo ra một chuỗi chứa nhiều nhất`L <= 51`chữ số. | 

Đối với vấn đề này, tổng số chữ số nhiều nhất là 5100. Do đó, giải pháp chỉ thực hiện vài nghìn thao tác ký tự và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    tokens = inp.split()

    if not tokens:
        return ""

    n = int(tokens[0])
    numbers = tokens[1:1 + n]

    answer = sum(s == s[::-1] for s in numbers)
    return str(answer)

def run(inp: str) -> str:
    return solve_data(inp).strip()

assert run("""4
3
546
74647
74565
""") == "2", "sample 1"

assert run("""5
1
11
12
121
122
""") == "3", "single digit and short boundary cases"

assert run("""6
1221
12321
1234
9999
1001
101
""") == "5", "odd and even length palindromes"

assert run("""3
100000000000000000000000000000000000000000000000000
999999999999999999999999999999999999999999999999999
123456789012345678901234567890123456789012345678901
""") == "1", "maximum-size values"

assert run("""4
7
8
9
0
""") == "4", "all one-digit values"

| Test input | Expected output | What it validates |
|---|---|---|
| `1, 11, 12, 121, 122` | `3` | Single digits, two-digit values, and odd-length palindromes |
| `1221, 12321, 1234, 9999, 1001, 101` | `5` | Both even and odd palindrome lengths plus a non-palindrome |
| Three 51-digit values | `1` | Maximum allowed digit length and values beyond fixed-width integer ranges |
| `7, 8, 9, 0` | `4` | Minimum-size numbers and the fact that every one-digit string is a palindrome |

## Edge Cases

A one-digit number such as `7` has no distinct pair of digits to compare. Reversing it gives the same string:

```văn bản 
1 
7```

The algorithm computes `s = "7"` and `s[::-1] = "7"`, so the equality test succeeds and the output is `1`. This avoids needing a special case for the middle digit.

For the maximum possible value, consider:

```1 
1000000000000000000000000000000000000000000000000000000```

The string has 51 digits. Its first digit is `1`, while its last digit is `0`, so the reversed string cannot equal the original. The algorithm produces `0`. Since the value is never converted to a machine integer, there is no overflow issue.

For an even-length palindrome:

```1 
1221```

The reverse of `1221` is also `1221`, so the answer is `1`. The algorithm does not need to identify a center or decide whether the length is odd or even. String reversal handles both cases uniformly.

For an odd-length palindrome:

```1 
12321```

The reverse is again `12321`, producing `1`. The middle digit `3` naturally maps to itself. A manual two-pointer solution would stop once the pointers meet, but the string-based solution avoids that boundary entirely.

Finally, consider a number with a mismatch close to the center:

```1 
12331 
``` 

Ngược lại của nó là`13321`, vì vậy đầu ra là`0`. Các chữ số bên ngoài khớp nhau và cặp tiếp theo cũng khớp nhau, nhưng cặp bên trong thì khác. So sánh chuỗi hoàn chỉnh với chuỗi đảo ngược của nó sẽ bắt không khớp ở mọi vị trí có thể, kể cả những vị trí mà việc so sánh từng phần bất cẩn có thể bỏ sót.
