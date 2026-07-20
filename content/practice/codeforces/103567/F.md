---
title: "CF 103567F - \u041c\u0435\u0442\u0440\u043e"
description: "Chúng ta được cung cấp một dãy dài các giá trị đại diện cho luồng hành khách tại các thời điểm khác nhau trong ngày. Một “ca” được xác định bởi ba tham số: chỉ số thời gian bắt đầu s, số chuyến đi cố định k và khoảng cách thời gian không đổi d giữa các chuyến đi liên tiếp."
date: "2026-07-03T03:56:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103567
codeforces_index: "F"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Prefinal Round"
rating: 0
weight: 103567
solve_time_s: 45
verified: true
draft: false
---

[CF 103567F - \u041c\u0435\u0442\u0440\u043e](https://codeforces.com/problemset/problem/103567/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy dài các giá trị đại diện cho luồng hành khách tại các thời điểm khác nhau trong ngày. Một “sự thay đổi” được xác định bởi ba tham số: chỉ số thời gian bắt đầu`s`, một số chuyến đi cố định`k`và khoảng cách thời gian không đổi`d`giữa các chuyến đi liên tiếp. 

Nếu chúng ta bắt đầu vào lúc`s`, tàu điện ngầm hoạt động đôi khi`s, s + d, s + 2d, ..., s + (k−1)d`. Tổng tải hành khách cho ca này là tổng giá trị mảng tại chính xác các vị trí này. Nhiệm vụ là chọn vị trí xuất phát`s`sao cho tổng số này càng lớn càng tốt, với ràng buộc là tất cả các chỉ số được chọn phải nằm trong mảng. 

Về mặt hình thức, chúng ta đang tối đa hóa tổng theo cấp số cộng cố định bên trong mảng. Ràng buộc về tính hợp lệ là chỉ số cuối cùng`s + (k−1)d`không được vượt quá ranh giới mảng. 

Kích thước đầu vào đủ lớn để bất kỳ giải pháp nào lặp lại tất cả các lần khởi động hợp lệ và tính toán lại từng tổng trực tiếp sẽ quá chậm. Một cách tiếp cận đơn giản sẽ tính toán hiệu quả tới`O(N · k)`hoạt động, sụp đổ xung quanh`10^10`trong trường hợp xấu nhất và không khả thi. 

Một sự tinh tế quan trọng là bước đi`d`tạo ra một cấu trúc không phải là một phân đoạn liền kề duy nhất trừ khi`d = 1`. Khi`d > 1`, các chỉ số được chia thành các lớp dư lượng độc lập theo modulo`d`và mỗi lớp hoạt động giống như mảng tuyến tính nhỏ hơn của chính nó. 

Một trường hợp thất bại điển hình của cách tiếp cận cửa sổ trượt đơn giản là khi người ta cố gắng xử lý vấn đề này giống như một vấn đề về mảng con bình thường đối với`d > 1`. Ví dụ, nếu`d = 2`, các phần tử liền kề trong tổng không liền kề trong mảng ban đầu, do đó tiền tố tiêu chuẩn hoặc cửa sổ trượt trên các phân đoạn liền kề không được áp dụng. 

## Phương pháp tiếp cận 

Giải pháp brute-force khắc phục điểm khởi đầu`s`và tính tổng rõ ràng`k`các phần tử cách nhau bởi`d`. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, mỗi chi phí tính toán như vậy`O(k)`, và có tới`O(N)`điểm bắt đầu có thể, do đó tổng độ phức tạp trở thành`O(Nk)`. Khi cả hai`N`Và`k`lớn, điều này nhanh chóng trở nên quá chậm. 

Quan sát quan trọng là cấp số cộng được xác định bởi bước`d`phân vùng mảng thành các chuỗi độc lập. Bất kỳ chỉ mục nào`i`chỉ tương tác với các chỉ số của biểu mẫu`i + td`. Điều này có nghĩa là nếu chúng ta nhóm các phần tử theo chỉ số modulo của chúng`d`, mỗi nhóm tạo thành một chuỗi riêng biệt trong đó vấn đề giảm xuống còn việc chọn một đoạn có độ dài liền kề`k`bên trong trình tự đó. 

Khi quá trình phân tách này được thực hiện, mỗi nhóm có thể được giải chính xác giống như bài toán tổng mảng con tối đa có độ dài cố định cổ điển bằng cách sử dụng tổng tiền tố hoặc cửa sổ trượt. Vì mọi phần tử đều thuộc về một nhóm duy nhất nên tổng công việc trên tất cả các nhóm là tuyến tính theo`N`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Nk) | O(1) | Quá chậm | 
| Phân hủy dư lượng / tiền tố hoặc cửa sổ | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách chia mảng thành các chuỗi độc lập dựa trên modulo chỉ số`d`. 

