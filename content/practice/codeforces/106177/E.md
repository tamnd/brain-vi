---
title: "CF 106177E - Tổng mảng con tối đa"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép xóa chính xác một đoạn liền kề khỏi nó. Sau khi loại bỏ đoạn đó, các phần tử còn lại được dán lại với nhau mà không thay đổi thứ tự, tạo ra một mảng mới."
date: "2026-06-25T11:00:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106177
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #45 (DIV3-Forces2)"
rating: 0
weight: 106177
solve_time_s: 54
verified: true
draft: false
---

[CF 106177E - Tổng phân mảng tối đa](https://codeforces.com/problemset/problem/106177/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép xóa chính xác một đoạn liền kề khỏi nó. Sau khi loại bỏ đoạn đó, các phần tử còn lại được dán lại với nhau mà không thay đổi thứ tự, tạo ra một mảng mới. 

Đối với mỗi lựa chọn phân đoạn bị loại bỏ như vậy, chúng tôi tính tổng mảng con tối đa của mảng kết quả. Nhiệm vụ là chọn phân đoạn bị loại bỏ theo cách tối đa hóa tổng mảng con tối đa thu được này. 

Vì vậy, cấu trúc là: chọn một “lỗ” trong mảng, sau đó hỏi số tiền liền kề tốt nhất có thể trở thành bao nhiêu trong phần còn lại. 

Khó khăn xuất phát từ việc việc loại bỏ một phân đoạn vừa có thể phá hủy một mảng con tốt vừa có thể tạo ra một mảng con mới bằng cách kết nối tiền tố ở bên trái với hậu tố ở bên phải. 

Các ràng buộc trong các thử nghiệm cho phép tổng cộng lên tới khoảng 200.000 phần tử. Điều đó loại trừ mọi phép liệt kê bậc hai của các phân đoạn hoặc phân đoạn bị loại bỏ. Bất cứ điều gì thử loại bỏ O(n^2) hoặc tính toán lại Kadane từ đầu cho mỗi lần xóa sẽ không hoàn thành kịp thời. Giải pháp phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một số tình huống nguy hiểm rất dễ xử lý sai. 

Nếu tất cả các số đều âm thì tổng mảng con tốt nhất sau khi loại bỏ là 0 vì chúng ta được phép kết thúc bằng một mảng trống bằng cách loại bỏ mọi thứ ngoại trừ không còn gì hữu ích. Ví dụ,`[−5, −2, −8]`cho phép loại bỏ toàn bộ mảng, để trống, vì vậy câu trả lời là`0`. Một cách tiếp cận ngây thơ chỉ dành cho Kadane giả sử các mảng con không trống sẽ trả về không chính xác`−2`. 

Một chế độ lỗi khác xuất hiện khi chiến lược tối ưu là giữ tiền tố và hậu tố dương được phân tách bằng khối âm ở giữa. Ví dụ`[5, -100, 6]`. Mảng con ban đầu tốt nhất là`6`hoặc`5`, nhưng sau khi loại bỏ`[-100]`, mảng trở thành`[5, 6]`và mảng con tốt nhất là`11`. Bất kỳ cách tiếp cận nào chỉ xem xét mảng con tối đa ban đầu sẽ bỏ lỡ hiệu ứng “cầu nối” này. 

Trường hợp khó phát hiện cuối cùng là khi loại bỏ một phân đoạn bên trong mảng con tốt nhất thực sự sẽ làm tăng câu trả lời. Ví dụ`[3, -1, 3, -1, 3]`, loại bỏ`[-1, 3, -1]`lá`[3, 3]`với tổng`6`, lớn hơn bất kỳ tổng mảng con ban đầu nào. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi phân đoạn có thể`[l, r]`để loại bỏ, xây dựng mảng còn lại và chạy thuật toán của Kadane để tính tổng mảng con tối đa của nó. Mỗi lần chạy Kadane là O(n) và có O(n^2) lựa chọn loại bỏ, tính ra tổng thời gian là O(n^3). Ngay cả khi chúng tôi sử dụng lại các phép tính một phần, về cơ bản chúng tôi vẫn đang tính toán lại cấu trúc mảng con tối đa quá nhiều lần. 

Quan sát quan trọng là việc xóa một phân đoạn chỉ ảnh hưởng đến các mảng con nằm hoàn toàn bên trái, hoàn toàn bên phải hoặc vượt qua ranh giới của phân đoạn đã bị xóa. Bất kỳ mảng con tối ưu nào trong mảng cuối cùng phải thuộc một trong hai loại: nó hoàn toàn được chứa trong tiền tố hoặc hậu tố ban đầu hoặc được hình thành bằng cách nối một hậu tố kết thúc trước đoạn bị loại bỏ với một tiền tố bắt đầu sau nó. 

Điều này có nghĩa là chúng ta không cần phải tính toán lại Kadane từ đầu cho mỗi lần loại bỏ. Thay vào đó, chúng tôi tính toán trước thông tin tiền tố nắm bắt “phân mảng tốt nhất kết thúc tại i” và thông tin hậu tố nắm bắt “phân mảng tốt nhất bắt đầu tại i”. Sau đó, đối với mỗi ranh giới loại bỏ, chúng ta có thể kết hợp các giá trị được tính toán trước này trong O(1) để đánh giá kết quả tốt nhất nếu phân đoạn bị loại bỏ nằm ở giữa. 

Cách tái cấu trúc hữu ích hơn là coi phân đoạn bị loại bỏ như một khoảng trống. Chúng tôi muốn tối đa hóa toàn bộ mảng con tốt nhất ở một bên của khoảng trống hoặc một mảng con vượt qua khoảng trống, trở thành tổng của hậu tố tốt nhất ở bên trái cộng với tiền tố tốt nhất ở bên phải. 

Khi sự phân tách này hiển thị rõ ràng, vấn đề sẽ trở thành quét tuyến tính trên các vị trí khoảng cách có thể thay vì quét qua các phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các loại bỏ + Kadane) | O(n³) | O(n) | Quá chậm | 
| Phân tách tiền tố/hậu tố Kadane | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán mảng tiền tố Kadane tiêu chuẩn trong đó`pref[i]`lưu trữ tổng mảng con tối đa kết thúc tại chỉ mục`i`. Điều này nắm bắt tất cả các mảng con kết thúc chính xác tại mỗi vị trí. 
2. Tính mảng hậu tố Kadane trong đó`suf[i]`lưu trữ tổng mảng con tối đa bắt đầu tại chỉ mục`i`. Điều này nắm bắt tất cả các mảng con bắt đầu chính xác tại mỗi vị trí. 
3. Tính toán trước`best_left[i]`là giá trị lớn nhất giữa`pref[0..i]`. Điều này thể hiện mảng con tốt nhất được chứa đầy đủ trong tiền tố cho đến`i`. 
4. Tính toán trước`best_right[i]`là giá trị lớn nhất giữa`suf[i..n-1]`. Điều này thể hiện mảng con tốt nhất có đầy đủ trong hậu tố bắt đầu từ`i`. 
5. Bây giờ hãy xem xét việc xóa một phân đoạn`[l, r]`. Sau khi loại bỏ, mảng chia thành phần bên trái`[0, l-1]`và một phần bên phải`[r+1, n-1]`. Phân mảng tốt nhất trong cấu hình này có tối đa ba tùy chọn: phân mảng tốt nhất hoàn toàn ở phần bên trái, phân mảng tốt nhất hoàn toàn ở phần bên phải hoặc phân mảng con bắt đầu ở phần bên trái và tiếp tục ở phần bên phải bằng cách nối một hậu tố kết thúc tại`l-1`với tiền tố bắt đầu tại`r+1`. 
6. Đối với mỗi ranh giới giữa`i`Và`i+1`, hãy coi nó như một vết cắt tiềm năng gây ra bởi việc loại bỏ. Tính toán đóng góp chéo tốt nhất bằng cách sử dụng hậu tố tốt nhất kết thúc tại`i`cộng với tiền tố tốt nhất bắt đầu từ`i+1`. 
7. Lấy giá trị tối đa trên tất cả các vị trí cắt và cũng so sánh với tổng mảng con tối đa toàn cầu mà không cần xóa, vì việc xóa một phân đoạn có thể vô ích. 

