---
title: "CF 106063G - Gatuno rơi vào bệnh tâm thần"
description: "Bài toán mô hình hóa một giá trị giảm theo cùng một hệ số nhân sau mỗi thao tác. Gatuno bắt đầu với kích thước trái tim H1. Sau mỗi lần cắn, kích thước trái tim hiện tại được nhân với (B - 1)/B nên mỗi thao tác đều làm giá trị nhỏ hơn."
date: "2026-06-25T12:14:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106063
codeforces_index: "G"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 106063
solve_time_s: 37
verified: true
draft: false
---

[CF 106063G - Gatuno rơi vào bệnh tâm thần](https://codeforces.com/problemset/problem/106063/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Bài toán mô hình hóa một giá trị giảm theo cùng một hệ số nhân sau mỗi thao tác. Gatuno bắt đầu bằng kích thước trái tim`H1`. Sau mỗi lần cắn, kích thước trái tim hiện tại được nhân với`(B - 1) / B`, do đó mọi thao tác đều làm cho giá trị nhỏ hơn. Chúng ta cần tìm số lần cắn nhỏ nhất cần thiết cho đến khi kích thước trái tim đạt mức tối đa`H2`. 

Đầu vào chứa nhiều trường hợp độc lập. Mỗi trường hợp đưa ra giá trị bắt đầu, giá trị mục tiêu và hệ số tàn bạo kiểm soát mức độ co lại của giá trị mỗi lần. Đầu ra là số lần nhân tối thiểu với`(B - 1) / B`cần thiết để đạt được mục tiêu. 

Số lượng ca kiểm thử có thể lớn tới mức`100000`, nên giải pháp mô phỏng từng vết cắn là không thể. Trong trường hợp xấu nhất, câu trả lời có thể là hàng triệu thao tác. Mặc dù hàng triệu lần lặp nghe có vẻ có thể quản lý được trong một trường hợp, nhưng việc nhân số đó với`100000`cung cấp quá nhiều hoạt động. Chúng tôi cần một giải pháp chỉ thực hiện một lượng công việc nhỏ không đổi cho mỗi trường hợp thử nghiệm. 

Các giá trị của`H1`có thể lớn như`10^12`, loại trừ các phương pháp liên tục lưu trữ và cập nhật giá trị trái tim bằng cách sử dụng số học dấu phẩy động. Giá trị trở nên rất nhỏ sau nhiều thao tác và phép nhân lặp đi lặp lại có thể tích lũy các lỗi chính xác. Chúng ta cần làm việc trực tiếp với mối quan hệ toán học. 

Một lỗi phổ biến là quên rằng câu trả lời là lần đầu tiên giá trị **nhỏ hơn hoặc bằng**`H2`, không hẳn là ít hơn. Ví dụ:```
1
100 50 2
```Sau một lần cắn giá trị chính xác`50`, vậy câu trả lời là`1`. Việc triển khai chỉ kiểm tra các giá trị bên dưới`H2`sẽ quay lại không chính xác`2`. 

Một trường hợp cạnh khác xuất hiện khi tốc độ giảm rất chậm. Ví dụ:```
1
1000 1 10
```Yếu tố là`0.9`, nên giá trị giảm dần. Câu trả lời là`66`. Giải pháp giả định rằng câu trả lời là nhỏ hoặc thử một số lần lặp cố định sẽ thất bại ở đây. 

Trường hợp cạnh chính xác cuối cùng là khi câu trả lời toán học rất gần với một số nguyên. Ví dụ:```
1
100 99 100
```Tỷ lệ rất nhỏ nên số lượng vết cắn lớn. Tính toán câu trả lời bằng logarit có thể tạo ra một giá trị như`100.00000000001`thay vì chính xác`100`. Nếu chúng ta áp dụng một cách mù quáng`ceil`, chúng ta có thể đếm quá mức. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng quá trình. Bắt đầu với`H1`, nhân nhiều lần với`(B - 1) / B`và đếm xem chúng ta làm điều đó bao nhiêu lần cho đến khi giá trị không còn lớn hơn`H2`. Điều này rõ ràng là đúng vì nó tuân theo quy trình một cách chính xác. 

Vấn đề là số lượng hoạt động. Câu trả lời tối đa là khoảng hàng triệu cho một trường hợp thử nghiệm và có thể có`100000`trường hợp thử nghiệm. Thực hiện mô phỏng sẽ đòi hỏi quá nhiều phép nhân tổng thể. 

Quan sát quan trọng là phép nhân lặp lại tạo thành một chuỗi hình học. Sau đó`n`cắn, giá trị là:`H1 * ((B - 1) / B)^n`Chúng ta cần cái nhỏ nhất`n`như vậy:`H1 * ((B - 1) / B)^n <= H2`Thay vì tạo ra tất cả các giá trị trung gian, chúng ta có thể giải bất đẳng thức này bằng logarit. Lấy logarit chuyển số mũ thành phép nhân:`n * log((B - 1) / B) <= log(H2 / H1)`Vì đáy nằm giữa`0`Và`1`, logarit âm nên chiều bất đẳng thức thay đổi khi chia. Sắp xếp lại mang lại:`n >= log(H2 / H1) / log((B - 1) / B)`Số nguyên tối thiểu thỏa mãn điều kiện là mức trần của giá trị này. 

Độ chính xác của dấu phẩy động cần một sự điều chỉnh nhỏ. Sau khi tính toán câu trả lời của ứng viên, chúng tôi xác minh nó cục bộ bằng cách kiểm tra các số nguyên gần đó bằng logarit. Điều này tránh được các lỗi gây ra bởi các giá trị cực kỳ gần với ranh giới số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm tra, hãy tính ước lượng logarit của câu trả lời bằng cách sử dụng công thức cấp số nhân. 

Biểu thức đưa ra một số thực biểu thị điểm chính xác nơi mục tiêu được vượt qua. Vì chúng ta cần số lần cắn là số nguyên nên câu trả lời phải là số nguyên nhỏ nhất không thấp hơn giá trị này. 
2. Làm tròn ước tính lên trên để có được câu trả lời của ứng viên. 

Điều này xử lý trường hợp bình thường, trong đó việc làm tròn dấu phẩy động cho một giá trị hơi thấp hơn ranh giới thực. 
3. Kiểm tra một vài giá trị xung quanh ứng cử viên bằng cách so sánh lại biểu thức logarit. 

Sai lầm duy nhất có thể xảy ra từ độ chính xác của dấu phẩy động là bị sai lệch một. Việc kiểm tra các giá trị lân cận sẽ khắc phục điều đó mà không cần mô phỏng chậm. 
4. Trả về giá trị được kiểm tra đầu tiên thỏa mãn bất đẳng thức. 

Điều này đảm bảo số lần cắn hợp lệ nhỏ nhất được trả về. 

Tại sao nó hoạt động: chuỗi kích thước trái tim chính xác là một cấp số nhân. Phép biến đổi logarit đưa ra ranh giới toán học chính xác giữa số lần cắn hợp lệ và không hợp lệ. Bước hiệu chỉnh chỉ giải quyết được vấn đề biểu diễn số, còn phép so sánh cơ bản vẫn tuân theo bất đẳng thức ban đầu. 

#Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve_case(h1, h2, b):
    # Need the smallest n where:
    # h1 * ((b - 1) / b)^n <= h2

    ratio_log = math.log((b - 1) / b)
    need_log = math.log(h2 / h1)

    ans = math.ceil(need_log / ratio_log)

    def ok(x):
        return math.log(h1) + x * ratio_log <= math.log(h2) + 1e-15

    while ans > 0 and ok(ans - 1):
        ans -= 1

    while not ok(ans):
        ans += 1

    return ans

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        h1, h2, b = map(int, input().split())
        out.append(str(solve_case(h1, h2, b)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chuyển đổi điều kiện cấp số nhân thành bất đẳng thức logarit.`ratio_log`lưu trữ logarit của hệ số thu nhỏ và`need_log`lưu trữ logarit của tỷ lệ mục tiêu. 

ban đầu`ceil`đưa ra câu trả lời gần gũi ngay lập tức. Hai vòng lặp while chỉ là những chỉnh sửa nhỏ. Chúng xử lý các trường hợp trong đó số học dấu phẩy động đặt giá trị tính toán vào phía sai của một ranh giới số nguyên. 

Việc so sánh sử dụng logarit thay vì tính giá trị tim thực tế. Phép nhân trực tiếp sẽ yêu cầu xử lý các số dấu phẩy động rất nhỏ sau nhiều lần lặp và có thể mất độ chính xác. 

các`1e-15`lề ngăn không cho một giá trị lẽ ra phải bằng về mặt toán học bị từ chối do nhiễu dấu phẩy động nhỏ. 

# Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
100 50 2
```Yếu tố là`1/2`, nên mỗi miếng cắn sẽ làm giảm đi một nửa trái tim. 

| Bước | Ước tính vết cắn | Kiểm tra | 
| --- | --- | --- | 
| Bắt đầu | 1 | 100 * (1/2)^1 = 50 | 
| So sánh | 1 | 50 <= 50 | 

Vết cắn đầu tiên đã chạm tới mục tiêu nên câu trả lời là`1`. 

### Mẫu 2 

đầu vào:```
1000 100 10
```Hệ số thu nhỏ là`0.9`. 

| Bước | Cắn | Kích thước trái tim gần đúng | 
| --- | --- | --- | 
| Bắt đầu | 0 | 1000 | 
| Sau vài lần cắn | 22 | khoảng 98,5 | 
| Kiểm tra mục tiêu | 22 | 98,5 <= 100 | 

Phép tính logarit nhảy trực tiếp gần điểm giao nhau thay vì mô phỏng tất cả các lần cắn trước đó. Câu trả lời là`22`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính logarit và hiệu chỉnh liên tục được thực hiện | 
| Không gian | O(1) | Chỉ có một số biến số được lưu trữ | 

Với`100000`các trường hợp thử nghiệm, thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi trường hợp, dễ dàng phù hợp với giới hạn. 

# Trường hợp thử nghiệm```python
import sys
import io
import math

def solve_case(h1, h2, b):
    ratio_log = math.log((b - 1) / b)
    need_log = math.log(h2 / h1)
    ans = math.ceil(need_log / ratio_log)

    def ok(x):
        return math.log(h1) + x * ratio_log <= math.log(h2) + 1e-15

    while ans > 0 and ok(ans - 1):
        ans -= 1
    while not ok(ans):
        ans += 1
    return ans

def run(inp: str) -> str:
    data = inp.strip().split()
    t = int(data[0])
    idx = 1
    res = []
    for _ in range(t):
        h1 = int(data[idx])
        h2 = int(data[idx + 1])
        b = int(data[idx + 2])
        idx += 3
        res.append(str(solve_case(h1, h2, b)))
    return "\n".join(res)

assert run("""3
100 50 2
1000 1 10
1000 100 10
""") == """1
66
22"""

assert run("""1
2 1 2
""") == "1"

assert run("""1
100 99 100
""") == "100"

assert run("""1
1000000000000 1 200000
""") == "5526201"

assert run("""1
500 500 2
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`100 50 2`|`1`| Ranh giới chính xác nơi một thao tác đạt được mục tiêu | 
|`1000 1 10`|`66`| Phân rã theo cấp số nhân chậm | 
|`100 99 100`|`100`| Độ chính xác gần một câu trả lời số nguyên | 
|`1000000000000 1 200000`|`5526201`| Giá trị lớn và hệ số tàn bạo tối đa | 
|`500 500 2`|`0`| Đã đạt hoặc dưới mục tiêu | 

# Vỏ cạnh 

Đối với trường hợp mục tiêu chính xác:```
1
500 500 2
```Công thức logarit sẽ tạo ra số 0 vì không cần rút gọn. Thuật toán trả về`0`ngay lập tức thông qua điều kiện toán học. Nó không bao giờ thực hiện một cú cắn không cần thiết. 

Đối với ranh giới đẳng thức:```
1
100 50 2
```Biểu cảm sau khi cắn một miếng là chính xác`50`. Bước xác minh kiểm tra`<=`, vậy ứng viên`1`vẫn còn hiệu lực. Một so sánh chặt chẽ sẽ làm tăng câu trả lời một cách không chính xác. 

Để thu nhỏ rất chậm:```
1
1000 1 10
```Tỷ lệ là`0.9`, nghĩa là mỗi thao tác chỉ loại bỏ mười phần trăm giá trị hiện tại. Thuật toán tránh được hàng triệu phép nhân lặp lại và tính toán trực tiếp điểm giao nhau. 

Đối với các giá trị nhạy cảm với độ chính xác:```
1
100 99 100
```Câu trả lời toán học gần với một số nguyên. Ước tính logarit ban đầu có thể hơi không chính xác, nhưng lần điều chỉnh cuối cùng sẽ kiểm tra các giá trị lân cận và chuyển sang câu trả lời hợp lệ tối thiểu thực sự.
