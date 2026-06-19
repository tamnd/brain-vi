---
title: "CF 100A - Trải thảm phòng"
description: "Chúng ta có một căn phòng hình vuông có cạnh dài n nên tổng diện tích là n × n. Chúng ta cũng có k tấm thảm hình vuông, mỗi tấm có chiều dài cạnh n1. Mọi tấm thảm luôn được căn chỉnh theo trục vì việc xoay bị cấm, nhưng vì các tấm thảm có hình vuông nên việc xoay sẽ không thực sự thay đổi bất cứ điều gì."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "A"
codeforces_contest_name: "Unknown Language Round 3"
rating: 1100
weight: 100
solve_time_s: 139
verified: true
draft: false
---

[CF 100A - Trải thảm trong phòng](https://codeforces.com/problemset/problem/100/A) 

**Đánh giá:** 1100 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một căn phòng hình vuông có cạnh dài`n`, vậy tổng diện tích là`n × n`. Chúng tôi cũng có`k`thảm vuông, mỗi cạnh có chiều dài`n1`. Mọi tấm thảm luôn được căn chỉnh theo trục vì việc xoay bị cấm, nhưng vì các tấm thảm có hình vuông nên việc xoay sẽ không thực sự thay đổi bất cứ điều gì. 

Câu hỏi đặt ra là liệu những tấm thảm này có thể bao phủ hoàn toàn căn phòng hay không. Thảm có thể chồng lên nhau nên có thể sử dụng thêm diện tích. Điều quan trọng duy nhất là liệu mọi điểm trong căn phòng có thể được che phủ bởi ít nhất một tấm thảm hay không. 

Những hạn chế là cực kỳ nhỏ. Độ dài các cạnh nhiều nhất là 12 và số lượng thảm nhiều nhất là 10. Ngay cả một phép tìm kiếm hình học đầy đủ cũng sẽ phù hợp về mặt kỹ thuật, nhưng bài toán ẩn chứa một quan sát toán học đơn giản hơn nhiều. Vì câu trả lời có thể được quyết định bằng một vài phép tính số học nên không có lý do gì để mô phỏng các vị trí. 

Cái bẫy chính là hiểu sai ý nghĩa của sự chồng chéo. Một ý tưởng ngây thơ là so sánh tổng diện tích thảm với diện tích phòng và trả lời CÓ bất cứ khi nào thảm có đủ diện tích. Điều đó là chưa đủ vì hình học vẫn còn quan trọng. 

Hãy xem xét ví dụ này:```
10 1 10
```Tấm thảm duy nhất phù hợp hoàn toàn với căn phòng nên câu trả lời là CÓ. 

Bây giờ hãy nhìn vào:```
10 4 4
```Tổng diện tích thảm là`4 × 4 × 4 = 64`, nhưng diện tích phòng là`100`. Câu trả lời rõ ràng là KHÔNG vì không có đủ tổng diện tích. 

Trường hợp thú vị hơn là:```
10 4 6
```Tổng diện tích thảm là`144`, lớn hơn diện tích phòng. Câu trả lời là CÓ. bốn`6 × 6`thảm có thể bao phủ căn phòng bằng cách chồng lên nhau một chút. 

Việc thực hiện bất cẩn cũng có thể cho rằng thảm phải lát căn phòng một cách hoàn hảo mà không bị chồng lên nhau. Ví dụ:```
10 4 6
```Việc giải thích ốp lát không thành công vì`10`không chia hết cho`6`, tuy nhiên câu trả lời đúng vẫn là CÓ vì được phép chồng chéo. 

Điều kiện thực sự là cần bao nhiêu tấm thảm dọc theo một bên của căn phòng. Vì mỗi tấm thảm trải dài`n1`các đơn vị theo chiều ngang và chiều dọc, chúng ta cần đủ thảm để trải dài`n`theo cả hai hướng. 

## Phương pháp tiếp cận 

Tư duy vũ phu là thử mọi vị trí có thể để đặt thảm trong phòng. Vì tọa độ rất nhỏ nên chúng ta có thể rời rạc hóa căn phòng và đặt các tấm thảm theo cách đệ quy trong khi kiểm tra xem liệu tất cả các ô có bị che phủ hay không. 

Cách tiếp cận đó hiệu quả vì những hạn chế rất nhỏ nhưng số lượng vị trí lại tăng lên chóng mặt. Ngay cả khi mỗi tấm thảm chỉ có khoảng 100 vị trí có thể, việc thử tất cả các cấu hình sẽ đòi hỏi khoảng`100^10`trong trường hợp xấu nhất, điều này hoàn toàn không thực tế. 

Quan sát quan trọng là chỉ có phạm vi bao phủ một chiều mới quan trọng. 

Giả sử chúng ta đặt những tấm thảm vào một lưới. Dọc theo một bên của căn phòng, mỗi tấm thảm góp phần nhiều nhất`n1`chiều dài. Để che một đoạn chiều dài`n`, chúng ta cần ít nhất:$$\left\lceil \frac{n}{n1} \right\rceil$$thảm dọc theo chiều đó. 

Bởi vì căn phòng có hai chiều nên chúng ta cần nhiều tấm thảm theo chiều ngang và chiều dọc giống nhau. Vậy số tấm thảm tối thiểu cần có là:$$\left\lceil \frac{n}{n1} \right\rceil^2$$Nếu chúng ta đã có ít nhất số thảm đó, chúng ta có thể sắp xếp chúng thành một lưới và bao phủ hoàn toàn căn phòng, có thể chồng lên nhau ở gần các cạnh. 

Điều này biến toàn bộ vấn đề thành một so sánh số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vị trí vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Quan sát toán học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị`n`,`k`, Và`n1`. 
2. Tính xem một bên của căn phòng cần bao nhiêu tấm thảm. 

Vì mỗi tấm thảm trải`n1`đơn vị theo một hướng, số lượng là:$$need = \left\lceil \frac{n}{n1} \right\rceil$$Phép chia trần số nguyên có thể được tính như sau:```
need = (n + n1 - 1) // n1
```3. Tính tổng số tấm thảm cần thiết để che phủ toàn bộ lưới điện.```
required = need * need
```Chúng tôi cần`need`thảm trải khắp và`need`trải thảm xuống, vậy tổng cộng là sản phẩm của họ. 
4. So sánh`k`với`required`. 

Nếu như`k >= required`, in`YES`. Nếu không thì in`NO`. 

### Tại sao nó hoạt động 

Mỗi tấm thảm bao phủ một diện tích cạnh hình vuông`n1`. Dọc theo một chiều duy nhất, bao gồm chiều dài`n`yêu cầu ít nhất`ceil(n / n1)`thảm vì mỗi tấm thảm đều đóng góp nhiều nhất`n1`đơn vị. 

Vì căn phòng là một hình vuông nên chúng ta cần nhiều hàng và cột thảm như vậy. MỘT`need × need`sự sắp xếp luôn bao phủ căn phòng, ngay cả khi một số tấm thảm trải dài ra ngoài ranh giới hoặc chồng lên những tấm thảm khác. 

Thuật toán tính toán chính xác số lượng khả thi tối thiểu này, vì vậy câu trả lời là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k, n1 = map(int, input().split())

need = (n + n1 - 1) // n1

if k >= need * need:
    print("YES")
else:
    print("NO")
```Bước đầu tiên đọc ba số nguyên trực tiếp từ đầu vào tiêu chuẩn. 

Biểu thức:```
(n + n1 - 1) // n1
```thực hiện phép chia trần bằng cách sử dụng số học số nguyên. Điều này an toàn hơn phép chia dấu phẩy động vì nó tránh được các vấn đề về độ chính xác. 

Phép nhân:```
need * need
```đại diện cho số lượng thảm trong một sự sắp xếp lưới vuông. Vì các ràng buộc rất nhỏ nên không thể xảy ra tràn trong Python, nhưng ngay cả trong các ngôn ngữ có số nguyên có kích thước cố định thì các giá trị vẫn rất nhỏ. 

Sự so sánh cuối cùng sẽ kiểm tra xem những tấm thảm có sẵn có đủ để xây dựng lưới che phủ đó hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 4 6
```| Biến | Giá trị | 
| --- | --- | 
| n | 10 | 
| k | 4 | 
| n1 | 6 | 
| cần | 2 | 
| bắt buộc | 4 | 

Từ`k = 4`Và`required = 4`, câu trả lời là CÓ. 

Ví dụ này cho thấy tại sao sự chồng chéo lại quan trọng. Hai tấm thảm dọc theo mỗi chiều là đủ vì`2 × 6 = 12`, đã bao gồm chiều dài`10`. 

### Ví dụ 2 

đầu vào:```
10 3 6
```| Biến | Giá trị | 
| --- | --- | 
| n | 10 | 
| k | 3 | 
| n1 | 6 | 
| cần | 2 | 
| bắt buộc | 4 | 

Vì chỉ có 3 tấm thảm nhưng bắt buộc phải có 4 tấm thảm nên câu trả lời là KHÔNG. 

Điều này chứng tỏ rằng chỉ có những tấm thảm lớn thôi là chưa đủ. Chúng tôi vẫn cần đủ số lượng để tạo thành vùng phủ sóng ở cả hai chiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp dễ dàng nằm trong giới hạn vì nó hoạt động liên tục bất kể giá trị đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    n, k, n1 = map(int, input().split())

    need = (n + n1 - 1) // n1

    if k >= need * need:
        print("YES")
    else:
        print("NO")

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
assert run("10 4 6\n") == "YES\n", "sample 1"

# exact fit with one carpet
assert run("10 1 10\n") == "YES\n", "single carpet fits exactly"

# not enough total coverage
assert run("10 4 4\n") == "NO\n", "insufficient area"

# one carpet short
assert run("10 3 6\n") == "NO\n", "needs 4 carpets"

# maximum style boundary
assert run("12 9 4\n") == "YES\n", "3x3 grid exactly"

# overlap-heavy case
assert run("11 4 10\n") == "YES\n", "large overlap still works"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 1 10`|`YES`| Bảo hiểm thảm đơn chính xác | 
|`10 4 4`|`NO`| Tổng mức bảo hiểm không đủ | 
|`10 3 6`|`NO`| Ranh giới nơi thiếu một tấm thảm | 
|`12 9 4`|`YES`| Yêu cầu lưới chính xác | 
|`11 4 10`|`YES`| Sự chồng chéo được cho phép và hữu ích | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
10 4 6
```Thuật toán tính toán:```
need = ceil(10 / 6) = 2
required = 2 × 2 = 4
```Từ`k = 4`, câu trả lời là CÓ. 

Trường hợp này đánh bại giả định sai lầm rằng thảm phải lát căn phòng một cách hoàn hảo. MỘT`6 × 6`thảm không chia đều kích thước phòng, nhưng sự chồng chéo cho phép che phủ hoàn toàn. 

Bây giờ hãy xem xét:```
10 4 4
```Thuật toán tính toán:```
need = ceil(10 / 4) = 3
required = 9
```Vì chỉ có 4 tấm thảm nên câu trả lời là KHÔNG. 

Điều này nắm bắt các giải pháp chỉ so sánh tổng diện tích. Mặc dù mỗi tấm thảm khá lớn nhưng bốn tấm thảm không thể trải khắp căn phòng theo cả hai chiều. 

Cuối cùng, hãy xem xét:```
10 1 10
```Việc tính toán trở thành:```
need = 1
required = 1
```Thuật toán in chính xác CÓ vì một tấm thảm đã bao phủ chính xác toàn bộ căn phòng.
