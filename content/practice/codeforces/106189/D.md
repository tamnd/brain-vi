---
title: "CF 106189D - Dãy con dao động"
description: "Chúng ta được cấp một dãy số nguyên và có thể xóa bất kỳ phần tử nào trong khi vẫn giữ nguyên thứ tự của các phần tử còn lại. Mục tiêu là giữ lại càng nhiều phần tử càng tốt sao cho trong dãy con thu được, mọi phần tử bên trong đều là một đỉnh nghiêm ngặt hoặc một thung lũng nghiêm ngặt."
date: "2026-06-25T06:47:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106189
codeforces_index: "D"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2025"
rating: 0
weight: 106189
solve_time_s: 39
verified: true
draft: false
---

[CF 106189D - Chuỗi con dao động](https://codeforces.com/problemset/problem/106189/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và có thể xóa bất kỳ phần tử nào trong khi vẫn giữ nguyên thứ tự của các phần tử còn lại. 

Mục tiêu là giữ lại càng nhiều phần tử càng tốt sao cho trong dãy con thu được, mọi phần tử bên trong đều là một đỉnh nghiêm ngặt hoặc một thung lũng nghiêm ngặt. Đối với mọi vị trí bên trong dãy con, giá trị của nó phải lớn hơn cả hai vị trí lân cận hoặc nhỏ hơn cả hai vị trí lân cận. Nói cách khác, sự khác biệt giữa các phần tử liên tiếp phải xen kẽ nhau về dấu. 

Đầu ra không phải là chuỗi con. Chúng ta phải in chỉ mục của các phần tử đã chọn trong mảng ban đầu. Nếu không tồn tại dãy con dao động có độ dài ít nhất bằng ba thì chúng ta in`0`. 

Độ dài mảng có thể đạt tới 50.000. Trong trường hợp xấu nhất, một giải pháp quy hoạch động bậc hai sẽ yêu cầu khoảng 2,5 tỷ phép so sánh, vượt xa thời gian sẵn có. Bất cứ điều gì xung quanh`O(n²)`được loại trừ ngay lập tức. Tuyến tính hoặc`O(n log n)`giải pháp là cần thiết. 

Các trường hợp phức tạp đến từ các giá trị bằng nhau và các chuỗi dài đều đều. 

Coi như:```
3
1 1 2
```Câu trả lời đúng là`0`. Việc triển khai bất cẩn chỉ kiểm tra mức tăng so với mức giảm có thể coi hai giá trị đầu tiên là một sự thay đổi hướng hợp lệ không chính xác. 

Coi như:```
5
1 2 3 4 5
```Mọi dãy số đều đơn điệu. Không tồn tại dao động có độ dài ít nhất bằng ba, nên câu trả lời là`0`. 

Coi như:```
5
1 5 3 4 2
```Toàn bộ chuỗi đã thay thế:```
1 < 5 > 3 < 4 > 2
```Câu trả lời chứa tất cả năm chỉ số. 

Coi như:```
6
1 4 7 5 2 6
```Các giá trị`4`Và`5`cả hai đều không cần thiết. Dãy con dao động dài nhất chỉ giữ lại các điểm ngoặt:```
1 < 7 > 2 < 6
```Một chiến lược ngây thơ giữ mọi thay đổi về giá trị sẽ không tạo ra một dao động hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là lập trình động. 

Đối với mọi vị trí và mọi vị trí trước đó, chúng ta có thể cố gắng mở rộng một chuỗi con trong khi ghi nhớ xem bước cuối cùng là tăng hay giảm. Điều này tìm ra một cách chính xác dãy con dao động dài nhất, nhưng nó đòi hỏi phải kiểm tra tất cả các cặp chỉ số. Với`n = 50,000`, công việc trong trường hợp xấu nhất là theo thứ tự`50,000²`, điều đó hoàn toàn không thể thực hiện được. 

Nhận xét quan trọng là trong một lượt chạy tăng dần, chỉ có yếu tố cuối cùng là quan trọng. Giả sử chúng ta hiện có:```
... < 3 < 5 < 8
```Nếu chúng ta muốn giảm trong tương lai, hãy giữ`8`ít nhất luôn tốt bằng việc giữ`5`hoặc`3`, bởi vì`8`mang lại nhiều tự do hơn cho bước đi xuống tiếp theo. 

Lý do tương tự cũng áp dụng cho việc giảm số lần chạy. Bên trong```
... > 10 > 7 > 4
```chỉ có điểm cuối nhỏ nhất là hữu ích. 

Điều này có nghĩa là mọi phân đoạn đơn điệu đều có thể được nén đến điểm cuối của nó. Những yếu tố tồn tại chính xác là những bước ngoặt, đỉnh cao và thung lũng. Một thuật toán tham lam có thể duy trì điều này trực tuyến. 

Chúng tôi xây dựng các chỉ số câu trả lời từ trái sang phải. 

Bất cứ khi nào ba giá trị được xem xét cuối cùng là đơn điệu:```
x ≤ y ≤ z
```hoặc```
x ≥ y ≥ z
```giá trị trung bình`y`không bao giờ có thể là đỉnh hoặc đáy trong dãy con dao động tối ưu. Chúng tôi loại bỏ nó và giữ điểm cuối cực đoan hơn. 

Sau khi xử lý toàn bộ mảng, các chỉ số còn lại tạo thành một dãy con dao động có độ dài tối đa. Đây chính là nguyên lý tham lam được sử dụng trong bài toán dãy con dao động cổ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(n²) | O(n2) hoặc O(n) | Quá chậm | 
| Bước ngoặt tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu danh sách câu trả lời bằng chỉ mục`0`. 
2. Xử lý chỉ số từ trái qua phải. 
3. Đối với chỉ mục hiện tại`i`, kiểm tra hai chỉ số được chọn cuối cùng và`i`. 
4. Hãy để giá trị của chúng là`x`,`y`, Và`z`. 
5. Nếu`x ≤ y ≤ z`, sau đó`y`nằm trong đoạn không giảm. Di dời`y`và thay thế nó bằng`i`. 
6. Nếu`x ≥ y ≥ z`, sau đó`y`nằm trong đoạn không tăng. Di dời`y`và thay thế nó bằng`i`. 
7. Nếu không, hướng thay đổi giữa`y`Và`z`. điểm`y`trở thành một đỉnh hoặc thung lũng, vì vậy hãy nối thêm`i`. 
8. Sau khi tất cả các chỉ số được xử lý, các chỉ số được lưu trữ sẽ mô tả một chuỗi con dao động dài nhất. 
9. Nếu còn lại ít hơn ba chỉ số, hãy in`0`. Nếu không thì in độ dài và các chỉ số. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của mảng, các chỉ mục được lưu trữ sẽ tạo thành chuỗi con dao động dài nhất có thể kết thúc bên trong tiền tố đó. 

Bất cứ khi nào ba giá trị ứng viên liên tiếp thỏa mãn```
x ≤ y ≤ z
```giá trị trung bình không bao giờ hữu ích. Bất kỳ yếu tố nào trong tương lai có thể theo sau`y`cũng có thể làm theo`z`, Và`z`ít nhất là cực đoan như vậy. Thay thế`y`với`z`không thể giảm chiều dài dao động tối đa có thể đạt được. 

Lập luận tương tự cũng đúng đối với```
x ≥ y ≥ z
```trong đó giá trị mới nhất ít nhất cũng hữu ích như giá trị ở giữa. 

Kết quả là mọi phần tử bị loại bỏ đều có vẻ dư thừa. Mỗi yếu tố bên trong được giữ lại tương ứng với một sự thay đổi hướng thực sự, khiến nó trở thành đỉnh hoặc thung lũng. Vì chỉ những điểm thừa bị loại bỏ nên chuỗi con thu được có độ dài tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

ans = [0]

for i in range(1, n):
    while len(ans) >= 2:
        x = a[ans[-2]]
        y = a[ans[-1]]
        z = a[i]

        if (x <= y <= z) or (x >= y >= z):
            ans.pop()
        else:
            break

    ans.append(i)

if len(ans) < 3:
    print(0)
else:
    print(len(ans))
    print(*ans)
```Danh sách`ans`lưu trữ các chỉ số, không phải giá trị, vì bài toán yêu cầu các vị trí trong chuỗi ban đầu. 

các`while`vòng lặp là trái tim của chiến lược tham lam. Nếu giá trị mới nhất tiếp tục xu hướng đơn điệu hiện tại thì điểm cuối trước đó không còn hữu ích và bị xóa. 

Sử dụng một`while`thay vì một`if`là quan trọng. Sau khi loại bỏ một chỉ số, một bộ ba mới được hình thành và cũng có thể đơn điệu. Quá trình tiếp tục cho đến khi điểm được chọn cuối cùng trở thành một bước ngoặt thực sự hoặc chỉ còn lại một điểm trước đó. 

Các giá trị bằng nhau được xử lý một cách tự nhiên bởi các điều kiện```
x <= y <= z
x >= y >= z
```Điều này ngăn các đoạn phẳng tạo ra dao động giả. 

Mỗi chỉ mục được chèn một lần và bị xóa nhiều nhất một lần, do đó tổng thời gian chạy vẫn giữ nguyên tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 4 7 5 2
```| Chỉ số hiện tại | Giá trị | Chỉ số được lưu trữ | Giá trị được lưu trữ | 
| --- | --- | --- | --- | 
| 0 | 1 | [0] | [1] | 
| 1 | 4 | [0,1] | [1,4] | 
| 2 | 7 | [0,2] | [1,7] | 
| 3 | 5 | [0,2,3] | [1,7,5] | 
| 4 | 2 | [0,2,4] | [1,7,2] | 

giá trị`4`biến mất vì nó nằm trong một đường chạy tăng dần. Sau đó,`5`biến mất vì nó nằm trong một đường chạy giảm dần. Những yếu tố còn sót lại chính là những bước ngoặt. 

### Ví dụ 2 

đầu vào:```
6
1 3 2 4 3 5
```| Chỉ số hiện tại | Giá trị | Chỉ số được lưu trữ | Giá trị được lưu trữ | 
| --- | --- | --- | --- | 
| 0 | 1 | [0] | [1] | 
| 1 | 3 | [0,1] | [1,3] | 
| 2 | 2 | [0,1,2] | [1,3,2] | 
| 3 | 4 | [0,1,2,3] | [1,3,2,4] | 
| 4 | 3 | [0,1,2,3,4] | [1,3,2,4,3] | 
| 5 | 5 | [0,1,2,3,4,5] | [1,3,2,4,3,5] | 

Mọi phần tử bên trong đều đã là đỉnh hoặc đáy nên không có gì bị loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được đẩy một lần và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | Các chỉ số câu trả lời được lưu trữ | 

Với`n = 50,000`, quét tuyến tính dễ dàng đủ nhanh cho các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    ans = [0]

    for i in range(1, n):
        while len(ans) >= 2:
            x = a[ans[-2]]
            y = a[ans[-1]]
            z = a[i]

            if (x <= y <= z) or (x >= y >= z):
                ans.pop()
            else:
                break

        ans.append(i)

    out = []

    if len(ans) < 3:
        out.append("0")
    else:
        out.append(str(len(ans)))
        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# sample-like cases
assert run("3\n1 3 2\n") == "3\n0 1 2"
assert run("3\n1 2 3\n") == "0"

# minimum size
assert run("1\n5\n") == "0"

# all equal
assert run("5\n7 7 7 7 7\n") == "0"

# already oscillating
assert run("5\n1 5 2 6 3\n") == "5\n0 1 2 3 4"

# long monotone run compressed to endpoints
assert run("6\n1 4 7 5 2 6\n") == "4\n0 2 4 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`0`| Kích thước tối thiểu | 
|`7 7 7 7 7`|`0`| Giá trị bằng nhau không tạo ra dao động | 
|`1 5 2 6 3`| Tất cả các chỉ số | Trình tự đã tối ưu | 
|`1 4 7 5 2 6`| Chỉ những bước ngoặt | Nén các đoạn đơn điệu | 

## Vỏ cạnh 

Hãy xem xét:```
3
1 1 2
```Xử lý`2`tạo ra mô hình:```
1 ≤ 1 ≤ 2
```Chỉ số ở giữa được loại bỏ. Bộ chỉ mục cuối cùng trở thành:```
[0, 2]
```Độ dài của nó chỉ là hai, do đó thuật toán in`0`. Các giá trị bằng nhau không bao giờ tạo ra đỉnh hoặc đáy sai. 

Coi như:```
3
1 2 3
```Mô hình luôn tăng:```
1 ≤ 2 ≤ 3
```Điểm giữa bị loại bỏ và chỉ còn lại điểm cuối. Một lần nữa độ dài cuối cùng là hai, vì vậy câu trả lời là`0`. 

Coi như:```
6
5 4 3 2 1 0
```Mỗi giá trị mới tiếp tục xu hướng giảm tương tự. Thuật toán liên tục loại bỏ điểm cuối trước đó và giữ lại điểm cuối mới nhất. Dãy con cuối cùng chỉ chứa chỉ số đầu tiên và cuối cùng, điều này đúng vì không tồn tại dao động có độ dài ít nhất là ba. 

Coi như:```
5
1 3 2 4 1
```Mọi phần tử bên trong đều đã là đỉnh hoặc đáy nghiêm ngặt:```
1 < 3 > 2 < 4 > 1
```Không có điều kiện đơn điệu nào được kích hoạt, tất cả các chỉ số vẫn được giữ nguyên và thuật toán trả về chuỗi đầy đủ.
