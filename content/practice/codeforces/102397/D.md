---
title: "CF 102397D - Bashar và vùng đất xấu (Dễ)"
description: "Có n ngôi nhà xếp thành một hàng, ngôi nhà i chứa một đồng xu [i]. Bashar có thể chọn bất kỳ ngôi nhà nào làm điểm xuất phát và bất kỳ ngôi nhà nào làm điểm dừng của mình. Trong khi đi giữa họ, anh ấy ghé thăm từng ngôi nhà trên đoạn đường đó và thu thập tất cả tiền xu của nó."
date: "2026-08-10T17:58:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "D"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 414
verified: true
draft: false
---

[CF 102397D - Bashar và vùng đất xấu (Dễ)](https://codeforces.com/problemset/problem/102397/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 54 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`n`những ngôi nhà được sắp xếp thành một dòng và ngôi nhà`i`chứa`a[i]`tiền xu. Bashar có thể chọn bất kỳ ngôi nhà nào làm điểm xuất phát và bất kỳ ngôi nhà nào làm điểm dừng của mình. Trong khi đi giữa họ, anh ấy ghé thăm từng ngôi nhà trên đoạn đường đó và thu thập tất cả tiền xu của nó. 

Nhiệm vụ là tìm khoảng cách đi bộ tối thiểu cần thiết để thu thập ít nhất`x`tiền xu. Nếu phân đoạn đã chọn bắt đầu tại nhà`l`và kết thúc ở nhà`r`, quãng đường đi được của nó là`r - l`, vì các nhà liền kề cách nhau một căn. Nếu không có đoạn nào chứa đủ số xu thì câu trả lời là`-1`. 

Đầu vào chứa`x`Và`n`, theo sau là`n`giá trị tiền xu. Những ràng buộc có`n <= 1000`, vì vậy ngay cả một`O(n^2)`thuật toán thực hiện tối đa khoảng nửa triệu lượt kiểm tra phân đoạn, việc này có thể dễ dàng quản lý được. Ràng buộc nhỏ làm cho giải pháp bạo lực trở nên khả thi, nhưng cấu trúc của vấn đề cũng cho phép`O(n)`giải pháp. 

Thực tế là mỗi`a[i]`là tích cực là tài sản quan trọng. Việc thêm một ngôi nhà khác vào một đoạn chỉ có thể làm tăng tổng số của nó và việc loại bỏ một ngôi nhà ở bên trái chỉ có thể làm giảm tổng số ngôi nhà đó. Hành vi đơn điệu này cho phép chúng ta duy trì một khoảng chuyển động mà không cần xem xét lại mọi cặp điểm cuối có thể. 

Có một số trường hợp ranh giới có thể khiến việc triển khai không chính xác dẫn đến âm thầm trả về khoảng cách sai. Ví dụ: nếu một ngôi nhà đã có đủ tiền```
5 1
5
```câu trả lời là`0`, bởi vì Bashar có thể bắt đầu và dừng lại ở cùng một ngôi nhà. Một triển khai khởi tạo câu trả lời cho ít nhất`1`sẽ sai. 

Trường hợp thứ hai là khi toàn bộ mảng vẫn chưa đủ:```
6 5
1 1 1 1 1
```Tổng cộng chỉ có`5`, vì vậy đầu ra đúng là`-1`. Việc triển khai cửa sổ trượt bất cẩn có thể khiến câu trả lời ban đầu không thay đổi và in ra một khoảng cách không hợp lệ. 

Trường hợp thứ ba kiểm tra khoảng cách so với số lượng nhà. Vì```
7 3
2 5 1
```đoạn chứa hai ngôi nhà đầu tiên có`7`tiền xu. Nó có hai ngôi nhà, nhưng Bashar chỉ đi bộ từ nhà`1`đến nhà`2`, vậy câu trả lời là`1`, không`2`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi ngôi nhà bắt đầu có thể và mở rộng ngôi nhà kết thúc từng vị trí một. Đối với mỗi cặp điểm cuối, chúng ta có thể tính toán hoặc duy trì tổng và kiểm tra xem nó có đạt đến`x`. có`n(n+1)/2`các đoạn liền kề nhau, nên trường hợp xấu nhất có khoảng`500,500`phân đoạn khi`n = 1000`. Với tổng tiền tố, mỗi phân đoạn có thể được kiểm tra theo thời gian không đổi, đưa ra`O(n^2)`thuật toán. Điều này đúng vì mọi khoảng có thể đều được xem xét rõ ràng và khoảng hợp lệ nhỏ nhất được chọn. 

Phương pháp brute-force hoạt động vì câu trả lời được đảm bảo là một trong những phân đoạn liền kề. Tuy nhiên, nó thực hiện công việc không cần thiết vì có nhiều khoảng thời gian chồng chéo lên nhau. Nếu một khoảng`[l, r]`đã có đủ tiền, việc kéo dài nó ra xa hơn bên phải không thể cải thiện khoảng cách đi bộ của nó. Quan trọng hơn, vì tất cả các giá trị đồng xu đều dương, nên khi cửa sổ đạt đến số tiền yêu cầu, chúng ta có thể di chuyển ranh giới bên trái của nó về phía trước một cách an toàn và xem liệu nó có thể trở nên ngắn hơn hay không. 

Điều này mang lại giải pháp cửa sổ trượt. Chúng tôi duy trì một cửa sổ`[left, right]`và số tiền xu hiện tại của nó. BẰNG`right`tiến về phía trước, chúng tôi thêm ngôi nhà mới vào cửa sổ. Bất cứ khi nào tổng ít nhất là`x`, cửa sổ hiện tại hợp lệ, vì vậy chúng tôi ghi lại khoảng cách của nó và liên tục xóa các ngôi nhà ở bên trái trong khi cửa sổ vẫn hợp lệ. Quá trình này chỉ kiểm tra mỗi ngôi nhà một số lần không đổi, giảm độ phức tạp xuống còn`O(n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Được chấp nhận vì những ràng buộc dễ dàng | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`left = 0`,`current_sum = 0`, Và`answer = n`như một giá trị lớn hơn mọi khoảng cách đi bộ có thể. Cửa sổ sẽ đại diện cho những ngôi nhà từ`left`thông qua điểm cuối bên phải hiện tại. 
2. Di chuyển`right`từ`0`bởi vì`n - 1`. Thêm vào`a[right]`ĐẾN`current_sum`, vì điểm cuối bên phải mới vừa vào cửa sổ đang hoạt động. 
3. Trong khi`current_sum >= x`, cửa sổ hiện tại đã chứa đủ tiền. Quãng đường đi bộ của nó là`right - left`, vậy hãy cập nhật`answer`với kết quả nhỏ hơn của câu trả lời hiện tại và khoảng cách này. 
4. Xóa`a[left]`từ`current_sum`và tăng dần`left`. Điều này cố gắng làm cho cửa sổ hợp lệ ngắn hơn. Chúng ta tiếp tục thực hiện việc này trong khi cửa sổ còn lại vẫn chứa ít nhất`x`tiền xu. 
5. Sau khi xử lý mọi điểm cuối bên phải có thể, hãy kiểm tra xem`answer`đã được cập nhật. Nếu không, không có đoạn liền kề nào đạt được`x`, vì vậy hãy in`-1`; nếu không thì in`answer`. 

Lý do chúng tôi có thể loại bỏ vĩnh viễn điểm cuối bên trái cũ là vì tất cả các giá trị đồng xu đều dương. Một lần`[left, right]`có đủ tiền, giữ`left`cố định trong khi mở rộng`right`không bao giờ có thể tạo ra một khoảng thời gian ngắn hơn khoảng thời gian mà chúng ta đã tìm được. Di chuyển`left`về phía trước là cách duy nhất để cải thiện khoảng cách. 

### Tại sao nó hoạt động 

Tại mọi điểm,`current_sum`chính xác là tổng số ngôi nhà trong cửa sổ hiện tại`[left, right]`. Bất cứ khi nào số tiền đó đạt đến`x`, cửa sổ hợp lệ và chúng tôi ghi lại khoảng cách của nó. Sau đó chúng tôi di chuyển`left`chuyển tiếp càng xa càng tốt trong khi vẫn duy trì tính hợp lệ, vì vậy đối với trường hợp cụ thể này`right`, cửa sổ kết quả là cửa sổ hợp lệ ngắn nhất kết thúc tại`right`. Mọi điểm cuối bên phải có thể đều được xử lý, do đó phân đoạn hợp lệ ngắn nhất trên toàn cầu phải được xem xét. Vì tất cả các giá trị đều dương nên không có điểm cuối bên trái bị loại bỏ nào sau này có thể trở nên hữu ích sau khi điểm cuối bên phải di chuyển về phía trước, điều này làm cho thuật toán một lượt trở nên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, n = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0
    answer = n

    for right in range(n):
        current_sum += a[right]

        while current_sum >= x:
            answer = min(answer, right - left)
            current_sum -= a[left]
            left += 1

    if answer == n:
        print(-1)
    else:
        print(answer)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên ghi số xu yêu cầu và số nhà. Mảng sau đó được lưu trữ để cửa sổ trượt có thể xóa các giá trị khỏi ranh giới bên trái của nó trong thời gian không đổi. 

Vòng lặp chính thêm`a[right]`chính xác khi nào về nhà`right`đi vào cửa sổ. Bên trong`while`vòng lặp chỉ chạy khi đoạn hiện tại có đủ tiền. Trước khi gỡ bỏ`a[left]`, chúng tôi ghi lại khoảng cách hiện tại`right - left`, đó là khoảng cách giữa hai nhà điểm cuối. 

Thứ tự ở đây rất quan trọng. Chúng ta phải đánh giá cửa sổ hợp lệ trước khi loại bỏ ngôi nhà ngoài cùng bên trái của nó. Nếu không, giải pháp một nhà có thể bị bỏ qua. Ví dụ, nếu`a[left] >= x`, khoảng cách đúng là`0`, và giá trị đó phải được ghi lại trước khi ngôi nhà bị dỡ bỏ. 

Câu trả lời ban đầu là`n`. Khoảng cách tối đa có thể là`n - 1`, Vì thế`n`nằm ngoài phạm vi câu trả lời thực sự một cách an toàn. Sau khi quét, giá trị không thay đổi có nghĩa là không tìm thấy cửa sổ hợp lệ nào. 

Số nguyên Python không bị tràn và tổng số lớn nhất có thể chỉ là`1000 * 1000 = 10^6`Dẫu sao thì. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu được hiểu là`x = 12`,`n = 5`, với giá trị đồng xu`[1, 3, 4, 5, 2]`. 

| đúng | trái | current_sum trước khi thu nhỏ | Cửa sổ hợp lệ | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | Không | 5 | 
| 1 | 0 | 4 | Không | 5 | 
| 2 | 0 | 8 | Không | 5 | 
| 3 | 0 | 13 | Đúng,`[0,3]`| 3 | 
| 3 | 1 | 12 | Đúng,`[1,3]`| 2 | 
| 4 | 2 | 11 | Không | 2 | 

Khi`right = 3`, cửa sổ hợp lệ đầu tiên là`[0, 3]`, chứa`1 + 3 + 4 + 5 = 13`tiền xu và có khoảng cách`3`. Loại bỏ những lá nhà đầu tiên`[1, 3]`, tổng của nó chính xác là`12`, và khoảng cách của nó là`2`. 

Lời giải thích mẫu đã nêu mô tả việc đi bộ từ ngôi nhà thứ hai đến ngôi nhà thứ tư, tương ứng với cửa sổ cuối cùng này. Như vậy câu trả lời là`2`theo định nghĩa khoảng cách là sự khác biệt giữa các vị trí ngôi nhà. Đầu ra mẫu được hiển thị của câu lệnh được cung cấp bị hỏng trong lời nhắc, nhưng kết quả mong muốn từ chuyển động được mô tả là`2`. 

### Mẫu 2 

đây`x = 13`,`n = 5`, và mảng là`[5, 1, 2, 3, 4]`. 

| đúng | trái | current_sum trước khi thu nhỏ | Cửa sổ hợp lệ | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | Không | 5 | 
| 1 | 0 | 6 | Không | 5 | 
| 2 | 0 | 8 | Không | 5 | 
| 3 | 0 | 11 | Không | 5 | 
| 4 | 0 | 15 | Đúng,`[0,4]`| 4 | 
| 4 | 1 | 10 | Không | 4 | 

Phân đoạn hợp lệ duy nhất là toàn bộ mảng, vì việc loại bỏ ngôi nhà đầu tiên sẽ làm giảm tổng từ`15`ĐẾN`10`, dưới mức yêu cầu`13`. Khoảng cách từ nhà`1`đến nhà`5`là`4`. 

Ví dụ này chứng minh tại sao thuật toán không thu nhỏ cửa sổ một cách mù quáng. Nó chỉ loại bỏ ngôi nhà ngoài cùng bên trái sau khi ghi lại khoảng thời gian hợp lệ hiện tại và nó dừng ngay lập tức khi việc loại bỏ một ngôi nhà khác sẽ khiến số tiền quá nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) |`right`tiến bộ`n`lần, trong khi`left`cũng tiến bộ nhiều nhất`n`lần. | 
| Không gian | O(n) | Mảng đầu vào được lưu trữ; bản thân trạng thái cửa sổ trượt sử dụng thêm không gian O(1). | 

Với`n <= 1000`, điều này thoải mái trong giới hạn nhất định. Ngay cả giải pháp bậc hai cũng sẽ phù hợp với các ràng buộc dễ dàng, nhưng giải pháp tuyến tính sẽ đơn giản hơn khi thuộc tính giá trị dương được nhận ra và cũng có tỷ lệ tự nhiên với phiên bản khó hơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    x, n = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0
    answer = n

    for right in range(n):
        current_sum += a[right]

        while current_sum >= x:
            answer = min(answer, right - left)
            current_sum -= a[left]
            left += 1

    print(-1 if answer == n else answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    output = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output

# Provided samples, reconstructed from the formatting in the statement.
assert run("12 5\n1 3 4 5 2\n") == "2", "sample 1"
assert run("13 5\n5 1 2 3 4\n") == "4", "sample 2"
assert run("6 5\n1 1 1 1 1\n") == "-1", "sample 3"

# Minimum-size input, one house is already enough.
assert run("7 1\n7\n") == "0", "single house"

# One house in the middle is enough, catching endpoint handling.
assert run("5 3\n2 5 1\n") == "0", "single-house window"

# The answer requires the last two houses.
assert run("7 4\n1 1 4 3\n") == "1", "last two houses"

# Every house is needed, so the answer is n - 1.
assert run("10 4\n1 2 3 4\n") == "3", "entire array"

# Maximum-size input, all equal values.
assert run("1000 1000\n" + "1 " * 999 + "1\n") == "999", "maximum size"

# Exact boundary: a segment reaches x exactly.
assert run("9 5\n2 3 4 1 1\n") == "2", "exact sum"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 1 / 7`|`0`| Đầu vào có kích thước tối thiểu và khoảng cách đi bộ bằng không | 
|`5 3 / 2 5 1`|`0`| Một ngôi nhà ở giữa là đủ | 
|`7 4 / 1 1 4 3`|`1`| Xử lý đúng hai căn nhà cuối cùng | 
|`10 4 / 1 2 3 4`|`3`| Toàn bộ mảng là bắt buộc | 
|`1000 1000 / all ones`|`999`| Kích thước đầu vào tối đa và câu trả lời lớn nhất có thể | 
|`9 5 / 2 3 4 1 1`|`2`| Tổng chính xác và thu hẹp ranh giới | 

## Vỏ cạnh 

Một ngôi nhà chứa đủ tiền phải tạo ra khoảng cách`0`. Vì```
7 1
7
```thuật toán thêm giá trị duy nhất, thấy rằng tổng ít nhất là`7`, hồ sơ`right - left = 0`, rồi loại bỏ ngôi nhà đó. Câu trả lời cuối cùng là`0`. 

Nếu tổng số vàng không đủ thì sẽ không có cửa sổ nào lọt vào vòng lặp thu hẹp. Vì```
6 5
1 1 1 1 1
```số tiền cuối cùng chỉ đạt`5`, Vì thế`answer`vẫn bằng giá trị ban đầu`n`. Thuật toán in`-1`. 

Một cửa sổ hợp lệ có thể xuất hiện ở cuối mảng. Vì```
7 4
1 1 4 3
```hai ngôi nhà cuối cùng chứa`4 + 3 = 7`. Khi`right = 3`, cửa sổ cuối cùng co lại thành`[2, 3]`, và khoảng cách được ghi là`3 - 2 = 1`. Điều này phát hiện các quá trình triển khai vô tình dừng lại trước khi xử lý nội dung cuối cùng. 

Khoảng cách ít hơn một số nhà trong một đoạn. Vì```
10 4
1 2 3 4
```tất cả bốn ngôi nhà đều được yêu cầu vì tổng số của chúng chính xác là`10`. Bản ghi thuật toán`right - left = 3`, không`4`. Sự khác biệt này rất cần thiết vì Bashar đi lại giữa các ngôi nhà thay vì đếm chính các ngôi nhà. 

Cuối cùng, kết quả khớp chính xác phải được coi là hợp lệ. Vì```
9 5
2 3 4 1 1
```ba ngôi nhà đầu tiên chứa chính xác`9`tiền xu, do đó thuật toán ghi lại khoảng cách`2`. sử dụng`>`thay vì`>=`sẽ từ chối phân đoạn này một cách không chính xác và là lỗi ranh giới phổ biến khi triển khai cửa sổ trượt.
