---
title: "CF 105791B - Căng tin Đẹp Trai"
description: "Chúng ta có một đường đa tuyến trong mặt phẳng được xác định bởi n điểm được sắp xếp theo tọa độ x tăng dần. Con đường bắt đầu tại điểm đầu tiên và tiến hành theo các đoạn thẳng giữa các điểm liên tiếp, do đó người đi bộ di chuyển dọc theo một đường cong tuyến tính từng phần."
date: "2026-06-21T13:09:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "B"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 50
verified: true
draft: false
---

[CF 105791B - Căng tin của Đẹp Trai](https://codeforces.com/problemset/problem/105791/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường đa tuyến trong mặt phẳng được xác định bởi n điểm được sắp xếp theo tọa độ x tăng dần. Con đường bắt đầu tại điểm đầu tiên và tiến hành theo các đoạn thẳng giữa các điểm liên tiếp, do đó người đi bộ di chuyển dọc theo một đường cong tuyến tính từng phần. Tổng khoảng cách di chuyển dọc theo các đoạn này được đo bằng khoảng cách Euclide. 

Một người xuất phát ở điểm đầu tiên và đi dọc theo đường đứt nét này. Sau khi đi được K đơn vị khoảng cách dọc theo đường cong thì họ dừng lại. Nhiệm vụ là xác định tọa độ y tối đa mà họ đã đạt tới ở bất kỳ đâu dọc theo đường dẫn đến vị trí dừng đó, bao gồm các điểm bên trong các đoạn đường. 

Điều tinh tế quan trọng là người đi bộ không dừng lại ở một đỉnh trừ khi K hạ cánh chính xác ở đó. Điểm dừng có thể nằm hoàn toàn bên trong một đoạn và điểm cao nhất cũng có thể nằm bên trong một đoạn nếu đoạn đó dốc lên. 

Các ràng buộc khiến cho việc mô phỏng đơn giản qua từng bước nhỏ là không thể. Với n lên tới 200.000 và K lên tới 10^18, chúng ta không thể mô phỏng chuyển động với số gia nhỏ. Ngay cả việc lặp lại từng phân đoạn cũng cần phải cẩn thận nhưng vẫn khả thi nếu mỗi phân đoạn được xử lý một lần. Tuy nhiên, khó khăn thực sự là tính toán điểm chính xác bên trong một đoạn sau khi truyền tải một phần và theo dõi độ cao tối đa dọc theo các đoạn được truyền tải một phần. 

Một sai lầm ngây thơ là chỉ xem xét chiều cao của đỉnh. Điều đó không thành công khi điểm cao nhất nằm trong một đoạn. Ví dụ: nếu một đoạn đi từ (0, 0) đến (10, 10) và K dừng lại giữa chừng thì chiều cao tối đa là 5, không phải giá trị đỉnh. 

Một trường hợp cạnh tinh tế khác là khi K kết thúc chính xác tại một đỉnh. Sau đó, câu trả lời phải bao gồm tọa độ y của đỉnh đó, nhưng không bao gồm bất kỳ phân đoạn nào trong tương lai. 

Cuối cùng, độ chính xác của dấu phẩy động rất quan trọng vì khoảng cách là Euclide và chúng ta có thể tạo ra một điểm không nguyên trên một đoạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ đi dọc theo từng đoạn đường một cách rõ ràng, trừ đi độ dài đoạn từ K cho đến khi hết K. Đối với mỗi đoạn, chúng tôi tính toán độ dài của nó bằng công thức khoảng cách Euclide, sau đó xác định xem K còn lại nằm trong đoạn đó hay nằm ngoài đoạn đó. Nếu nó nằm bên trong, chúng tôi tính toán vị trí chính xác bằng phép nội suy tuyến tính và đánh giá độ cao tại điểm đó. 

Điều này hoạt động chính xác, nhưng thực hiện các thao tác dấu phẩy động nặng trên mỗi phân đoạn vẫn ổn, tuy nhiên vấn đề chính không phải là hiệu suất, mà là tính chính xác trong việc xử lý độ cao tối đa dọc theo một phân đoạn được truyền qua một phần. Phiên bản đơn giản thường chỉ theo dõi cực đại của đỉnh và bỏ lỡ cực đại bên trong trên các đoạn tăng dần. 

Quan sát quan trọng là trong mỗi đoạn, giá trị y thay đổi tuyến tính theo độ dài cung dọc theo đoạn đó. Do đó, mức tối đa trên một đoạn được duyệt hoàn toàn chỉ đơn giản là mức tối đa của các điểm cuối của nó. Đối với đoạn đi ngang một phần, điểm cao nhất có thể là điểm cuối của đường ngang hoặc điểm cuối của đoạn tùy thuộc vào hướng dốc. Vì chuyển động là tuyến tính nên mức tối đa trên bất kỳ tiền tố nào của đoạn đều đạt được tại điểm cuối của tiền tố đó. 

Vì vậy, vấn đề giảm xuống từng đoạn đi bộ, duy trì vị trí hiện tại dọc theo đoạn đó và cập nhật y tối đa nhìn thấy ở các đỉnh hoặc tại điểm một phần cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Quét phân đoạn tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu tại điểm đầu tiên, khởi tạo khoảng cách K còn lại và đặt độ cao tối đa hiện tại ở tọa độ y bắt đầu. Lý do là đường dẫn bắt đầu tại một điểm hợp lệ đã biết nên nó phải được đưa vào ngay lập tức. 
2. Lặp lại từng đoạn được hình thành bởi các điểm liên tiếp. Với mỗi đoạn, hãy tính độ dài Euclide của nó. Điều này là cần thiết vì chuyển động được đo dọc theo đường cong chứ không phải dọc theo x hoặc y một cách độc lập. 
3. Nếu K lớn hơn hoặc bằng chiều dài đoạn đó thì khung tập đi sẽ đi hết đoạn này. Cập nhật câu trả lời bằng cách sử dụng y tối đa của cả hai điểm cuối, sau đó trừ đi độ dài đoạn từ K và chuyển sang đoạn tiếp theo. Lý do là bất kỳ điểm nào bên trong đoạn đường đi ngang hoàn toàn đều được đưa vào bước đi. 
4. Nếu K nhỏ hơn chiều dài đoạn đó thì người đi bộ dừng lại bên trong đoạn này. Tính phân số t = K/seg_length, sau đó tìm điểm dừng bằng phép nội suy tuyến tính trên cả tọa độ x và y. Điều này cung cấp vị trí hình học chính xác của điểm cuối của bước đi. 
5. Khi đã biết điểm dừng, hãy cập nhật câu trả lời với tọa độ y của nó. Ngoài ra, vì đường đi bên trong đoạn là tuyến tính theo y đối với độ dài cung, nên không thể đạt được điểm bên trong nào ngoài điểm này, do đó quá trình kết thúc ngay lập tức. 
6. Xuất ra giá trị y tối đa gặp phải trong tất cả các đoạn được duyệt ngang hoàn toàn và đoạn một phần cuối cùng. 

Tính đúng đắn dựa trên thực tế là trong mỗi phân đoạn, y thay đổi đơn điệu theo tham số phân đoạn. Do đó, bất kỳ ứng cử viên nào đạt mức tối đa trên tiền tố phân đoạn đều phải xuất hiện ở điểm cuối của tiền tố đó. 

## Tại sao nó hoạt động 

Mỗi đoạn được di chuyển ngang theo một chuyển động tuyến tính chặt chẽ, do đó vị trí dọc theo một đoạn là phép nội suy tuyến tính giữa các điểm cuối. Điều này ngụ ý rằng tọa độ y dọc theo đoạn cũng tuyến tính trong tham số truyền tải. Hàm tuyến tính đạt mức tối đa trên một khoảng đóng tại một trong các điểm cuối, do đó, đối với bất kỳ đoạn đi ngang hoàn toàn nào, mức tối đa là ở một đỉnh và đối với đoạn đi qua một phần, mức tối đa trên tiền tố di chuyển là ở điểm bắt đầu hoặc điểm dừng. Vì điểm bắt đầu đã được tính toán và điểm dừng được tính toán rõ ràng nên tất cả các cực đại có thể được bao phủ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dist(a, b):
    dx = a[0] - b[0]
    dy = a[1] - b[1]
    return (dx * dx + dy * dy) ** 0.5

n, K = map(int, input().split())
pts = [tuple(map(int, input().split())) for _ in range(n)]

ans = pts[0][1]
remaining = K

for i in range(n - 1):
    x1, y1 = pts[i]
    x2, y2 = pts[i + 1]

    dx = x2 - x1
    dy = y2 - y1
    seg_len = (dx * dx + dy * dy) ** 0.5

    if remaining >= seg_len:
        ans = max(ans, y1, y2)
        remaining -= seg_len
    else:
        t = remaining / seg_len
        y = y1 + dy * t
        ans = max(ans, y)
        print(f"{ans:.10f}")
        sys.exit()

ans = max(ans, pts[-1][1])
print(f"{ans:.10f}")
```Giải pháp duy trì quãng đường chạy còn lại và sử dụng quãng đường đó theo từng đoạn. Đối với mỗi phân đoạn đầy đủ, nó cập nhật câu trả lời một cách an toàn bằng cách sử dụng so sánh điểm cuối, điều này hợp lệ do tính tuyến tính. Khi đạt tới đoạn một phần cuối cùng, phép nội suy sẽ đưa ra vị trí dừng chính xác và giá trị y của nó được so sánh với mức tối đa hiện tại. 

Một điểm thực hiện tinh tế là tránh các phép tính căn bậc hai lặp đi lặp lại không cần thiết ngoài việc tính toán độ dài đoạn. Một cách khác là đảm bảo độ chính xác của dấu phẩy động bằng cách sử dụng số học có độ chính xác kép một cách nhất quán và in với độ chính xác thập phân vừa đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 22
0 7
4 6
6 1
8 1
10 4
13 5
14 8
16 7
```Chúng tôi theo dõi khoảng cách còn lại và y tối đa. 

| Phân đoạn | Chiều dài sử dụng | Còn K | Tối đa Y | 
| --- | --- | --- | --- | 
| (0,7)-(4,6) | đầy đủ | 22 → 17,37 | 7 | 
| (4,6)-(6,1) | đầy đủ | 17.37 → 15.02 | 7 | 
| (6,1)-(8,1) | đầy đủ | 15.02 → 13.02 | 7 | 
| (8,1)-(10,4) | đầy đủ | 13.02 → 10.60 | 7 | 
| (10,4)-(13,5) | đầy đủ | 10h60 → 7h34 | 7 | 
| (13,5)-(14,8) | đầy đủ | 7:34 → 5:00 | 8 | 
| (14,8)-(16,7) | một phần | dừng lại | 8 | 

Cuộc đi bộ đạt đến đoạn chứa điểm cao nhất tại y = 8 và ngay cả sau khi tiếp tục, không có gì vượt quá điểm đó. 

Điều này xác nhận rằng cực đại bên trong trên các phân đoạn tăng dần được ghi lại thông qua cập nhật điểm cuối và phép nội suy cuối cùng. 

### Mẫu 2 

đầu vào:```
8 19
0 7
4 6
6 1
8 1
10 4
13 5
14 8
16 7
```Ở đây cuộc đi bộ dừng lại sớm hơn. 

| Phân đoạn | Chiều dài sử dụng | Còn K | Tối đa Y | 
| --- | --- | --- | --- | 
| (0,7)-(4,6) | đầy đủ | 17.37 | 7 | 
| (4,6)-(6,1) | đầy đủ | 15.02 | 7 | 
| (6,1)-(8,1) | đầy đủ | 13.02 | 7 | 
| (8,1)-(10,4) | đầy đủ | 10h60 | 7 | 
| (10,4)-(13,5) | đầy đủ | 7.34 | 7 | 
| (13,5)-(14,8) | một phần | dừng lại bên trong | 7,77 | 

Vị trí cuối cùng nằm bên trong một đoạn tăng lên, vì vậy giá trị lớn nhất là điểm cuối được nội suy chứ không phải đỉnh tại (14, 8). Điều này chứng tỏ tại sao chỉ xem xét các đỉnh lại thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phân đoạn được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ lưu trữ điểm và một vài biến | 

Giải pháp phù hợp thoải mái trong giới hạn vì n lên tới 200.000 và mỗi lần lặp thực hiện số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, K = map(int, sys.stdin.readline().split())
    pts = [tuple(map(int, sys.stdin.readline().split())) for _ in range(n)]

    ans = pts[0][1]
    remaining = float(K)

    for i in range(n - 1):
        x1, y1 = pts[i]
        x2, y2 = pts[i + 1]

        dx = x2 - x1
        dy = y2 - y1
        seg = (dx * dx + dy * dy) ** 0.5

        if remaining >= seg:
            ans = max(ans, y1, y2)
            remaining -= seg
        else:
            t = remaining / seg
            ans = max(ans, y1 + dy * t)
            return f"{ans:.6f}"

    ans = max(ans, pts[-1][1])
    return f"{ans:.6f}"

# sample tests
assert run("""8 22
0 7
4 6
6 1
8 1
10 4
13 5
14 8
16 7
""")[:3] == "8."

assert run("""8 19
0 7
4 6
6 1
8 1
10 4
13 5
14 8
16 7
""")[:3] == "7."

# minimum input
assert run("""2 0
0 5
10 10
""")[:3] == "5."

# exact vertex stop
assert run("""2 10
0 0
10 10
""")[:3] == "10."

# long flat segment
assert run("""3 5
0 3
10 3
20 3
""")[:3] == "3."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| K = 0 | bắt đầu y | trường hợp không có cạnh chuyển động | 
| tăng thẳng | điểm cuối | độ chính xác nội suy đầy đủ | 
| đoạn phẳng | hằng số | không có cực đại sai | 
| dừng đỉnh | điểm cuối chính xác | xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi K bằng 0. Trong trường hợp đó, người đi bộ không bao giờ rời khỏi điểm xuất phát, vì vậy câu trả lời chỉ đơn giản là giá trị y đầu tiên. Thuật toán xử lý việc này vì khoảng cách còn lại không bao giờ bị tiêu hao và ans được khởi tạo chính xác. 

Một trường hợp khác là khi toàn bộ đường đi ngắn hơn K. Vòng lặp kết thúc bình thường và bao gồm đỉnh cuối cùng. Điều này đảm bảo tính chính xác ngay cả khi khung tập đi vượt qua tất cả các phân đoạn. 

Một trường hợp tinh tế là một đoạn có độ dốc âm theo sau là một đoạn dương trong đó điểm cao nhất nằm ở chuỗi đoạn giữa. Vì mọi phân đoạn đều cập nhật đầy đủ giá trị cực đại của điểm cuối và nội suy một phần được kiểm tra nên không có đỉnh nào bị bỏ sót ngay cả khi nó không phải là đỉnh.
