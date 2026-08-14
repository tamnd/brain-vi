---
title: "CF 102309F - Fullerene của gấu trúc Orz"
description: "Chúng ta có một fullerene C60 cố định, có 60 nguyên tử carbon là các đỉnh của một khối đa diện cắt ngắn, khối đa diện quen thuộc của quả bóng đá. Mỗi đỉnh có thể không thay đổi hoặc nhận một trong n loại nguyên tử."
date: "2026-08-13T06:46:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "F"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 242
verified: true
draft: false
---

[CF 102309F - Fullerene của Orz Pandas](https://codeforces.com/problemset/problem/102309/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một fullerene C60 cố định, có 60 nguyên tử carbon là các đỉnh của một khối đa diện cắt ngắn, khối đa diện quen thuộc của quả bóng đá. Mỗi đỉnh có thể không thay đổi hoặc nhận một trong n loại nguyên tử. Do đó, mỗi đỉnh có n + 1 trạng thái có thể: trống, loại 1, loại 2, v.v. Nhiệm vụ là đếm các màu ở đỉnh thu được cho đến khi chuyển động quay vật lý của phân tử. Đầu vào chứa một số nguyên không âm n cho mỗi trường hợp thử nghiệm cho đến EOF và mỗi đầu ra là số lượng phân tử riêng biệt modulo 1000000007. Tuyên bố chính thức xác nhận rằng chỉ các phép quay mới xác định được hai dẫn xuất, do đó hình ảnh phản chiếu vẫn khác biệt. 

Giá trị của n có thể là bất kỳ số nguyên 32 bit có dấu không âm nào, do đó, nó có thể lớn bằng 2147483647. Số đỉnh được cố định là 60, có nghĩa là không cần thuật toán tùy thuộc vào kích thước biểu đồ thay đổi. Thách thức là số lượng các màu có thể có là số mũ của 60 và bản thân n có thể là hàng tỷ. Bất kỳ phương pháp nào xem xét rõ ràng ngay cả một phần nhỏ của tất cả các chất tạo màu đều không thể thực hiện được. Chúng ta cần một công thức chỉ chứa một số lũy thừa môđun không đổi. 

Có ba trường hợp cạnh rất dễ xử lý sai. Đầu tiên, n = 0 có nghĩa là chỉ có phân tử chưa biến đổi, vì vậy câu trả lời phải là 1. Một phương pháp diễn giải n là số trạng thái thay vì số loại nguyên tử sẽ trả về 0 không chính xác hoặc xử lý không chính xác trạng thái trống. 

Thứ hai, không được phép đưa vào những phản ánh. Nhóm đối xứng hai mươi mặt đầy đủ có 120 phần tử, nhưng chỉ có 60 phần tử bảo toàn hướng của nó là phép quay. Sử dụng 120 trong bổ đề Burnside sẽ xác định được các phân tử bất đối bằng hình ảnh phản chiếu của chúng và giải quyết được một vấn đề khác. Nhóm quay của một vật đối xứng hai mươi mặt có cấp 60. 

Thứ ba, phép chia cho 60 phải được thực hiện theo modulo 1000000007 bằng cách sử dụng nghịch đảo mô-đun của 60. Việc thực hiện phép chia số nguyên sau khi rút gọn từng số hạng một cách độc lập là không hợp lệ. Vì 60 và 1000000007 là số nguyên tố cùng nhau nên phép nhân với 60^-1 modulo 1000000007 sẽ cho kết quả chính xác như mong muốn. 

Ví dụ, đầu vào nhỏ nhất là```
0
```và đầu ra đúng là```
1
```Có chính xác một phân tử có thể tồn tại vì không có loại nguyên tử nào tồn tại và mọi đỉnh phải trống. 

Đối với một trường hợp nhỏ khác,```
1
```có hai trạng thái trên mỗi đỉnh, trống hoặc loại nguyên tử đơn. Công thức Burnside cho 544393230 modulo 1000000007. Việc triển khai bất cẩn sử dụng n thay vì n + 1 vì số trạng thái đỉnh sẽ chỉ tính một màu và trả về 1. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là gán một trong n + 1 trạng thái cho mỗi một trong số 60 đỉnh. Điều đó tạo ra chính xác (n + 1)^60 màu được dán nhãn. Đối với mỗi màu, chúng ta có thể tạo tất cả 60 phép quay, chọn một đại diện chuẩn và chèn đại diện đó vào một bộ. Điều này đúng vì hai chất tạo màu được dán nhãn thuộc về cùng một phân tử khi cái này quay với cái kia. 

Vấn đề là số lượng màu. Trong trường hợp xấu nhất, n = 2147483647, do đó, tìm kiếm vũ phu chứa 2147483648^60 bài tập, khoảng 10^558. Thậm chí chỉ chạm vào từng nhiệm vụ một lần là không thể. Tổng quát hơn, một phép liệt kê đơn giản sẽ tốn Θ(60(n + 1)^60) nếu chúng ta kiểm tra mọi phép quay cho mỗi màu. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải tạo ra một màu sắc. Điều quan trọng đối với bổ đề Burnside là có bao nhiêu màu không thay đổi sau mỗi phép quay. 

Bổ đề Burnside nói rằng số quỹ đạo là số lượng màu trung bình cố định bởi mỗi phần tử nhóm. Đối với một phép quay có hoán vị của 60 đỉnh bao gồm c chu kỳ độc lập, việc tô màu được cố định chính xác khi mọi đỉnh trong mỗi chu kỳ nhận được cùng một trạng thái. Mỗi chu trình có thể chọn độc lập một trong n + 1 trạng thái, sao cho phép quay cố định (n + 1)^c màu. 

Khối hai mươi mặt cắt cụt có nhóm đối xứng quay của khối hai mươi mặt, với 60 phép quay. Các phép quay không đồng nhất của nó chỉ có ba bậc có thể. Có 15 phép quay cấp 2, 20 phép quay cấp 3 và 24 phép quay cấp 5. Những số lượng này cũng có thể được nhìn thấy về mặt hình học: có 15 trục hai lần, 10 trục ba lần với hai phép quay không đồng nhất trên mỗi trục và 6 trục năm lần với bốn phép quay không đồng nhất trên mỗi trục. Bản thân khối 20 mặt cắt cụt có 60 đỉnh, 90 cạnh, 12 hình ngũ giác và 20 hình lục giác. 

Một phép quay không đồng nhất không có đỉnh cố định. Do đó, phép quay cấp 2 chia 60 đỉnh thành 30 chu kỳ có độ dài 2, phép quay cấp 3 chia chúng thành 20 chu kỳ có độ dài 3 và phép quay cấp 5 chia chúng thành 12 chu kỳ có độ dài 5. Do đó, toàn bộ phép tính thu gọn thành 

[ 
\frac{(n+1)^{60}+15(n+1)^{30}+20(n+1)^{20}+24(n+1)^{12}}{60}. 
] 

Cách tiếp cận bạo lực hoạt động vì nó xây dựng rõ ràng mọi lớp tương đương. Nó thất bại vì về mặt thiên văn học có rất nhiều chất tạo màu được dán nhãn. Bổ đề Burnside cho phép chúng ta đếm các lớp đó một cách gián tiếp và số đỉnh cố định có nghĩa là chỉ cần bốn lũy thừa mô-đun cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(60(n + 1)^60) | O((n + 1)^60) | Quá chậm | 
| Bổ đề Burnside | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n và xác định q = n + 1. Trạng thái bổ sung biểu thị việc giữ nguyên một nguyên tử carbon, trong khi n trạng thái còn lại biểu thị các loại nguyên tử có sẵn. 
2. Xét 60 đối xứng quay của khối hai mươi mặt bị cắt cụt. Phép quay đồng nhất có 60 chu kỳ một đỉnh, 15 phép quay có 30 chu kỳ hai đỉnh, 20 phép quay có 20 chu kỳ ba đỉnh và 24 phép quay có 12 chu kỳ năm đỉnh. 
3. Để nhận dạng, mỗi đỉnh có thể được chọn độc lập nên số lượng màu cố định là q^60. 
4. Đối với phép quay bậc 2, mọi cặp đỉnh trong một chu kỳ phải nhận cùng một trạng thái. Có 30 chu kỳ nên số lượng màu cố định là q^30. 
5. Đối với phép quay bậc 3, có 20 chu kỳ, cho q^20 màu cố định. 
6. Đối với một phép quay bậc 5, có 12 chu kỳ, cho q^12 màu cố định. 
7. Áp dụng bổ đề Burnside và tính trung bình cộng cho tất cả 60 phép quay:

[ 
trả lời = 
(q^{60}+15q^{30}+20q^{20}+24q^{12})/60. 
] 
8. Tính tất cả lũy thừa modulo 1000000007 bằng cách sử dụng hàm mũ mô-đun của Python. Phép nhân với nghịch đảo mô đun của 60 sẽ thay thế phép chia cuối cùng. 

Bất biến đằng sau phép tính là màu được cố định bởi một phép quay phải không đổi trên mỗi chu kỳ của phép quay đó. Ngược lại, việc gán một trạng thái tùy ý một cách độc lập cho mỗi chu kỳ luôn tạo ra một màu cố định theo phép quay đó. Do đó, một hoán vị với chu trình c sẽ xác định chính xác các màu q^c. Sau đó, Burnside đếm mọi lớp tương đương quay chính xác một lần sau khi lấy trung bình số lượng màu cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007
INV60 = 616666671

def solve(n):
    q = (n + 1) % MOD

    p60 = pow(q, 60, MOD)
    p30 = pow(q, 30, MOD)
    p20 = pow(q, 20, MOD)
    p12 = pow(q, 12, MOD)

    total = (
        p60
        + 15 * p30
        + 20 * p20
        + 24 * p12
    ) % MOD

    return total * INV60 % MOD

def main():
    out = []
    for line in sys.stdin:
        line = line.strip()
        if line:
            out.append(str(solve(int(line))))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Các hằng số ở trên cùng chứa mô đun và nghịch đảo mô đun của 60. Nghịch đảo là 616666671 vì 

[ 
60 \cdot 616666671 \equiv 1 \pmod {1000000007}. 
] 

các`solve`trước tiên, hàm chuyển đổi số loại nguyên tử thành số trạng thái có thể có trên mỗi đỉnh. Lấy`n + 1`trước khi giảm modulo, mô đun là an toàn vì phép lũy thừa mô đun bảo toàn phần dư cần thiết. 

Mỗi cuộc gọi đến`pow(q, exponent, MOD)`thực hiện phép lũy thừa bằng cách bình phương, do đó các giá trị số mũ 60, 30, 20 và 12 chỉ yêu cầu một số phép nhân không đổi. Số nguyên Python cũng tránh bị tràn, mặc dù mọi giá trị trung gian trong lũy ​​thừa mô-đun vẫn bị giới hạn bởi mô-đun. 

Các hệ số 15, 20 và 24 phải được áp dụng cho số chu kỳ tương ứng. Việc hoán đổi các hệ số này là nguyên nhân phổ biến dẫn đến các câu trả lời sai vì số mũ được xác định bởi số chu kỳ chứ không phải theo thứ tự của phép quay. 

Vòng lặp đầu vào cố tình đọc cho đến khi EOF. Vấn đề ban đầu không cung cấp số lượng trường hợp thử nghiệm, do đó việc đọc một số nguyên trên mỗi dòng đầu vào không trống sẽ xử lý cả định dạng chính thức và nhiều trường hợp thử nghiệm trong một tệp. 

## Ví dụ đã hoạt động 

Mẫu chính thức chứa trường hợp thử nghiệm duy nhất n = 10, có câu trả lời là 130650357. 

Đối với dấu vết thứ hai, hãy xem xét n = 0. 

| n | q | q^60 | q^30 | q^20 | q^12 | Tử số Burnside | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 1 | 1 + 15 + 20 + 24 = 60 | 1 | 

Mỗi phép quay cố định màu duy nhất có thể, vì vậy tổng các màu cố định là 60. Chia cho 60 phép quay để lại một quỹ đạo. Điều này xác nhận rằng đạo hàm trống được tính chính xác. 

Với n = 1, có hai trạng thái trên mỗi đỉnh. 

| n | q | q^60 mod M | q^30 mod M | q^20 mod M | q^12 mod M | Tử số mod M | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 536396504 | 73741817 | 1048576 | 4096 | 663593576 | 544393230 | 

Danh tính đóng góp 2^60, trong khi mỗi phép quay không đồng nhất buộc các đỉnh trong cùng một chu kỳ phải đồng ý. Bốn phần đóng góp màu cố định được kết hợp với các hệ số 1, 15, 20 và 24 trước khi nhân với nghịch đảo của 60. 

Đối với mẫu được cung cấp n = 10, q = 11. Phép tính là 

[ 
\frac{11^{60}+15\cdot11^{30}+20\cdot11^{20}+24\cdot11^{12}}{60} 
\pmod {1000000007}, 
] 

tạo ra 130650357. 

Dấu vết này chứng tỏ rằng thuật toán không bao giờ cần biết vị trí thực tế của 60 đỉnh. Chỉ có cấu trúc chu trình của mỗi lớp đối xứng mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Bốn lũy thừa mô-đun với số mũ có kích thước không đổi | 
| Không gian | O(1) | Chỉ một số nguyên không đổi được lưu trữ | 

Số mũ thực tế được cố định ở 60, 30, 20 và 12, do đó thời gian chạy về mặt kỹ thuật là O(1) đối với n. Việc mô tả nó là O(log n) phản ánh độ phức tạp tiêu chuẩn của phép lũy thừa mô-đun và vẫn hợp lệ. Giới hạn trên rất lớn của n không ảnh hưởng đến khối lượng công việc được yêu cầu. Giải pháp sử dụng bộ nhớ không đổi và dễ dàng phù hợp với giới hạn 1 giây và 256 MB đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1000000007
INV60 = 616666671

def solve_one(n):
    q = (n + 1) % MOD
    total = (
        pow(q, 60, MOD)
        + 15 * pow(q, 30, MOD)
        + 20 * pow(q, 20, MOD)
        + 24 * pow(q, 12, MOD)
    ) % MOD
    return total * INV60 % MOD

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        out = []
        for line in sys.stdin:
            line = line.strip()
            if line:
                out.append(str(solve_one(int(line))))
        return "\n".join(out)
    finally:
        sys.stdin = old_stdin

# Provided sample
assert run("10\n") == "130650357", "provided sample"

# Minimum-size input
assert run("0\n") == "1", "n = 0"

# Small binary-state case
assert run("1\n") == "544393230", "n = 1"

# Another small case
assert run("2\n") == "696266552", "n = 2"

# Repeated equal values, checking independent test-case handling
assert run("10\n10\n0\n") == "130650357\n130650357\n1", "repeated test cases"

# Maximum signed 32-bit nonnegative value.
# The expected value is computed independently from the closed formula,
# rather than relying on the implementation's intermediate variables.
n = 2147483647
q = (n + 1) % MOD
expected_max = (
    pow(q, 60, MOD)
    + 15 * pow(q, 30, MOD)
    + 20 * pow(q, 20, MOD)
    + 24 * pow(q, 12, MOD)
) % MOD
expected_max = expected_max * INV60 % MOD

assert run(str(n) + "\n") == str(expected_max), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Đầu vào tối thiểu và đạo hàm rỗng | 
|`1`|`544393230`| Sử dụng đúng trạng thái n + 1 | 
|`2`|`696266552`| Tính toán Burnside không cần thiết | 
|`10`|`130650357`| Mẫu chính thức | 
|`10, 10, 0`|`130650357, 130650357, 1`| Nhiều trường hợp thử nghiệm và giá trị lặp lại | 
|`2147483647`| Tính theo công thức tham khảo | Đầu vào 32-bit có dấu tối đa và lũy thừa mô-đun lớn | 

## Vỏ cạnh 

Với n = 0, đầu vào là```
0
```và q = 1. Mỗi vòng quay sẽ sửa một màu hoàn toàn trống. Bốn số hạng là 1, 15, 20 và 24, cho tử số Burnside là 60 và câu trả lời là 1. Thuật toán xử lý việc này một cách tự nhiên mà không cần nhánh đặc biệt. 

Với n = 1, đầu vào là```
1
```và q = 2. Danh tính sửa được 2^60 màu, xoay theo thứ tự 2 sửa được 2^30, xoay theo thứ tự 3 sửa được 2^20 và xoay theo thứ tự 5 sửa được 2^12. Sau khi áp dụng bội số 15, 20 và 24 rồi chia cho 60 modulo 1000000007, kết quả là 544393230. Điều này mắc phải lỗi phổ biến là quên trạng thái chưa sửa đổi. 

Đối với cấu hình chirus, thuật toán sử dụng chính xác 60 phép quay thay vì 120 phép đối xứng không gian. Một sự phản xạ không được phép xác định hai đạo hàm, do đó phải loại trừ một nửa đảo ngược hướng của nhóm đối xứng hai mươi mặt đầy đủ. Đây là lý do tại sao mẫu số Burnside là 60 và tại sao các hệ số có tổng bằng 60 là 1 + 15 + 20 + 24. 

Để có đầu vào tối đa,```
2147483647
```chúng ta có q = 2147483648. Thuật toán không bao giờ xây dựng q^60 dưới dạng số nguyên thông thường. Mỗi công suất được giảm theo modulo 1000000007 bằng cách sử dụng lũy ​​thừa mô-đun, do đó giá trị lớn của n không gây ra vấn đề tràn hoặc hiệu suất. Phép nhân cuối cùng với INV60 thực hiện phép chia được yêu cầu trong trường mô-đun. 

Cuối cùng, khi một số trường hợp thử nghiệm chứa n giống nhau, mọi trường hợp đều được đánh giá độc lập. Không có biểu đồ có thể thay đổi hoặc trạng thái được tính toán trước nào phụ thuộc vào trường hợp trước đó, do đó, các đầu vào lặp lại phải tạo ra các đầu ra giống hệt nhau. Vòng lặp đầu vào dựa trên EOF cũng có nghĩa là dòng cuối cùng trống hoặc khoảng trắng ở cuối không tạo ra trường hợp kiểm thử giả.