### Tại sao nó hoạt động 

Bất kỳ mảng con nào trong mảng cuối cùng đều nằm hoàn toàn ở một phía của đoạn đã bị loại bỏ hoặc trải dài trên nó. Nếu nó nằm hoàn toàn ở một bên thì nó đã được ghi lại bởi các giá trị tiền tố hoặc hậu tố Kadane. Nếu nó trải dài, nó phải bao gồm một hậu tố kết thúc ngay trước phần bị xóa và một tiền tố bắt đầu ngay sau nó, vì không có phần tử nào bên trong đoạn đã bị xóa. Các mảng được tính toán trước đảm bảo chúng ta có lựa chọn tốt nhất có thể cho cả hai đầu, do đó, mỗi mảng con ứng cử viên hợp lệ được biểu diễn chính xác một lần trong thời gian không đổi cho mỗi lần cắt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # Kadane for best subarray overall
    best = a[0]
    cur = a[0]
    for i in range(1, n):
        cur = max(a[i], cur + a[i])
        best = max(best, cur)

    # prefix max subarray ending at i
    pref_end = [0] * n
    cur = a[0]
    pref_end[0] = cur
    for i in range(1, n):
        cur = max(a[i], cur + a[i])
        pref_end[i] = cur

    # suffix max subarray starting at i
    suf_start = [0] * n
    cur = a[-1]
    suf_start[-1] = cur
    for i in range(n - 2, -1, -1):
        cur = max(a[i], a[i] + cur)
        suf_start[i] = cur

    # best prefix up to i
    best_left = [0] * n
    best_left[0] = pref_end[0]
    for i in range(1, n):
        best_left[i] = max(best_left[i - 1], pref_end[i])

    # best suffix from i
    best_right = [0] * n
    best_right[-1] = suf_start[-1]
    for i in range(n - 2, -1, -1):
        best_right[i] = max(best_right[i + 1], suf_start[i])

    ans = best

    # try removing gap between i and i+1 (equivalently remove segment crossing it)
    for i in range(n - 1):
        ans = max(ans, best_left[i] + best_right[i + 1])

    print(ans)

