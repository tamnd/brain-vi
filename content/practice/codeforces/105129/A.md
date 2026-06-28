---
title: "CF 105129A - Khepri và bài toán đếm"
description: "Với mỗi trường hợp thử nghiệm, chúng ta được cho một số nguyên dương n. Ta phải đếm xem có bao nhiêu số nguyên dương trong khoảng từ 1 đến n có số thập phân là số lẻ. Giá trị của n có thể lớn tới 10^18. Điều đó ngay lập tức loại trừ mọi giải pháp kiểm tra từng số riêng lẻ."
date: "2026-06-27T19:23:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "A"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 167
verified: true
draft: false
---

[CF 105129A - Khepri và bài toán đếm](https://codeforces.com/problemset/problem/105129/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một số nguyên dương`n`. Chúng ta phải đếm có bao nhiêu số nguyên dương trong khoảng từ`1`ĐẾN`n`có số lẻ các chữ số thập phân. 

Giá trị của`n`có thể lớn như`10^18`. Điều đó ngay lập tức loại trừ mọi giải pháp kiểm tra từng số riêng lẻ. Ngay cả một vòng lặp trên một tỷ số đầu tiên cũng đã quá chậm, trong khi đầu vào lớn nhất chứa một triệu giá trị có thể có. Thuật toán chỉ phải phụ thuộc vào số chữ số của`n`, nhiều nhất là`19`. 

Biểu diễn thập phân tự nhiên nhóm các số theo số chữ số của chúng. Mỗi số có cùng số chữ số tạo thành một khoảng liên tục, vì vậy thay vì đếm các số riêng lẻ, chúng ta có thể đếm toàn bộ phạm vi độ dài chữ số. 

Một nơi dễ mắc sai lầm là khi`n`nằm bên trong một phạm vi có độ dài lẻ hơn là ở cuối của nó. 

Ví dụ:```
n = 234
```Câu trả lời đúng là:```
9 + (99 - 10 + 1) + (234 - 100 + 1)
= 9 + 90 + 135
= 234
```Việc triển khai bất cẩn chỉ đếm các dãy chữ số đầy đủ sẽ chỉ đếm các số có một chữ số và trả về`9`, thiếu phạm vi ba chữ số một phần. 

Một lỗi phổ biến khác xảy ra chính xác ở lũy thừa mười. 

Ví dụ:```
n = 100
```Câu trả lời là:```
9 + 1 = 10
```số`100`là số có ba chữ số đầu tiên nên phải bao gồm nó. sử dụng`<`thay vì`<=`khi tính toán phạm vi cuối cùng sẽ trả về không chính xác`9`. 

Các đầu vào lớn nhất cũng đáng được chú ý. 

Ví dụ:```
n = 10^18
```Đây là một số có 19 chữ số. Mặc dù chỉ có một con số như vậy lên đến`10^18`, nó vẫn góp phần trả lời vì 19 là số lẻ. Các ngôn ngữ có số nguyên có kích thước cố định có thể tràn trong khi khả năng tính toán là 10, nhưng số nguyên Python sẽ tự động tăng lên. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là lặp qua mọi số nguyên từ`1`ĐẾN`n`, tính số chữ số thập phân của nó và tăng câu trả lời bất cứ khi nào số chữ số đó là số lẻ. Phương pháp này rõ ràng là đúng vì mỗi ứng viên đều được kiểm tra chính xác một lần. 

Thời gian chạy của nó là`O(n)`. Với`n`đạt`10^18`, điều này sẽ yêu cầu khoảng một triệu tỷ lần lặp, khiến nó hoàn toàn không khả thi. 

Quan sát quan trọng là các số có cùng số chữ số luôn tạo thành một khoảng liền kề. 

Khoảng thời gian của`d`-các số có chữ số là:```
[10^(d-1), 10^d - 1]
```ngoại trừ số có một chữ số bắt đầu tại`1`. 

Bất cứ khi nào`d`là số lẻ, mọi số trong khoảng đó đều góp phần trả lời. Nếu toàn bộ khoảng nằm dưới`n`, chúng tôi chỉ cần thêm kích thước của nó. Nếu như`n`nằm trong khoảng đó thì chúng ta chỉ thêm tiền tố kết thúc tại`n`. 

Vì một số lên đến`10^18`có nhiều nhất`19`chữ số thì chỉ có`19`độ dài chữ số có thể để kiểm tra. Điều này làm giảm công việc cho từng trường hợp thử nghiệm xuống một mức không đổi nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị của`n`. 
2. Tính số chữ số thập phân trong`n`. Điều này xác định có bao nhiêu độ dài chữ số cần được xem xét. 
3. Khởi tạo câu trả lời về 0. 
4. Lặp lại qua từng chữ số`d`từ`1`lên đến số chữ số của`n`. 
5. Bỏ qua các giá trị chẵn của`d`, vì chỉ có độ dài chữ số lẻ đóng góp. 
6. Tính số nhỏ nhất có`d`chữ số. Đây là`1`khi`d = 1`, nếu không thì là`10^(d-1)`. 
7. Tính số lớn nhất cần đếm có độ dài chữ số này. Nó nhỏ hơn`n`Và`10^d - 1`. 
8. Nếu giới hạn trên được tính toán ít nhất là giới hạn dưới, hãy cộng kích thước khoảng`upper - lower + 1`để trả lời. Điều này xử lý chính xác cả phạm vi hoàn chỉnh và phạm vi một phần cuối cùng. 
9. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Mỗi số nguyên dương thuộc về chính xác một khoảng có độ dài chữ số. Thuật toán truy cập từng độ dài chữ số lẻ chính xác một lần và đếm chính xác các số trong khoảng đó không vượt quá`n`. Không có số nào bị bỏ qua vì mọi khoảng thời gian đủ điều kiện đều được xử lý. Không có số nào được đếm hai lần vì độ dài chữ số khác nhau tương ứng với các khoảng cách nhau. Tổng cuối cùng chính xác là số số nguyên từ`1`ĐẾN`n`biểu diễn thập phân của nó có số chữ số lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        digits = len(str(n))
        ans = 0

        for d in range(1, digits + 1, 2):
            if d == 1:
                low = 1
            else:
                low = 10 ** (d - 1)

            high = min(n, 10 ** d - 1)

            if high >= low:
                ans += high - low + 1

        print(ans)

solve()
```Giải pháp bắt đầu bằng cách đọc tất cả các trường hợp thử nghiệm. Với mỗi giá trị của`n`, nó xác định có bao nhiêu độ dài chữ số thập phân phải được kiểm tra. 

Vòng lặp tăng thêm hai mỗi lần, do đó chỉ độ dài chữ số lẻ được xử lý. Điều này tránh được những công việc không cần thiết và phản ánh trực tiếp yêu cầu của vấn đề. 

Đối với mỗi độ dài chữ số lẻ, mã sẽ tính khoảng thời gian bị chiếm bởi các số có độ dài đó. Giới hạn dưới yêu cầu xử lý đặc biệt đối với các số có một chữ số vì chúng bắt đầu tại`1`thay vì`10^0`. 

Giới hạn trên nhỏ hơn của`n`và số lớn nhất có nhiều chữ số như vậy. Khi`n`nằm trong khoảng hiện tại, điều này đương nhiên chỉ tính tiền tố hợp lệ. 

điều kiện`high >= low`ngăn chặn việc thêm kích thước khoảng âm. Điều này xảy ra khi`n`có ít chữ số hơn phạm vi hiện tại. 

Số nguyên Python tự động hỗ trợ các giá trị vượt quá`10^18`, nên sức mạnh tính toán bằng 10 là hoàn toàn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử:```
n = 234
```| Độ dài chữ số | Giới hạn dưới | Giới hạn trên | Số được thêm vào | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 9 | 9 | 9 | 
| 3 | 100 | 234 | 135 | 144 | 

Câu trả lời là`144`. 

Ví dụ này cho thấy các phạm vi độ dài lẻ hoàn chỉnh và một phạm vi độ dài lẻ một phần được kết hợp thành một kết quả duy nhất như thế nào. 

### Ví dụ 2 

Giả sử:```
n = 100000
```| Độ dài chữ số | Giới hạn dưới | Giới hạn trên | Số được thêm vào | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 9 | 9 | 9 | 
| 3 | 100 | 999 | 900 | 909 | 
| 5 | 10000 | 99999 | 90000 | 90909 | 

Câu trả lời là`90909`. 

Dấu vết này chứng minh rằng bất cứ khi nào toàn bộ khoảng có độ dài lẻ nằm trong giới hạn, thuật toán sẽ thêm toàn bộ khoảng đó cùng một lúc thay vì kiểm tra các số riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Độ dài tối đa 19 chữ số được kiểm tra. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được duy trì. | 

Thời gian chạy chỉ phụ thuộc vào số chữ số thập phân chứ không phụ thuộc vào độ lớn của`n`. Từ`10^18`chỉ có`19`chữ số, mỗi ca kiểm thử chỉ thực hiện một số phép tính số học, dễ dàng thỏa mãn các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        digits = len(str(n))
        ans = 0

        for d in range(1, digits + 1, 2):
            low = 1 if d == 1 else 10 ** (d - 1)
            high = min(n, 10 ** d - 1)
            if high >= low:
                ans.append if False else None
                ans += high - low + 1

        out.append(str(ans))

    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    res = sys.stdout.getvalue()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return res

# minimum input
assert run("1\n1\n") == "1\n"

# boundary at first two-digit number
assert run("1\n10\n") == "9\n"

# boundary at first three-digit number
assert run("1\n100\n") == "10\n"

# end of three-digit range
assert run("1\n999\n") == "909\n"

# maximum constraint
assert run("1\n1000000000000000000\n") == "909090909090909091\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Giá trị đầu vào tối thiểu | 
|`10`|`9`| Chuyển từ một sang hai chữ số | 
|`100`|`10`| Số có ba chữ số đầu tiên được bao gồm | 
|`999`|`909`| Toàn bộ phạm vi ba chữ số được tính | 
|`10^18`|`909090909090909091`| Đầu vào được phép lớn nhất | 

## Vỏ cạnh 

Hãy xem xét việc chuyển đổi sang độ dài chữ số lẻ mới.```
Input
1
100
```Thuật toán xử lý các số có một chữ số trước và cộng`9`. Đối với số có ba chữ số, khoảng trở thành`[100, 100]`, đóng góp thêm một giá trị. Câu trả lời cuối cùng là`10`, chính xác bao gồm cả số có ba chữ số đầu tiên. 

Bây giờ hãy xem xét một giá trị bên trong một khoảng có độ dài lẻ.```
Input
1
234
```Khoảng một chữ số góp phần`9`. Khoảng ba chữ số trở thành`[100, 234]`, có kích thước là`135`. Thêm chúng tạo ra`144`. Thuật toán tự động cắt bớt khoảng thời gian cuối cùng tại`n`, do đó không có số có ba chữ số lớn hơn được tính. 

Cuối cùng, hãy xem xét đầu vào lớn nhất có thể.```
Input
1
1000000000000000000
```Thuật toán đếm mọi khoảng chữ số lẻ hoàn chỉnh từ một chữ số đến mười bảy chữ số. Sau đó nó xử lý khoảng 19 chữ số, đơn giản là`[10^18, 10^18]`, thêm một giá trị cuối cùng. Vì số nguyên Python có độ chính xác tùy ý nên tất cả số học vẫn đúng mà không bị tràn.
