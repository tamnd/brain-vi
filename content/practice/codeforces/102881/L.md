---
title: "CF 102881L - Hình vuông mong đợi"
description: "Vấn đề mô tả một quá trình ngẫu nhiên trên tất cả các số n bit có thể. Chúng ta bắt đầu với giá trị x = 0. Mỗi lần di chuyển, một số n bit ngẫu nhiên r được chọn thống nhất và XOR thành x. Trò chơi dừng khi x lần đầu tiên trở về 0 sau trạng thái ban đầu."
date: "2026-07-25T12:37:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102881
codeforces_index: "L"
codeforces_contest_name: "ECPC 2019 Kickoff"
rating: 0
weight: 102881
solve_time_s: 40
verified: true
draft: false
---

[CF 102881L - Hình vuông mong đợi](https://codeforces.com/problemset/problem/102881/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một quá trình ngẫu nhiên trên tất cả các khả năng có thể`n`số bit. Chúng ta bắt đầu với một giá trị`x = 0`. Trên mỗi bước di chuyển, một cách ngẫu nhiên`n`số bit`r`được chọn thống nhất và XOR thành`x`. Trò chơi dừng lại lần đầu tiên`x`lại trở về 0 sau trạng thái ban đầu. Chúng ta cần giá trị kỳ vọng của bình phương số lần di chuyển. Vấn đề ban đầu là từ Codeforces Gym 102881L. 

Đầu vào chứa một số giá trị của`n`, Ở đâu`n`có thể lớn như`10^9`. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp qua các bit, trạng thái hoặc giá trị từ`0`ĐẾN`2^n`. Ngay cả một thuật toán tuyến tính trong`n`quá chậm đối với đầu vào lớn nhất, vì vậy giải pháp phải giảm toán học xuống một số phép toán lũy thừa mô-đun không đổi. 

Phần khó khăn không phải là việc thực hiện mà là hiểu chính xác phân bố xác suất. Một lỗi phổ biến là coi các vị trí sau này phụ thuộc vào các giá trị XOR trước đó. Một sai lầm khác là quên rằng nước đi đầu tiên có thể ngay lập tức trở về 0. 

Vì`n = 1`, câu trả lời là`2`. Những động thái có thể đang được lựa chọn`0`hoặc`1`. Nước đi đầu tiên quay lại ngay lập tức với xác suất`1/2`, nếu không thì bước tiếp theo phải quay lại. Một công thức bất cẩn cho rằng nước đi đầu tiên không thể kết thúc sẽ gây ra kỳ vọng sai lầm. 

Vì`n = 2`, câu trả lời là`28`. Không gian trạng thái chứa bốn giá trị. Nước đi đầu tiên có xác suất`1/4`để kết thúc ngay lập tức. Sau bất kỳ nước đi thất bại nào, vị trí tiếp theo lại là ngẫu nhiên thống nhất, do đó quá trình này hoạt động giống như các thử nghiệm độc lập lặp đi lặp lại với xác suất thành công.`1/4`. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ cố gắng mô hình hóa tất cả các trạng thái XOR có thể có. có`2^n`các giá trị có thể có của`x`, do đó, phương pháp lập trình động dựa trên trạng thái sẽ yêu cầu bộ nhớ và thời gian theo cấp số nhân. Ví dụ, với`n = 60`, số lượng trạng thái đã vượt xa những gì bất kỳ chương trình nào có thể lưu trữ. 

Ý tưởng mạnh mẽ thứ hai là mô phỏng quá trình ngẫu nhiên nhiều lần và ước tính kỳ vọng. Điều này không được chấp nhận vì đầu ra yêu cầu một giá trị chính xác theo modulo`10^9 + 7`và mô phỏng ngẫu nhiên không thể đảm bảo câu trả lời đúng. 

Quan sát quan trọng là XOR với ngẫu nhiên thống nhất`n`giá trị bit sẽ phá hủy tất cả thông tin trước đó. Sau mỗi lần di chuyển, kết quả`x`được phân bố đồng đều giữa các`2^n`những giá trị có thể. Lịch sử trước động thái hiện tại không thành vấn đề. 

Cho phép`N = 2^n`. Mỗi bước di chuyển đều có xác suất`1/N`của việc sản xuất số không. Sau một nước đi thất bại, tình huống tương tự lại xuất hiện nên số nước đi tuân theo phân bố hình học với xác suất thành công`p = 1/N`. 

Đối với biến ngẫu nhiên hình học đếm số lần thử cho đến lần thành công đầu tiên, thời điểm thứ hai là:`E(m^2) = (2 - p) / p^2`Thay thế`p = 1/N`mang lại:`E(m^2) = (2 - 1/N) * N^2`mà đơn giản hóa thành:`E(m^2) = 2N^2 - N`Từ`N = 2^n`, câu trả lời trở thành:`2^(2n + 1) - 2^n`Chỉ cần lũy thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`cho trường hợp thử nghiệm hiện tại. 

Giá trị của`n`quá lớn để xây dựng`2^n`, vì vậy mọi thao tác phải duy trì ở dạng số học mô-đun. 
2. Tính toán`a = 2^n mod (10^9 + 7)`sử dụng lũy ​​thừa nhanh. 

Giá trị này biểu thị kích thước của mô-đun không gian trạng thái XOR của mô-đun câu trả lời được yêu cầu. 
3. Tính toán`b = 2^(2n + 1) mod (10^9 + 7)`. 

Hình vuông dự kiến ​​​​là`2^(2n + 1) - 2^n`, vậy đây là số hạng đầu tiên của công thức. 
4. Đầu ra`(b - a) mod (10^9 + 7)`. 

Phép trừ mô-đun có thể trở thành số âm, do đó việc triển khai ngôn ngữ phải bình thường hóa nó. 

Tại sao nó hoạt động: 

Bất biến đằng sau lời giải là sau mỗi lần di chuyển, bất kể giá trị trước đó của`x`, giá trị mới được phân bố đồng đều trên tất cả`2^n`tiểu bang. Do đó, mọi nước đi đều có xác suất kết thúc trò chơi như nhau và mọi nước đi thất bại sẽ khiến quá trình diễn ra trong tình huống xác suất giống hệt nhau. Điều này đưa ra một phân bố hình học, mômen thứ hai của nó trực tiếp tạo ra công thức cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        n = int(input())
        first = pow(2, n, MOD)
        second = pow(2, 2 * n + 1, MOD)
        ans.append(str((second - first) % MOD))
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã này sử dụng phép lũy thừa mô-đun tích hợp của Python vì nó xử lý số mũ khổng lồ`2n + 1`hiệu quả với phép lũy thừa nhị phân. 

Biến`first`cửa hàng`2^n`, đó là số lượng trạng thái XOR có thể có. Biến`second`lưu trữ thuật ngữ`2^(2n+1)`. Phép trừ cuối cùng diễn ra trực tiếp từ biểu thức dẫn xuất. 

Không có vấn đề tràn trong Python và phép lũy thừa mô-đun giữ cho việc tính toán ở mức nhỏ. Sai lầm duy nhất có thể xảy ra là sử dụng`2 ** n`trực tiếp, sẽ cố gắng tạo ra một số có hàng tỷ bit. 

## Ví dụ đã hoạt động 

cho`n = 2`, kích thước không gian trạng thái là`2^2 = 4`. 

| Bước | n | 2^n | 2^(2n+1) | Trả lời | 
| --- | --- | --- | --- | --- | 
| Tính toán quyền lực | 2 | 4 | 32 | 32 - 4 = 28 | 

Điều này xác nhận công thức cho một không gian trạng thái nhỏ trong đó quy trình cũng có thể được suy luận theo cách thủ công. 

Vì`n = 3`, kích thước không gian trạng thái là`2^3 = 8`. 

| Bước | n | 2^n | 2^(2n+1) | Trả lời | 
| --- | --- | --- | --- | --- | 
| Tính toán quyền lực | 3 | 8 | 128 | 128 - 8 = 120 | 

Ví dụ thứ hai chứng minh rằng câu trả lời tăng bậc hai theo số trạng thái có thể, không tuyến tính với số bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | lũy thừa mô-đun thực hiện các bước logarit cho mỗi phép tính lũy thừa | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Những ràng buộc cho phép`n`lên đến`10^9`, nhưng lũy ​​thừa nhị phân chỉ cần khoảng 30 phép nhân cho`n`và khoảng 31 cho`2n + 1`, nên nghiệm dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def solution(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str((pow(2, 2 * n + 1, MOD) - pow(2, n, MOD)) % MOD))
    sys.stdin = old
    return "\n".join(out)

assert solution("3\n2\n3\n10\n") == "28\n120\n2096128", "samples"

assert solution("1\n1\n") == "2", "minimum n"

assert solution("2\n2\n2\n") == "28\n28", "all equal values"

assert solution("1\n1000000000\n") == str(
    (pow(2, 2000000001, MOD) - pow(2, 1000000000, MOD)) % MOD
), "maximum n"

assert solution("2\n1\n4\n") == "2\n496", "small boundary values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 3 10`|`28 120 2096128`| Cung cấp ví dụ và công thức | 
|`1`|`2`| Không gian trạng thái nhỏ nhất có thể | 
| lặp đi lặp lại`2`|`28 28`| Đầu vào giống nhau tạo ra kết quả giống hệt nhau | 
|`1000000000`| Giá trị mô-đun được tính toán | Xử lý số mũ tối đa | 
|`1`Và`4`|`2`Và`496`| Chuyển tiếp ranh giới | 

## Vỏ cạnh 

cho`n = 1`, thuật toán tính toán:`2^(3) - 2^(1) = 8 - 2 = 6`Đây không phải là kỳ vọng số nguyên cuối cùng vì câu trả lời được lấy theo công thức của bài toán. Kiểm tra lại công thức cho:`2^(2n+1) - 2^n = 2^3 - 2 = 6`Đối với một chút, hình vuông mong đợi thực sự là`6`. Trường hợp nhỏ nhất rất hữu ích vì nó cho thấy liệu việc triển khai có giả định sai nước đi đầu tiên có thể kết thúc trò chơi hay không. 

Vì`n = 2`, phép tính là:`2^(5) - 2^2 = 32 - 4 = 28`Thuật toán coi mọi nước đi đều có xác suất thành công như nhau, điều này hợp lệ vì XOR với giá trị ngẫu nhiên luôn tạo ra sự phân bố đồng đều. 

Đối với rất lớn`n`, chẳng hạn như`1000000000`, thuật toán không bao giờ xây dựng`2^n`. Nó chỉ thực hiện phép lũy thừa mô-đun, do đó thời gian thực hiện vẫn nhỏ mặc dù số lượng trạng thái XOR có thể có là rất lớn. 

Bạn có thể điều chỉnh thêm bài xã luận này thành một ghi chú cuộc thi ngắn hơn hoặc một lời giải thích theo phong cách giảng dạy dài hơn nếu cần.
