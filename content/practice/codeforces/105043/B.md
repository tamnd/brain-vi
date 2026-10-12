---
title: "CF 105043B - \u0414\u0432\u043e\u0438\u0447\u043d\u044b\u0439 \u043f\u0430\u0443\u043a \u043f\u043b\u0435\u0442\u0451\u0442 \u043f\u0430\u0443\u0442\u0438\u043d\u0443"
description: "Chúng ta được cho một dãy chiều cao của các cột được sắp xếp theo một đường thẳng. Mỗi vị trí có một chiều cao và chúng ta cũng có một giá trị ngưỡng x. Nhiệm vụ là tìm một đoạn liền kề của các cây cột này sao cho mỗi cây cột bên trong đoạn đó có chiều cao tối đa là x."
date: "2026-06-28T01:31:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105043
codeforces_index: "B"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u041d\u0422\u041e: \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0446\u0438\u043e\u043d\u043d\u0430\u044f \u0431\u0435\u0437\u043e\u043f\u0430\u0441\u043d\u043e\u0441\u0442\u044c. \u0421\u0435\u043a\u0446\u0438\u044f - \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0430"
rating: 0
weight: 105043
solve_time_s: 77
verified: false
draft: false
---

[CF 105043B - \u0414\u0432\u043e\u0438\u0447\u043d\u044b\u0439 \u043f\u0430\u0443\u043a \u043f\u043b\u0435\u0442\u0451\u0442 \u043f\u0430\u0443\u0442\u0438\u043d\u0443](https://codeforces.com/problemset/problem/105043/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy chiều cao của các cột được sắp xếp theo một đường thẳng. Mỗi vị trí đều có một độ cao và chúng ta cũng có một giá trị ngưỡng`x`. Nhiệm vụ là tìm một đoạn liền kề của các cột này sao cho mỗi cột bên trong đoạn đó có chiều cao tối đa.`x`. Trong số tất cả các phân đoạn hợp lệ như vậy, chúng tôi muốn có độ dài tối đa có thể. 

Nói một cách đơn giản hơn, hãy tưởng tượng bạn đang đi dọc theo mảng và chỉ được phép ở những vị trí có chiều cao không vượt quá`x`. Bất cứ khi nào bạn va vào một cây cột quá cao, bạn phải dừng đoạn đó và bắt đầu một đoạn mới sau đó. Câu trả lời là khoảng thời gian dài nhất không bị gián đoạn của các vị trí “được phép”. 

Kích thước đầu vào có thể rất lớn, lên tới 700.000 phần tử. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra tất cả các phân đoạn một cách rõ ràng. Cách tiếp cận bậc hai sẽ liên quan đến việc xem xét tất cả các vị trí bắt đầu và mở rộng sang phải, trong trường hợp xấu nhất thực hiện theo thứ tự$n^2$kiểm tra, quá chậm đối với hạn chế này. Do đó, giải pháp phải xử lý mảng theo thời gian tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử đều lớn hơn`x`. Trong trường hợp đó, không có phân đoạn hợp lệ nào tồn tại và câu trả lời là 0. Một trường hợp góc khác là khi tất cả các phần tử đều hợp lệ, trong trường hợp đó câu trả lời đơn giản là`n`. 

Một sai lầm ngây thơ thường xuất phát từ việc coi vấn đề là “đếm các phần tử ≤ x” trên toàn cầu, thay vì tôn trọng tính liền kề. Ví dụ, nếu mảng là`[1, 100, 2, 3]`với`x = 50`, các phần tử hợp lệ là 1, 2, 3 nhưng chúng không tạo thành một phân đoạn. Câu trả lời đúng là 2 chứ không phải 3. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi chỉ mục bắt đầu, hãy mở rộng một cửa sổ sang bên phải trong khi tất cả các phần tử vẫn giữ nguyên ≤ x và tính độ dài của nó. Điều này sẽ tìm thấy chính xác tất cả các phân đoạn hợp lệ, nhưng trong trường hợp xấu nhất khi tất cả các phần tử đều ≤ x, mỗi chỉ mục bắt đầu sẽ quét đến cuối, tạo ra khoảng$n + (n-1) + \dots + 1$các phép toán là bậc hai. 

Quan sát quan trọng là tính hợp lệ chỉ phụ thuộc vào việc mỗi phần tử riêng lẻ có ≤ x hay không và điều kiện cho một phân đoạn hoàn toàn là cục bộ. Chúng tôi không cần duy trì cấu trúc phức tạp hoặc tính toán lại bất cứ điều gì trên các phân đoạn. Thay vào đó, mảng phân chia một cách tự nhiên thành các khối liền kề tối đa có giá trị “tốt”. Mỗi lần chúng ta gặp một phần tử lớn hơn`x`, nó phá vỡ hoàn toàn cấu trúc, đặt lại độ dài đoạn hiện tại về 0. 

Điều này giúp giảm vấn đề xuống còn một lần quét tuyến tính trong đó chúng tôi theo dõi độ dài của lần chạy hợp lệ hiện tại và duy trì mức tối đa trong tất cả các lần chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Quét đơn | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì hai giá trị: độ dài của phân đoạn hợp lệ hiện tại và câu trả lời tốt nhất được thấy cho đến nay. 

1. Khởi tạo`cur = 0`Và`best = 0`. Biến`cur`theo dõi chuỗi các phần tử hợp lệ hiện tại, trong khi`best`lưu trữ chuỗi tối đa được tìm thấy cho đến nay. 
2. Lặp lại từng chiều cao trong mảng. Đối với mỗi phần tử, hãy kiểm tra xem nó có phải là ≤ x hay không. Điều này xác định liệu nó có thể mở rộng phân đoạn hợp lệ hiện tại hay không. 
3. Nếu phần tử là ≤ x, tăng dần`cur`bằng 1 vì phân đoạn hợp lệ tiếp tục mà không bị gián đoạn. Điều này phản ánh rằng vị trí hiện tại kéo dài hoạt động hiện tại. 
4. Nếu phần tử lớn hơn x, hãy đặt lại`cur`đến 0. Điều này là cần thiết vì điều kiện hợp lệ yêu cầu tất cả các phần tử trong phân đoạn phải thỏa mãn ràng buộc và phần tử này phá vỡ hoàn toàn tính liên tục. 
5. Sau khi cập nhật`cur`, cập nhật`best = max(best, cur)`để ghi lại phân đoạn hợp lệ dài nhất được thấy cho đến thời điểm này. 
6. Sau khi xử lý toàn bộ mảng, xuất ra`best`. 

### Tại sao nó hoạt động 

Ở bất kỳ vị trí nào,`cur`biểu thị chính xác độ dài của hậu tố liền kề hiện tại kết thúc tại vị trí đó trong đó tất cả các giá trị đều là x. Mỗi lần chúng tôi đặt lại, chúng tôi sẽ cắt mảng chính xác ở các phần tử không hợp lệ, đây là những điểm dừng duy nhất có thể có cho các phân đoạn hợp lệ. Vì mọi phân đoạn hợp lệ phải nằm hoàn toàn giữa hai phần tử (hoặc ranh giới) không hợp lệ nên mọi phân đoạn ứng cử viên đều được nắm bắt đầy đủ dưới dạng một số giá trị của`cur`, đảm bảo không có phân đoạn hợp lệ nào bị bỏ sót và không có phân đoạn không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    cur = 0
    best = 0

    for v in a:
        if v <= x:
            cur += 1
            if cur > best:
                best = cur
        else:
            cur = 0

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai đọc đầu vào trong một lần và chỉ duy trì trạng thái bổ sung không đổi. Chi tiết quan trọng là việc thiết lập lại xảy ra ngay lập tức khi gặp giá trị lớn hơn`x`, đảm bảo rằng không có phần tử không hợp lệ nào làm ô nhiễm một phân đoạn. Cập nhật tối đa được thực hiện sau khi mở rộng lần chạy hiện tại, điều này đảm bảo giá trị cuối cùng của mỗi lần chạy được ghi lại chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 5 2 3 6
```Chúng tôi theo dõi quá trình quét từng bước. 

| tôi | giá trị | ≤ x? | cur | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | vâng | 1 | 1 | 
| 2 | 5 | không | 0 | 1 | 
| 3 | 2 | vâng | 1 | 1 | 
| 4 | 3 | vâng | 2 | 2 | 
| 5 | 6 | không | 0 | 2 | 

Đoạn liền kề hợp lệ dài nhất xuất hiện giữa chỉ số 3 và 4, cho độ dài 2. 

Điều này xác nhận rằng việc đặt lại sẽ chia mảng thành các phân đoạn độc lập một cách chính xác và chúng tôi không bao giờ hợp nhất qua các ranh giới không hợp lệ. 

### Ví dụ 2 

đầu vào:```
5 4
1 4 3 2 1
```| tôi | giá trị | ≤ x? | cur | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | vâng | 1 | 1 | 
| 2 | 4 | vâng | 2 | 2 | 
| 3 | 3 | vâng | 3 | 3 | 
| 4 | 2 | vâng | 4 | 4 | 
| 5 | 1 | vâng | 5 | 5 | 

Tất cả các phần tử đều thỏa mãn điều kiện, do đó toàn bộ mảng tạo thành một phân đoạn hợp lệ. 

Điều này cho thấy thuật toán xử lý trường hợp “không ngắt” một cách tự nhiên mà không cần bất kỳ logic đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có hai biến số nguyên được duy trì | 

Quét tuyến tính là cần thiết vì hạn chế lớn về`n`và nó dễ dàng phù hợp với giới hạn thời gian thông thường ngay cả đối với kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case: mixed breaks
assert run("5 4\n1 5 2 3 6\n") == "2"

# all valid
assert run("5 4\n1 4 3 2 1\n") == "5"

# all invalid
assert run("4 10\n20 30 40 50\n") == "0"

# single element valid
assert run("1 5\n3\n") == "1"

# single element invalid
assert run("1 5\n10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nghỉ hỗn hợp | 2 | thiết lập lại tính đúng đắn của logic | 
| tất cả đều hợp lệ | 5 | vệt toàn mảng | 
| tất cả đều không hợp lệ | 0 | đoạn hợp lệ trống | 
| hợp lệ duy nhất | 1 | trường hợp tích cực tối thiểu | 
| đơn không hợp lệ | 0 | trường hợp không tối thiểu | 

## Vỏ cạnh 

Một mảng hoàn toàn không hợp lệ như`5 3 / 10 11 12 13 14`thể hiện cơ chế thiết lập lại kích hoạt liên tục. Thuật toán không bao giờ tăng`cur`, vì vậy nó vẫn bằng 0 trong suốt, và`best`giữ nguyên bằng 0, nắm bắt chính xác rằng không có phân đoạn hợp lệ nào tồn tại. 

Một mảng hoàn toàn hợp lệ như`5 10 / 1 2 3 4 5`không bao giờ kích hoạt thiết lập lại. Biến`cur`tích lũy đến`n`, Và`best`được cập nhật ở mỗi bước, kết thúc tại`5`. Điều này cho thấy thuật toán không yêu cầu phát hiện rõ ràng ranh giới phân đoạn, vì tính liên tục được xử lý một cách tự nhiên bằng cách tích lũy. 

Một trường hợp hỗn hợp như`6 4 / 1 2 10 3 4 5`hiển thị một dấu ngắt đơn chia mảng thành hai phân đoạn ứng cử viên`[1,2]`Và`[3,4,5]`. Thuật toán đánh giá chính xác cả hai thông qua các lần chạy độc lập`cur`, Và`best`nắm bắt tối đa trong số đó, đó là`3`.
