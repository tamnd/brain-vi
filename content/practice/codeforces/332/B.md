---
title: "CF 332B - Độ phi lý tối đa"
description: "Chúng ta được cung cấp một danh sách các số được đặt trên một dòng, trong đó mỗi vị trí đại diện cho một luật và giá trị của nó thể hiện mức độ “hữu ích” hoặc “có giá trị” của nó. Chúng ta phải chọn chính xác hai khối liền kề có độ dài k cố định."
date: "2026-06-06T10:03:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 332
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 193 (Div. 2)"
rating: 1500
weight: 332
solve_time_s: 76
verified: true
draft: false
---

[CF 332B - Mức vô lý tối đa](https://codeforces.com/problemset/problem/332/B) 

**Đánh giá:** 1500 
**Tags:** cấu trúc dữ liệu, dp, triển khai 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số được đặt trên một dòng, trong đó mỗi vị trí đại diện cho một luật và giá trị của nó thể hiện mức độ “hữu ích” hoặc “có giá trị” của nó. Chúng ta phải chọn chính xác hai khối liền kề nhau có chiều dài cố định`k`. Hai khối này không thể chồng lên nhau và chúng tôi muốn tổng của tất cả các giá trị bên trong cả hai khối càng lớn càng tốt. 

Một cách khác để thấy điều đó là chúng ta đang trượt một cửa sổ có kích thước`k`trên mảng và tính tổng của nó ở mọi vị trí bắt đầu hợp lệ. Từ các tổng cửa sổ này, chúng ta phải chọn hai cửa sổ sao cho chỉ số của chúng không trùng nhau và tổng của chúng là lớn nhất. Đầu ra không phải là giá trị mà là các chỉ số bắt đầu của hai cửa sổ này, với quy tắc ràng buộc ưu tiên các chỉ số bắt đầu nhỏ hơn trước. 

Các ràng buộc rất lớn, lên tới 200.000 phần tử. Bất kỳ giải pháp nào thử tất cả các cặp cửa sổ sẽ yêu cầu kiểm tra đại khái$O(n^2)$sự kết hợp, quá chậm. Thậm chí$O(nk)$các giải pháp tính toán lại tổng cửa sổ nhiều lần cũng sẽ thất bại trong trường hợp xấu nhất. Điều này buộc chúng ta hướng tới một giải pháp tuyến tính hoặc gần tuyến tính, điển hình là$O(n)$hoặc$O(n \log n)$. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu tất cả các giá trị đều giống nhau thì bất kỳ cặp phân đoạn hợp lệ nào cũng là tối ưu, nhưng quy tắc ràng buộc buộc chúng ta phải chọn phân đoạn đầu tiên sớm nhất có thể và sau đó là phân đoạn thứ hai tương thích sớm nhất. Một trường hợp tinh tế khác xảy ra khi hai phân đoạn tốt nhất nằm liền kề nhau thay vì cách nhau một chút; một lựa chọn tham lam ngây thơ có thể chọn phân khúc thứ hai tốt nhất tại địa phương để ngăn chặn sự ghép đôi tốt hơn trên toàn cầu. 

Ví dụ, hãy xem xét:```
n = 5, k = 2
values = [5, 1, 5, 1, 5]
```Lựa chọn tốt nhất là các phân đoạn bắt đầu từ 1 và 3 (hoặc 3 và 5), nhưng việc chọn phân đoạn đơn tốt nhất trước tiên một cách tham lam có thể chặn cặp tối ưu. 

Khó khăn chính là phân đoạn thứ hai tốt nhất phụ thuộc vào vị trí kết thúc của phân đoạn đầu tiên, vì vậy chúng tôi cần một cấu trúc cho phép chúng tôi kết hợp hiệu quả các lựa chọn tiền tố tốt nhất với việc quét hậu tố. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tính tổng của mọi mảng con có độ dài`k`, lưu trữ chúng và sau đó thử từng cặp mảng con không chồng chéo. Nếu có khoảng`n`mảng con có thể, sau đó kiểm tra tất cả các cặp là$O(n^2)$, và mỗi lần kiểm tra là$O(1)$, vậy độ phức tạp tổng cộng là$O(n^2)$, quá lớn đối với 200.000. 

Chúng ta có thể cải thiện điều này bằng cách nhận thấy rằng khi chúng ta biết tổng của mọi cửa sổ có kích thước`k`, vấn đề giảm xuống việc chọn hai chỉ số`i < j - k + 1`tối đa hóa`sum[i] + sum[j]`. Thay vì thử tất cả các cặp, chúng ta có thể tính toán trước phân đoạn đầu tiên tốt nhất có thể cho mọi vị trí`j`nơi đoạn thứ hai bắt đầu. 

Đối với mỗi vị trí`j`, chúng tôi muốn biết điều tốt nhất`i`ở bên trái của nó. Điều đó đương nhiên gợi ý việc duy trì tiền tố tối đa trên tổng cửa sổ. Sau đó với mỗi`j`, chúng tôi ghép nó với giá trị hợp lệ tốt nhất`i`trước nó và tính toán câu trả lời. 

Điều này làm giảm vấn đề từ ghép nối bậc hai thành một lượt duy nhất với tổng cửa sổ được tính toán trước và vị trí tốt nhất tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu hóa tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi mảng thành một mảng tổng cửa sổ, trong đó mỗi chỉ mục biểu thị tổng của một mảng con có độ dài`k`. 

1. Tính tổng tiền tố của mảng đầu vào sao cho có thể tính tổng cửa sổ bất kỳ trong thời gian không đổi. Điều này tránh việc tính toán lại các khoản tiền nhiều lần và đảm bảo chúng ta luôn tuyến tính về tổng thể. 
2. Xây dựng một mảng`w`, Ở đâu`w[i]`là tổng của đoạn bắt đầu từ`i`(1-lập chỉ mục). Mỗi giá trị đại diện cho “điểm” của việc chọn phân khúc tại vị trí đó. 
3. Tạo một mảng`best_left`, Ở đâu`best_left[i]`lưu trữ chỉ mục của cửa sổ tốt nhất trong số tất cả các cửa sổ bắt đầu từ`1`ĐẾN`i`. “Tốt nhất” có nghĩa là số tiền lớn nhất; nếu bằng nhau, chúng tôi giữ chỉ số nhỏ hơn. Cấu trúc tiền tố này cho phép chúng tôi biết ngay phân đoạn đầu tiên tối ưu cho bất kỳ vị trí phân khúc thứ hai nào. 
4. Lặp lại tất cả các vị trí bắt đầu hợp lệ của đoạn thứ hai. Đối với đoạn thứ hai bắt đầu lúc`i`, phân đoạn đầu tiên phải kết thúc trước khi nó bắt đầu, vì vậy điểm bắt đầu hợp lệ cuối cùng là`i - k`. 
5. Đối với mỗi hợp lệ`i`, tính tổng giá trị như`w[i] + w[best_left[i - k]]`và theo dõi tối đa. Áp dụng tie-break bằng cách ưu tiên chỉ số đầu tiên nhỏ hơn, sau đó chỉ số thứ hai nhỏ hơn. 
6. Xuất ra hai vị trí bắt đầu đã lưu. 

Ý tưởng chính là khi chúng tôi cố định phân đoạn thứ hai, phân đoạn đầu tiên tốt nhất đã được xác định theo thời gian không đổi thông qua tiền tố tối đa. 

Tại sao nó hoạt động: ở mọi vị trí`i`,`best_left[i]`lưu trữ phân đoạn đầu tiên tối ưu trong số tất cả các ứng cử viên hợp lệ. Bất kỳ cặp tối ưu nào cũng phải có phân đoạn đầu tiên trong phạm vi đó và việc ghép nó với phân đoạn thứ hai cố định không ảnh hưởng đến mức tối ưu bên trong của nó. Điều này làm giảm vấn đề tối ưu hóa toàn cục thành các quyết định cục bộ độc lập qua việc phân chia tiền tố và hậu tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

# prefix sums
pref = [0] * (n + 1)
for i in range(n):
    pref[i + 1] = pref[i] + a[i]

# window sums
w = [0] * (n - k + 1)
for i in range(n - k + 1):
    w[i] = pref[i + k] - pref[i]

# best position to the left
best_left = [0] * len(w)
best_left[0] = 0

for i in range(1, len(w)):
    if w[i] > w[best_left[i - 1]]:
        best_left[i] = i
    else:
        best_left[i] = best_left[i - 1]

best_i = 0
best_j = k
best_val = -1

for j in range(k, len(w)):
    i = best_left[j - k]
    val = w[i] + w[j]
    if val > best_val:
        best_val = val
        best_i = i
        best_j = j
    elif val == best_val and (i < best_i or (i == best_i and j < best_j)):
        best_i = i
        best_j = j

print(best_i + 1, best_j + 1)
```Mảng tiền tố là tiêu chuẩn và đảm bảo tổng mỗi cửa sổ được tính theo thời gian không đổi. Mảng`w`nén vấn đề vào việc chọn hai phần tử thay vì hai phân đoạn. 

các`best_left`mảng là cấu trúc quan trọng: nó đảm bảo rằng với mọi phân đoạn thứ hai có thể, chúng tôi có thể truy xuất ngay phân đoạn đầu tiên hợp lệ nhất mà không cần quét. 

Việc phá vỡ ràng buộc được xử lý rõ ràng ở vòng lặp cuối cùng bằng cách so sánh các chỉ số sau khi khớp tổng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
3 6 1 1 6
```Tổng cửa sổ: 

| tôi | cửa sổ | tổng hợp | 
| --- | --- | --- | 
| 0 | [3,6] | 9 | 
| 1 | [6,1] | 7 | 
| 2 | [1,1] | 2 | 
| 3 | [1,6] | 7 | 

Bây giờ xây dựng`best_left`: 

| tôi | w[i] | best_left[i] | 
| --- | --- | --- | 
| 0 | 9 | 0 | 
| 1 | 7 | 0 | 
| 2 | 2 | 0 | 
| 3 | 7 | 0 | 

Đối với mỗi phân đoạn thứ hai: 

- j = 2: i tốt nhất trong [0..0] → i=0, tổng = 9 + 2 = 11 
- j = 3: i tốt nhất trong [0..1] → i=0, tổng = 9 + 7 = 16 

Cặp tốt nhất là (0, 3) ở dạng 0 chỉ mục → (1, 4). 

Điều này cho thấy cách lựa chọn tiền tố đảm bảo chúng ta không bao giờ bỏ lỡ phân đoạn đầu tiên tối ưu toàn cầu khi đánh giá các vị trí sau. 

### Ví dụ 2 

đầu vào:```
6 2
1 2 3 1 2 3
```Tổng cửa sổ:```
[3, 5, 4, 3, 5]
```Chúng tôi so sánh: 

- Cặp tốt nhất xảy ra với cửa sổ ở mức 1 và 4 (1 và 4 được lập chỉ mục 0), cho 5 + 5 = 10. 

| j | tốt nhất tôi | tổng cặp | 
| --- | --- | --- | 
| 2 | 0 | 8 | 
| 3 | 1 | 8 | 
| 4 | 1 | 10 | 

Điều này chứng tỏ cách thuật toán chờ đợi phân khúc thứ hai tốt hơn một cách chính xác ngay cả khi những phân khúc trước đó có vẻ đầy hứa hẹn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | tổng tiền tố, tổng cửa sổ và hai lần quét tuyến tính | 
| Không gian | O(n) | mảng cho tổng tiền tố và tổng cửa sổ | 

Giải pháp chạy thoải mái trong giới hạn vì mọi phần tử đều được xử lý với số lần không đổi và không tồn tại phép lặp lồng nhau trên các phân đoạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    w = [0] * (n - k + 1)
    for i in range(n - k + 1):
        w[i] = pref[i + k] - pref[i]

    best_left = [0] * len(w)
    for i in range(1, len(w)):
        if w[i] > w[best_left[i - 1]]:
            best_left[i] = i
        else:
            best_left[i] = best_left[i - 1]

    best_i = best_j = 0
    best_val = -1

    for j in range(k, len(w)):
        i = best_left[j - k]
        val = w[i] + w[j]
        if val > best_val or (val == best_val and (i < best_i or (i == best_i and j < best_j))):
            best_val = val
            best_i, best_j = i, j

    return f"{best_i + 1} {best_j + 1}"

# provided sample
assert run("5 2\n3 6 1 1 6\n") == "1 4"

# minimum case
assert run("2 1\n5 6\n") == "1 2"

# all equal
assert run("6 2\n1 1 1 1 1 1\n") == "1 3"

# clear separated peaks
assert run("6 2\n1 100 1 1 100 1\n") == "1 4"

# adjacent best vs later best
assert run("7 2\n5 1 5 1 5 1 5\n") == "1 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 2 ...`|`1 4`| độ chính xác của mẫu | 
|`2 1 ...`|`1 2`| hành vi kích thước tối thiểu | 
| tất cả đều bình đẳng |`1 3`| sự ổn định ràng buộc | 
| hai đỉnh |`1 4`| ghép nối tối ưu không chồng chéo | 
| đỉnh xen kẽ |`1 3`| ghép nối toàn cầu chính xác vì tham lam | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, mọi cửa sổ đều có cùng số điểm. Mảng tiền tố tối đa luôn giữ chỉ mục sớm nhất, do đó phân đoạn đầu tiên được cố định càng sớm càng tốt và phân đoạn thứ hai trở thành vị trí hợp lệ sớm nhất sau nó. Điều này phù hợp chính xác với yêu cầu hòa giải. 

Khi hai cửa sổ tốt nhất ở gần nhau và cách xa nhau một chút, thuật toán vẫn hoạt động chính xác vì tính liền kề chỉ được thực thi thông qua phân tách chỉ mục chứ không phải thông qua ưu tiên. Ngay cả khi cửa sổ sau là tối ưu cục bộ, nó sẽ không thay thế tiền tố tối ưu toàn cục trước đó khi xây dựng`best_left`. 

Đối với đầu vào nhỏ như`n = 2k`, có chính xác một cặp phân đoạn hợp lệ và thuật toán giảm chính xác để chọn cặp đó mà không có bất kỳ sự mơ hồ nào.
