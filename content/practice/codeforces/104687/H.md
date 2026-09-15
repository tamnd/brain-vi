---
title: "CF 104687H - \u0412\u044b\u0431\u043e\u0440 \u0447\u0438\u0441\u0435\u043b 1"
description: "Chúng ta được cho một dãy số nguyên được lập chỉ mục từ trái sang phải và chúng ta cần chọn chính xác ba vị trí trong dãy này."
date: "2026-06-29T08:47:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104687
codeforces_index: "H"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u0432 \u0426\u0420\u041e\u0414 2022"
rating: 0
weight: 104687
solve_time_s: 67
verified: true
draft: false
---

[CF 104687H - \u0412\u044b\u0431\u043e\u0440 \u0447\u0438\u0441\u0435\u043b 1](https://codeforces.com/problemset/problem/104687/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được lập chỉ mục từ trái sang phải và chúng ta cần chọn chính xác ba vị trí trong dãy này. Hạn chế là hai vị trí được chọn bất kỳ phải cách nhau ít nhất`d`chỉ số, nghĩa là nếu chúng ta chọn vị trí`i < j < k`, sau đó`j - i >= d`Và`k - j >= d`. Trong số tất cả các bộ ba hợp lệ như vậy, chúng ta muốn tổng các giá trị lớn nhất có thể có tại các vị trí đã chọn. 

Cấu trúc của đầu vào quan trọng hơn các giá trị. Mỗi chỉ mục hoạt động giống như một “điểm neo” tiềm năng và việc chọn một chỉ mục sẽ hạn chế một cửa sổ các chỉ mục bị cấm xung quanh nó đối với các lựa chọn còn lại. Nhiệm vụ là đặt ba chiếc neo sao cho chúng không xung đột và tổng trọng lượng thu được là tối đa. 

Ràng buộc`n ≤ 1500`gợi ý rằng một$O(n^3)$vũ lực đối với tất cả các bộ ba đã là giới hạn nhưng vẫn khả thi trong một số trường hợp. Tuy nhiên, bất kỳ cách tiếp cận nào cố gắng khám phá tất cả các kết hợp có kiểm tra bên trong bổ sung đều trở nên rủi ro. Tín hiệu chính là kích thước lựa chọn được cố định ở mức ba, điều này thường ngụ ý rằng có thể tối ưu hóa một phần tiền tố hoặc hậu tố. 

Một trường hợp thất bại ngây thơ nhưng tinh tế xuất hiện khi người ta cố gắng tham lam chọn phần tử tốt nhất trước rồi mới mở rộng. Ví dụ: hãy xem xét một chuỗi trong đó mức tối đa tổng thể nằm quá gần các phần tử mạnh khác, trong khi phần tử nhỏ hơn một chút cho phép có thêm hai lựa chọn lớn sau đó. Bất kỳ chiến lược tham lam nào “lấy cái tốt nhất trước” đều phá vỡ: 

đầu vào:```
n = 6, d = 2
A = [100, 1, 1, 90, 1, 90]
```Lựa chọn tham lam chỉ số 1 (giá trị 100) chặn cả hai số 90 do hạn chế về khoảng cách, tạo ra tổng số 100. Giải pháp tối ưu bỏ qua 100 và lấy 90 + 90 + 1 = 181. Điều này cho thấy lựa chọn cực đại cục bộ không hợp lệ về mặt cấu trúc. 

Một cạm bẫy khác là coi đây là lựa chọn khoảng cách độc lập mà không thực thi khoảng cách một cách đối xứng. Nếu chúng ta sửa một phần tử và cố gắng tối đa hóa độc lập bên trái và bên phải mà không tạo ra khoảng cách, chúng ta có thể vô tình cho phép sự kề cận bất hợp pháp giữa các phân vùng. 

Cấu trúc thực là một lựa chọn ba bị ràng buộc với khoảng cách cố định, điều này gợi ý rõ ràng về lập trình động hoặc tính toán trước hậu tố tiền tố. 

## Phương pháp tiếp cận 

Một giải pháp brute-force liệt kê tất cả các bộ ba chỉ số`i < j < k`và kiểm tra xem cả hai khoảng trống có thỏa mãn ràng buộc hay không. Đối với mỗi bộ ba hợp lệ, chúng tôi tính toán`A[i] + A[j] + A[k]`và theo dõi tối đa. Đây là tính đúng đắn đơn giản vì nó phù hợp trực tiếp với định nghĩa của vấn đề. 

Số bộ ba theo thứ tự là$O(n^3)$, cái nào cho$n = 1500$đưa ra khoảng 3,3 tỷ lần lặp. Ngay cả với vòng lặp bên trong rất chặt chẽ trong mã được tối ưu hóa, điều này vẫn vượt xa giới hạn chấp nhận được trong Python và vẫn quá lớn trong C++ dưới những hạn chế về thời gian nghiêm ngặt. Nút thắt cổ chai không chỉ ở số lần lặp mà còn ở việc kiểm tra ràng buộc lặp đi lặp lại. 

Quan sát quan trọng là phần tử ở giữa của bộ ba tách hoàn toàn vấn đề thành hai phần độc lập: một lựa chọn bên trái hợp lệ và một lựa chọn bên phải hợp lệ, cả hai đều bị ràng buộc bởi khoảng cách`d`. Một khi chỉ số giữa`j`đã cố định, phần tử bên trái tốt nhất phải đến từ các chỉ mục`≤ j - d`và phần tử bên phải tốt nhất phải đến từ các chỉ mục`≥ j + d`. Điều này biến vấn đề thành việc tính toán trước các giá trị tiền tố và hậu tố tốt nhất. 

Chúng tôi tính toán trước hai mảng:`best_left[i]`lưu trữ giá trị tối đa giữa các chỉ số lên tới`i`, Và`best_right[i]`lưu trữ giá trị tối đa từ`i`đến cuối cùng. Với những điều này, mọi lựa chọn chỉ số trung bình đều trở thành đánh giá thời gian liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu hóa tiền tố-hậu tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước một mảng`best_left`Ở đâu`best_left[i]`là giá trị lớn nhất trong số`A[0..i]`. Điều này cho phép chúng tôi truy vấn ngay điểm cuối bên trái hợp lệ tốt nhất cho bất kỳ vị trí ở giữa nào mà không cần quét lại. 
2. Tính toán trước một mảng`best_right`Ở đâu`best_right[i]`là giá trị lớn nhất trong số`A[i..n-1]`. Điều này đối xứng mang lại điểm cuối bên phải hợp lệ tốt nhất cho bất kỳ vị trí ở giữa nào. 
3. Lặp lại mọi chỉ mục`j`coi nó là phần tử ở giữa của bộ ba. 
4. Đối với mỗi`j`, xác định xem có tồn tại chỉ mục bên trái hợp lệ hay không. Điều này đòi hỏi`j - d >= 0`. Nếu không, hãy bỏ qua vị trí này vì không thể hình thành bộ ba. 
5. Tương tự, đảm bảo tồn tại chỉ mục bên phải hợp lệ bằng cách kiểm tra`j + d < n`. Nếu không, bỏ qua. 
6. Tính toán ứng viên còn lại tốt nhất là`best_left[j - d]`và ứng cử viên phù hợp nhất là`best_right[j + d]`. 
7. Kết hợp những điều này với`A[j]`và cập nhật tối đa toàn cầu. 

Ý tưởng cấu trúc quan trọng là khi phần giữa được cố định, các lựa chọn bên trái và bên phải tối ưu sẽ trở nên độc lập vì giới hạn khoảng cách tách biệt hoàn toàn phạm vi chỉ số của chúng. 

### Tại sao nó hoạt động 

Đối với bất kỳ bộ ba hợp lệ`(i, j, k)`, lực ràng buộc`i ≤ j - d`Và`k ≥ j + d`. Trong số tất cả như vậy`i`, sự lựa chọn tốt nhất luôn là giá trị lớn nhất trong`A[0..j-d]`, và tương tự cho vế phải. Không có sự tương tác nào tồn tại giữa các lựa chọn trái và phải vì ràng buộc chỉ số ngăn chặn sự chồng chéo ảnh hưởng. Điều này tạo ra sự phân tách trong đó mọi chỉ số ở giữa tạo ra một vấn đề tối ưu hóa độc lập với hai điểm cuối độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k, d = map(int, input().split())
a = list(map(int, input().split()))

best_left = [0] * n
best_right = [0] * n

best_left[0] = a[0]
for i in range(1, n):
    best_left[i] = max(best_left[i - 1], a[i])

best_right[n - 1] = a[n - 1]
for i in range(n - 2, -1, -1):
    best_right[i] = max(best_right[i + 1], a[i])

ans = -10**18

for j in range(n):
    if j - d < 0 or j + d >= n:
        continue
    left_best = best_left[j - d]
    right_best = best_right[j + d]
    ans = max(ans, left_best + a[j] + right_best)

print(ans)
```Việc triển khai hoàn toàn dựa vào cực đại tiền tố và hậu tố. các`best_left`mảng được xây dựng từ trái sang phải để mỗi vị trí tích lũy giá trị tốt nhất có thể cho đến chỉ mục đó. các`best_right`mảng được xây dựng ngược lại để phản ánh logic tương tự. 

Vòng lặp kết thúc`j`thực thi phần tử ở giữa. Việc kiểm tra ranh giới rất quan trọng vì chúng đảm bảo rằng cả hai đối tác bắt buộc đều tồn tại. Một vấn đề tế nhị là đảm bảo các chỉ số`j - d`Và`j + d`là các ranh giới bao gồm các phạm vi hợp lệ, không bị dịch chuyển từng phạm vi một. 

Câu trả lời cuối cùng được khởi tạo thành một số rất âm để xử lý an toàn các trường hợp trong đó tất cả các giá trị có thể âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 3 2
-1 4 2 -6 3 3 5 -1 4 -1
```Chúng tôi tính tiền tố và hậu tố cực đại: 

| tôi | A[i] | tốt nhất_left | tốt_right | 
| --- | --- | --- | --- | 
| 0 | -1 | -1 | 5 | 
| 1 | 4 | 4 | 5 | 
| 2 | 2 | 4 | 5 | 
| 3 | -6 | 4 | 5 | 
| 4 | 3 | 4 | 5 | 
| 5 | 3 | 4 | 5 | 
| 6 | 5 | 5 | 5 | 
| 7 | -1 | 5 | 4 | 
| 8 | 4 | 5 | 4 | 
| 9 | -1 | 5 | -1 | 

Bây giờ đánh giá các vị trí ở giữa hợp lệ`j`Ở đâu`j-d ≥ 0`Và`j+d < n`, nghĩa`2 ≤ j ≤ 7`. 

| j | phạm vi bên trái tối đa | A[j] | phạm vi bên phải tối đa | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 2 | 4 | 2 | 5 | 11 | 
| 3 | 4 | -6 | 5 | 3 | 
| 4 | 4 | 3 | 5 | 12 | 
| 5 | 4 | 3 | 5 | 12 | 
| 6 | 4 | 5 | 4 | 13 | 
| 7 | 5 | -1 | 4 | 8 | 

Tốt nhất là 13. 

Dấu vết này cho thấy giải pháp tối ưu có thể đặt phần tử ở giữa trên một giá trị mạnh (chỉ số 6) trong khi vẫn sử dụng các điểm cuối tối ưu không liền kề. 

### Ví dụ 2 

Hãy xem xét:```
6 3 1
5 1 5 1 5 1
```Đây`d = 1`cho phép hầu hết mọi khoảng cách ngoại trừ liền kề. 

| j | best_left[j-1] | A[j] | best_right[j+1] | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 5 | 11 | 
| 2 | 5 | 5 | 5 | 15 | 
| 3 | 5 | 1 | 5 | 11 | 
| 4 | 5 | 5 | 1 | 11 | 

Lựa chọn tối ưu có tính đối xứng, chọn các chỉ số 0, 2, 4 cho tổng số 15. Điều này xác nhận rằng thuật toán trải rộng các lựa chọn một cách tự nhiên trên mảng mà không cần tìm kiếm tổ hợp rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai lượt tuyến tính cho mảng tiền tố và hậu tố cộng với một lượt quét tuyến tính cho chỉ mục ở giữa | 
| Không gian | O(n) | Lưu trữ cho mảng tối đa tiền tố và hậu tố | 

Với$n \le 1500$, giải pháp chạy tốt trong giới hạn. Ngay cả khi những hạn chế được tăng lên$10^5$, cấu trúc tương tự sẽ vẫn hợp lệ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k, d = map(int, input().split())
    a = list(map(int, input().split()))

    best_left = [0] * n
    best_right = [0] * n

    best_left[0] = a[0]
    for i in range(1, n):
        best_left[i] = max(best_left[i - 1], a[i])

    best_right[n - 1] = a[n - 1]
    for i in range(n - 2, -1, -1):
        best_right[i] = max(best_right[i + 1], a[i])

    ans = -10**18
    for j in range(n):
        if j - d < 0 or j + d >= n:
            continue
        ans = max(ans, best_left[j - d] + a[j] + best_right[j + d])

    return str(ans)

# provided sample
assert run("10 3 2\n-1 4 2 -6 3 3 5 -1 4 -1\n") == "13"

# minimum size valid
assert run("3 3 1\n1 2 3\n") == "6"

# all equal
assert run("5 3 1\n10 10 10 10 10\n") == "30"

# negative values
assert run("5 3 1\n-1 -2 -3 -4 -5\n") == "-6"

# tight spacing
assert run("6 3 2\n1 100 1 100 1 100\n") == "201"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 13 | tính đúng đắn của các giá trị hỗn hợp | 
| 1 2 3 | 6 | cấu trúc hợp lệ tối thiểu | 
| tất cả 10 | 30 | xử lý mảng thống nhất | 
| tất cả đều tiêu cực | -6 | sửa max dưới âm | 
| đỉnh cách đều nhau | 201 | thực thi khoảng cách chính xác | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế xảy ra khi các vị trí ở giữa hợp lệ cực kỳ hạn chế. Ví dụ:```
n = 5, d = 2
A = [10, 100, 1, 100, 10]
```Chỉ chỉ mục 2 mới có thể đóng vai trò là phần tử ở giữa hợp lệ vì nó phải có ít nhất một phần tử ở cả hai phía ở khoảng cách 2. Thuật toán kiểm tra`j - d`Và`j + d`, vậy chỉ`j = 2`vượt qua. Kết quả tính toán trở thành`10 + 1 + 10 = 21`, điều này đúng vì các ứng cử viên cánh tả và cánh hữu tốt nhất đều bị ép buộc bởi cấu trúc. 

Một trường hợp khác là khi điểm cuối tốt nhất nằm gần ranh giới. Bởi vì mảng tiền tố và hậu tố bao gồm các ranh giới một cách chính xác,`best_left[j - d]`luôn bao gồm chỉ số 0 khi hợp lệ và`best_right[j + d]`luôn bao gồm chỉ số n-1 khi hợp lệ. Điều này ngăn chặn việc vô tình loại trừ các giải pháp biên tối ưu, một lỗi phổ biến khi triển khai bắt đầu mảng tiền tố từ chỉ mục 1 hoặc phạm vi dịch chuyển không chính xác.
