---
title: "CF 102465A - Thành phố ánh sáng"
description: "Chúng ta có N đèn được đánh số từ 1 đến N. Ban đầu mọi đèn đều sáng. Mỗi lệnh k chứa một số nguyên dương x và lệnh đó sẽ bật tắt mọi ánh sáng có số là bội số của x. Đèn bật tắt thay đổi từ bật sang tắt hoặc từ tắt sang bật."
date: "2026-08-09T03:35:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "A"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 361
verified: true
draft: false
---

[CF 102465A - Thành phố ánh sáng](https://codeforces.com/problemset/problem/102465/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 1 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có N đèn được đánh số từ 1 đến N. Ban đầu mọi đèn đều sáng. Mỗi lệnh k chứa một số nguyên dương x và lệnh đó sẽ bật tắt mọi ánh sáng có số là bội số của x. Đèn bật tắt thay đổi từ bật sang tắt hoặc từ tắt sang bật. 

Các lệnh đến theo một thứ tự cố định và chúng ta cần số lượng đèn tắt lớn nhất ngay sau bất kỳ lệnh nào. Trạng thái ban đầu, trước lệnh đầu tiên, không có đèn tắt. 

Các ràng buộc nhỏ về số lượng lệnh, với k<100, nhưng N có thể đạt tới 10 6. Một mô phỏng trực tiếp cần phải chạm vào từng bội số của mỗi lệnh. Nếu tất cả 100 lệnh là 1 thì mỗi lệnh chạm vào tất cả 10 6 đèn, tạo ra 10 8 đèn cập nhật riêng lẻ. Như vậy là quá nhiều so với giới hạn 1 giây, đặc biệt là trong Python. Chúng ta cần thể hiện cả một nhóm đèn một cách gọn gàng hơn. 

Biểu diễn tự nhiên là một bitset. Một bit đại diện cho một ánh sáng, với bit 0 tương ứng với ánh sáng 1, bit 1 tương ứng với ánh sáng 2, v.v. Tập hợp các đèn được bật bằng lệnh khi đó là một số nguyên lớn. Việc chuyển đổi toàn bộ nhóm trở thành một thao tác XOR duy nhất và việc đếm số đèn tắt sẽ trở thành số lượng dân số với`int.bit_count()`. Python triển khai các thao tác này trên các từ máy được đóng gói bên trong, do đó công việc được thực hiện bằng mã gốc được tối ưu hóa thay vì một thao tác Python trên mỗi ánh sáng. của Python`int.bit_count()`trả về trực tiếp số lượng bit đã đặt. 

Có một số trường hợp khó xử lý. Với`N=1`, một lệnh`1`bật tắt đèn duy nhất nên câu trả lời là 1.```
1
1
1
```Đầu ra là`1`. Một giải pháp chỉ kiểm tra trạng thái cuối cùng và vô tình bỏ qua trạng thái sau lệnh vẫn có tác dụng ở đây, nhưng bài học tổng quát hơn là mức tối đa có thể xảy ra tại một thời điểm trung gian. 

Lệnh lặp đi lặp lại là một cái bẫy khác.```
10
2
2
2
```Sau lệnh đầu tiên, đèn 2, 4, 6, 8 và 10 tắt nên câu trả lời là 5. Lệnh thứ hai bật lại tất cả các đèn này nhưng câu trả lời vẫn là 5. Giải pháp chỉ kiểm tra cấu hình cuối cùng sẽ xuất sai 0. 

Giá trị lệnh lớn nhất có thể cũng cần xử lý ranh giới chính xác.```
5
2
5
1
```Sau lệnh`5`, chỉ có đèn số 5 tắt. Yêu cầu`1`bật tắt mọi đèn, tắt 4 đèn và bật 5 đèn. Đầu ra đúng là 4. Một vòng lặp vô tình dừng ở`N - 1`sẽ nhớ ánh sáng 5. 

## Phương pháp tiếp cận 

Giải pháp đơn giản này lưu trữ trạng thái của mọi ánh sáng và xử lý từng lệnh bằng cách duyệt qua bội số của nó. Đối với lệnh x, chúng ta truy cập x,2x,3x,…, chuyển đổi từng đèn tương ứng, duy trì số lượng đèn tắt hiện tại và cập nhật câu trả lời. Điều này đúng vì mỗi đèn bị ảnh hưởng đều được truy cập chính xác một lần cho lệnh đó và việc duy trì số lượng tăng dần sẽ tránh quét tất cả N đèn sau mỗi thao tác. 

Vấn đề là trường hợp xấu nhất. Nếu mọi lệnh đều`1`, mỗi lệnh thăm N đèn. Với N=10 6 và k=100, tức là 100⋅10 6 =10 8 cập nhật ánh sáng. Cách tiếp cận tham chiếu C++ có thể xử lý mô phỏng đơn giản này bằng mã gốc được tối ưu hóa, nhưng vòng lặp tương tự trong Python không phù hợp với giới hạn 1 giây. Các ràng buộc bài toán chính thức thực sự là N 10 6, k 100, với giới hạn thời gian là 1 giây. 

Quan sát quan trọng là một lệnh không cần phải được biểu diễn dưới dạng 1.000.000 giá trị Boolean riêng biệt. Nó tự nhiên là một tập hợp các vị trí và việc chuyển đổi tập hợp chính xác là XOR. Chúng ta có thể mã hóa toàn bộ trạng thái của thành phố dưới dạng một số nguyên Python. Nếu bit j−1 là 1 thì đèn j tắt. Mặt nạ cho lệnh x có bit j−1 được đặt chính xác khi x chia j. Việc áp dụng lệnh sau đó chỉ đơn giản là`state ^= mask`. 

Có một chi tiết bổ sung vì việc xây dựng mặt nạ triệu bit bằng cách thực hiện liên tục`mask |= 1 << position`bản thân nó sẽ thực hiện một số lượng lớn các hoạt động ở cấp độ Python. Mặt nạ có mẫu nhị phân rất đều đặn, vì vậy chúng tôi xây dựng nó theo phương pháp đại số. 

Đối với m=⌊N/x⌋, mx bit đầu tiên của mẫu mong muốn chứa một bit được đặt cho mỗi vị trí x. giá trị 

2 x −1 2 mx −1 ​ 

có biểu diễn nhị phân bao gồm m bản sao của mẫu`1`cách nhau bởi chính xác x−1 số 0. Việc dịch chuyển nó theo các vị trí x−1 sẽ di chuyển các bit đã đặt đó tới các vị trí x−1,2x−1,…,mx−1, tương ứng chính xác với các đèn x,2x,…,mx. 

Cách tiếp cận bạo lực hoạt động vì nó duy trì rõ ràng mọi ánh sáng, nhưng không thành công vì cùng một triệu vị trí có thể được duyệt qua một trăm lần. Việc quan sát bitset cho phép chúng tôi thực hiện toàn bộ lần chuyển đổi dưới dạng XOR trên các bit được đóng gói và đếm kết quả bằng thao tác đếm tổng thể gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(kN) trường hợp xấu nhất | O(N) | Quá chậm trong Python | 
| Bitset | O(kN/w) phép toán từ đóng gói | O(kN/w) | Đã chấp nhận | 

Ở đây w là độ rộng từ máy được sử dụng nội bộ bởi các số nguyên có độ chính xác tùy ý của Python. Việc triển khai thực tế lưu trữ mỗi mặt nạ lệnh dưới dạng một số nguyên Python duy nhất. 

## Hướng dẫn thuật toán 

1. Đọc N, k và dãy lệnh. Thứ tự lệnh phải được giữ nguyên vì câu hỏi yêu cầu mức tối đa trong suốt quá trình chứ không chỉ đơn thuần là trạng thái cuối cùng. 
2. Đối với mỗi giá trị lệnh riêng biệt x, hãy xây dựng một mặt nạ bit có các bit được thiết lập tương ứng chính xác với bội số của x không vượt quá N. Đặt m=N//x. biểu thức 

( 2 x −1 2 mx −1 ​ )2 x−1 

tạo ra các bit tập hợp tại các vị trí chính xác x−1,2x−1,…,mx−1. 
3. Lưu trữ các mặt nạ đã tạo trong từ điển. Nếu cùng một lệnh xuất hiện nhiều lần, mặt nạ của nó sẽ được sử dụng lại thay vì được tạo lại. 
4. Bắt đầu với`state = 0`. Bit 0 có nghĩa là đèn tương ứng bật sáng, vì vậy ban đầu mọi đèn đều được biểu thị chính xác. 
5. Xử lý các lệnh theo thứ tự ban đầu. XOR trạng thái hiện tại với mặt nạ của lệnh. XOR là thao tác đúng vì số 1 trong mặt nạ lệnh có nghĩa là đèn phải được bật. XOR thay đổi 0 thành 1 và 1 thành 0. 
6. Sau mỗi lệnh hãy tính`state.bit_count()`và cập nhật tối đa. Điều này kiểm tra mọi thời điểm có thể mà mức tối đa có thể xảy ra. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của lệnh, bit j−1 của`state`chính xác là 1 khi đèn j được bật một số lần lẻ. Mỗi lệnh có giá trị chia cho j đều đóng góp một XOR với bit đó. Số lần bật tắt chẵn sẽ đưa đèn về trạng thái ban đầu, trong khi số lẻ sẽ tắt đèn. Do đó các bit thiết lập của`state`chính xác là đèn hiện đang tắt. Lấy`bit_count()`do đó đưa ra số lượng đèn tắt chính xác sau tiền tố đó và lấy mức tối đa trên tất cả các tiền tố sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    k = int(input())

    commands = [int(input()) for _ in range(k)]

    masks = {}

    def build_mask(x):
        if x in masks:
            return masks[x]

        m = n // x

        # (2^(m*x) - 1) / (2^x - 1)
        # has one bit set every x positions, starting at bit 0.
        pattern = ((1 << (m * x)) - 1) // ((1 << x) - 1)

        # Shift the set bits from positions 0, x, 2x, ...
        # to positions x-1, 2x-1, 3x-1, ...
        mask = pattern << (x - 1)

        masks[x] = mask
        return mask

    state = 0
    answer = 0

    for x in commands:
        state ^= build_mask(x)
        answer = max(answer, state.bit_count())

    print(answer)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc với`sys.stdin.readline`, theo yêu cầu cho lập trình cạnh tranh. Chỉ có một ca kiểm thử trong bài toán, do đó không cần vòng lặp ca kiểm thử bên ngoài.`m = n // x`là số bội của`x`đó là những con số ánh sáng hợp lệ. Khi`x = n`, chúng tôi nhận được`m = 1`, và mặt nạ kết quả chỉ chứa bit`n-1`, tương ứng với ánh sáng n. Khi`x = 1`, mặt nạ chứa mọi bit từ 0 đến`n-1`, do đó lệnh sẽ bật tắt chính xác mọi đèn. 

Việc xây dựng mặt nạ chỉ sử dụng số học số nguyên. mẫu số`(1 << x) - 1`đại diện cho một khối gồm x số nhị phân. Chia số nguyên đơn vị`(1 << (m * x)) - 1`bởi khối đó tạo ra mẫu lặp lại cần thiết cho bội số. Sự thay đổi cuối cùng căn chỉnh bit được đặt đầu tiên với ánh sáng x, thay vì ánh sáng 1.`state ^= mask`được cố tình thực hiện theo thứ tự lệnh ban đầu. Các lệnh chuyển đổi thành các thao tác ở trạng thái cuối cùng, nhưng mức tối đa được lấy sau mỗi tiền tố, vì vậy việc sắp xếp lại các lệnh sẽ thay đổi câu trả lời. 

Số nguyên Python có độ chính xác tùy ý, do đó không có hiện tượng tràn số nguyên khi trạng thái đạt tới một triệu bit. Tích hợp sẵn`bit_count()`đếm trực tiếp các bit đã đặt đó. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là:```
10
4
6
2
1
3
```Trạng thái liên quan là bộ đèn hiện đang tắt. 

| Lệnh | Mặt nạ ảnh hưởng | Tắt đèn sau lệnh | Tắt tính | Tối đa | 
| --- | --- | --- | --- | --- | 
| 6 | 6 | {6} | 1 | 1 | 
| 2 | 2, 4, 6, 8, 10 | {2, 4, 8, 10} | 4 | 4 | 
| 1 | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | {1, 3, 5, 6, 7, 9} | 6 | 6 | 
| 3 | 3, 6, 9 | {1, 5, 6, 7} | 4 | 6 | 

Câu trả lời là`6`. Dấu vết cho thấy lý do tại sao chúng ta phải kiểm tra trạng thái sau mỗi lệnh thay vì chỉ nhìn vào trạng thái cuối cùng. 

Ví dụ thứ hai thực hiện các lệnh lặp lại:```
10
2
2
2
```| Lệnh | Mặt nạ ảnh hưởng | Tắt đèn sau lệnh | Tắt tính | Tối đa | 
| --- | --- | --- | --- | --- | 
| 2 | 2, 4, 6, 8, 10 | {2, 4, 6, 8, 10} | 5 | 5 | 
| 2 | 2, 4, 6, 8, 10 | {} | 0 | 5 | 

Mặt nạ giống nhau được sử dụng lại cho cả hai lệnh. XOR nó hai lần sẽ hủy lần chuyển đổi đầu tiên, nhưng trạng thái trung gian với năm đèn tắt vẫn được xem xét, đưa ra câu trả lời`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(kN/w) công việc từ đóng gói | Mỗi lệnh thực hiện XOR số nguyên lớn và đếm tổng thể trên khoảng N bit | 
| Không gian | O(kN/w) | Tối đa k mặt nạ, mỗi mặt nạ chứa N bit | 

Trạng thái lớn nhất chứa 10 bit 6, khoảng 125 KB. Ngay cả việc lưu giữ 100 mặt nạ riêng biệt cũng chỉ cần khoảng 12,5 MB dung lượng lưu trữ bit thô, thấp hơn giới hạn bộ nhớ 256 MB. Biểu diễn đóng gói cũng tránh được 10.8 cập nhật ánh sáng ở cấp độ Python mà mô phỏng trực tiếp có thể yêu cầu. 

Cấu trúc mặt nạ sử dụng các phép toán số nguyên có độ chính xác tùy ý gốc của Python thay vì vòng lặp Python trên tất cả các bội số. Đây là chi tiết triển khai quan trọng giúp phương pháp tiếp cận bitset trở nên thực tế trong giới hạn 1 giây ban đầu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    k = int(input())
    commands = [int(input()) for _ in range(k)]

    masks = {}

    def build_mask(x):
        if x in masks:
            return masks[x]

        m = n // x
        pattern = ((1 << (m * x)) - 1) // ((1 << x) - 1)
        mask = pattern << (x - 1)

        masks[x] = mask
        return mask

    state = 0
    answer = 0

    for x in commands:
        state ^= build_mask(x)
        answer = max(answer, state.bit_count())

    return answer

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return str(solve()) + "\n"
    finally:
        sys.stdin = old_stdin

# Provided sample
assert run("""10
4
6
2
1
3
""") == "6\n", "sample 1"

# Minimum size
assert run("""1
1
1
""") == "1\n", "single light"

# Repeated commands
assert run("""10
2
2
2
""") == "5\n", "repeated command"

# Boundary command N, followed by command 1
assert run("""5
2
5
1
""") == "4\n", "boundary multiple"

# Maximum-size input, all commands equal
assert run("1000000\n100\n" + "1\n" * 100) == "1000000\n", \
    "maximum N and repeated command"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 / 4 / 6 2 1 3`| 6 | Cung cấp mẫu và trung gian tối đa | 
|`1 / 1 / 1`| 1 | Mặt nạ N tối thiểu và bit đơn | 
|`10 / 2 / 2 2`| 5 | Chuyển đổi lặp đi lặp lại và trạng thái trung gian | 
|`5 / 2 / 5 1`| 4 | Bội số ở chính xác ranh giới trên | 
|`1000000 / 100 / 1 ... 1`| 1000000 | N tối đa, k tối đa và tái sử dụng mặt nạ lặp đi lặp lại | 

## Vỏ cạnh 

Đối với trường hợp tối thiểu,```
1
1
1
```chúng tôi có`x = 1`,`m = 1`, và mặt nạ được xây dựng là`1`. XOR trạng thái ban đầu`0`với`1`cho`state = 1`, có số lượng dân số là 1. Thuật toán trả về`1`, đó là mức tối đa duy nhất có thể. 

Đối với các lệnh lặp lại,```
10
2
2
2
```mặt nạ cho`2`đại diện cho các đèn 2, 4, 6, 8 và 10. XOR đầu tiên tạo ra năm bit cố định, do đó câu trả lời trở thành 5. XOR thứ hai xóa chính xác năm bit đó. Trạng thái cuối cùng bằng 0 nhưng mức tối đa được ghi vẫn là 5. Đây là lý do tại sao thuật toán đánh giá`bit_count()`sau mỗi lệnh. 

Đối với lệnh bằng N,```
5
2
5
1
```mặt nạ đầu tiên chỉ có tập hợp bit 4, biểu thị ánh sáng 5. Do đó, trạng thái có một tập hợp bit. Lệnh thứ hai có mỗi một trong năm bit được đặt, do đó XOR thay đổi trạng thái từ`10000`ĐẾN`01111`. Bốn bit được thiết lập, đưa ra câu trả lời đúng là 4. Việc sử dụng cấu trúc của`n // x`xử lý điểm cuối một cách chính xác.

 Đối với trường hợp lặp lại kích thước tối đa,```
1000000
100
1
1
...
1
```mặt nạ cho`1`được tái sử dụng cho tất cả 100 lệnh. Sau mỗi lệnh đánh số lẻ, tất cả một triệu đèn đều tắt, trong khi sau mỗi lệnh đánh số chẵn, tất cả đều sáng. Do đó, mức tối đa là 1.000.000. Thuật toán không xây dựng lại mặt nạ triệu bit 100 lần và các thay đổi trạng thái chỉ là các thao tác XOR lặp lại trên cùng một số nguyên được đóng gói.
