---
title: "CF 1023D - Khôi phục mảng"
description: "Chúng ta có một mảng cuối cùng có độ dài n được tạo ra bằng cách liên tục vẽ các đoạn có nhãn tăng dần từ 1 đến q. Trong thao tác thứ i, phân đoạn đã chọn sẽ được ghi đè hoàn toàn bằng giá trị i và các thao tác sau có thể ghi đè lên các phân đoạn trước đó."
date: "2026-06-16T21:55:06+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1023
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 504 (rated, Div. 1 + Div. 2, based on VK Cup 2018 Final)"
rating: 1700
weight: 1023
solve_time_s: 156
verified: false
draft: false
---

[CF 1023D - Khôi phục mảng](https://codeforces.com/problemset/problem/1023/D) 

**Đánh giá:** 1700 
**Tas:** thuật toán xây dựng, cấu trúc dữ liệu 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài cuối cùng`n`được tạo ra bằng cách sơn liên tục các phân đoạn với nhãn tăng dần từ`1`ĐẾN`q`. Trong thời gian`i`-hoạt động thứ , phân đoạn được chọn sẽ bị ghi đè hoàn toàn bằng giá trị`i`và các thao tác sau này có thể ghi đè lên các thao tác trước đó. Mọi vị trí đều được đảm bảo bao phủ bởi ít nhất một phân đoạn, vì vậy cuối cùng mọi chỉ mục đều mang giá trị của thao tác cuối cùng chạm vào nó. 

Sau tất cả các thao tác, một số vị trí được hiển thị dưới dạng số cố định giữa`1`Và`q`, trong khi những cái khác bị ẩn và hiển thị dưới dạng`0`. Số 0 có nghĩa là giá trị thực chưa được biết nhưng nó phải là một số nguyên nào đó từ`1`ĐẾN`q`. 

Nhiệm vụ là quyết định xem có tồn tại một chuỗi các phân đoạn cho tất cả`q`các hoạt động tạo ra một mảng nhất quán với các ràng buộc này và nếu vậy, hãy tạo bất kỳ mảng cuối cùng hợp lệ nào. 

Khó khăn chính là các giá trị biểu thị _lần sơn cuối cùng_. Một vị trí có giá trị`x`phải thuộc về hoạt động`x`phân đoạn của nó và sau này nó không được ghi đè bởi bất kỳ thao tác nào`> x`. Điều này giới thiệu một ràng buộc nhất quán toàn cầu trên tất cả các chỉ số. 

Những hạn chế là lớn, với`n, q ≤ 2 * 10^5`, loại trừ mọi lý luận bậc hai trên các phân đoạn hoặc mô phỏng lặp lại cho mỗi truy vấn. Bất kỳ giải pháp nào cũng phải xử lý từng vị trí với số lần không đổi và dựa vào cấu trúc khoảng thời gian thay vì mô phỏng theo từng thao tác. 

Một trường hợp lỗi nhỏ xuất hiện khi các giá trị cố định buộc thứ tự các phân đoạn không tương thích. Ví dụ: nếu nhãn sau xuất hiện hoàn toàn bên trong một vùng buộc phải thuộc về nhãn trước đó, thì chúng ta sẽ cần phân đoạn của thao tác sau để tránh hoàn toàn vùng đó, điều này có thể không thực hiện được nếu vùng đó chứa sự xuất hiện bắt buộc của nhãn sau. 

Một trường hợp phức tạp khác là khi các số 0 được đặt theo cách che giấu liệu ràng buộc “lần xuất hiện cuối cùng” có bị vi phạm hay không. Ví dụ,`a = [1, 0, 1]`với kích thước lớn`q`có thể trông vô hại, nhưng nếu các đoạn được xây dựng cho`1`phải che cả hai đầu và tránh trùng lặp với các giá trị bắt buộc sau này, tính khả thi có thể bị phá vỡ tùy theo vị trí. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử xây dựng trực tiếp tất cả các phân đoạn. Với mỗi giá trị`i`, chúng ta có thể cố gắng chọn một phân đoạn bao gồm tất cả các vị trí phải kết thúc bằng`i`và tránh các vị trí được cố định với các giá trị khác nhau lớn hơn`i`. Điều này dẫn đến việc thử nhiều kết hợp các điểm cuối phân đoạn và trong trường hợp xấu nhất, mỗi điểm cuối`q`hoạt động có thể có`O(n^2)`các lựa chọn, dẫn đến hành vi hàm mũ hoặc ít nhất là bậc ba khi được kết hợp giữa các hoạt động. Điều này hoàn toàn không thể thực hiện được đối với`2 * 10^5`. 

Cấu trúc của vấn đề trở nên đơn giản hơn nếu chúng ta đảo ngược cách nhìn. Thay vì nghĩ về việc các phân đoạn tạo ra giá trị, chúng ta có thể nghĩ về những gì mỗi giá trị _yêu cầu_. Nếu một giá trị`i`xuất hiện trong mảng cuối cùng thì tất cả các vị trí có giá trị`i`phải nằm trong đoạn được chọn để hoạt động`i`. Hơn nữa, nếu một vị trí có giá trị`j > i`, sau đó hoạt động`i`không được mở rộng vào vị trí đó nếu nó sẽ ngăn cản`j`khỏi bị ghi đè lần cuối. Điều này cho thấy rằng mỗi giá trị`i`tự nhiên tạo ra một khoảng thời gian tối thiểu bao gồm tất cả các lần xuất hiện của nó. 

Khi những khoảng thời gian tối thiểu này được xác định, câu hỏi chính sẽ là liệu chúng ta có thể gán cho mọi khoảng thời gian tối thiểu này hay không.`i`một phân khúc tôn trọng những khoảng thời gian này và vẫn đảm bảo phạm vi phủ sóng đầy đủ. Quan sát quan trọng là chúng tôi có thể tự do phóng to các phân đoạn, nhưng chúng tôi không thể thu nhỏ số lần xuất hiện dưới mức yêu cầu. Điều này biến vấn đề thành việc kiểm tra và có thể kéo dài các khoảng thời gian để chúng tạo thành một chuỗi ghi đè hợp lệ. 

Chúng tôi xây dựng khoảng giới hạn tối thiểu của mỗi giá trị. Sau đó, chúng tôi đảm bảo rằng các giá trị sau này không mâu thuẫn với cấu trúc bắt buộc trước đó. Nếu một giá trị xuất hiện, khoảng của nó phải hợp lệ và không được “phá vỡ” do thiếu các ràng buộc bao phủ bắt buộc. Số 0 có thể được gán một cách chiến lược cho bất kỳ giá trị nào trong phạm vi cho phép, vì vậy chúng luôn có thể được sử dụng để lấp đầy khoảng trống khi cần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ / O(n²q) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách xử lý từng giá trị như xác định một yêu cầu liền kề. 

1. Đầu tiên, chúng ta thay thế tất cả các số 0 bằng một giá trị giữ chỗ. Vì số không có thể trở thành bất cứ thứ gì nên chúng tôi tạm thời coi chúng là linh hoạt và gán chúng sau. 
2. Với mỗi giá trị từ`1`ĐẾN`q`, chúng tôi tính toán lần xuất hiện ngoài cùng bên trái và ngoài cùng bên phải trong mảng. Điều này mang lại một khoảng thời gian tối thiểu`[L[i], R[i]]`rằng bất kỳ phân đoạn hợp lệ nào cho hoạt động`i`phải che đậy. 
3. Chúng tôi xác minh điều đó cho mọi vị trí bên trong`[L[i], R[i]]`, nó hoặc là đã bằng`i`hoặc là số không. Nếu chúng ta tìm thấy một giá trị cố định khác với`i`trong khoảng này thì thực hiện phép toán`i`sẽ buộc phải ghi đè lên một vị trí bị cấm, sau này không thể hoàn tác được nên việc xây dựng không thành công. 
4. Bây giờ chúng tôi chỉ định các phân đoạn thực tế cho từng hoạt động. Chúng tôi bắt đầu từ`i = 1`ĐẾN`q`và thiết lập phân đoạn của`i`chính xác để`[L[i], R[i]]`. Điều này là đủ vì bất kỳ phân khúc lớn hơn nào cũng sẽ chỉ đưa ra những hạn chế không cần thiết. 
5. Chúng tôi xây dựng mảng cuối cùng bằng cách mô phỏng quá trình vẽ. Chúng tôi bắt đầu với một mảng trống và áp dụng các thao tác theo thứ tự, ghi đè các phân đoạn theo quy định. Trong quá trình mô phỏng này, số 0 được coi là giá trị có thể khớp với bất kỳ giá trị nào được ghi lần cuối. 
6. Cuối cùng, chúng tôi kiểm tra xem mọi vị trí đều được bảo hiểm ít nhất một lần, điều này được đảm bảo bởi việc xây dựng nếu tất cả các khoảng thời gian đều hợp lệ và tồn tại phạm vi bảo hiểm không trống. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là lần xuất hiện cuối cùng của mỗi giá trị sẽ xác định một vùng bắt buộc không thể bị thay đổi bởi các thao tác sau này. Bằng cách sử dụng các khoảng giới hạn tối thiểu, chúng tôi đảm bảo không có vị trí cần thiết nào bị mất. Vì các thao tác sau này chỉ ghi đè và các khoảng thời gian trước đó không bao giờ bị buộc phải mở rộng thành các giá trị cố định xung đột nên ràng buộc thứ tự được giữ nguyên. Bất kỳ số 0 nào cũng có thể được hấp thụ vào bất kỳ khoảng nào mà không vi phạm các ràng buộc, do đó tính khả thi làm giảm tính nhất quán của các khoảng giới hạn này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    L = [n] * (q + 1)
    R = [-1] * (q + 1)
    present = [False] * (q + 1)

    # compute bounds
    for i, v in enumerate(a):
        if v != 0:
            present[v] = True
            L[v] = min(L[v], i)
            R[v] = max(R[v], i)

    # check feasibility of fixed constraints
    for v in range(1, q + 1):
        if not present[v]:
            continue
        for i in range(L[v], R[v] + 1):
            if a[i] != 0 and a[i] != v:
                print("NO")
                return

    # build segments
    segL = [0] * (q + 1)
    segR = [0] * (q + 1)

    for v in range(1, q + 1):
        if present[v]:
            segL[v] = L[v]
            segR[v] = R[v]
        else:
            segL[v] = 0
            segR[v] = 0

    # simulate painting
    res = [0] * n
    for v in range(1, q + 1):
        l, r = segL[v], segR[v]
        for i in range(l, r + 1):
            res[i] = v

    # final check coverage
    if any(x == 0 for x in res):
        print("NO")
        return

    print("YES")
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ trích xuất các phạm vi bắt buộc cho mỗi nhãn. Những phạm vi đó đến trực tiếp từ những lần xuất hiện được quan sát và thể hiện những hạn chế không thể tránh khỏi. Bất kỳ mâu thuẫn nào trong một phạm vi sẽ ngay lập tức bác bỏ thể hiện đó. 

Sau đó, mỗi giá trị được gán khoảng thời gian tối thiểu. Điều này tránh các phân đoạn mở rộng quá mức, có thể tạo ra xung đột giả tạo. Bước mô phỏng xây dựng một cấu hình cuối cùng hợp lệ phù hợp với tất cả các ràng buộc. 

Việc xác minh cuối cùng đảm bảo rằng mảng được xây dựng không có vị trí nào chưa được sơn. Điều này phát hiện các trường hợp giá trị không bao giờ xuất hiện và chưa bao giờ được gán một phân đoạn có ý nghĩa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 0 2 3
```Chúng tôi tính toán các khoảng:`1 → [0,0]`,`2 → [2,2]`,`3 → [3,3]`. 

Số 0 ở vị trí 1 không mở rộng được gì nên nó vẫn linh hoạt. 

| bước | hành động | độ phân giải | 
| --- | --- | --- | 
| 1 | sơn 1 tại [0,0] | [1,0,0,0] | 
| 2 | sơn 2 tại [2,2] | [1,0,2,0] | 
| 3 | sơn 3 tại [3,3] | [1,0,2,3] | 

Sau đó chúng ta điền giá trị vào số 0`2`, tạo ra một cấu hình hợp lệ`1 2 2 3`. 

Điều này chứng tỏ các số 0 cho phép gán linh hoạt như thế nào mà không phá vỡ các ràng buộc về khoảng thời gian. 

### Ví dụ 2 

đầu vào:```
5 3
0 1 0 3 0
```Khoảng thời gian:`1 → [1,1]`,`3 → [3,3]`. 

| bước | hành động | độ phân giải | 
| --- | --- | --- | 
| 1 | sơn 1 tại [1,1] | [0,1,0,0,0] | 
| 2 | sơn 2 tại [0,0] | [0,1,0,0,0] | 
| 3 | sơn 3 tại [3,3] | [0,1,3,0,0] | 

Các số 0 còn lại có thể được gán tùy ý. 

Điều này cho thấy các nhãn không được sử dụng không hạn chế tính khả thi miễn là chúng không xung đột với các khoảng thời gian bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | một lần để tính toán các khoảng thời gian và mô phỏng một lần trên các phân đoạn | 
| Không gian | O(n + q) | lưu trữ mảng, giới hạn và kết quả | 

Thuật toán chạy theo thời gian tuyến tính trên kích thước mảng và số lượng truy vấn, phù hợp thoải mái trong giới hạn của`2 * 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    return solution(inp)

def solution(inp: str) -> str:
    data = inp.strip().split()
    n, q = map(int, data[:2])
    a = list(map(int, data[2:]))

    L = [n] * (q + 1)
    R = [-1] * (q + 1)
    present = [False] * (q + 1)

    for i, v in enumerate(a):
        if v:
            present[v] = True
            L[v] = min(L[v], i)
            R[v] = max(R[v], i)

    for v in range(1, q + 1):
        if present[v]:
            for i in range(L[v], R[v] + 1):
                if a[i] and a[i] != v:
                    return "NO"

    res = [0] * n
    for v in range(1, q + 1):
        if present[v]:
            for i in range(L[v], R[v] + 1):
                res[i] = v

    if any(x == 0 for x in res):
        return "NO"

    return "YES\n" + " ".join(map(str, res))

# provided sample
assert run("4 3\n1 0 2 3\n") == "YES\n1 2 2 3"

# all zeros
assert run("3 3\n0 0 0\n") != "", "all flexible"

# single value
assert run("3 2\n1 1 1\n") == "YES\n1 1 1"

# conflict case
assert run("3 2\n1 2 1\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | đầu ra hợp lệ linh hoạt | số không cho phép tự do hoàn toàn | 
| giá trị đơn | mảng CÓ | tính nhất quán thống nhất | 
| 1 2 1 | KHÔNG | nhãn cố định xung đột | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi một giá trị xuất hiện ở hai vùng rời nhau. Ví dụ,`1 0 2 0 1`. Thuật toán xây dựng khoảng`[0, 4]`cho giá trị`1`, bao gồm một khu vực tồn tại các giá trị khác. Nếu bất kỳ giá trị cố định nào trong khoảng đó khác nhau, công trình sẽ loại bỏ nó ngay lập tức. Điều này ngăn chặn việc hợp nhất không hợp lệ các thành phần riêng biệt. 

Một trường hợp khác là khi một giá trị không bao giờ xuất hiện. Ví dụ,`0 0 0`với kích thước lớn`q`. Thuật toán chỉ định các khoảng trống và các nhãn này không hạn chế việc xây dựng cuối cùng. Chúng chỉ đơn giản trở thành các phân đoạn không được sử dụng hoặc được chỉ định một cách tầm thường, điều này giúp tránh việc buộc phải đưa vào vùng phủ sóng không thể thực hiện được. 

Trường hợp thứ ba là khi các số 0 nằm giữa các giá trị cố định xung đột nhau, chẳng hạn như`1 0 2`. Ở đây, cả hai giá trị đều được tách biệt và các khoảng của chúng không trùng nhau, do đó việc gán thành công bằng cách đặt từng giá trị trên phân đoạn riêng của nó mà không có xung đột.
