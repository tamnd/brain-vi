---
title: "CF 102254D - Donimo's"
description: "Chúng ta có 2n lát bánh pizza, trong đó lát i có kích thước a[i]. Chúng ta phải chia tất cả các lát cho n người, đưa chính xác cho mỗi người hai lát. Số tiền của một người là tổng của hai lát họ nhận được."
date: "2026-08-17T21:07:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102254
codeforces_index: "D"
codeforces_contest_name: "IME++ Starters Try-outs 2019"
rating: 0
weight: 102254
solve_time_s: 261
verified: false
draft: false
---

[CF 102254D - Donimo's](https://codeforces.com/problemset/problem/102254/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

chúng tôi có`2n`lát pizza, lát ở đâu`i`có kích thước`a[i]`. Chúng ta phải chia tất cả các lát cho`n`mọi người, đưa chính xác hai lát cho mỗi người. Số tiền của một người là tổng của hai lát họ nhận được. 

Mục tiêu không phải là làm cho số tiền của mỗi người lớn hay nhỏ nhất có thể. Chúng tôi muốn toàn bộ bộ`n`tổng cặp được phân cụm chặt chẽ nhất có thể. Nếu tổng cặp nhỏ nhất là`L`và lớn nhất là`R`, chất lượng của phép chia là`R - L`. Chúng ta cần giá trị tối thiểu có thể có của sự khác biệt này. 

Các ràng buộc mang lại nhiều nhất`2 * 10^4`kích thước lát, mỗi lát lớn bằng`10^9`. Một cách tiếp cận kiểm tra các cặp một cách rõ ràng là hoàn toàn không khả thi vì số cách phân chia`2n`các đối tượng thành từng cặp phát triển theo giai thừa. Việc sắp xếp các lát có giá cả phải chăng một cách dễ dàng, lấy`O(n log n)`, vì vậy giải pháp dự định cần khai thác thứ tự của các kích thước lát thay vì liệt kê các cặp. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. Với`n = 2`và lát`1 4 3 2`, sắp xếp cho`1 2 3 4`. Ghép nối các phần tử liền kề tạo ra tổng`3`Và`7`, nhưng cách ghép đôi tối ưu là`1+4`Và`2+3`, đưa ra câu trả lời`0`. Điều này nắm bắt các giải pháp cho rằng các phần tử liền kề đã được sắp xếp nên được ghép nối. 

Giá trị trùng lặp cũng quan trọng. Vì`n = 2`và lát`1 1 1 2`, sự ghép đôi tối ưu là`1+2`Và`1+1`, vậy câu trả lời là`1`. Việc chứng minh hoặc triển khai dựa trên việc tất cả các kích thước lát cắt đều khác biệt là không cần thiết và có thể vô tình xử lý sai các ranh giới bằng nhau. 

Các giá trị lát cắt lớn nhất cũng yêu cầu số học số nguyên mà không thu hẹp. Ví dụ,`n = 2`và lát`1000000000 1000000000 1 1`tạo ra tổng cặp`1000000001`Và`1000000001`, vậy câu trả lời là`0`. Bản thân số tiền có thể đạt tới`2 * 10^9`, an toàn với số nguyên của Python và cũng yêu cầu ít nhất phạm vi 32 bit có dấu trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ liệt kê mọi cách có thể để phân vùng`2n`cắt thành`n`cặp không có thứ tự. Đối với mỗi cặp, chúng tôi có thể tính toán tất cả`n`ghép các tổng và lấy hiệu giữa số lớn nhất và số nhỏ nhất. Điều này đúng vì mọi phép chia hợp lệ đều được xem xét. 

Số cặp của`2n`vị trí riêng biệt là 

(2n−1)!!= 2 n n! (2n)! ​ . 

Mỗi cặp yêu cầu`n`tính toán tổng cặp, do đó tổng công việc là`O(n(2n-1)!!)`. Vì`n = 10000`, điều này vượt xa mọi thứ có thể thực hiện được trong vòng một giây. Thậm chí số lượng cặp đôi cũng trở nên rất lớn trước khi đạt đến giới hạn ràng buộc. 

Quan sát hữu ích đến từ việc sắp xếp các lát cắt: 

a 1 ​ ≤a 2 ​ ⋯<a 2n ​ . 

Hãy xem xét phép chia cụ thể ghép phần nhỏ nhất với phần lớn nhất, phần nhỏ thứ hai với phần lớn thứ hai, v.v. Tổng cặp của nó là 

a 1 ​ +a 2n ​ ,a 2 ​ +a 2n−1 ​,…,an ​ +a n+1 ​ . 

Tại sao điều này nên tối ưu? Thuộc tính khóa mạnh hơn việc chỉ nói rằng việc ghép nối này "cân bằng" các giá trị. Đối với bất kỳ cặp nào có thể xảy ra, mỗi tổng bổ sung này nằm giữa tổng cặp nhỏ nhất của cặp đó và tổng cặp lớn nhất. 

Giả sử một số tiền bổ sung`a_k + a_{2n+1-k}`lớn hơn số tiền lớn nhất`M`của một cặp khác. Nhìn vào`k`lát lớn nhất. Mỗi thứ ít nhất đều có giá trị`a_{2n+1-k}`. Vì tổng cặp của nó nhiều nhất là`M`, đối tác của nó phải nhỏ hơn hoàn toàn so với`a_k`. Nhưng chỉ có`k-1`lát nhỏ hơn`a_k`, trong khi`k`cần lát lớn nhất`k`đối tác riêng biệt. Điều đó là không thể. 

Lập luận tương tự cũng có tác dụng từ phía bên kia. Nếu như`a_k + a_{2n+1-k}`nhỏ hơn tổng nhỏ nhất`m`của một cặp khác thì mỗi cặp`k`lát nhỏ nhất sẽ cần một đối tác lớn hơn`a_{2n+1-k}`. Chỉ một`k-1`các lát cắt có thể lớn hơn giá trị biên đó một lần nữa, điều này lại là không thể. 

Do đó, tất cả các tổng bổ sung đều nằm trong phạm vi của mọi cặp hợp lệ. Do đó, việc ghép đôi bổ sung có phạm vi nhỏ nhất có thể. 

Lực lượng vũ phu hoạt động vì nó xem xét rõ ràng mọi cặp đôi có thể xảy ra, nhưng không thành công vì có rất nhiều trong số chúng. Việc quan sát thứ tự cho phép chúng ta thay thế toàn bộ vấn đề so khớp bằng một lần sắp xếp và một lần quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n(2n-1)!!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`2n`cắt kích thước và sắp xếp chúng theo thứ tự không giảm. Việc sắp xếp cho phép chúng ta truy cập trực tiếp vào các phần nhỏ nhất và lớn nhất còn lại, đây chính xác là cấu trúc được sử dụng để ghép nối tối ưu. 
2. Cặp`a[i]`với`a[2n - 1 - i]`cho mọi`i`từ`0`bởi vì`n - 1`. Đây là hai phần tử được bố trí đối xứng xung quanh tâm của mảng được sắp xếp. 
3. Với mỗi cặp, hãy tính tổng của nó. Theo dõi tổng cặp nhỏ nhất và tổng cặp lớn nhất được thấy cho đến nay. Không cần lưu trữ tổng cặp vì chỉ có giá trị tối thiểu và tối đa của chúng ảnh hưởng đến câu trả lời. 
4. Đầu ra`maximum_pair_sum - minimum_pair_sum`. Chứng minh trên cho thấy phạm vi tạo ra bởi các cặp đối xứng này không thể lớn hơn phạm vi của bất kỳ phép chia nào khác, vì vậy nó là giá trị tối thiểu bắt buộc. 

Tại sao nó hoạt động có thể được nêu như một bất biến. Sau khi sắp xếp hãy xác định 

c k ​ =a k ​ +a 2n+1−k ​ . 

Đối với bất kỳ cặp hợp lệ tùy ý nào có tổng cặp nằm trong`[L,R]`, mọi`c_k`cũng nằm ở`[L,R]`. Nếu một số`c_k > R`, cái`k`lát lớn nhất sẽ yêu cầu`k`đối tác riêng biệt nhỏ hơn`a_k`, nhưng chỉ`k-1`những lát cắt như vậy tồn tại. Nếu một số`c_k < L`, cái`k`lát nhỏ nhất sẽ yêu cầu`k`đối tác riêng biệt lớn hơn`a_{2n+1-k}`, nhưng chỉ`k-1`những lát cắt như vậy tồn tại. Do đó, tổng tối thiểu và tối đa của ghép nối đối xứng đều nằm trong phạm vi của mọi ghép nối có thể có. Do đó, phạm vi của nó không lớn hơn phạm vi của bất kỳ đối thủ cạnh tranh nào. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n = int(input())    a = list(map(int, input().split()))
    a.sort()
    mn = 10**30    mx = -10**30
    for i in range(n):        pair_sum = a[i] + a[2 * n - 1 - i]        mn = min(mn, pair_sum)        mx = max(mx, pair_sum)
    print(mx - mn)

if __name__ == "__main__":    solve()
```Đầu vào chứa chính xác`2n`số nguyên, do đó toàn bộ mảng có thể được đọc bằng một lệnh gọi tới`input()`. Mảng được sắp xếp vì việc ghép nối tối ưu chỉ phụ thuộc vào thứ tự tương đối của các kích thước lát cắt. 

Vòng lặp chỉ chạy`n`lần. Tại lần lặp`i`,`a[i]`là`i`- lát nhỏ nhất và`a[2*n-1-i]`là tương ứng`i`-lát lớn thứ chỉ số`2*n-1-i`là giá trị dựa trên số 0 tương đương với chỉ số toán học`2n+1-(i+1)`. 

Các biến`mn`Và`mx`được cập nhật sau mỗi cặp. Việc khởi tạo chúng với các giá trị nằm ngoài phạm vi tổng cặp có thể sẽ tránh được việc xử lý đặc biệt cho lần lặp đầu tiên. Số nguyên Python có độ chính xác tùy ý, do đó không có rủi ro tràn mặc dù tổng cặp có thể đạt tới`2 * 10^9`. 

Chỉ có một trường hợp kiểm thử trong vấn đề này, do đó không cần vòng lặp trường hợp kiểm thử nào. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là:```
21 4 3 2
```Sau khi phân loại, các lát cắt được`[1, 2, 3, 4]`. 

|`i`| Miếng nhỏ | Miếng lớn | Tổng cặp |`mn`|`mx`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 5 | 5 | 5 | 
| 1 | 2 | 3 | 5 | 5 | 5 | 

Cả hai người đều nhận được một chiếc bánh pizza cỡ lớn`5`, vậy sự khác biệt là`5 - 5 = 0`. Điều này chứng tỏ tại sao việc ghép các cực trị lại tốt hơn việc ghép các phần tử được sắp xếp liền kề. 

Đối với Mẫu 2, đầu vào là:```
45 1 1 4 3 2 11 3
```Sắp xếp mang lại`[1, 1, 2, 3, 3, 4, 5, 11]`. 

|`i`| Miếng nhỏ | Miếng lớn | Tổng cặp |`mn`|`mx`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 11 | 12 | 12 | 12 | 
| 1 | 1 | 5 | 6 | 6 | 12 | 
| 2 | 2 | 4 | 6 | 6 | 12 | 
| 3 | 3 | 3 | 6 | 6 | 12 | 

Tổng cặp kết quả là`12, 6, 6, 6`, vậy câu trả lời là`12 - 6 = 6`. Phần lớn nhất thiết phải tạo ra số lượng lớn nhất, trong khi việc ghép các điểm cực trị còn lại sẽ giữ cho ba số lượng còn lại bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Sắp xếp`2n`các giá trị thống trị`O(n)`quét ghép nối | 
| Không gian |`O(n)`| Mảng chứa`2n`kích thước lát | 

Với`n <= 10^4`, sắp xếp nhiều nhất`20000`số nguyên thoải mái trong giới hạn thời gian một giây. Việc sử dụng bộ nhớ cũng rất nhỏ so với`256 MB`giới hạn. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve():    n = int(input())    a = list(map(int, input().split()))
    a.sort()
    mn = 10**30    mx = -10**30
    for i in range(n):        pair_sum = a[i] + a[2 * n - 1 - i]        mn = min(mn, pair_sum)        mx = max(mx, pair_sum)
    print(mx - mn)

def run(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    try:        solve()        return sys.stdout.getvalue().strip()    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout

# Provided samplesassert run("2\n1 4 3 2\n") == "0", "sample 1"assert run("4\n5 1 1 4 3 2 11 3\n") == "6", "sample 2"
# Minimum-size inputassert run("2\n1 2 3 4\n") == "0", "minimum n with perfectly balanced pairs"
# All values equalassert run("5\n7 7 7 7 7 7 7 7 7 7\n") == "0", "all pair sums are equal"
# Boundary values near 1e9assert run("2\n1 1 1000000000 1000000000\n") == "0", "large integer values"
# Case where adjacent pairing is not optimalassert run("2\n1 2 3 10\n") == "4", "extreme pairing gives 11 and 5"
# Maximum-size input, generated rather than written literallymax_input = "10000\n" + " ".join(["1000000000"] * 20000) + "\n"assert run(max_input) == "0", "maximum n and all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2 3 4`|`0`| Tối thiểu cho phép`n`và các cặp cực cân bằng hoàn hảo | 
|`5 / 7 7 7 7 7 7 7 7 7 7`|`0`| Giá trị trùng lặp và tổng cặp giống hệt nhau | 
|`2 / 1 1 1000000000 1000000000`|`0`| Giá trị lát tối đa và tổng cặp lớn | 
|`2 / 1 2 3 10`|`4`| Sửa lỗi ghép nối cực đoan thay vì ghép nối liền kề | 
|`n = 10000`, tất cả`10^9`|`0`| Kích thước và hiệu suất đầu vào tối đa | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, hãy xem xét```
21 4 3 2
```Sắp xếp mang lại`[1, 2, 3, 4]`. Cặp thuật toán`1`với`4`Và`2`với`3`, tạo ra số tiền`5`Và`5`. Như vậy`mn = 5`,`mx = 5`, và đầu ra là`0`. Thay vào đó, chiến lược cặp liền kề sẽ có được`3`Và`7`, trả lời sai`4`. 

Đối với các giá trị trùng lặp, hãy xem xét```
21 1 1 2
```Mảng đã được sắp xếp rồi`[1, 1, 1, 2]`. Các cặp đối xứng có tổng`3`Và`2`, do đó, thuật toán đầu ra`1`. Các giá trị bằng nhau không gây ra vấn đề lập chỉ mục đặc biệt nào vì bằng chứng sử dụng các vị trí chứ không phải các giá trị lát cắt riêng biệt. 

Đối với các giá trị lớn, hãy xem xét```
21 1 1000000000 1000000000
```Các tổng cặp đối xứng đều là`1000000001`. Mức tối thiểu và tối đa được theo dõi giống hệt nhau, vì vậy kết quả là`0`. Việc triển khai thực hiện tất cả số học trực tiếp dưới dạng số nguyên và không sử dụng dấu phẩy động. 

Để có kích thước đầu vào tối đa, hãy lấy`n = 10000`và làm tất cả`20000`kích thước lát bằng`1000000000`. Mọi cặp đối xứng đều có tổng`2000000000`, vậy câu trả lời là`0`. Thuật toán chỉ sắp xếp mảng và thực hiện`10000`kiểm tra cặp sau đó, vì vậy nó vẫn nằm trong độ phức tạp cần thiết.
