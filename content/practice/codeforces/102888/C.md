---
title: "CF 102888C - \u6570\u7801\u7ba1"
description: "Màn hình bao gồm n chữ số bảy đoạn, nhưng chỉ có thể xuất hiện các chữ số 0, 2, 5, 6, 8 và 9 vì các đoạn khác bị hỏng. Sau khi xoay toàn bộ màn hình 180 độ, có hai điều xảy ra cùng một lúc."
date: "2026-07-25T12:21:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102888
codeforces_index: "C"
codeforces_contest_name: "The 15-th Beihang University Collegiate Programming Contest (BCPC 2020) - Preliminary"
rating: 0
weight: 102888
solve_time_s: 46
verified: true
draft: false
---

[CF 102888C - \u6570\u7801\u7ba1](https://codeforces.com/problemset/problem/102888/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Màn hình hiển thị bao gồm`n`chữ số bảy đoạn, nhưng chỉ có chữ số`0`,`2`,`5`,`6`,`8`, Và`9`có thể xuất hiện vì các đoạn khác bị hỏng. 

Sau khi xoay toàn bộ màn hình 180 độ, có hai điều xảy ra cùng một lúc. Thứ tự của các chữ số bị đảo ngược và mỗi chữ số được biến đổi theo phép quay. Các ánh xạ hợp lệ là:`0 ↔ 0`,`2 ↔ 2`,`5 ↔ 5`,`8 ↔ 8`,`6 ↔ 9`. 

Các số 0 đứng đầu được phép cả trước và sau khi xoay. 

Nhiệm vụ là đếm xem chiều dài bao nhiêu`n`chuỗi chữ số vẫn giống hệt nhau sau lần quay này. Từ`n`có thể có tới`10^10`các chữ số trong biểu diễn thập phân của nó? Không. Tuyên bố này có nghĩa là`n < 10^10`, Vì thế`n`bản thân nó có thể lớn tới khoảng mười tỷ. Giá trị như vậy là quá lớn đối với bất kỳ thuật toán nào xử lý từng vị trí riêng lẻ. Bất kỳ giải pháp nào có độ phức tạp tỷ lệ thuận với`n`là không thể. Câu trả lời phải được lấy từ một công thức toán học trực tiếp và được tính bằng lũy ​​thừa nhanh. 

Một sai lầm dễ mắc phải là quên rằng phép quay sẽ đảo ngược thứ tự của các chữ số. Ví dụ, khi`n = 2`, chuỗi`69`là hợp lệ bởi vì nó trở thành`69`một lần nữa sau khi đảo ngược và trao đổi`6`Và`9`. Việc xử lý ánh xạ một cách độc lập ở mỗi vị trí sẽ từ chối nó một cách không chính xác. 

Một trường hợp tinh vi khác là số chữ số lẻ. Đối với đầu vào```
1
```các chữ số hợp lệ duy nhất là`0`,`2`,`5`, Và`8`, vậy đáp án đúng là`4`. Việc thực hiện bất cẩn cũng có thể cho phép`6`hoặc`9`, mặc dù mỗi cái trong số chúng thay đổi thành cái kia sau khi xoay. 

Trường hợp cạnh thứ ba là số 0 đứng đầu. Đối với đầu vào```
2
```chuỗi`00`là hoàn toàn hợp lệ và phải được tính. Việc loại bỏ các số 0 đứng đầu sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê mọi độ dài có thể`n`chuỗi trên sáu chữ số có sẵn, xoay nó và so sánh kết quả với chuỗi gốc. Điều này đúng vì mọi ứng viên đều được kiểm tra một cách rõ ràng. Thật không may là có`6^n`ứng viên, điều này hoàn toàn không khả thi. Thậm chí`n = 30`sẽ yêu cầu hơn`2 × 10^23`séc. 

Quan sát quan trọng là mọi vị trí chỉ tương tác với vị trí được phản ánh của nó. Nếu vị trí`i`chứa chữ số`x`, sau đó định vị`n - 1 - i`buộc phải chứa dạng xoay của`x`. 

Đối với mỗi cặp được nhân đôi, có chính xác sáu phép gán hợp lệ:`(0,0)`,`(2,2)`,`(5,5)`,`(8,8)`,`(6,9)`,`(9,6)`. 

Mỗi cặp độc lập với mọi cặp khác, vì vậy nếu có`⌊n/2⌋`cặp, họ đóng góp`6^(⌊n/2⌋)`khả năng. 

Nếu như`n`là số lẻ, một chữ số vẫn ở giữa. Vì việc đảo chiều không làm nó chuyển động nên nó phải quay vào chính nó. Chỉ một`0`,`2`,`5`, Và`8`thỏa mãn điều kiện này, đưa ra bốn lựa chọn. 

Câu trả lời đơn giản là`6^(n/2)`khi`n`là chẵn, 

và`6^(n/2) × 4`khi`n`thật kỳ quặc. 

Từ`n`là cực kỳ lớn, số mũ được tính bằng modulo lũy thừa nhị phân`998244353`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(6^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Tính toán`pairs = n // 2`. Mỗi cặp được nhân đôi có thể được chọn độc lập. 
3. Tính toán`ans = 6^pairs mod 998244353`sử dụng lũy ​​thừa mô đun nhanh. 
4. Nếu`n`là lẻ, nhân lên`ans`qua`4`modulo`998244353`, vì chữ số ở giữa phải là một trong`0`,`2`,`5`, hoặc`8`. 
5. Đầu ra`ans`. 

### Tại sao nó hoạt động 

Đối với mỗi cặp được nhân đôi, việc chọn chữ số bên trái sẽ xác định duy nhất chữ số bên phải thông qua ánh xạ xoay. Không có lựa chọn nào cho một cặp ảnh hưởng đến bất kỳ cặp nào khác, vì vậy tổng số chuỗi hợp lệ là tích của số lựa chọn cho mỗi cặp. Khi độ dài là số lẻ, vị trí ở giữa ánh xạ lên chính nó, do đó chỉ cho phép các chữ số tự xoay. Đây là những ràng buộc duy nhất nên công thức tính là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input())

ans = pow(6, n // 2, MOD)
if n % 2:
    ans = ans * 4 % MOD

print(ans)
```Trước tiên, chương trình sẽ đếm xem có bao nhiêu cặp được nhân đôi. Tích hợp sẵn của Python`pow(base, exp, mod)`thực hiện phép lũy thừa nhị phân trong`O(log exp)`thời gian, đó là điều cần thiết bởi vì`n`có thể cực kỳ lớn. 

Trường hợp duy nhất còn lại là màn hình có độ dài lẻ. Vị trí trung tâm không bao giờ thay đổi vị trí sau khi đảo chiều nên phải chứa chữ số tự xoay. Nhân với bốn tài khoản cho những khả năng này. 

Không cần xử lý đặc biệt đối với các số 0 đứng đầu vì đối số đếm đã bao gồm chúng một cách tự nhiên. 

## Ví dụ đã hoạt động 

Hãy xem xét`n = 1`. 

| Bước | cặp | trả lời | 
| --- | --- | --- | 
| Ban đầu | 0 | 1 | 
| Tính toán`6^pairs`| 0 | 1 | 
| Độ dài lẻ, nhân với 4 | 0 | 4 | 

Bốn chuỗi hợp lệ là`0`,`2`,`5`, Và`8`. Điều này xác nhận rằng chỉ các chữ số tự xoay mới có thể chiếm giữ trung tâm. 

Bây giờ hãy xem xét`n = 2`. 

| Bước | cặp | trả lời | 
| --- | --- | --- | 
| Ban đầu | 1 | 1 | 
| Tính toán`6^pairs`| 1 | 6 | 
| Độ dài chẵn | 1 | 6 | 

Sáu chuỗi hợp lệ là`00`,`22`,`55`,`69`,`88`, Và`96`. Dấu vết này cho thấy mỗi cặp phản chiếu có đúng sáu phép gán hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| lũy thừa mô-đun sử dụng lũy ​​thừa nhị phân. | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ. | 

Ngay cả khi`n`gần với`10^10`, phép lũy thừa nhị phân chỉ thực hiện khoảng 34 lần lặp, dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 998244353

def solve():
    input = sys.stdin.readline
    n = int(input())
    ans = pow(6, n // 2, MOD)
    if n % 2:
        ans = ans * 4 % MOD
    print(ans)

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

# basic cases
assert run("1\n") == "4\n", "length 1"
assert run("2\n") == "6\n", "length 2"

# custom cases
assert run("3\n") == "24\n", "odd length"
assert run("4\n") == "36\n", "two mirrored pairs"
assert run("5\n") == "144\n", "odd with two pairs"
assert run("10000000000\n") == str(pow(6, 5000000000, MOD)) + "\n", "very large n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`24`| Xử lý chữ số trung tâm | 
|`4`|`36`| Nhiều cặp nhân đôi độc lập. | 
|`5`|`144`| Đếm cặp kết hợp với chữ số ở giữa. | 
|`10000000000`|`6^5000000000 mod 998244353`| Số mũ cực lớn được xử lý hiệu quả. | 

## Vỏ cạnh 

Đối với đầu vào```
1
```thuật toán tính toán các cặp được nhân đôi bằng 0, đưa ra`6^0 = 1`. Vì độ dài là số lẻ nên nó nhân với 4 và trả về`4`. chữ số`6`Và`9`bị loại trừ vì cả hai đều không thay đổi sau khi xoay. 

Đối với đầu vào```
2
```thuật toán tính toán một cặp được nhân đôi và trả về`6`. Một trong sáu khả năng đó là`00`, cho thấy các số 0 đứng đầu được tính chính xác mà không cần bất kỳ logic đặc biệt nào. 

Đối với đầu vào```
2
```cặp hợp lệ`(6,9)`đóng góp một trong sáu lựa chọn. Vì thuật toán tính các bài tập được nhân đôi thay vì kiểm tra các vị trí một cách độc lập, các chuỗi như`69`Và`96`được bao gồm đúng một lần.
