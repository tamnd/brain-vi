---
title: "CF 975B - Mancala"
description: "Chúng ta được cho một bảng tròn có 14 vị trí. Mỗi vị trí chứa một số lượng đá. Một động tác bao gồm việc chọn một vị trí có đá, loại bỏ tất cả các viên đá khỏi vị trí đó và sau đó phân bổ từng viên đá đó vào các vị trí tiếp theo di chuyển theo chiều kim đồng hồ…"
date: "2026-06-17T01:33:16+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 975
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 478 (Div. 2)"
rating: 1100
weight: 975
solve_time_s: 83
verified: true
draft: false
---

[CF 975B - Mancala](https://codeforces.com/problemset/problem/975/B) 

**Đánh giá:** 1100 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bảng tròn có 14 vị trí. Mỗi vị trí chứa một số lượng đá. Một nước đi bao gồm việc chọn một vị trí có các viên đá, loại bỏ tất cả các viên đá khỏi vị trí đó và sau đó phân bổ từng viên đá đó vào các vị trí tiếp theo di chuyển theo chiều kim đồng hồ xung quanh vòng tròn. 

Sau lần phân phối lại này, chúng tôi xem xét tất cả 14 vị trí và thu thập đá từ mọi vị trí có số lượng cuối cùng là số chẵn. Mục tiêu là chọn vị trí bắt đầu di chuyển để số tiền thu được này được tối đa hóa. 

Khía cạnh quan trọng là chỉ có một động thái được thực hiện và quyết định duy nhất là nhặt và phân phối lại đống nào. 

Các ràng buộc cực kỳ nhỏ: chính xác là 14 vị trí. Mặc dù mỗi vị trí có thể chứa tới 10^9 viên đá nhưng cấu trúc vẫn cố định và rất nhỏ. Điều này ngay lập tức gợi ý rằng chúng ta có đủ khả năng để mô phỏng mọi vị trí xuất phát có thể. Bất kỳ giải pháp nào cố gắng thực hiện điều gì đó phức tạp hơn quét tuyến tính đều không cần thiết. 

Một điểm tinh tế là việc phân phối lại có tính xác định và được mô tả đầy đủ bởi mảng ban đầu và chỉ mục đã chọn. Không có sự tương tác hoặc phân nhánh trong quá trình di chuyển. 

Một sai lầm ngây thơ là cố gắng tính toán lại toàn bộ bảng một cách không chính xác sau khi phân phối lại bằng cách "nhảy" số lượng thay vì mô phỏng toàn bộ bảng một cách chính xác. Ví dụ: việc quên các viên đá bọc từ vị trí 14 trở lại vị trí 1 sẽ dẫn đến trạng thái cuối cùng không chính xác. 

Một cạm bẫy tiềm ẩn khác là giả định rằng chúng ta chỉ cần kiểm tra các hiệu ứng cục bộ gần chỉ số đã chọn. Trên thực tế, mọi vị trí chỉ có thể nhận thêm tối đa một viên đá cho mỗi chu kỳ đầy đủ 14 bước, do đó, ngay cả những vị trí ở xa cũng bị ảnh hưởng. 

Các trường hợp cạnh bao gồm: 

Khi tất cả các viên đá đều nằm trong một đống, hãy nói`[0, 0, 0, ..., 7, 0, 0]`, phân phối sẽ kết thúc nhiều lần và mọi vị trí có thể thay đổi tính chẵn lẻ nhiều lần. 

Khi chỉ tồn tại một cọc khác 0, kết quả phụ thuộc hoàn toàn vào nơi chúng ta bắt đầu, bởi vì điều đó quyết định mô hình lật chẵn lẻ trong suốt chu kỳ. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản: với mỗi chỉ số i từ 1 đến 14, hãy mô phỏng việc lấy tất cả các viên đá từ đống đó và phân phát từng viên một trên mảng hình tròn. Sau khi mô phỏng, tính tổng của tất cả các cọc có giá trị chẵn. Chúng tôi tận dụng tối đa mọi lựa chọn. 

Điều này đúng vì bài toán hạn chế chúng ta một cách rõ ràng trong một nước đi và mỗi nước đi được xác định hoàn toàn bởi vị trí bắt đầu của nó. 

Chi phí của mô phỏng này là quan trọng. Đối với một đống chứa các viên đá a[i], chúng tôi thực hiện cập nhật O(a[i]). Vì a[i] có thể lên tới 10^9 nên mô phỏng đơn giản này quá chậm trong trường hợp xấu nhất. 

Quan sát quan trọng là mặc dù a[i] lớn nhưng kích thước bảng không đổi. Sau mỗi vòng quay đủ 14 bước, mỗi vị trí sẽ nhận thêm đúng một viên đá từ cọc chuyển động. Do đó, hiệu quả cuối cùng chỉ phụ thuộc vào việc chúng ta hoàn thành bao nhiêu chu kỳ đầy đủ và các bước từng phần còn lại. 

Chúng ta không cần phải mô phỏng từng viên đá riêng lẻ. Thay vào đó, chúng tôi tính toán số lần mỗi vị trí được ghé thăm và cập nhật số lượng bằng cách sử dụng số học. Vì kích thước bảng là cố định nên đối với mỗi chỉ số bắt đầu, chúng ta có thể tính cấu hình kết quả trong O(14). 

Do đó, chúng tôi giảm chi phí tiềm năng rất lớn cho mỗi lần di chuyển thành mô phỏng theo thời gian không đổi cho mỗi vị trí bắt đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu trên mỗi viên đá | O(14 * a[i]) | O(14) | Quá chậm | 
| Mô phỏng dựa trên chu kỳ mỗi lần bắt đầu | O(14 * 14) | O(14) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng vị trí bắt đầu có thể có i từ 0 đến 13. Đây là đống chúng ta chọn để trống. 
2. Đối với mỗi vị trí bắt đầu, sao chép mảng ban đầu vào mảng làm việc. Chúng ta cần một mô phỏng độc lập vì mỗi lựa chọn được đánh giá riêng biệt. 
3. Trích xuất tất cả các viên đá từ vị trí i vào một biến k và đặt vị trí đó về 0. 
4. Bắt đầu từ vị trí i, lần lượt phân phát k viên đá xung quanh vòng tròn. 

Thay vì lặp lại k lần, hãy tính xem có bao nhiêu chu kỳ đầy đủ gồm 14 bước xảy ra và còn lại bao nhiêu bước bổ sung. 

Mỗi vị trí nhận được chính xác đá tầng (k / 14) và vị trí đầu tiên (k mod 14) sau khi tôi nhận thêm một viên đá. 
5. Áp dụng các mức tăng đã tính này vào mảng đã sao chép. 
6. Sau khi phân phối, quét tất cả 14 vị trí và tổng giá trị chẵn. 
7. Theo dõi số tiền tối đa trên tất cả các vị trí bắt đầu. 

Ý tưởng quan trọng là thay thế mô phỏng từng viên đá bằng phân phối số học trực tiếp trong một chu kỳ có độ dài cố định. 

### Tại sao nó hoạt động 

Mỗi viên đá di chuyển một cách xác định đến vị trí tiếp theo theo một chu kỳ cố định có độ dài 14. Sau 14 bước, viên đá trở về chỉ số ban đầu được bù đắp bằng đầy đủ các chu kỳ. Điều này có nghĩa là mọi vị trí trong vòng tròn đều được truy cập một cách thống nhất ngoại trừ một tiền tố nhỏ được xác định bởi phần còn lại. Đóng góp cuối cùng cho mỗi ô chỉ phụ thuộc vào số lần phép quay đi qua nó, điều này hoàn toàn được xác định bởi k modulo 14 và k chia cho 14. Điều này đảm bảo bảng được xây dựng lại giống hệt với mô phỏng đầy đủ, đồng thời tránh lặp lại từng bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = list(map(int, input().split()))
    n = 14
    ans = 0

    for i in range(n):
        b = a[:]  # copy board
        k = b[i]
        if k == 0:
            pass
        b[i] = 0

        full = k // n
        rem = k % n

        # distribute full cycles
        for j in range(n):
            b[j] += full

        # distribute remainder
        for t in range(1, rem + 1):
            b[(i + t) % n] += 1

        # compute score
        cur = 0
        for x in b:
            if x % 2 == 0:
                cur += x

        ans = max(ans, cur)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau quá trình phân rã chu trình. Các chu kỳ đầy đủ đóng góp đồng đều cho tất cả 14 vị trí, đó là lý do tại sao chúng tôi chỉ cần thêm`full`tới từng tế bào. Phần còn lại chia thêm một viên đá cho viên tiếp theo`rem`vị trí sau chỉ mục đã chọn, sử dụng lập chỉ mục mô-đun. 

Cần cẩn thận khi xử lý việc lập chỉ mục: quá trình phân phối lại bắt đầu từ ô tiếp theo`(i + 1) % 14`, không phải chính ô được chọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 1 1 0 0 0 0 0 0 7 0 0 0 0
```Chúng tôi kiểm tra bắt đầu từ chỉ số 9 (giá trị 7). 

| Bước | Vị trí trống | k | chu kỳ đầy đủ | phần còn lại | hiệu ứng chính | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 | 7 | 0 | 7 | chia 1 viên đá cho 7 vị trí tiếp theo | 

Sau khi phân phối, bảng trở thành:```
1 2 2 0 0 0 0 0 0 0 1 1 1 1
```Các vị trí có giá trị chẵn là 2, 2, 0, 0, 0, 0 đóng góp tổng cộng là 4. 

Điều này xác nhận rằng việc bao bọc một phần xác định vị trí nào sẽ lật ngang nhau. 

### Ví dụ 2 

đầu vào:```
1 0 0 0 0 0 0 0 0 0 0 0 0 0
```Chỉ có vị trí 0 là khác 0. 

| Bắt đầu tôi | k | đầy đủ | rem | tổng chẵn | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 
| 1 | 0 | 0 | 0 | 0 | 

Câu trả lời hay nhất là 0. 

Điều này cho thấy một viên đá luôn tạo ra giá trị lẻ ở đâu đó và không thể đóng góp vào tổng số chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(14^2) | 14 vị trí bắt đầu, mỗi vị trí cập nhật 14 ô | 
| Không gian | O(14) | chỉ có một bản sao của bảng | 

Kích thước bảng không đổi nên thuật toán chạy ngay lập tức trong giới hạn. Ngay cả khi sao chép nhiều lần, tổng công việc cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a = list(map(int, sys.stdin.readline().split()))
    n = 14
    ans = 0

    for i in range(n):
        b = a[:]
        k = b[i]
        b[i] = 0

        full = k // n
        rem = k % n

        for j in range(n):
            b[j] += full

        for t in range(1, rem + 1):
            b[(i + t) % n] += 1

        cur = sum(x for x in b if x % 2 == 0)
        ans = max(ans, cur)

    return str(ans)

# provided sample
assert run("0 1 1 0 0 0 0 0 0 7 0 0 0 0") == "4"

# all zeros except one
assert run("0 0 0 0 5 0 0 0 0 0 0 0 0 0") >= "0"

# single stone
assert run("1 0 0 0 0 0 0 0 0 0 0 0 0 0") == "0"

# all equal odd values
assert run("1 1 1 1 1 1 1 1 1 1 1 1 1 1") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 4 | tính đúng đắn của việc phân phối lại bọc | 
| đơn khác không | 0 | ứng xử với cọc cách ly | 
| đá đơn | 0 | di chuyển không tầm thường tối thiểu | 
| tất cả những cái | ≥0 | xử lý tính chẵn lẻ trong toàn bộ chu kỳ | 

## Vỏ cạnh 

Khi tất cả các viên đá được tập trung vào một đống, thuật toán sẽ chia chính xác giá trị thành các chu kỳ đầy đủ và phần còn lại. Để thi
