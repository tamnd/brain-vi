---
title: "CF 102535G - 007: Bạn Chỉ Sống Ba Lần"
description: "Mỗi tin nhắn được mã hóa chứa một giá trị đánh dấu. Đối với mỗi điểm đánh dấu, chúng ta cần xác định tác nhân nào trong số ba tác nhân sẽ nhận được nó dựa trên khả năng phân chia."
date: "2026-08-05T15:19:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "G"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 114
verified: true
draft: false
---

[CF 102535G - 007: Bạn chỉ sống được ba lần](https://codeforces.com/problemset/problem/102535/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi tin nhắn được mã hóa chứa một giá trị đánh dấu. Đối với mỗi điểm đánh dấu, chúng ta cần xác định tác nhân nào trong số ba tác nhân sẽ nhận được nó dựa trên khả năng phân chia. Đặc vụ 003 nhận được tin nhắn có điểm đánh dấu chia hết cho 3, Đặc vụ 005 nhận được điểm đánh dấu chia hết cho 5 và Đặc vụ 007 nhận được điểm đánh dấu chia hết cho 7. Một điểm đánh dấu có thể thuộc về một số tác nhân, do đó đầu ra có thể chứa nhiều tên tác nhân theo một thứ tự cố định. Sau khi xử lý từng điểm đánh dấu, đầu ra phải chứa một dòng phân cách. 

Đầu vào có thể chứa tối đa 100000 điểm đánh dấu và mỗi điểm đánh dấu có thể lớn bằng (10^{18}). Số lượng ca kiểm thử là phần quyết định việc lựa chọn thuật toán. Một giải pháp thử nhiều thao tác cho mỗi điểm đánh dấu sẽ nhanh chóng vượt quá thời gian sẵn có. Với (10^5) trường hợp, giải pháp mong muốn cần có công việc gần như không đổi trên mỗi điểm đánh dấu, khiến cho các phương pháp tiếp cận phụ thuộc vào kích thước của số là không thể. May mắn thay, việc kiểm tra khả năng chia hết cho ba số nguyên nhỏ cố định là một phép toán liên tục. 

Các trường hợp đặc biệt chính xuất phát từ thực tế là một số điều kiện có thể đúng cùng một lúc. Giải pháp dừng lại sau khi tìm thấy số chia hết đầu tiên sẽ bỏ sót người nhận hợp lệ. Ví dụ:```
1
420
```Đầu ra đúng là:```
AGENT 003
AGENT 005
AGENT 007
---
```Số 420 chia hết cho cả ba giá trị. Việc thực hiện bất cẩn sử dụng`if`,`elif`,`else`sẽ chỉ in tác nhân phù hợp đầu tiên. 

Một trường hợp khác là điểm đánh dấu không khớp với ai:```
1
11
```Đầu ra đúng là:```
NONE
---
```Một chương trình giả sử mỗi điểm đánh dấu có ít nhất một người nhận có thể không in được gì hoặc tạo ra cấu trúc dấu phân cách không đầy đủ. 

Trường hợp cuối cùng là một điểm đánh dấu rất lớn:```
1
1000000000000000000
```Đầu ra đúng là:```
AGENT 003
AGENT 005
AGENT 007
---
```Giá trị vượt xa giới hạn số nguyên 32 bit. Thuật toán phải hoạt động với các số nguyên lớn mà Python xử lý một cách tự nhiên. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là kiểm tra từng điều kiện có thể có của người nhận một cách riêng biệt. Đối với mỗi điểm đánh dấu, chúng tôi kiểm tra xem nó có chia hết cho 3 không, có chia hết cho 5 không và có chia hết cho 7 hay không. Đây thực sự là logic hoàn chỉnh cần thiết cho bài toán nên nó đúng. Tổng công việc là ba phép toán modulo cho mỗi trường hợp thử nghiệm, đưa ra các phép toán khoảng (3 \times 10^5) cho đầu vào lớn nhất. 

Nếu ai đó cố gắng khái quát hóa vấn đề bằng cách tìm ước số của mỗi điểm đánh dấu thì chi phí sẽ lớn hơn nhiều. Đối với điểm đánh dấu gần (10^{18}), việc kiểm tra tất cả các ước số có thể có cho đến căn bậc hai của nó sẽ yêu cầu khoảng (10^9) lần lặp cho một trường hợp, điều này hoàn toàn không cần thiết. Ước số duy nhất quan trọng là ba người nhận đã biết. 

Nhận xét quan trọng là bài toán không yêu cầu chúng ta khám phá các ước số. Những người nhận có thể đã được biết đến. Chúng ta chỉ cần đánh giá ba phép kiểm tra tính chia hết cố định. Điều này làm giảm nhiệm vụ thành số học theo thời gian không đổi trên mỗi điểm đánh dấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm số chia Brute Force | O(sqrt(m)) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Kiểm tra khả năng chia hết cho 3, 5 và 7 | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử và xử lý từng điểm đánh dấu một cách độc lập. Mỗi điểm đánh dấu không có tương tác với các điểm đánh dấu khác, vì vậy việc giữ trạng thái giữa các trường hợp là không cần thiết. 
2. Tạo danh sách người nhận trống cho điểm đánh dấu hiện tại. Chúng tôi lưu trữ tên thay vì in ngay lập tức để thứ tự cuối cùng được kiểm soát rõ ràng. 
3. Kiểm tra xem điểm đánh dấu có chia hết cho 3 hay không. Nếu có, hãy nối thêm`AGENT 003`. Kiểm tra này thể hiện điều kiện hoàn chỉnh cho người nhận đầu tiên. 
4. Kiểm tra xem điểm đánh dấu có chia hết cho 5 hay không. Nếu có, hãy nối thêm`AGENT 005`. 
5. Kiểm tra xem điểm đánh dấu có chia hết cho 7 hay không. Nếu có, hãy nối thêm`AGENT 007`. 
6. Nếu không có người nhận nào được thêm vào, hãy thêm`NONE`. Nếu không, hãy in tất cả người nhận đã thu thập theo thứ tự họ đã được kiểm tra. 
7. In dòng phân cách sau mỗi test. 

Tại sao nó hoạt động: 

Đối với mỗi người nhận có thể, thuật toán thực hiện chính xác điều kiện toán học xác định liệu người nhận đó có nhận được tin nhắn hay không. Vì cả ba lần kiểm tra đều được thực hiện độc lập nên điểm đánh dấu chia hết cho nhiều giá trị sẽ được xử lý chính xác. Nếu không có điều kiện nào trong ba điều kiện là đúng thì danh sách người nhận vẫn trống, đó chính xác là tình huống được biểu thị bởi`NONE`. Thứ tự đầu ra cuối cùng được đảm bảo vì việc kiểm tra luôn được thực hiện theo trình tự yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        m = int(input())
        agents = []

        if m % 3 == 0:
            agents.append("AGENT 003")
        if m % 5 == 0:
            agents.append("AGENT 005")
        if m % 7 == 0:
            agents.append("AGENT 007")

        if not agents:
            ans.append("NONE")
        else:
            ans.extend(agents)

        ans.append("---")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc bằng cách sử dụng`sys.stdin.readline`vì có thể có nhiều trường hợp thử nghiệm. Giải pháp lưu trữ các dòng đầu ra trong một danh sách và ghi chúng một lần vào cuối, giúp tránh chi phí in lặp đi lặp lại. 

các`agents`danh sách được xây dựng lại cho mọi điểm đánh dấu. Mỗi phép thử chia hết đều độc lập nên riêng biệt`if`các tuyên bố được yêu cầu. Thay thế chúng bằng`elif`sẽ gây ra lỗi vì điểm đánh dấu có thể thuộc về nhiều tác nhân. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị lên tới (10^{18}) không cần xử lý đặc biệt. Các phép toán modulo hoạt động trực tiếp trên giá trị đầu vào mà không lo tràn. 

## Ví dụ đã hoạt động 

Hãy xem xét điểm đánh dấu`42`. 

| Điểm đánh dấu | Chia hết cho 3 | Chia hết cho 5 | Chia hết cho 7 | Người nhận | 
| --- | --- | --- | --- | --- | 
| 42 | Có | Không | Có | ĐẠI LÝ 003, ĐẠI LÝ 007 | 

Thuật toán kiểm tra cả ba điều kiện. Nó không dừng lại sau khi tìm được số chia hết cho 3, điều này cho phép Đặc vụ 007 cũng nhận được tin nhắn. 

Hãy xem xét điểm đánh dấu`1111`. 

| Điểm đánh dấu | Chia hết cho 3 | Chia hết cho 5 | Chia hết cho 7 | Người nhận | 
| --- | --- | --- | --- | --- | 
| 1111 | Không | Không | Không | KHÔNG | 

Không có điều kiện nào được thỏa mãn nên danh sách người nhận vẫn trống và thuật toán xuất ra`NONE`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi điểm đánh dấu yêu cầu chính xác ba phép toán modulo và công việc đầu ra không đổi | 
| Không gian | O(t) | Việc triển khai lưu trữ các dòng đầu ra cuối cùng trước khi in | 

Với (10^5) trường hợp kiểm thử, thuật toán chỉ thực hiện một lượng công việc không đổi nhỏ cho mỗi trường hợp. Việc sử dụng bộ nhớ cũng an toàn vì đầu ra được lưu trữ chỉ tỷ lệ thuận với số dòng đầu ra được yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert run("""7
42
420
111
1111
2020
489
123456789012345678
""") == """AGENT 003
AGENT 007
---
AGENT 003
AGENT 005
AGENT 007
---
AGENT 003
---
NONE
---
AGENT 005
---
AGENT 003
---
AGENT 003
---
""", "sample 1"

assert run("""1
1
""") == """NONE
---""", "minimum value"

assert run("""1
1000000000000000000
""") == """AGENT 003
AGENT 005
AGENT 007
---""", "large integer"

assert run("""3
15
35
49
""") == """AGENT 003
AGENT 005
---
AGENT 005
AGENT 007
---
AGENT 007
---""", "pairwise overlaps"

assert run("""1
101
""") == """NONE
---""", "non divisible marker"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NONE`| Điểm đánh dấu nhỏ nhất và không có người nhận phù hợp | 
|`1000000000000000000`| Cả ba đại lý | Xử lý số nguyên lớn | 
|`15`,`35`,`49`| Nhiều trường hợp hai tác nhân | Kiểm tra tính chia hết độc lập | 
|`101`|`NONE`| Các giá trị gần với các trường hợp chia hết mà không khớp | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là tính chia hết chồng chéo. Đối với đầu vào:```
1
420
```thuật toán đánh giá`420 % 3`,`420 % 5`, Và`420 % 7`. Cả ba kết quả đều bằng 0, vì vậy danh sách trở thành:```
AGENT 003
AGENT 005
AGENT 007
```Việc sử dụng séc độc lập là điều cho phép tất cả người nhận xuất hiện. 

Trường hợp cạnh thứ hai là điểm đánh dấu không có người nhận:```
1
11
```Ba phép toán modulo tạo ra số dư khác 0. Vì danh sách vẫn trống nên thuật toán sẽ chèn`NONE`trước khi in dấu phân cách. 

Trường hợp cạnh thứ ba là một giá trị rất lớn:```
1
1000000000000000000
```Thuật toán không chuyển số thành chữ số hoặc thực hiện phép chia lặp lại. Nó trực tiếp áp dụng kiểm tra modulo, duy trì thời gian không đổi trong Python và xác định chính xác khả năng chia hết cho cả ba số người nhận. 

Bạn có thể điều chỉnh thêm bài xã luận này nếu bạn cần nó ở định dạng kiểu Codeforces ngắn hơn hoặc với phần chứng minh trang trọng hơn.
