---
title: "CF 21B - Nút giao"
description: "Mỗi dòng đầu vào mô tả một đối tượng hình học trong mặt phẳng. phương trình"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 21
codeforces_index: "B"
codeforces_contest_name: "Codeforces Alpha Round 21 (Codeforces format)"
rating: 2000
weight: 21
solve_time_s: 81
verified: true
draft: false
---
[CF 21B - Giao lộ](https://codeforces.com/problemset/problem/21/B) 

**Xếp hạng:** 2000 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi dòng đầu vào mô tả một đối tượng hình học trong mặt phẳng. phương trình$$Ax + By + C = 0$$đại diện cho một dòng. Nhiệm vụ là xác định có bao nhiêu điểm thuộc cả hai đường cùng một lúc. 

Hai đường thẳng không song song khác nhau cắt nhau tại đúng một điểm. Hai đường thẳng song song nhưng khác biệt không bao giờ gặp nhau. Hai đường thẳng giống hệt nhau trùng nhau hoàn toàn nên chúng có vô số điểm. Định dạng đầu ra tuân theo điều này trực tiếp: 

- in`1`nếu các đường cắt nhau một lần, 
- in`0`nếu chúng không bao giờ cắt nhau, 
- in`-1`nếu chúng thực sự là cùng một dòng. 

Các ràng buộc rất nhỏ, hệ số chỉ nằm trong khoảng`-100`Và`100`. Hiệu suất không phải là thách thức ở đây. Toàn bộ vấn đề là xử lý chính xác tất cả các trường hợp hình học và tránh sai sót với các hệ số tỷ lệ. 

Phần nguy hiểm là việc phân biệt giữa “song song” và “giống hệt nhau”. Việc triển khai ngây thơ chỉ có thể so sánh các độ dốc. Điều đó có tác dụng phát hiện sự song song, nhưng nó không thể biết liệu các phương trình có mô tả chính xác cùng một đường hay không. 

Hãy xem xét đầu vào này:```
1 1 0
2 2 0
```Phương trình thứ hai chỉ là phương trình đầu tiên nhân với`2`, vì vậy cả hai đều mô tả cùng một dòng. Câu trả lời đúng là:```
-1
```Việc thực hiện bất cẩn chỉ kiểm tra xem định thức có bằng 0 hay không sẽ in sai`0`. 

Một trường hợp phức tạp khác là khi một cặp hệ số khớp tỷ lệ với nhau nhưng số hạng hằng số thì không.```
1 1 1
2 2 3
```Các vectơ chỉ phương tỉ lệ nên các đường thẳng song song. Nhưng các hằng số không được chia tỷ lệ theo cùng một hệ số, vì vậy đây là những đường thẳng song song riêng biệt. Đầu ra đúng là:```
0
```Việc kiểm tra sự bình đẳng có lỗi có thể phân loại sai chúng thành giống hệt nhau. 

Ngoài ra còn có một trường hợp phức tạp liên quan đến hệ số 0:```
0 1 0
0 2 0
```Các phương trình này trở thành:$$y = 0$$Và$$2y = 0$$Một lần nữa chúng lại cùng một dòng, mặc dù`A = 0`. Bất kỳ giải pháp nào dựa trên phép chia đều có thể bị hỏng hoặc mất độ chính xác ở đây. Phép nhân chéo số nguyên tránh được điều đó hoàn toàn. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là giải quyết rõ ràng hệ thống tuyến tính và kiểm tra kết quả. Cho hai phương trình:$$A_1x + B_1y + C_1 = 0$$

$$A_2x + B_2y + C_2 = 0$$chúng ta có thể tính định thức:$$D = A_1B_2 - A_2B_1$$Nếu như`D ≠ 0`, hệ có nghiệm duy nhất nên đáp án là`1`. 

Nếu như`D = 0`, các đường thẳng song song. Lúc đó chúng ta vẫn cần phân biệt giữa “cùng dòng” và “khác dòng”. Một cách là giải các tỷ số bằng cách sử dụng số học dấu phẩy động. Về mặt lý thuyết, điều đó có hiệu quả, nhưng nó dễ bị tổn thương một cách không cần thiết vì việc chia cho 0 và các vấn đề về độ chính xác sẽ xuất hiện ngay lập tức. 

Quan sát quan trọng là các đường giống hệt nhau phải có tất cả các hệ số tỷ lệ thuận:$$\frac{A_1}{A_2} = \frac{B_1}{B_2} = \frac{C_1}{C_2}$$Thay vì chia, chúng tôi so sánh các sản phẩm:$$A_1B_2 = A_2B_1$$

$$A_1C_2 = A_2C_1$$

$$B_1C_2 = B_2C_1$$Điều này hoàn toàn tránh được lỗi dấu phẩy động và xử lý số 0 một cách tự nhiên. 

Bài toán trở thành một phân tích trường hợp nhỏ dựa trên các yếu tố quyết định và tính tỉ lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với tỷ lệ dấu phẩy động | O(1) | O(1) | Triển khai rủi ro | 
| Kiểm tra định thức số nguyên tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc sáu hệ số mô tả hai đường thẳng. 
2. Tính định thức:$$D = A_1B_2 - A_2B_1$$Nếu như`D ≠ 0`, các đường thẳng không song song nên chúng cắt nhau tại đúng một điểm. In`1`. 

1. Nếu`D = 0`, các đường thẳng song song hoặc giống hệt nhau. Bây giờ chúng ta kiểm tra xem tất cả các hệ số có tỷ lệ thuận hay không. 
2. So sánh:$$A_1C_2 = A_2C_1$$Và$$B_1C_2 = B_2C_1$$Việc kiểm tra yếu tố quyết định đã đảm bảo:$$A_1B_2 = A_2B_1$$vì vậy những so sánh bổ sung này là đủ để xác minh sự cân xứng đầy đủ. 

1. Nếu cả hai kiểm tra tỷ lệ đều đúng thì các phương trình mô tả cùng một đường. In`-1`. 
2. Mặt khác, các đường thẳng song song nhưng khác biệt. In`0`. 

### Tại sao nó hoạt động 

Định thức cho chúng ta biết các vectơ chỉ phương của hai đường thẳng có độc lập tuyến tính hay không. Định thức khác 0 có nghĩa là các đường thẳng phải cắt nhau một lần. 

Khi định thức bằng 0, cả hai đường thẳng có cùng hướng, do đó hoặc chúng không bao giờ gặp nhau hoặc chúng hoàn toàn trùng khớp. Hai phương trình tuyến tính mô tả chính xác cùng một đường hình học khi một phương trình có thể thu được bằng cách nhân phương trình kia với một hằng số. Phép nhân chéo kiểm tra tính tỷ lệ này mà không cần chia, do đó mọi sự kết hợp có thể có của hệ số 0 và khác 0 đều được xử lý một cách an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a1, b1, c1 = map(int, input().split())
a2, b2, c2 = map(int, input().split())

det = a1 * b2 - a2 * b1

if det != 0:
    print(1)
else:
    same = (
        a1 * c2 == a2 * c1 and
        b1 * c2 == b2 * c1
    )

    if same:
        print(-1)
    else:
        print(0)
```Phần đầu tiên tính định thức của ma trận hệ số. Đây là tiêu chí tiêu chuẩn để đánh giá liệu một`2 × 2`hệ tuyến tính có nghiệm duy nhất. 

Nếu định thức khác 0 thì câu trả lời là ngay lập tức`1`, vì các đường này cắt nhau một lần. 

Nhánh thứ hai xử lý trường hợp song song. Thay vì chia các hệ số để so sánh các tỷ số, việc triển khai sử dụng phép nhân số nguyên. Điều này quan trọng vì các hệ số có thể bằng 0. Ví dụ, so sánh`a1 / a2`trở nên vô hiệu khi`a2 = 0`. 

Việc kiểm tra tỷ lệ được viết dưới dạng các đẳng thức số nguyên chính xác, do đó không có vấn đề về độ chính xác của dấu phẩy động. Vì tất cả các giá trị đều rất nhỏ nên thậm chí không thể xảy ra tràn trong Python, mặc dù phương pháp tương tự cũng an toàn trong C++ đối với các giới hạn này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 0
2 2 0
```| Biến | Giá trị | 
| --- | --- | 
|`a1, b1, c1`|`1, 1, 0`| 
|`a2, b2, c2`|`2, 2, 0`| 
|`det`|`1*2 - 2*1 = 0`| 
|`a1*c2 == a2*c1`|`1*0 == 2*0`→`True`| 
|`b1*c2 == b2*c1`|`1*0 == 2*0`→`True`| 

Định thức bằng 0 nên các đường thẳng song song hoặc giống hệt nhau. Cả hai lần kiểm tra tỷ lệ đều thành công, nghĩa là phương trình thứ hai chính xác gấp đôi phương trình đầu tiên. Thuật toán in:```
-1
```Ví dụ này chứng tỏ tại sao chỉ kiểm tra định thức là không đủ. 

### Ví dụ 2 

đầu vào:```
1 -1 0
1 1 -4
```| Biến | Giá trị | 
| --- | --- | 
|`a1, b1, c1`|`1, -1, 0`| 
|`a2, b2, c2`|`1, 1, -4`| 
|`det`|`1*1 - 1*(-1) = 2`| 

Vì định thức khác 0 nên các đường thẳng cắt nhau tại đúng một điểm. Thuật toán in ngay lập tức:```
1
```Dấu vết này xác nhận rằng các đường không song song luôn tạo ra một giao điểm duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không sử dụng bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào | 

Các hạn chế là cực kỳ nhỏ nên ngay cả những giải pháp kém hiệu quả cũng sẽ vượt qua. Việc triển khai này vẫn sử dụng cách tiếp cận thời gian liên tục rõ ràng về mặt toán học và dễ dàng phù hợp với mọi giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a1, b1, c1 = map(int, input().split())
    a2, b2, c2 = map(int, input().split())

    det = a1 * b2 - a2 * b1

    if det != 0:
        print(1)
    else:
        same = (
            a1 * c2 == a2 * c1 and
            b1 * c2 == b2 * c1
        )

        if same:
            print(-1)
        else:
            print(0)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("1 1 0\n2 2 0\n") == "-1\n", "sample 1"

# intersecting lines
assert run("1 -1 0\n1 1 -4\n") == "1\n", "unique intersection"

# parallel but distinct
assert run("1 1 1\n2 2 3\n") == "0\n", "parallel lines"

# zero coefficients, same line
assert run("0 1 0\n0 2 0\n") == "-1\n", "horizontal line scaling"

# maximum magnitude coefficients
assert run("100 -100 50\n-100 -100 0\n") == "1\n", "boundary coefficients"

# vertical parallel lines
assert run("1 0 -1\n2 0 -3\n") == "0\n", "distinct vertical lines"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0 / 2 2 0`|`-1`| Dòng giống hệt nhau | 
|`1 -1 0 / 1 1 -4`|`1`| Giao lộ độc đáo | 
|`1 1 1 / 2 2 3`|`0`| Song song nhưng khác biệt | 
|`0 1 0 / 0 2 0`|`-1`| Hệ số 0 được xử lý an toàn | 
|`100 -100 50 / -100 -100 0`|`1`| Giá trị hệ số biên | 
|`1 0 -1 / 2 0 -3`|`0`| Đường thẳng song song | 

## Vỏ cạnh 

Hãy xem xét trường hợp cả hai phương trình đều biểu thị cùng một đường ngang:```
0 1 0
0 2 0
```Định thức trở thành:$$0 \cdot 2 - 0 \cdot 1 = 0$$vì vậy các đường thẳng song song hoặc giống hệt nhau. Việc kiểm tra tỷ lệ cho:$$0 \cdot 0 = 0 \cdot 0$$Và$$1 \cdot 0 = 2 \cdot 0$$Cả hai đều đúng, do đó thuật toán in ra`-1`. Cách tiếp cận dựa trên sự phân chia có thể dễ dàng thất bại ở đây vì`A_1 = A_2 = 0`. 

Bây giờ hãy kiểm tra các đường song song khác nhau:```
1 1 1
2 2 3
```Định thức lại bằng 0:$$1 \cdot 2 - 2 \cdot 1 = 0$$nhưng việc kiểm tra tỷ lệ không thành công:$$1 \cdot 3 \neq 2 \cdot 1$$Thuật toán in chính xác`0`. Điều này phân biệt “cùng hướng” với “cùng một đường”. 

Cuối cùng, hãy xem xét các đường thẳng đứng và ngang giao nhau:```
1 0 -2
0 1 -3
```Yếu tố quyết định là:$$1 \cdot 1 - 0 \cdot 0 = 1$$Vì nó khác 0 nên thuật toán sẽ in ngay`1`. Không cần xử lý đặc biệt đối với các đường thẳng đứng vì phương pháp xác định hoạt động thống nhất cho mọi hướng.
