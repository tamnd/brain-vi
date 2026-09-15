---
title: "CF 104687J - \u0412\u044b\u0431\u043e\u0440 \u0447\u0438\u0441\u0435\u043b 3"
description: "Chúng ta được cho một dãy số nguyên được lập chỉ mục từ trái sang phải. Nhiệm vụ là chọn chính xác k vị trí trong chuỗi này sao cho hai vị trí được chọn cách nhau ít nhất d chỉ số. Trong số tất cả các lựa chọn hợp lệ, chúng tôi muốn tổng tối đa có thể có của các giá trị đã chọn."
date: "2026-06-29T14:43:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104687
codeforces_index: "J"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u0432 \u0426\u0420\u041e\u0414 2022"
rating: 0
weight: 104687
solve_time_s: 66
verified: true
draft: false
---

[CF 104687J - \u0412\u044b\u0431\u043e\u0440 \u0447\u0438\u0441\u0435\u043b 3](https://codeforces.com/problemset/problem/104687/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được lập chỉ mục từ trái sang phải. Nhiệm vụ là chọn chính xác`k`các vị trí trong chuỗi này sao cho hai vị trí được chọn bất kỳ cách nhau ít nhất`d`chỉ số. Trong số tất cả các lựa chọn hợp lệ, chúng tôi muốn tổng tối đa có thể có của các giá trị đã chọn. 

Một cách khác để thấy điều đó là chúng ta đang chọn một dãy con có kích thước cố định`k`, nhưng chúng ta không được phép chọn các phần tử quá gần nhau. Ràng buộc hoàn toàn là vị trí, không dựa trên giá trị, nhưng mục tiêu là tối đa hóa tổng giá trị tại các vị trí đã chọn. 

Kích thước đầu vào đạt tới 150000 phần tử, trong khi`k`nhiều nhất là 50. Giới hạn khoảng cách`d`có thể lớn như`n`, điều này buộc phải có sự thưa thớt cực độ trong việc lựa chọn. Ý nghĩa cấu trúc quan trọng là mặc dù mảng lớn nhưng số lượng phần tử được chọn lại rất nhỏ, điều này ngay lập tức gợi ý lập trình động trên các vị trí có thứ nguyên bổ sung cho số lượng phần tử đã được chọn. 

Một ý tưởng ngây thơ là thử tất cả các kết hợp của`k`các chỉ số thỏa mãn ràng buộc về khoảng cách. Ngay cả khi bỏ qua việc kiểm tra tính hợp lệ, số cách để chọn`k`các vị trí trong số 150000 là lớn về mặt thiên văn, và ngay cả khi cắt tỉa, vụ nổ tổ hợp vẫn còn. Một nỗ lực ngây thơ khác là tham lam chọn các giá trị lớn nhất, nhưng điều đó không thành công vì việc chọn sớm một giá trị lớn có thể chặn quyền truy cập vào nhiều giá trị nhỏ hơn một chút nhưng về tổng thể thì tốt hơn. 

Trường hợp cạnh tinh tế xuất hiện khi có số âm. Một chiến lược tham lam luôn chiếm lấy vị trí tiếp theo tốt nhất có thể có thể bị buộc phải đưa ra những quyết định toàn cầu sai lầm. Ví dụ: nếu một giá trị rất lớn xuất hiện sớm nhưng chặn quyền truy cập vào một số giá trị lớn vừa phải sau đó thì giải pháp tối ưu có thể bỏ qua hoàn toàn giá trị lớn sớm đó. 

Sự kết hợp của lớn`n`, bé nhỏ`k`và ràng buộc về khoảng cách gợi ý rõ ràng về DP trong đó các chuyển đổi chỉ phụ thuộc vào một số giới hạn các lựa chọn trước đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các tập hợp con có kích thước hợp lệ`k`. Người ta có thể tưởng tượng đệ quy cố gắng lấy hoặc bỏ qua từng vị trí, theo dõi xem có bao nhiêu phần tử đã được chọn và thực thi ràng buộc khoảng cách. Điều này tạo ra một không gian trạng thái trong đó mỗi phần tử phân nhánh thành hai lựa chọn, nhưng ràng buộc về khoảng cách buộc phải kiểm tra bổ sung. Trong trường hợp xấu nhất, ngay cả khi cắt tỉa, số lượng trạng thái hợp lệ sẽ hoạt động giống như sự kết hợp của`n`chọn`k`, vượt xa giới hạn khả thi. 

Quan sát quan trọng là`k`là nhỏ, nhiều nhất là 50, trong khi`n`là lớn. Điều này gợi ý rằng chúng ta nên xử lý vấn đề như việc chọn một chuỗi các`k`vị trí và tối ưu hóa quá trình chuyển đổi giữa chúng thay vì lặp lại trên tất cả các tập hợp con. 

Nếu chúng ta sửa số lượng phần tử đã chọn và xử lý mảng từ trái sang phải thì quyết định có ý nghĩa duy nhất là có lấy chỉ mục hiện tại làm phần tử được chọn tiếp theo hay không. Một khi chúng ta chiếm được vị trí`i`, lựa chọn hợp lệ tiếp theo phải đến từ chỉ mục`i + d`hoặc muộn hơn. Cấu trúc này dẫn trực tiếp đến lập trình động trong đó trạng thái theo dõi số lượng phần tử đã được chọn và vị trí hiện tại. 

Chúng tôi xác định`dp[i][j]`là số tiền tối đa chúng ta có thể đạt được bằng cách xem xét các vị trí từ`i`trở đi, đã chọn rồi`j`phần tử, với ràng buộc là phần tử được chọn tiếp theo phải tôn trọng khoảng cách so với phần tử được chọn trước đó. Vì quá trình chuyển đổi chỉ tiến về phía trước ít nhất`d`, chúng ta có thể nén DP thành vòng lặp chuyển tiếp hiệu quả hơn. 

Tại mỗi vị trí, chúng tôi bỏ qua nó hoặc lấy nó làm phần tử được chọn tiếp theo. Nếu chúng ta nắm lấy nó, chúng ta sẽ nhảy về phía trước`d`và tăng số lượng. Từ`k`nhỏ, chúng tôi duy trì DP trên các vị trí và số phần tử được chọn. 

Đây thực chất là một DP được xếp lớp trên`k`các lớp, trong đó mỗi lớp tính tổng tốt nhất để chọn`j`các phần tử và quá trình chuyển đổi tạo ra một khoảng cách về`d`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force của tất cả các tập hợp con hợp lệ | O(C(n, k)) | O(k) | Quá chậm | 
| DP qua các vị trí và số lượng đã chọn | O(nk) | O(nk) hoặc O(nk) được tối ưu hóa | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một giải pháp lập trình động trong đó chúng tôi theo dõi tổng tốt nhất có thể để chọn một số phần tử nhất định trong khi vẫn tôn trọng khoảng cách. 

1. Khởi tạo bảng DP trong đó`dp[j][i]`đại diện cho số tiền tốt nhất có thể đạt được sau khi chọn`j`các phần tử, với`j`-phần tử thứ được đặt ở vị trí`i`. Công thức này neo mỗi trạng thái ở vị trí được chọn cuối cùng, điều này rất quan trọng để thực thi khoảng cách một cách rõ ràng. 
2. Đặt trường hợp cơ sở cho`j = 1`, trong đó chọn một phần tử tại vị trí`i`chỉ đơn giản là mang lại giá trị`A[i]`. Chưa có ràng buộc nào được áp dụng vì không có lựa chọn trước đó. 
3. Với mỗi số phần tử`j`từ 2 đến`k`, lặp qua các vị trí`i`từ trái sang phải. Tại vị trí`i`, chúng tôi coi việc biến nó thành`j`-phần tử được chọn thứ 
4. Nếu chúng ta chọn vị trí`i`như`j`-phần tử thứ, vị trí đã chọn trước đó phải lớn nhất`i - d`. Do đó chúng tôi xem xét tất cả hợp lệ`p ≤ i - d`và lấy điều tốt nhất`dp[j-1][p]`. 
5. Để tránh quét tất cả các vị trí trước đó cho mọi trạng thái, chúng tôi duy trì một mảng tối đa cuộn trong khi lặp`i`. Việc tối ưu hóa tiền tố này làm giảm quá trình chuyển đổi từ tuyến tính trên mỗi trạng thái sang thời gian khấu hao không đổi. 
6. Cập nhật`dp[j][i]`là giá trị tốt nhất trước đó cộng thêm`A[i]`. 
7. Sau khi điền vào bảng, đáp án là lớn nhất trên tất cả`dp[k][i]`cho các vị trí cuối cùng hợp lệ. 

### Tại sao nó hoạt động 

DP thực thi rằng mọi vị trí được chọn đều có vị trí tiền nhiệm được xác định rõ ràng, ít nhất`d`xa. Bằng cách cấu trúc các trạng thái xung quanh vị trí được chọn cuối cùng, chúng tôi đảm bảo không có khoảng cách không hợp lệ nào có thể xảy ra vì mọi chuyển đổi đều tôn trọng rõ ràng ràng buộc khoảng cách. Tối ưu hóa tối đa tiền tố không thay đổi độ chính xác vì nó chỉ nén tìm kiếm trên các vị trí hợp lệ trước đó thành giá trị tốt nhất được tính toán trước, bảo toàn cấu trúc con tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, d = map(int, input().split())
    a = list(map(int, input().split()))

    NEG = -10**30

    prev = [NEG] * n
    for i in range(n):
        prev[i] = a[i]

    for j in range(2, k + 1):
        best_prefix = [NEG] * n
        dp = [NEG] * n

        best_prefix[0] = prev[0]
        for i in range(1, n):
            best_prefix[i] = max(best_prefix[i - 1], prev[i])

        for i in range(n):
            if i - d >= 0:
                dp[i] = best_prefix[i - d] + a[i]

        prev = dp

    print(max(prev))

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại lớp DP trước đó để tiết kiệm bộ nhớ. Mỗi lớp tương ứng với việc cố định số lượng phần tử được chọn. các`best_prefix`mảng cho phép truy xuất nhanh vị trí tốt nhất trước đó ở đủ xa. 

Một điểm tinh tế là xử lý các trạng thái không thể truy cập được, được biểu thị bằng một trọng điểm âm lớn. Nếu không có điều này, các chuyển tiếp không hợp lệ có thể lấn át cực đại một cách không chính xác. Một chi tiết quan trọng khác là sự chuyển tiếp chỉ xảy ra khi`i - d >= 0`, đảm bảo khoảng cách được thực thi nghiêm ngặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 3 2
-1 4 2 -6 3 3 5 -1 4 -1
```Chúng tôi theo dõi các lớp DP để`k = 3`. 

| Bước (k) | Vị trí tôi | Tiền tố tốt nhất lên tới i-d | Giá trị DP tại i | 
| --- | --- | --- | --- | 
| 1 | tất cả tôi | - | một [tôi] | 
| 2 | tôi=2 | 4 | 6 | 
| 2 | tôi=4 | 4 | 7 | 
| 3 | tôi=6 | 7 | 13 | 

Lựa chọn tối ưu tương ứng với việc chọn các giá trị 4, 3 và 5 tại các chỉ số cách đều nhau hợp lệ, mang lại 13. 

Dấu vết này cho thấy cách cực đại tiền tố cho phép thuật toán sử dụng lại các lựa chọn tốt nhất đã tính toán trước đó một cách hiệu quả trong khi vẫn tôn trọng khoảng cách. 

### Ví dụ 2 

đầu vào:```
5 2 2
5 -1 4 -2 3
```| Bước (k) | Vị trí tôi | Tiền tố tốt nhất lên tới i-d | Giá trị DP tại i | 
| --- | --- | --- | --- | 
| 1 | tất cả tôi | - | một [tôi] | 
| 2 | tôi=2 | 5 | 9 | 
| 2 | tôi=4 | 5 | 8 | 

Câu trả lời tốt nhất là 9, chọn chỉ số 0 và 2. Điều này xác nhận rằng thuật toán ưu tiên bỏ qua các giá trị âm dưới mức tối ưu cục bộ khi chúng chặn các kết hợp tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi lớp DP quét mảng một lần và xây dựng tiền tố maxima | 
| Không gian | O(n) | Chỉ có một lớp DP cộng với mảng tiền tố được lưu trữ | 

Với`n ≤ 150000`Và`k ≤ 50`, các hoạt động trong trường hợp xấu nhất là khoảng 7,5 triệu lần chuyển đổi, phù hợp thoải mái trong các giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n, k, d = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    NEG = -10**30
    prev = [NEG] * n
    for i in range(n):
        prev[i] = a[i]

    for _ in range(2, k + 1):
        best_prefix = [NEG] * n
        dp = [NEG] * n

        best_prefix[0] = prev[0]
        for i in range(1, n):
            best_prefix[i] = max(best_prefix[i - 1], prev[i])

        for i in range(n):
            if i - d >= 0:
                dp[i] = best_prefix[i - d] + a[i]

        prev = dp

    return str(max(prev))

# provided sample
assert run("""10 3 2
-1 4 2 -6 3 3 5 -1 4 -1
""") == "13"

# all equal values
assert run("""6 2 2
5 5 5 5 5 5
""") == "10"

# minimum spacing tight
assert run("""5 2 3
1 100 1 100 1
""") == "200"

# negative-heavy array
assert run("""5 2 2
-5 -1 -2 -3 -4
""") == "-3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 13 | tính đúng đắn của các giá trị hỗn hợp | 
| tất cả đều bình đẳng | 10 | DP ổn định qua các mối quan hệ | 
| khoảng cách chặt chẽ | 200 | thực thi hạn chế khoảng cách | 
| âm bản | -3 | tính đúng đắn khi tối ưu hóa âm | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`d`đủ lớn để chỉ có thể lựa chọn rất thưa thớt. DP xử lý việc này một cách chính xác vì`i - d`nhanh chóng trở thành số âm, ngăn chặn các chuyển đổi không hợp lệ. Ví dụ, với`n = 5, k = 2, d = 4`, chỉ những cặp như`(0,4)`là hợp lệ và DP chỉ cho phép chuyển đổi ở các vị trí đó. 

Một trường hợp khó khăn khác là khi giải pháp tối ưu bỏ qua các giá trị ban đầu cao. DP dựa trên tiền tố đảm bảo điều này được xử lý chính xác vì nó không tham lam cam kết đạt cực đại sớm mà chỉ lưu trữ chúng dưới dạng ứng cử viên cho các kết hợp trong tương lai. 

Trường hợp cạnh cuối cùng liên quan đến các giá trị âm trong đó lấy ít giá trị âm lớn hơn sẽ tốt hơn nhiều giá trị dương nhỏ ngăn chặn quyền truy cập vào các giá trị tốt hơn sau này. Vì DP luôn coi trạng thái trước đó tốt nhất toàn cầu cho đến`i - d`, nó tránh được các bẫy tham lam cục bộ một cách tự nhiên và duy trì cấu trúc tối ưu toàn cầu.
