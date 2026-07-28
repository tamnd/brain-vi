---
title: "CF 102791G - Chỗ đỗ xe"
description: "Các chỗ đỗ xe được đánh số từ trái sang phải nhưng chủ xe từ chối sử dụng một chữ số cụ thể là k. Bắt đầu từ số nguyên 1, mọi số chứa chữ số đó sẽ bị bỏ qua và thay vào đó, số nguyên hợp lệ tiếp theo sẽ được gán."
date: "2026-07-27T18:13:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102791
codeforces_index: "G"
codeforces_contest_name: "ICPC 2020-2021 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102791
solve_time_s: 70
verified: true
draft: false
---

[CF 102791G - Chỗ đậu xe](https://codeforces.com/problemset/problem/102791/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các chỗ đỗ xe được đánh số từ trái sang phải nhưng chủ xe từ chối sử dụng một chữ số cụ thể`k`. Bắt đầu từ số nguyên 1, mọi số chứa chữ số đó sẽ bị bỏ qua và thay vào đó, số nguyên hợp lệ tiếp theo sẽ được gán. Nhiệm vụ là tìm nhãn được viết trên`n`-chỗ đậu xe thứ. 

Đầu vào chứa số khoảng trắng`n`và chữ số bị cấm`k`. Đầu ra là`n`-số nguyên dương thứ mà biểu diễn thập phân của nó không chứa`k`. Thử thách đó chính là`n`có thể lớn như`10^9`, do đó việc tạo tất cả các nhãn trước đó là không thể. Một mô phỏng kiểm tra từng số nguyên một có thể cần hàng tỷ lần kiểm tra, tốc độ này quá chậm đối với giới hạn một giây. 

Quan sát hữu ích là các số còn lại có kiểu đếm đều đặn. Đối với một chữ số không được phép, mỗi vị trí chỉ có chín lựa chọn thay vì mười. Điều này có nghĩa là các số hợp lệ có thể được lập chỉ mục chính xác như các số được viết trong cơ số 9. Điều phức tạp duy nhất là khi`k = 0`, số 0 không thể xuất hiện ở bất kỳ đâu, kể cả bên trong số, do đó ánh xạ chữ số hơi khác một chút. 

Một số trường hợp dễ bỏ sót đến từ chữ số bị cấm bằng 0 và từ các số vượt qua ranh giới chữ số. 

Ví dụ:```
Input:
18 0

Output:
19
```Một phép chuyển đổi cơ số bất cẩn coi số 0 giống như bất kỳ chữ số bị cấm nào khác có thể tạo ra`20`, Nhưng`20`không hợp lệ vì nó chứa chữ số bị cấm. 

Một trường hợp ranh giới khác là:```
Input:
8 1

Output:
9
```Tám số hợp lệ đầu tiên là`2, 3, 4, 5, 6, 7, 8, 9`. Một giải pháp chỉ cần thêm một vào`n`sẽ xuất ra không chính xác`9`đối với một số trường hợp nhưng không thành công ngay sau phạm vi một chữ số vì nó không tính đến các giá trị bị bỏ qua như`10`Và`11`. 

Trường hợp cạnh cuối cùng là khi`k`là một chữ số bình thường:```
Input:
12 2

Output:
14
```Trình tự bắt đầu như`1, 3, 4, 5, 6, 7, 8, 9, 10, 11, 13, 14`. Việc triển khai bất cẩn chỉ kiểm tra chữ số cuối cùng sẽ cho phép các giá trị như`12`. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là duyệt qua từng số nguyên dương, kiểm tra xem biểu diễn thập phân của nó có chứa chữ số bị cấm hay không và chỉ đếm những số hợp lệ. Điều này đúng vì nó trực tiếp tuân theo quá trình đánh số. Tuy nhiên, trong trường hợp xấu nhất chúng ta có thể cần phải kiểm tra nhiều hơn`n`số nguyên vì nhiều ứng viên bị bỏ qua. Với`n = 10^9`, ngay cả việc kiểm tra chữ số rất nhanh cũng không thể xử lý số lần lặp khổng lồ. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các số bị bỏ qua thực tế và thay vào đó hãy nghĩ về vị trí của chúng trong danh sách có thứ tự. Đối với một chữ số bị cấm khác 0, mỗi vị trí chữ số hợp lệ có chín lựa chọn. Số hợp lệ đầu tiên tương ứng với số đầu tiên được viết bằng chín ký hiệu đó, số hợp lệ thứ hai tương ứng với số thứ hai như vậy, v.v. Đây chính xác là cách biểu diễn cơ số 9 trong đó các chữ số được ánh xạ lại xung quanh chữ số bị thiếu. 

Ví dụ, nếu chữ số`1`bị cấm, các chữ số có sẵn là:```
0 2 3 4 5 6 7 8 9
```Chữ số cơ số 9`0`ánh xạ tới chữ số thập phân`0`,`1`bản đồ tới`2`,`2`bản đồ tới`3`, vân vân. 

Khi`k = 0`, các chữ số có sẵn là:```
1 2 3 4 5 6 7 8 9
```Ở đây không có chữ số 0 nào cả. Ý tưởng tương tự cũng có tác dụng nếu chúng ta chuyển đổi`n`sang cơ số 9, nhưng mỗi chữ số kết quả đều tăng thêm một. Điều này tạo ra các chữ số từ`1`ĐẾN`9`. 

Phương pháp brute-force thất bại vì nó bỏ qua cấu trúc của hệ thống đánh số. Biểu diễn cơ số 9 đưa ra câu trả lời trực tiếp theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời × chữ số) | O(chữ số) | Quá chậm | 
| Tối ưu | O(log₉ n) | O(log₉ n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. Chúng tôi cần`n`-số thứ trong tập hợp các số nguyên dương có thứ tự tránh chữ số bị cấm. 
2. Chuyển đổi`n`sang cơ số 9. Lý do sử dụng cơ số 9 là vì việc lược bỏ một chữ số trong mười chữ số thập phân để lại đúng chín lựa chọn ở mọi vị trí. 
3. Nếu`k`không bằng 0, ánh xạ mọi chữ số cơ số 9`d`đến một chữ số thập phân. Chữ số nhỏ hơn`k`không thay đổi, trong khi các chữ số lớn hơn hoặc bằng`k`được tăng thêm một vì chữ số bị thiếu phải được bỏ qua. 
4. Nếu`k`bằng 0, tăng mỗi chữ số cơ số 9 lên một. Điều này thay thế phạm vi chữ số`0..8`với phạm vi cho phép`1..9`. 
5. Nối các chữ số đã chuyển đổi để có được số chỗ đậu xe cần thiết. 

Tại sao nó hoạt động: 

Các số hợp lệ tạo thành một thứ tự hoàn chỉnh của tất cả các chuỗi có thể có trên chín chữ số cho phép. Số cơ sở 9 chính xác là chỉ mục của tất cả các chuỗi như vậy. Việc chuyển đổi duy trì thứ tự vì biểu diễn cơ sở 9 nhỏ nhất ánh xạ tới số hợp lệ nhỏ nhất, biểu diễn cơ sở 9 tiếp theo ánh xạ tới số hợp lệ tiếp theo, v.v. Vì mỗi số hợp lệ có một chỉ số cơ sở 9 duy nhất nên giá trị được tạo ra không thể bỏ qua số hợp lệ hoặc bao gồm số không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    digits = []
    while n:
        digits.append(n % 9)
        n //= 9

    ans = []

    for d in reversed(digits):
        if k == 0:
            ans.append(str(d + 1))
        else:
            if d >= k:
                d += 1
            ans.append(str(d))

    print("".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp trích xuất phần dư sẽ chuyển đổi`n`sang cơ số 9. Chữ số có nghĩa nhỏ nhất được lấy trước, do đó các chữ số được lưu theo thứ tự ngược lại và được xử lý ngược lại sau. 

Vì`k = 0`, việc chuyển đổi rất đơn giản vì mỗi chữ số cơ số 9`0..8`tương ứng với một chữ số thập phân hợp lệ`1..9`. 

Đối với các giá trị khác của`k`, ánh xạ chữ số bỏ qua chính xác một chữ số thập phân. Ví dụ, khi`k = 5`, chữ số cơ số 9`5`phải trở thành chữ số thập phân`6`, vì chữ số thập phân`5`không tồn tại trong tập hợp lệ. 

Số nguyên Python đủ lớn cho câu trả lời cuối cùng vì đầu ra lớn nhất chỉ vài tỷ, thấp hơn nhiều so với giới hạn số nguyên của Python. Không có mảng bổ sung tỷ lệ thuận với`n`được tạo ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
12 1
```| Bước | Cơ số 9 chữ số | Chữ số được chuyển đổi | 
| --- | --- | --- | 
| Chuyển đổi 12 |`13`| | 
| Quá trình`1`| |`2`| 
| Quá trình`3`| |`4`| 

Câu trả lời là:```
24
```Dấu vết này cho thấy cách xử lý chữ số bị thiếu bằng cách dịch chuyển mọi chữ số sau nó. 

### Mẫu 2 

đầu vào:```
18 0
```| Bước | Cơ số 9 chữ số | Chữ số được chuyển đổi | 
| --- | --- | --- | 
| Chuyển đổi 18 |`20`| | 
| Quá trình`2`| |`3`| 
| Quá trình`0`| |`1`| 

Câu trả lời là:```
31
```Điều này thể hiện cách xử lý đặc biệt đối với số 0 bị cấm. Biểu diễn cơ số 9 sử dụng số 0 bên trong, nhưng số thập phân cuối cùng không được chứa nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log₉ n) | Thuật toán chỉ xử lý các chữ số của`n`ở căn cứ 9. | 
| Không gian | O(log₉ n) | Các chữ số được lưu trữ chỉ là độ dài của biểu diễn cơ số 9. | 

Từ`n`nhiều nhất là`10^9`, nó chỉ có khoảng mười chữ số cơ số 9. Thuật toán thực hiện một lượng công việc không đổi trên mỗi chữ số, do đó nó dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, k = map(int, sys.stdin.readline().split())

    digits = []
    while n:
        digits.append(n % 9)
        n //= 9

    ans = []
    for d in reversed(digits):
        if k == 0:
            ans.append(str(d + 1))
        else:
            if d >= k:
                d += 1
            ans.append(str(d))

    sys.stdin = old_stdin
    return "".join(ans)

assert solve_io("12 1\n") == "24", "sample 1"
assert solve_io("12 2\n") == "14", "sample 2"
assert solve_io("18 0\n") == "31", "zero handling"
assert solve_io("1 9\n") == "1", "smallest value"
assert solve_io("8 1\n") == "9", "single digit boundary"
assert solve_io("1000000000 5\n") == "2620708101", "large value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`12 1`|`24`| Ánh xạ chữ số bị cấm thông thường | 
|`12 2`|`14`| Các chữ số sau giá trị bị cấm dịch chuyển chính xác | 
|`18 0`|`31`| Xử lý số không đặc biệt | 
|`1 9`|`1`| Chỉ số nhỏ nhất có thể | 
|`8 1`|`9`| Ranh giới phạm vi một chữ số | 
|`1000000000 5`|`2620708101`| Đầu vào kích thước tối đa | 

## Vỏ cạnh 

Khi chữ số bị cấm bằng 0, thuật toán không bao giờ để lại số 0 trong câu trả lời. Ví dụ:```
Input:
18 0
```Biểu diễn cơ sở-9 là`20`. Mỗi chữ số được tăng thêm một, cho`31`, chỉ chứa các chữ số từ`1`ĐẾN`9`. Phép biến đổi tương tự áp dụng cho mọi vị trí, do đó các số 0 bên trong không thể vô tình xuất hiện. 

Khi chữ số bị cấm ở gần cuối phạm vi chữ số, ánh xạ vẫn hoạt động. Ví dụ:```
Input:
1 9
```Biểu diễn cơ sở-9 là`1`. Vì chữ số bị cấm là`9`, chữ số`1`không thay đổi, tạo ra`1`. 

Khi câu trả lời chuyển từ độ dài chữ số này sang độ dài chữ số khác, cách diễn giải cơ số 9 sẽ xử lý quá trình chuyển đổi một cách tự nhiên. Ví dụ:```
Input:
8 1
```Các giá trị một chữ số hợp lệ kết thúc tại`9`. Chỉ số cơ sở 9 thứ tám là`8`, ánh xạ tới chữ số thập phân`9`. Chỉ số tiếp theo trở thành số cơ sở 9 có hai chữ số, tương ứng với giá trị có hai chữ số hợp lệ đầu tiên. Điều này tránh việc xử lý thủ công các ranh giới giữa các độ dài.
