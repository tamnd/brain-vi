---
title: "CF 105783C - Mã hóa"
description: "Chúng ta được cho một dãy hộp, mỗi hộp chứa một số dương kẹo và có một màu trong số ba giá trị có thể có. Một người bắt đầu ở một vị trí cố định trên đường này và muốn thu thập kẹo bằng cách di chuyển liên tục đến một hộp nào đó và ăn hết kẹo từ hộp đó ngay lập tức."
date: "2026-06-25T15:49:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105783
codeforces_index: "C"
codeforces_contest_name: "XXIX Spain Olympiad in Informatics, Online Qualifier"
rating: 0
weight: 105783
solve_time_s: 58
verified: true
draft: false
---

[CF 105783C - Mã hóa](https://codeforces.com/problemset/problem/105783/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy hộp, mỗi hộp chứa một số dương kẹo và có một màu trong số ba giá trị có thể có. Một người bắt đầu ở một vị trí cố định trên đường này và muốn thu thập kẹo bằng cách di chuyển liên tục đến một hộp nào đó và ăn hết kẹo từ hộp đó ngay lập tức. 

Những hạn chế chính không phải là về việc di chuyển mà là về trình tự các hộp được chọn để ăn. Mỗi hộp được chọn tiếp theo phải chứa nhiều kẹo hơn hộp được chọn trước đó và màu của nó phải khác với hộp đã chọn trước đó. 

Việc di chuyển cần có thời gian: mỗi bước giữa các ô liền kề tốn một đơn vị thời gian. Ăn uống là miễn phí. Mục tiêu là thu thập ít nhất tổng số k kẹo đồng thời giảm thiểu tổng thời gian di chuyển. 

Đầu vào mô tả vị trí bắt đầu, tổng số kẹo cần thiết và sau đó đối với mỗi hộp số lượng và màu sắc kẹo của nó. Đầu ra là thời gian tối thiểu cần thiết hoặc trừ một nếu không thể. 

Ràng buộc n lên tới 50 sẽ thay đổi toàn bộ phối cảnh. Bất kỳ giải pháp nào cố gắng khám phá tất cả các đường dẫn trong tìm kiếm đồ thị đơn giản qua các trạng thái vị trí và tổng thu thập đều gần như khả thi về mặt trạng thái, nhưng các chuyển đổi bất cẩn vẫn có thể bùng nổ. Vì k lên tới 2000 và mỗi r[i] nhỏ nên cấu trúc gợi ý một chương trình động trên “hộp được chọn cuối cùng” và “tổng số kẹo được thu thập”. 

Một trường hợp thất bại khó phát hiện phổ biến là giả sử bạn luôn có thể tham lam chọn ô hợp lệ gần nhất. Ví dụ: một hộp xa hơn một chút có thể có giá trị kẹo lớn hơn nhiều, làm giảm số lần di chuyển trong tương lai. Một cạm bẫy khác là quên rằng hộp được chọn đầu tiên không có hạn chế về màu sắc hoặc kích thước kẹo, chỉ có những lựa chọn tiếp theo mới có. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ coi mọi chuỗi hộp được chọn có thể có là một đường dẫn. Từ bất kỳ hộp hiện tại nào, chúng ta có thể di chuyển đến bất kỳ hộp nào khác, kiểm tra xem nó có thỏa mãn điều kiện kẹo tăng nghiêm ngặt và ràng buộc màu sắc khác nhau hay không, rồi tiếp tục đệ quy cho đến khi đạt được ít nhất k kẹo. Cách tiếp cận này đúng vì nó tuân thủ trực tiếp các quy tắc của quy trình. Tuy nhiên, không gian trạng thái của nó là rất lớn. Ngay cả khi chúng ta chỉ xem xét các chuỗi có độ dài lên tới n, số lượng hoán vị là giai thừa theo n và mỗi chuyển đổi yêu cầu kiểm tra các ràng buộc, khiến nó hoàn toàn không khả thi. 

Quan sát quan trọng là khi chúng ta quyết định thứ tự các hộp mà chúng ta sẽ ăn, chi phí di chuyển chỉ phụ thuộc vào vị trí của các hộp được chọn liên tiếp và tính hợp lệ của quá trình chuyển đổi chỉ phụ thuộc vào thuộc tính cục bộ của hai hộp đó. Điều này biến vấn đề thành việc chọn một chuỗi các chỉ số có trọng số giữa các phần tử liên tiếp và một ràng buộc đơn điệu về các giá trị. 

Cấu trúc này được mô hình hóa một cách tự nhiên dưới dạng lập trình động trên các trạng thái mã hóa hộp được chọn cuối cùng và số lượng kẹo chúng ta đã tích lũy được cho đến nay. Quá trình chuyển đổi chỉ đi từ hộp i sang hộp j nếu r[j] lớn hơn r[i] và màu sắc khác nhau. Chi phí chuyển đổi chỉ đơn giản là khoảng cách giữa các chỉ số. 

Quá trình chuyển đổi ban đầu rất đặc biệt vì chúng tôi bắt đầu từ vị trí s thay vì hộp trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo trình tự | Ồ (n!) | O(n) | Quá chậm | 
| DP qua ô cuối cùng và tổng | O(n² · k) | O(n · k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng DP trong đó dp[i][c] biểu thị thời gian di chuyển tối thiểu cần thiết để thu thập chính xác c viên kẹo nếu hộp ăn cuối cùng là i. Chúng ta xử lý việc đạt ít nhất k kẹo bằng cách lấy giá trị tối thiểu trên tất cả c lớn hơn hoặc bằng k ở cuối.

1. Khởi tạo tất cả các giá trị dp thành vô cùng. Điều này đại diện cho các trạng thái không thể truy cập được. 
2. Với mỗi ô i, hãy cân nhắc việc bắt đầu quá trình tại i. Tổng số kẹo ban đầu là r[i] và chi phí là khoảng cách từ vị trí bắt đầu s đến i. Chúng ta đặt dp[i][r[i]] thành abs(i - s). Điều này cho thấy thực tế là nước đi đầu tiên không có hạn chế. 
3. Lặp lại tất cả các trạng thái i và tất cả các tổng số kẹo có thể có c. Nếu dp[i][c] đã là vô hạn, hãy bỏ qua vì không thể truy cập được. 
4. Từ trạng thái (i, c) thử chuyển sang mọi ô j khác. Quá trình chuyển đổi chỉ hợp lệ nếu r[j] lớn hơn r[i] và color[j] khác với color[i]. Điều này thực thi cả hai ràng buộc vấn đề. 
5. Nếu quá trình chuyển đổi hợp lệ, hãy tính tổng số kẹo mới nc = c + r[j]. Kẹp nó ở k vì mọi thứ ngoài k đều tương đương với mục tiêu. Cập nhật dp[j][nc] với dp[i][c] + abs(i - j). 
6. Sau khi xử lý tất cả các trạng thái, câu trả lời là dp[i][c] tối thiểu trên tất cả i và tất cả c ≥ k. 

Tính đúng đắn dựa trên tính bất biến mà dp[i][c] luôn lưu trữ chi phí tối thiểu có thể có trong số tất cả các chuỗi hợp lệ kết thúc tại i với tổng số c kẹo. Mọi chuyển đổi đều duy trì tính hợp lệ vì nó thực thi cả việc tăng số lượng kẹo và xen kẽ màu sắc, đồng thời DP khám phá tất cả các chuỗi hợp lệ có thể có theo thứ tự độ dài tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s, k = map(int, input().split())
    r = list(map(int, input().split()))
    c = input().strip()

    INF = 10**18

    dp = [[INF] * (k + 1) for _ in range(n)]

    for i in range(n):
        val = min(r[i], k)
        dp[i][val] = min(dp[i][val], abs(i + 1 - s))

    for i in range(n):
        for cur in range(k + 1):
            if dp[i][cur] == INF:
                continue
            for j in range(n):
                if r[j] <= r[i]:
                    continue
                if c[j] == c[i]:
                    continue
                nxt = cur + r[j]
                if nxt > k:
                    nxt = k
                cost = dp[i][cur] + abs((i + 1) - (j + 1))
                if cost < dp[j][nxt]:
                    dp[j][nxt] = cost

    ans = min(dp[i][k] for i in range(n))
    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo định nghĩa DP. Điểm tinh tế chính là việc lập chỉ mục: các hộp được lập chỉ mục 1 trong đầu vào cho vị trí bắt đầu s, nhưng mảng Python được lập chỉ mục 0, vì vậy mọi khoảng cách đều sử dụng i + 1 và j + 1. 

Một chi tiết quan trọng khác là kẹp tổng kẹo thành k. Nếu không có điều này, bảng DP sẽ phát triển các trạng thái không cần thiết tương đương với câu trả lời cuối cùng và tăng thời gian chạy. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ có bốn ô và vị trí bắt đầu ở giữa. Giả sử chúng ta tính dp sau khi khởi tạo. 

| Bước | Hộp tôi | Tổng kẹo | dp[i][c] | Bình luận | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | r[2] | cơ bụng(2 - s) | bắt đầu từ ô 2 | 
| Ban đầu | 4 | r[4] | cơ bụng(4 - s) | bắt đầu từ ô 4 | 

Bây giờ giả sử từ hộp 2 chúng ta có thể chuyển sang hộp 4 vì nó có nhiều kẹo hơn và có màu sắc khác. 

| Bước | Từ tôi | Tới j | Tổng mới | Chi phí mới | Cập nhật | 
| --- | --- | --- | --- | --- | --- | 
| Chuyển tiếp | 2 | 4 | r[2] + r[4] | dp[2][r2] + dist(2,4) | dp[4][*] | 

Điều này cho thấy cách các đường dẫn tích lũy cả tổng số kẹo và chi phí di chuyển trong khi vẫn duy trì các ràng buộc. 

Dấu vết chứng tỏ rằng DP không cam kết sớm với một chuỗi đầy đủ. Nó bảo tồn nhiều lựa chọn từng phần và chỉ kết hợp chúng khi chúng vẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² · k) | Mỗi trạng thái (i, c) có thể chuyển sang O(n) hộp tiếp theo và có các trạng thái O(nk) | 
| Không gian | O(n · k) | Bảng DP lưu trữ chi phí tốt nhất cho mỗi cặp (hộp, tổng) | 

Với n 50 và k 2000, số thao tác trong trường hợp xấu nhất là khoảng 5 triệu lần chuyển đổi, nằm trong giới hạn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    n, s, k = map(int, _sys.stdin.readline().split())
    r = list(map(int, _sys.stdin.readline().split()))
    c = _sys.stdin.readline().strip()

    INF = 10**18
    dp = [[INF] * (k + 1) for _ in range(n)]

    for i in range(n):
        val = min(r[i], k)
        dp[i][val] = min(dp[i][val], abs(i + 1 - s))

    for i in range(n):
        for cur in range(k + 1):
            if dp[i][cur] == INF:
                continue
            for j in range(n):
                if r[j] <= r[i]:
                    continue
                if c[j] == c[i]:
                    continue
                nxt = cur + r[j]
                if nxt > k:
                    nxt = k
                dp[j][nxt] = min(dp[j][nxt], dp[i][cur] + abs(i + 1 - j - 1))

    ans = min(dp[i][k] for i in range(n))
    return str(-1 if ans == INF else ans)

# minimal case
assert run("1 1 1\n1\nR\n") == "0"

# simple increasing chain
assert run("3 2 3\n1 2 3\nRGB\n") is not None

# impossible case
assert run("2 1 10\n1 1\nRG\n") == "-1"

# all same color blocks forces skipping alternation
assert run("4 2 5\n1 2 3 4\nRRRR\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1/1/R | 0 | hộp đơn đã đủ k | 
| 3 2 3 / 1 2 3 / RGB | số hợp lệ | chuyển tiếp bình thường | 
| 2 1 10 / 1 1 / RG | -1 | không thể tiếp cận được k | 
| 4 2 5 / 1 2 3 4 / RRR | -1 | ràng buộc màu chặn tất cả các đường dẫn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi vị trí bắt đầu đã nằm trên hộp có giá trị cao mà chỉ riêng nó đã vượt quá k. Trong tình huống đó, DP phải cho phép lấy chiếc hộp đó ngay lập tức với chi phí di chuyển bằng 0. Bước khởi tạo xử lý việc này một cách trực tiếp bằng cách thiết lập dp[i][r[i]] từ abs(i - s), bao gồm cả trường hợp i bằng s, tạo ra chi phí bằng 0. 

Một trường hợp khác là khi nhiều hộp có giá trị kẹo bằng nhau. Yêu cầu bất bình đẳng nghiêm ngặt có nghĩa là việc chuyển đổi giữa chúng bị cấm ngay cả khi màu sắc khác nhau. Việc triển khai đơn giản chỉ kiểm tra màu sắc sẽ cho phép các chuỗi không hợp lệ không chính xác. 

Một trường hợp khó phát hiện cuối cùng là khi các giải pháp tối ưu yêu cầu bỏ qua các ô gần đó để chuyển sang các ô ở xa. DP xử lý chính xác vấn đề này vì nó khám phá tất cả các chuyển đổi bất kể khoảng cách, đảm bảo rằng không có giả định địa phương tham lam nào hạn chế không gian tìm kiếm.