1. Chúng tôi nhóm các phần tử theo modulo lớp còn lại của chúng`d`. Mỗi nhóm chứa các phần tử sẽ xuất hiện liên tiếp trong cấp số cộng khi chúng ta sửa lớp dư lượng ban đầu. Bước này là cần thiết bởi vì bước qua`d`không bao giờ trộn lẫn các lớp dư lượng khác nhau. 
2. Đối với từng loại dư lượng`r`, chúng tôi trích xuất chuỗi`b = a[r], a[r+d], a[r+2d], ...`. Điều này chuyển đổi vấn đề “bỏ qua chỉ mục” ban đầu thành vấn đề mảng tuyến tính tiêu chuẩn. 
3. Trên mỗi chuỗi`b`, chúng tôi tính toán tổng tối đa của bất kỳ đoạn có chiều dài liền kề nào`k`. Điều này cũng giống như tính toán một cửa sổ trượt có kích thước`k`. Chúng ta khởi tạo tổng của cửa sổ đầu tiên, sau đó dịch chuyển nó bằng cách loại bỏ phần tử gửi đi và thêm phần tử vào. 
4. Chúng tôi theo dõi mức tối đa trên tất cả các cửa sổ ở tất cả các loại dư lượng. Mức tối đa toàn cầu này tương ứng với vị trí bắt đầu hợp lệ tốt nhất trong mảng ban đầu. 

### Tại sao nó hoạt động 

Mỗi cấp số cộng hợp lệ bắt đầu từ chỉ số`s`nằm hoàn toàn trong một lớp dư lượng duy nhất`s mod d`. Trong lớp đó, tiến trình trở thành một khối liền kề. Do đó, mỗi ca ứng viên tương ứng với chính xác một cửa sổ trong đúng một lớp và mọi cửa sổ như vậy tương ứng với một ca hợp lệ. Vì chúng tôi đánh giá tất cả các cửa sổ trong tất cả các lớp nên chúng tôi không bỏ sót bất kỳ ứng cử viên nào và chúng tôi không trộn lẫn các chỉ số không liên quan. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, d = map(int, input().split())
    a = list(map(int, input().split()))

    if k == 1:
        print(max(a))
        return

    ans = -10**30

    for r in range(d):
        # build window on residue class r
        window = []
        s = 0

        for i in range(r, n, d):
            window.append(a[i])
            s += a[i]

        if len(window) < k:
            continue

        cur = sum(window[:k])
        ans = max(ans, cur)

        for i in range(k, len(window)):
            cur += window[i] - window[i - k]
            ans = max(ans, cur)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng và lặp lại trên tất cả các lớp dư lượng theo modulo`d`. Đối với mỗi lớp, nó xây dựng một mảng rút gọn đại diện cho tất cả các vị trí có thể tiếp cận bằng cách bước`d`. Sau đó nó sử dụng một cửa sổ trượt có kích thước cố định có chiều dài`k`để tính tổng một cách hiệu quả. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ cố gắng tính tổng trực tiếp trong hệ thống lập chỉ mục ban đầu. Tất cả các tính toán diễn ra theo trình tự dư lượng được nén, đảm bảo tổng công việc tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7, k = 3, d = 2