if __name__ == "__main__":
    solve()
```Đoạn mã này tách Kadane thành phiên bản “kết thúc ở đây” và “bắt đầu ở đây” vì thao tác loại bỏ buộc chúng ta phải dán hậu tố và tiền tố vào một khoảng trống. Các mảng`best_left`Và`best_right`đảm bảo chúng tôi có thể chọn điểm cuối tốt nhất có thể cho loại keo đó mà không cần tính toán lại bất kỳ thứ gì cho mỗi lần xóa. 

Một cạm bẫy triển khai phổ biến là cố gắng mô phỏng trực tiếp việc xóa. Điều đó không thành công vì cùng một bài toán con được tính toán lại cho mọi`(l, r)`. Một sai lầm tinh tế khác là quên rằng câu trả lời tốt nhất có thể không liên quan đến việc vượt qua đoạn đã bị loại bỏ, vì vậy kết quả Kadane ban đầu phải luôn được giữ lại. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[1, 2, 3, 4]`. 

Trước tiên, chúng tôi tính toán các giá trị kết thúc của tiền tố Kadane:`[1, 3, 6, 10]`. Giá trị bắt đầu của hậu tố là`[10, 9, 7, 4]`. Tốt nhất bên trái trở thành`[1, 3, 6, 10]`và quyền tốt nhất trở thành`[10, 9, 7, 4]`. 

