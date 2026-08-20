---
title: "CF 102189C - Trình tạo nhật ký thay đổi"
description: "Chúng tôi so sánh giá trị cũ của một số thông số trò chơi với giá trị mới của chúng sau một bản vá. Đối với mỗi tham số, giá trị cũ là a[i] và giá trị mới là b[i]. Toàn bộ thay đổi nhận được chính xác một nhãn dựa trên cách mọi tham số hoạt động."
date: "2026-08-19T16:08:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102189
codeforces_index: "C"
codeforces_contest_name: "12-\u0439 \u043e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0442\u0443\u0440\u043d\u0438\u0440 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0432 \u0410\u0431\u0430\u043a\u0430\u043d\u0435"
rating: 0
weight: 102189
solve_time_s: 179
verified: true
draft: false
---

[CF 102189C - Trình tạo nhật ký thay đổi](https://codeforces.com/problemset/problem/102189/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi so sánh giá trị cũ của một số thông số trò chơi với giá trị mới của chúng sau một bản vá. Với mỗi tham số, giá trị cũ là`a[i]`và giá trị mới là`b[i]`. Toàn bộ thay đổi nhận được chính xác một nhãn dựa trên cách mọi tham số hoạt động. 

Nếu mọi cặp đều bằng nhau thì kết quả là`Unchanged`. Nếu không có tham số nào giảm thì kết quả là`Increased`, ngay cả khi một số tham số vẫn giữ nguyên. Đối xứng, nếu không có tham số nào tăng lên thì kết quả là`Reduced`. Nếu ít nhất một tham số tăng và ít nhất một tham số giảm thì kết quả là`Rescaled`. 

Thứ tự của những lần kiểm tra này rất quan trọng.`Unchanged`thỏa mãn cả hai điều kiện`Increased`và điều kiện để`Reduced`, vì vậy nó phải được công nhận trước tiên. Tổng quát hơn, bốn loại này được xác định hoàn toàn bằng việc chúng ta thấy tăng hay giảm. 

Số lượng tham số nhiều nhất là 1000 nên ngay cả quét tuyến tính cũng chỉ thực hiện được khoảng một nghìn phép so sánh. Các giá trị có thể lớn như`10^9`, nhưng số nguyên Python xử lý trực tiếp các giá trị này và thuật toán không bao giờ thực hiện số học có thể trở nên lớn. Không có lý do gì để sắp xếp các mảng, thử kết hợp hoặc xây dựng bất kỳ cấu trúc phụ trợ nào. 

Trường hợp cạnh chính là một mảng trong đó tất cả các giá trị đều bằng nhau. Ví dụ,```
3
5 5 5
5 5 5
```sản xuất`Unchanged`. Việc thực hiện bất cẩn để kiểm tra`b[i] >= a[i]`đầu tiên sẽ gọi sai cái này`Increased`, bởi vì sự bình đẳng được cho phép ở đó.`Unchanged`phải được kiểm tra trước khi phân loại đơn điệu. 

Một trường hợp ranh giới khác là sự kết hợp giữa tăng và bằng:```
3
5 7 9
5 8 9
```Kết quả là`Increased`. Tham số thứ nhất và thứ ba không thay đổi, trong khi tham số thứ hai tăng lên. Việc yêu cầu mọi tham số tăng nghiêm ngặt sẽ từ chối trường hợp này một cách không chính xác. 

Vấn đề tương tự xảy ra đối với việc giảm:```
3
9 7 5
9 6 5
```Câu trả lời đúng là`Reduced`, vì không có tham số nào tăng và một tham số giảm. Một sự so sánh chặt chẽ như`b[i] < a[i]`đối với mọi vị trí sẽ từ chối nó một cách không chính xác vì hai vị trí không thay đổi. 

Cuối cùng, chỉ cần tăng và giảm một lần là đủ`Rescaled`:```
3
10 20 30
11 19 30
```Tham số thứ ba không thay đổi không thành vấn đề. Vì cả hai hướng đều xảy ra nên không`Increased`cũng không`Reduced`là có thể. 

## Phương pháp tiếp cận 

Một cách tiếp cận hoàn toàn đầy đủ có thể coi mọi tham số đều có một trong bốn mối quan hệ cục bộ có thể có, chẳng hạn như không thay đổi, tăng, giảm hoặc trạng thái khác và liệt kê tất cả các kết hợp có thể có trước khi quyết định phân loại chung nào phù hợp. Với`n`các tham số, tạo ra nhiều kết hợp theo cấp số nhân, lên đến`4^n`. Ở mức tối đa`n = 1000`, đây là đại khái`2^2000`, vượt xa mọi thứ mà chương trình một giây có thể xử lý. Cách tiếp cận này đúng về nguyên tắc vì nó xem xét mọi tập hợp thay đổi cục bộ có thể có, nhưng nó khám phá thông tin mà câu trả lời không thực sự phụ thuộc vào. 

Việc triển khai ngây thơ hợp lý hơn sẽ kiểm tra bốn loại một cách độc lập. Nó có thể quét các mảng một lần để kiểm tra`Unchanged`, quét lại để kiểm tra`Increased`, quét lại để tìm`Reduced`, và cuối cùng quyết định`Rescaled`. Điều này đã đủ nhanh cho ràng buộc nhất định, tối đa là`4n = 4000`so sánh cặp, do đó không có vấn đề hiệu suất thực sự trong giải pháp ngây thơ thực tế. 

Quan sát hữu ích là chúng ta không cần biết giá trị chính xác của những thay đổi. Chúng ta chỉ cần hai sự thật: có tham số nào tăng hay không và có tham số nào giảm hay không. Trong khi quét một cặp`(a[i], b[i])`,`b[i] > a[i]`chứng tỏ có sự gia tăng, trong khi`b[i] < a[i]`chứng tỏ có sự giảm sút. Sự bình đẳng không góp phần vào thực tế nào. 

Điều đó làm giảm toàn bộ phân loại thành hai thuộc tính boolean. Nếu không có thuộc tính nào xảy ra thì tất cả các giá trị đều bằng nhau. Nếu chỉ có thuộc tính tăng xảy ra thì sự thay đổi là`Increased`. Nếu chỉ có tính chất giảm xảy ra thì đó là`Reduced`. Nếu cả hai đều xảy ra thì đó là`Rescaled`. 

Do đó, việc triển khai tối ưu sẽ thực hiện một lượt và ghi lại xem mỗi hướng đã xuất hiện hay chưa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê đầy đủ | O(4^n) | O(n) | Quá chậm | 
| Kiểm tra danh mục riêng biệt | O(n) | O(n) | Đã chấp nhận | 
| Phân loại một lần | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`, theo sau là mảng cũ`a`và mảng mới`b`. Hai mảng có độ dài bằng nhau nên vị trí`i`luôn so sánh cùng một tham số trước và sau bản vá. 
2. Khởi tạo hai biến boolean,`increased`Và`reduced`, ĐẾN`False`. Chúng thể hiện liệu chúng ta đã gặp phải ít nhất một lần tăng hoặc giảm nghiêm ngặt hay chưa. 
3. Quét từng cặp`(a[i], b[i])`. Nếu như`b[i] > a[i]`, bộ`increased`ĐẾN`True`. Nếu như`b[i] < a[i]`, bộ`reduced`ĐẾN`True`. Bình đẳng không thay đổi cờ vì tham số không thay đổi tương thích với cả hai phân loại đơn điệu. 
4. Nếu cả hai cờ đều`False`, in`Unchanged`. Không có vị trí nào thay đổi, vì vậy đây là cách phân loại duy nhất có thể. 
5. Nếu`increased`là`True`Và`reduced`là`False`, in`Increased`. Mọi tham số đều được giữ nguyên hoặc tăng lên, đó chính xác là điều kiện bắt buộc. 
6. Nếu`reduced`là`True`Và`increased`là`False`, in`Reduced`. Mọi thông số đều giữ nguyên hoặc giảm. 
7. Nếu cả hai cờ đều`True`, in`Rescaled`. Ít nhất một tham số được di chuyển theo mỗi hướng, do đó không thể áp dụng phân loại đơn điệu. 

Điều bất biến là sau khi xử lý lần đầu tiên`k`thông số,`increased`đúng khi có ít nhất một trong số đó`k`các thông số tăng lên và`reduced`đúng khi có ít nhất một giảm. Việc xử lý cặp tiếp theo sẽ cập nhật chính xác thuộc tính có thể thay đổi. Rốt cuộc`n`các cặp đã được xử lý, hai cờ mô tả đầy đủ hành vi chung nên việc phân loại cuối cùng không thể sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    increased = False
    reduced = False

    for x, y in zip(a, b):
        if y > x:
            increased = True
        elif y < x:
            reduced = True

    if not increased and not reduced:
        print("Unchanged")
    elif increased and not reduced:
        print("Increased")
    elif reduced and not increased:
        print("Reduced")
    else:
        print("Rescaled")

if __name__ == "__main__":
    solve()
```Đầu vào được đọc dưới dạng hai mảng số nguyên vì mỗi vị trí trong`a`tương ứng trực tiếp với cùng một vị trí trong`b`. Không có nhiều trường hợp thử nghiệm trong vấn đề này, vì vậy`solve()`được gọi đúng một lần. 

Vòng lặp sử dụng`zip(a, b)`để so sánh các tham số tương ứng mà không cần quản lý chỉ mục theo cách thủ công. Đối với mỗi cặp,`if`Và`elif`các nhánh loại trừ lẫn nhau. Một cặp bằng nhau không thực hiện nhánh nào, đó chính xác là những gì chúng ta cần. 

Quyết định bốn chiều cuối cùng sẽ kiểm tra hai lá cờ. Trường hợp cả hai đều sai được kiểm tra trước để phân biệt`Unchanged`từ`Increased`Và`Reduced`. Không có mối quan tâm riêng lẻ nào bởi vì`zip`xử lý chính xác vị trí tương ứng của hai mảng. 

Không cần số học số nguyên ngoài việc so sánh, vì vậy`10^9`giá trị bị ràng buộc không gây ra vấn đề tràn. Bản thân các mảng yêu cầu bộ nhớ O(n), trong khi trạng thái phân loại chỉ sử dụng hai boolean bổ sung. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên:```
4
55 50 45 40
50 45 40 35
```Mỗi giá trị mới đều nhỏ hơn giá trị cũ tương ứng. 

| Tham số | Cũ | Mới | tăng | giảm | 
| --- | --- | --- | --- | --- | 
| 1 | 55 | 50 | Sai | Đúng | 
| 2 | 50 | 45 | Sai | Đúng | 
| 3 | 45 | 40 | Sai | Đúng | 
| 4 | 40 | 35 | Sai | Đúng | 

Cuối cùng,`increased`là sai và`reduced`là đúng, vậy câu trả lời là`Reduced`. Dấu vết cho thấy mức giảm lặp đi lặp lại không cần phải đếm chúng. Một mức giảm là đủ để đặt cờ và các mức giảm bổ sung sẽ không thay đổi phân loại. 

Bây giờ hãy xem xét mẫu thứ hai:```
3
550 675 800
600 700 800
```Hai tham số đầu tiên tăng và tham số cuối cùng không thay đổi. 

| Tham số | Cũ | Mới | tăng | giảm | 
| --- | --- | --- | --- | --- | 
| 1 | 550 | 600 | Đúng | Sai | 
| 2 | 675 | 700 | Đúng | Sai | 
| 3 | 800 | 800 | Đúng | Sai | 

Trạng thái cuối cùng là`increased = True`Và`reduced = False`, vậy câu trả lời là`Increased`. Tham số cuối cùng không thay đổi không ngăn cản kết quả`Increased`, vì định nghĩa cho phép các tham số giữ nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cặp tham số được kiểm tra chính xác một lần. | 
| Không gian | O(n) | Hai mảng đầu vào chứa`n`giá trị từng giá trị; bản thân việc phân loại sử dụng không gian bổ sung O(1). | 

Với tối đa 1000 tham số, thuật toán chỉ thực hiện một số phép so sánh tuyến tính. Nó thoải mái trong giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def classify(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    b = list(map(int, sys.stdin.readline().split()))

    increased = False
    reduced = False

    for x, y in zip(a, b):
        if y > x:
            increased = True
        elif y < x:
            reduced = True

    if not increased and not reduced:
        print("Unchanged")
    elif increased and not reduced:
        print("Increased")
    elif reduced and not increased:
        print("Reduced")
    else:
        print("Rescaled")

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided samples
assert classify("""\
4
55 50 45 40
50 45 40 35
""") == "Reduced", "sample 1"

assert classify("""\
3
550 675 800
600 700 800
""") == "Increased", "sample 2"

assert classify("""\
4
50 55 60 65
40 50 60 70
""") == "Rescaled", "sample 3"

assert classify("""\
3
1 2 3
1 2 3
""") == "Unchanged", "sample 4"

# Minimum size
assert classify("""\
1
0
0
""") == "Unchanged", "single unchanged parameter"

# Single strict increase at the boundary
assert classify("""\
1
0
1000000000
""") == "Increased", "maximum value increase"

# Single strict decrease at the boundary
assert classify("""\
1
1000000000
0
""") == "Reduced", "maximum value decrease"

# Increase, equality, and decrease together
assert classify("""\
3
0 500000000 1000000000
1 500000000 999999999
""") == "Rescaled", "both directions with equality in the middle"

# Maximum n, all equal
n = 1000
values = " ".join(["1000000000"] * n)
assert classify(f"{n}\n{values}\n{values}\n") == "Unchanged", \
    "maximum n with all values equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 / 0`|`Unchanged`| Kích thước đầu vào tối thiểu và sự bằng nhau | 
|`1 / 0 / 1000000000`|`Increased`| Ranh giới giá trị và tăng phần tử đơn | 
|`1 / 1000000000 / 0`|`Reduced`| Giảm phần tử đơn và ranh giới giá trị | 
|`3 / 0 500000000 1000000000 / 1 500000000 999999999`|`Rescaled`| Cả hai hướng cộng với tham số không thay đổi | 
| 1000 giá trị bằng nhau |`Unchanged`| Tối đa`n`và đầu vào hoàn toàn bằng nhau | 

## Vỏ cạnh 

Đối với trường hợp đều bằng nhau```
3
5 5 5
5 5 5
```mọi so sánh đều ngầm hiểu nhánh đẳng thức, vì vậy cả hai cờ vẫn sai trong suốt quá trình quét. Điều kiện cuối cùng`not increased and not reduced`sản xuất`Unchanged`. Đây là lý do tại sao việc kiểm tra`Unchanged`thông qua điều kiện đẳng thức rõ ràng là không cần thiết khi sử dụng hai cờ. 

Đối với sự gia tăng trộn lẫn với các thông số không thay đổi,```
3
5 7 9
5 8 9
```cặp đầu tiên giữ nguyên cả hai lá cờ, bộ thứ hai`increased`thành true và điều thứ ba không thay đổi gì. Từ`reduced`vẫn sai, câu trả lời là`Increased`. Thuật toán không yêu cầu mọi tham số phải thay đổi một cách chính xác. 

Đối với trường hợp giảm tương ứng,```
3
9 7 5
9 6 5
```cặp thứ nhất bằng nhau, cặp thứ hai`reduced`thành true và giá trị thứ ba bằng nhau. Trạng thái cuối cùng là`increased = False`,`reduced = True`, cho`Reduced`. 

Để thay đổi kích thước,```
3
10 20 30
11 19 30
```bộ cặp đầu tiên`increased`, bộ thứ hai`reduced`, và thứ ba không làm gì cả. Khi cả hai cờ đều đúng, phân loại cuối cùng là`Rescaled`. Tham số thứ ba không thay đổi không thể hoàn tác cả hai thực tế vì sự tồn tại của một mức tăng và một mức giảm mới là vấn đề quan trọng. 

Ranh giới giá trị tối đa hoạt động giống hệt với các giá trị thông thường. Vì```
1
0
1000000000
```sự so sánh`1000000000 > 0`bộ`increased`, cho`Increased`. Đối với đầu vào ngược, cờ giảm được đặt và kết quả là`Reduced`. Vì giải pháp chỉ so sánh các số nguyên nên không cần xử lý đặc biệt cho các điểm cuối`0`Và`10^9`.