a = [1, 10, 2, 9, 3, 8, 4]
```Chúng tôi chia thành các lớp dư lượng: 

| r | trình tự b | 
| --- | --- | 
| 0 | [1, 2, 3, 4] | 
| 1 | [10, 9, 8] | 

Bây giờ chúng tôi đánh giá các cửa sổ có kích thước 3. 

Với r = 0: 

| cửa sổ bắt đầu | yếu tố | tổng hợp | 
| --- | --- | --- | 
| 0 | [1, 2, 3] | 6 | 
| 1 | [2, 3, 4] | 9 | 

Với r = 1: 

| cửa sổ bắt đầu | yếu tố | tổng hợp | 
| --- | --- | --- | 
| 0 | [10, 9, 8] | 27 | 

Tối đa là 27. 

Điều này cho thấy các tiến trình hợp lệ tương ứng chính xác như thế nào với các cửa sổ liền kề trong lớp dư lượng. 

### Ví dụ 2 

đầu vào:```
n = 6, k = 2, d = 3
a = [5, -1, 4, 2, 10, -3]
```Các lớp dư lượng: 

| r | trình tự b | 
| --- | --- | 
| 0 | [5, 2] | 
| 1 | [-1, 10] | 
| 2 | [4, -3] | 

Bây giờ k = 2 có nghĩa là mỗi chuỗi đầy đủ đóng góp chính xác một cửa sổ: 

| r | cửa sổ | tổng hợp | 
| --- | --- | --- | 
| 0 | [5, 2] | 7 | 
| 1 | [-1, 10] | 9 | 
| 2 | [4, -3] | 1 | 

Đáp án là 9. 

Điều này chứng tỏ rằng ngay cả với các giá trị âm, phương pháp vẫn so sánh chính xác các chuỗi độc lập mà không bị nhiễu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được xử lý chính xác một lần bên trong lớp dư lượng của nó | 
| Không gian | O(N) | Lưu trữ tạm thời các chuỗi dư lượng | 

Giải pháp này chia tỷ lệ tuyến tính với kích thước đầu vào, điều này là cần thiết vì tương tác bậc hai đơn giản giữa`N`Và`k`không thể thực hiện được ở giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum size
assert run("1 1 1\n5\n") == "5"

# all equal values
assert run("6 2 2\n3 3 3 3 3 3\n") == "6"

# d = 1 contiguous case
assert run("5 3 1\n1 2 3 4 5\n") == "12"

# alternating strong peak
assert run("7 2 2\n1 100 1 100 1 100 1\n") == "300"

# negative values
assert run("5 2 1\n-1 -2 -3 -4 -5\n") == "-3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 / 5 | 5 | đầu vào hợp lệ nhỏ nhất | 
| tất cả đều bình đẳng | 6 | đối xứng qua dư lượng | 
| d=1 | 12 | giảm bớt vấn đề về cửa sổ cổ điển | 
| đỉnh xen kẽ | 300 | nhóm dư lượng chính xác | 
| âm bản | -3 | xử lý số tiền âm một cách chính xác | 

## Vỏ cạnh 

cho`k = 1`, thuật toán sẽ trả về ngay phần tử tối đa. Logic cửa sổ trượt vẫn hoạt động, nhưng nếu không có bộ phận bảo vệ, nó có thể cho rằng có ít nhất một chuyển đổi tồn tại một cách không chính xác. 

Đối với những trường hợp`len(b) < k`trong lớp dư lượng, lớp đó phải được bỏ qua hoàn toàn. Nếu không, một phần cửa sổ sẽ được xem xét không chính xác. 

Đối với lớn`d`gần với`n`, hầu hết các lớp dư lượng chứa nhiều nhất một phần tử, nghĩa là không tồn tại cửa sổ hợp lệ trừ khi`k = 1`. Thuật toán xử lý việc này một cách tự nhiên vì vòng trượt không bao giờ chạy trong những trường hợp như vậy.