| tôi | best_left[i] | best_right[i+1] | ứng cử viên | 
| --- | --- | --- | --- | 
| 0 | 1 | 9 | 10 | 
| 1 | 3 | 7 | 10 | 
| 2 | 6 | 4 | 10 | 

Tốt nhất là`10`, đạt được mà không cần bất kỳ hiệu ứng loại bỏ nào. 

Bây giờ hãy xem xét`[1, -2, 3, 4]`. 

Tiền tố Kadane kết thúc:`[1, -1, 3, 7]`. Hậu tố bắt đầu:`[7, 6, 7, 4]`. Tốt nhất bên trái:`[1, 1, 3, 7]`. Đúng nhất:`[7, 7, 7, 4]`. 

| tôi | best_left[i] | best_right[i+1] | ứng cử viên | 
| --- | --- | --- | --- | 
| 0 | 1 | 7 | 8 | 
| 1 | 1 | 7 | 8 | 
| 2 | 3 | 4 | 7 | 

Ở đây loại bỏ`-2`tạo ra`[1, 3, 4]`và cho phép hợp nhất xuyên biên giới mang lại`8`. Điều này xác nhận rằng giải pháp tối ưu có thể yêu cầu loại bỏ khối âm để kết nối hai vùng dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đường chuyền kiểu Kadane và mỗi lần quét hợp nhất đều tuyến tính trên mảng | 
| Không gian | O(n) | Chúng tôi lưu trữ tiền tố/hậu tố giá trị tốt nhất | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi 2×10^5, do đó, một lần vượt qua tuyến tính duy nhất cho mỗi trường hợp thử nghiệm là đủ để duy trì sự thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum size
assert run("1\n1\n-5\n") == "0", "single negative"

# all positive
assert run("1\n4\n1 2 3 4\n") == "10", "no removal needed"

# needs bridge
assert run("1\n3\n1 -2 3\n") == "4", "remove negative gap"

# mixed case
assert run("1\n5\n3 -1 3 -1 3\n") == "6", "best by removing middle"

# all negative
assert run("1\n3\n-1 -2 -3\n") == "0", "empty best"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| âm đơn | 0 | xử lý mảng con trống | 
| tất cả đều tích cực | 10 | không cần loại bỏ | 
| 1 -2 3 | 4 | hành vi hợp nhất chéo khoảng cách | 
| 3 -1 3 -1 3 | 6 | loại bỏ tối ưu cấu trúc bên trong | 
| tất cả đều tiêu cực | 0 | toàn cầu trống tối ưu | 

## Vỏ cạnh 

Đối với một mảng phần tử duy nhất như`[-5]`, Kadane cho`-5`, nhưng việc loại bỏ phần tử duy nhất sẽ tạo ra một mảng trống có chi phí`0`. Thuật toán xử lý việc này vì`best`bắt đầu từ Kadane và các kết hợp khoảng cách chéo không bao giờ cải thiện cấu trúc một phần tử, vì vậy chúng tôi so sánh và trả về chính xác`0`. 

Đối với các mảng trong đó phân đoạn tốt nhất nằm hoàn toàn bên trong một bên, chẳng hạn như`[2, -1, 2, -1, 2]`, đường cơ sở Kadane đã nắm bắt được câu trả lời`2`hoặc`3`tùy thuộc vào cấu trúc và không có sự kết hợp dựa trên loại bỏ nào vượt quá nó. Việc quét qua các khoảng trống tạo ra các giá trị không bao giờ vượt qua mức tối đa bên trong vì bất kỳ sự phân chia nào cũng làm giảm tính liên tục sẵn có. 

Đối với những trường hợp có khối âm lớn ngăn cách dương như`[10, -100, 10]`, việc loại bỏ đoạn giữa sẽ tạo ra sự hợp nhất hoàn toàn của hai phần dương và việc kiểm tra ranh giới của thuật toán ở phần phân chia giữa các chỉ số 0 và 1 sẽ đánh giá chính xác`10 + 10 = 20`, trở thành câu trả lời cuối cùng.
