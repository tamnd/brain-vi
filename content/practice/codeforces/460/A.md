---
title: "CF 460A - Vasya và Tất"
description: "Vasya bắt đầu với một số đôi tất nhất định. Mỗi buổi sáng anh ấy sử dụng đúng một cặp và cặp đó sẽ biến mất vĩnh viễn vào cuối ngày. Có một sự kiện đặc biệt: cứ sau mỗi ngày số của nó là bội số của m, mẹ cậu lại mua cho cậu một đôi tất mới."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 460
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 262 (Div. 2)"
rating: 900
weight: 460
solve_time_s: 92
verified: true
draft: false
---

[CF 460A - Vasya và tất](https://codeforces.com/problemset/problem/460/A) 

**Xếp hạng:** 900 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vasya bắt đầu với một số đôi tất nhất định. Mỗi buổi sáng anh ấy sử dụng đúng một cặp và cặp đó sẽ biến mất vĩnh viễn vào cuối ngày. Có một sự kiện đặc biệt: sau mỗi ngày có số là bội số của`m`, mẹ anh ấy mua cho anh ấy một đôi tất mới. Vì việc mua hàng diễn ra vào buổi tối nên cặp mới không thể được sử dụng cho đến ngày hôm sau. 

Đầu vào chứa số lượng đôi tất ban đầu`n`và khoảng thời gian bổ sung`m`. Chúng ta phải xác định xem Vasya có thể tiếp tục đi tất trong bao nhiêu ngày liên tục trước khi đến một buổi sáng không có đôi tất nào. 

Những hạn chế là rất nhỏ. Cả hai`n`Và`m`nhiều nhất là 100. Ngay cả một mô phỏng trực tiếp hàng ngày cũng chỉ thực hiện được vài trăm lần lặp. Không cần cấu trúc dữ liệu phức tạp hoặc toán học nâng cao. Thách thức chính là xác định thời điểm bổ sung chính xác. 

Một sai lầm dễ mắc phải là mang tất thay thế trước khi sử dụng tất trong ngày thay vì sau khi kết thúc ngày. Coi như:```
1 2
```Câu trả lời đúng là:```
1
```Vasya mang đôi duy nhất của mình vào ngày 1 và không có tất nào vào ngày thứ 2. Việc thực hiện bất cẩn để cấp một chiếc thay thế vào đầu ngày thứ 2 sẽ khiến anh ấy tiếp tục một cách không chính xác. 

Một trường hợp khó phát hiện khác là khi chiếc tất thay thế được giao vào cùng ngày chiếc tất ban đầu cuối cùng được sử dụng.```
2 2
```Câu trả lời đúng là:```
3
```Ngày thứ 2 tiêu thụ cặp ban đầu thứ hai, nhưng cặp thay thế được mua vào tối hôm đó sẽ tạo ra thêm một ngày có thể sử dụng được. Việc quên xử lý giao dịch mua hàng buổi tối sẽ tạo ra 2 thay vì 3. 

Trường hợp cạnh thứ ba xảy ra khi các thay thế tạo ra các thay thế tiếp theo sau đó.```
9 3
```Câu trả lời là:```
13
```Những chiếc tất thu được vào ngày thứ 3, 6 và 9 sẽ được mang sau đó, điều này cho phép đến ngày thứ 12 và nhận được một chiếc tất thay thế khác. Chỉ tính những chiếc tất thay thế được tạo ra bởi những chiếc tất ban đầu sẽ bỏ lỡ hiệu ứng dây chuyền này. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng cuộc sống hàng ngày của Vasya. Nếu anh ta có sẵn ít nhất một đôi vào buổi sáng, anh ta sẽ sử dụng một đôi và sống sót trong ngày hôm đó. Vào cuối ngày, nếu số ngày chia hết cho`m`, một cặp mới được thêm vào. Quá trình dừng lại khi không có sẵn đôi tất nào vào đầu ngày. 

Mô phỏng này đúng vì nó tuân theo chính xác phát biểu vấn đề. Mọi thao tác đều tương ứng với một sự kiện có thật: sử dụng một cặp vào buổi sáng và có thể nhận được một cặp mới vào buổi tối. 

Đối với những ràng buộc nhất định, cách tiếp cận này đã đủ nhanh. Ngay cả trong trường hợp xấu nhất, tổng số ngày cũng chỉ có vài trăm. Từ`n ≤ 100`, mô phỏng thực hiện ít hơn một nghìn lần lặp. 

Ngoài ra còn có một quan sát toán học. Mọi`m`ngày, một chiếc tất bổ sung xuất hiện. Người ta có thể tính toán nhiều lần những chiếc tất thừa này cho đến khi không còn sự bổ sung nào nữa. Mặc dù cách này có hiệu quả nhưng nó phức tạp hơn mức cần thiết và không mang lại lợi ích thiết thực nào cho các giới hạn nhất định. 

Mô phỏng vừa là giải pháp đơn giản nhất vừa đáng tin cậy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(câu trả lời) | O(1) | Đã chấp nhận | 
| Đếm toán học | O(câu trả lời / m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Lưu số lượng đôi tất hiện có vào một biến. 
3. Duy trì bộ đếm ngày bắt đầu từ 0. 
4. Khi có sẵn ít nhất một đôi tất, hãy mô phỏng một ngày. 

Sự tồn tại của một đôi tất có nghĩa là Vasya có thể bắt đầu một ngày mới một cách thành công. 
5. Tăng bộ đếm ngày lên 1. 

Ngày này bây giờ được tính là sống sót. 
6. Giảm số lượng tất xuống 1. 

Một đôi được đeo trong ngày và bỏ đi sau đó. 
7. Nếu số ngày hiện tại chia hết cho`m`, tăng số lượng tất lên 1. 

Đây là mô hình mua hàng buổi tối do mẹ của Vasya thực hiện. 
8. Lặp lại cho đến khi số tất trở thành 0 trước khi bắt đầu ngày hôm sau. 
9. Xuất ra số ngày sống sót. 

### Tại sao nó hoạt động 

Khi bắt đầu mỗi vòng lặp, số lượng tất chính xác bằng số đôi có sẵn vào buổi sáng hôm đó. Vòng lặp diễn ra khi và chỉ khi Vasya có thể mang tất vào ngày hôm đó. Trong quá trình lặp lại, một cặp sẽ được tiêu thụ và bất kỳ giao dịch mua buổi tối nào sẽ được thêm vào theo quy tắc. Sau những cập nhật này, số lượng tất lại khớp với số lượng đôi có sẵn vào sáng hôm sau. 

Bởi vì bất biến này được duy trì cho mỗi ngày mô phỏng nên thuật toán tái tạo chính xác quy trình thực. Bộ đếm ngày tăng một lần cho mỗi ngày Vasya mang tất thành công, vì vậy giá trị cuối cùng là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    days = 0
    socks = n

    while socks > 0:
        days += 1
        socks -= 1

        if days % m == 0:
            socks += 1

    print(days)

if __name__ == "__main__":
    solve()
```Biến`socks`lưu trữ số lượng cặp có sẵn vào đầu mỗi ngày. Mỗi lần lặp đại diện cho chính xác một ngày. 

Thứ tự của các hoạt động quan trọng. Đầu tiên, ngày được tính và một cặp được tiêu thụ. Chỉ sau đó, chúng tôi mới kiểm tra xem có nên nhận thay thế hay không. Điều này phù hợp với tuyên bố cho biết cặp mới được mua vào buổi tối. 

Điều kiện vòng lặp cũng rất quan trọng. Chúng tôi chỉ tiếp tục khi có ít nhất một cặp vào đầu ngày. Khi`socks`trở thành 0 sau tất cả các cập nhật từ ngày hôm trước, sáng hôm sau không thể bắt đầu nên quá trình mô phỏng dừng lại. 

Vì tất cả các giá trị vẫn rất nhỏ nên không có lo ngại về việc tràn bộ nhớ hoặc sử dụng bộ nhớ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
```| Ngày | Tất lúc bắt đầu | Sau khi mặc | Thay thế? | Tất ở cuối | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | Không | 1 | 
| 2 | 1 | 0 | Có | 1 | 
| 3 | 1 | 0 | Không | 0 | 

Đầu ra:```
3
```Dấu vết cho thấy tình huống quan trọng trong đó ngày thứ 2 tạo ra sự thay thế. Mặc dù cặp ban đầu thứ hai đã được tiêu thụ nhưng việc mua vào buổi tối sẽ cho phép thêm một ngày nữa. 

### Ví dụ 2 

đầu vào:```
9 3
```| Ngày | Tất lúc bắt đầu | Sau khi mặc | Thay thế? | Tất ở cuối | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 8 | Không | 8 | 
| 2 | 8 | 7 | Không | 7 | 
| 3 | 7 | 6 | Có | 7 | 
| 4 | 7 | 6 | Không | 6 | 
| 5 | 6 | 5 | Không | 5 | 
| 6 | 5 | 4 | Có | 5 | 
| 7 | 5 | 4 | Không | 4 | 
| 8 | 4 | 3 | Không | 3 | 
| 9 | 3 | 2 | Có | 3 | 
| 10 | 3 | 2 | Không | 2 | 
| 11 | 2 | 1 | Không | 1 | 
| 12 | 1 | 0 | Có | 1 | 
| 13 | 1 | 0 | Không | 0 | 

Đầu ra:```
13
```Ví dụ này thể hiện hiệu ứng dây chuyền. Sản phẩm thay thế nhận được vào ngày thứ 12 chỉ tồn tại vì những sản phẩm thay thế trước đó đã giữ cho Vasya được cung cấp đủ thời gian cho đến ngày thứ 12. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(câu trả lời) | Một lần lặp được thực hiện mỗi ngày còn sót lại | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Câu trả lời chỉ là vài trăm ngay cả ở kích thước đầu vào tối đa. Mô phỏng dễ dàng phù hợp với giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n, m = map(int, input().split())

    days = 0
    socks = n

    while socks > 0:
        days += 1
        socks -= 1

        if days % m == 0:
            socks += 1

    print(days)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample
assert run("2 2\n") == "3", "sample 1"

# custom cases
assert run("1 2\n") == "1", "minimum input"
assert run("2 3\n") == "2", "no replacement before exhaustion"
assert run("9 3\n") == "13", "chain of replacements"
assert run("100 100\n") == "101", "maximum values and boundary replacement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`1`| Đầu vào hợp lệ tối thiểu | 
|`2 3`|`2`| Kiệt sức trước ngày thay thế đầu tiên | 
|`9 3`|`13`| Thay thế tạo ra thay thế sau này | 
|`100 100`|`101`| Giá trị lớn nhất và bổ sung ngày 100 | 

## Vỏ cạnh 

Hãy xem xét:```
1 2
```Ngày 1 chỉ dùng một đôi, không để lại chiếc tất nào. Vì ngày 1 không chia hết cho 2 nên không có sự thay thế nào được thêm vào. Vòng lặp dừng ngay lập tức và xuất ra`1`. Thuật toán xử lý việc này một cách chính xác vì các thay thế chỉ được thêm sau khi kiểm tra số của ngày hiện tại. 

Coi như:```
2 2
```Sau ngày thứ 2, số lượng tất giảm từ 1 xuống 0, sau đó ngay lập tức tăng trở lại 1 vì ngày 2 là bội số của 2. Thuật toán thực hiện phép trừ trước khi bổ sung, khớp với chuỗi sự kiện thực tế. Đầu ra trở thành`3`. 

Coi như:```
9 3
```Sự thay thế nhận được vào ngày thứ 12 xuất phát từ việc tồn tại đủ lâu nhờ những sự thay thế trước đó. Mô phỏng nắm bắt được hiệu ứng xếp tầng này một cách tự nhiên vì mọi chiếc tất mới mua sẽ được thêm trở lại vào cùng một nhóm và sau đó có thể góp phần đạt được bội số của 3 trong tương lai. Câu trả lời cuối cùng là`13`, đúng như yêu cầu.
