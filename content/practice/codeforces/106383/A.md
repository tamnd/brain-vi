---
title: "CF 106383A - Giai thừa điên cuồng"
description: "Tác vụ yêu cầu số bit 0 ở cuối biểu diễn nhị phân của giai thừa. Đầu vào là một số nguyên n và đầu ra gấp bao nhiêu lần giá trị nhị phân của n! có thể chia cho 2 trước khi nó trở thành số lẻ. Giá trị của n!"
date: "2026-06-25T10:19:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106383
codeforces_index: "A"
codeforces_contest_name: "2026 Spring UT CS104c Midterm #1"
rating: 0
weight: 106383
solve_time_s: 31
verified: true
draft: false
---

[CF 106383A - Giai thừa điên cuồng](https://codeforces.com/problemset/problem/106383/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tác vụ yêu cầu số bit 0 ở cuối biểu diễn nhị phân của giai thừa. Đầu vào là một số nguyên duy nhất`n`và đầu ra gấp bao nhiêu lần giá trị nhị phân của`n!`có thể chia cho 2 trước khi nó trở thành số lẻ. Giá trị của`n!`bản thân nó trở nên quá lớn để xây dựng trực tiếp, ngay cả đối với đầu vào tối đa được phép. 

Sự thay đổi quan trọng là ngừng suy nghĩ về các chữ số của giai thừa và thay vào đó hãy nghĩ về các thừa số. Số 0 ở cuối trong hệ nhị phân có nghĩa là số đó có hệ số là`2`. Nếu một số chia hết cho`2^k`nhưng không phải bởi`2^(k+1)`, biểu diễn nhị phân của nó kết thúc bằng chính xác`k`số không. Vấn đề là yêu cầu số mũ của`2`bên trong`n!`. 

Giới hạn đầu vào đạt`10^6`. Một vòng lặp trên tất cả các số lên đến`n`là hoàn toàn hợp lý nhưng việc xây dựng giai thừa là không thể vì số chữ số của`n!`tăng nhanh hơn nhiều so với kích thước đầu vào. Bất kỳ cách tiếp cận nào lưu trữ hoặc thao tác nhiều lần chính giai thừa sẽ hết thời gian hoặc bộ nhớ. 

Những trường hợp phức tạp chủ yếu là về việc đếm chính xác các thừa số. Vì`n = 1`, giai thừa là`1`, có biểu diễn nhị phân là`1`, vì vậy câu trả lời là không. Một giải pháp bắt đầu đếm từ điểm sai có thể cộng sai hệ số hai. 

Ví dụ:```
Input:
1

Output:
0
```Không có số chẵn trong`1!`, do đó không có bit 0 ở cuối. 

Một sai lầm phổ biến khác là chỉ đếm bội số của 2 và bỏ qua lũy thừa cao hơn. Ví dụ:```
Input:
8

Output:
7
```Các số chẵn đóng góp thành thừa số của hai như sau:`2`đóng góp một,`4`đóng góp hai,`6`đóng góp một, và`8`đóng góp ba. Tổng cộng là`1 + 2 + 1 + 3 = 7`. Việc thực hiện bất cẩn chỉ đếm số chẵn sẽ nhận được`4`thay vì. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính giai thừa và chia liên tục cho hai trong khi nó vẫn chẵn. Điều này đúng về mặt toán học vì mỗi phép chia sẽ loại bỏ một số 0 ở cuối khỏi biểu diễn nhị phân. Vấn đề là kích thước của giai thừa. Vì`n = 10^6`, giai thừa có hàng triệu chữ số, vì vậy việc xây dựng và chia nó đòi hỏi phải thao tác với một số nguyên khổng lồ. 

Một cách tốt hơn là đếm xem có bao nhiêu thừa số của 2 xuất hiện trong phép nhân`1 * 2 * 3 * ... * n`. Mỗi số chẵn đóng góp ít nhất một thừa số của hai. Các số chia hết cho 4 đóng góp một hệ số bổ sung, các số chia hết cho 8 đóng góp một hệ số khác và mô hình này vẫn tiếp tục. 

Số bội số của`2`trong số những người đầu tiên`n`số nguyên là`floor(n / 2)`. Số lượng các yếu tố bổ sung đến từ bội số của`4`là`floor(n / 4)`. Số lượng các yếu tố bổ sung từ bội số của`8`là`floor(n / 8)`. Cộng các giá trị này cho đến khi lũy thừa của hai lớn hơn`n`đưa ra số lượng chính xác các số 0 nhị phân ở cuối. 

Ý tưởng vũ lực không thành công vì nó cố gắng biểu diễn kết quả của phép nhân. Ý tưởng được tối ưu hóa chỉ tính các thừa số nguyên tố quan trọng, vì vậy nó làm giảm vấn đề từ việc thao tác một số lượng lớn đến việc thực hiện một số phép chia logarit nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(kích thước của n! × số phần) | O(kích thước của n!) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và khởi tạo câu trả lời về 0. Đáp án sẽ lưu tổng số thừa số của 2 có trong`n!`. 
2. Bắt đầu với giá trị`2`và nhân nó nhiều lần với`2`. Với mỗi lũy thừa của hai`p`, thêm vào`n // p`để trả lời. 
3. Dừng lại khi cường độ dòng điện của hai người trở nên lớn hơn`n`. Không có số từ`1`ĐẾN`n`có thể chứa nhiều thừa số của hai, vì vậy lũy thừa lớn hơn không đóng góp được gì. 
4. In câu trả lời tích lũy. 

Lý do điều này có tác dụng là vì mỗi lần xuất hiện hệ số hai trong các số từ`1`ĐẾN`n`tạo một số 0 ở cuối trong hệ nhị phân. Đếm bội số của`2`tìm các thừa số đầu tiên, đếm bội số của`4`tìm ra các yếu tố bổ sung mà những con số đó chứa đựng và việc tiếp tục với lũy thừa cao hơn sẽ nắm bắt được mọi đóng góp có thể có. 

Tại sao nó hoạt động: 

Số mũ của một số nguyên tố`p`trong một giai thừa được đưa ra bằng cách cộng số bội của`p`, thì số bội của`p²`, thì số bội của`p³`, vân vân. Nguyên tố ở đây là`2`, vậy tổng chính xác là:`floor(n/2) + floor(n/4) + floor(n/8) + ...`Giá trị này là lớn nhất`k`như vậy`2^k`chia rẽ`n!`. Vì số 0 ở cuối nhị phân tương ứng chính xác với một thừa số hai nên giá trị này là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ans = 0
    power = 2

    while power <= n:
        ans += n // power
        power *= 2

    print(ans)

if __name__ == "__main__":
    solve()
```Biến`ans`lưu trữ tổng số yếu tố của hai được tìm thấy cho đến nay. Vòng lặp bắt đầu từ`2`bởi vì mọi yếu tố được tính phải đến từ bội số của hai. 

phép nhân`power *= 2`di chuyển qua trình tự`2, 4, 8, 16, ...`. Mỗi lần lặp sẽ đếm xem có bao nhiêu số chứa lũy thừa của hai này làm ước số. Phép chia số nguyên cho chính xác số bội số. 

Số nguyên Python không bị tràn nên không cần xử lý đặc biệt cho câu trả lời. Câu trả lời lớn nhất có thể nhỏ hơn nhiều so với giai thừa và vòng lặp chỉ chạy khoảng hai mươi lần đối với`n = 10^6`. 

## Ví dụ đã hoạt động 

cho`n = 8`: 

| quyền lực | n // quyền lực | trả lời | 
| --- | --- | --- | 
| 2 | 4 | 4 | 
| 4 | 2 | 6 | 
| 8 | 1 | 7 | 

Sức mạnh tiếp theo sẽ là`16`, lớn hơn`8`, do đó thuật toán dừng lại. Kết quả khẳng định rằng tất cả các thừa số của hai từ`8!`đã được tính. 

Vì`n = 10`: 

| quyền lực | n // quyền lực | trả lời | 
| --- | --- | --- | 
| 2 | 5 | 5 | 
| 4 | 2 | 7 | 
| 8 | 1 | 8 | 

giá trị`10!`chứa tám thừa số của hai, vì vậy biểu diễn nhị phân của nó kết thúc bằng tám bit 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Thuật toán chỉ kiểm tra lũy thừa của hai tối đa`n`. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Đầu vào tối đa là`10^6`, do đó vòng lặp thực hiện khoảng 20 lần. Mức này thấp hơn nhiều so với giới hạn sẵn có và tránh tạo ra giá trị to lớn của`n!`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    ans = 0
    power = 2
    while power <= n:
        ans += n // power
        power *= 2

    sys.stdin = old_stdin
    return str(ans) + "\n"

# provided samples
assert solve_data("1\n") == "0\n", "sample 1"
assert solve_data("8\n") == "7\n", "sample 2"
assert solve_data("1000000\n") == "999993\n", "sample 3"

# custom cases
assert solve_data("2\n") == "1\n", "smallest even factorial"
assert solve_data("3\n") == "1\n", "boundary before power of two"
assert solve_data("16\n") == "15\n", "power of two boundary"
assert solve_data("32\n") == "31\n", "larger power of two boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`1`| Kiểm tra đóng góp khác 0 đầu tiên. | 
|`3`|`1`| Kiểm tra xem một số ngay trước lũy thừa của hai có được xử lý chính xác hay không. | 
|`16`|`15`| Kiểm tra sự đóng góp từ nhiều cấp độ quyền hạn của hai. | 
|`32`|`31`| Kiểm tra ranh giới lũy thừa hai lớn hơn. | 

## Vỏ cạnh 

Đối với đầu vào:```
1
```vòng lặp không bao giờ chạy vì`2 > 1`. Câu trả lời vẫn là 0, phù hợp với thực tế là`1! = 1`không có bit 0 ở cuối. 

Đối với đầu vào:```
8
```thuật toán truy cập quyền hạn`2`,`4`, Và`8`. Nó thêm`8 // 2 = 4`,`8 // 4 = 2`, Và`8 // 8 = 1`, sản xuất`7`. Điều này xử lý trường hợp các số như`8`đóng góp nhiều thừa số của 2 và ngăn ngừa lỗi thường gặp là chỉ đếm số chẵn. 

Đối với đầu vào:```
1000000
```thuật toán không bao giờ cố gắng xây dựng`1000000!`. Nó chỉ kiểm tra quyền hạn của hai lên đến`524288`, thêm từng đóng góp. Câu trả lời cuối cùng là`999993`, hoàn toàn xuất phát từ việc tính hệ số hơn là xây dựng giai thừa.
