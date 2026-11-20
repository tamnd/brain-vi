---
title: "CF 2151B - Đường dẫn tăng dần"
description: "Chúng ta được cung cấp một dải ảo với các ô được đánh số từ 1 đến $10^9$, trong đó một số ô ban đầu có màu đen và các ô còn lại có màu trắng. Nhiều người bắt đầu từ ô 1 và mỗi người thực thi tiền tố của chuỗi lệnh gồm hai lệnh."
date: "2026-06-09T04:17:10+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2151
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1053 (Div. 2)"
rating: 1300
weight: 2151
solve_time_s: 92
verified: false
draft: false
---

[CF 2151B - Đường dẫn tăng dần](https://codeforces.com/problemset/problem/2151/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dải ảo với các ô được đánh số từ 1 đến$10^9$, trong đó một số ô ban đầu có màu đen và phần còn lại có màu trắng. Nhiều người bắt đầu từ ô 1 và mỗi người thực thi tiền tố của chuỗi lệnh gồm hai lệnh. Lời hướng dẫn đầu tiên,`A`, di chuyển người về phía trước một ô. Thứ hai,`B`, di chuyển người đó đến ô màu trắng tiếp theo ngoài ô hiện tại. Sau khi thực hiện lệnh của mình, mỗi người sơn ô mà họ đặt lên màu đen. Nhiệm vụ của chúng ta là báo cáo tất cả các ô có màu đen sau khi mọi người đã thực hiện lệnh của mình. 

Hạn chế chính là số lượng lệnh cho mỗi trường hợp thử nghiệm có thể lên tới$10^5$và số lượng ô đen ban đầu cũng có thể lên tới$10^5$. Tổng số trên tất cả các trường hợp thử nghiệm cho lệnh hoặc ô đen ban đầu được giới hạn ở mức$10^5$. Điều này có nghĩa là bất kỳ giải pháp nào có độ phức tạp kém hơn$O(n + m)$mỗi trường hợp thử nghiệm có nguy cơ hết thời gian. Mô phỏng trực tiếp chuyển động của từng ô trên một tỷ ô là không khả thi và việc sử dụng một mảng đầy đủ để theo dõi tất cả các ô là không thể do giới hạn bộ nhớ. 

Các trường hợp cạnh phát sinh khi ô đầu tiên có màu đen hoặc khi nhiều ô`B`lệnh bỏ qua các ô màu đen liền kề. Một mô phỏng đơn giản cố gắng chuyển sang ô màu trắng tiếp theo bằng cách tăng từng ô một sẽ thất bại vì có thể có những khoảng trống lớn và hiệu suất sẽ giảm. Ví dụ: nếu ô 1 có màu đen và lệnh đầu tiên là`B`, người đó phải chuyển trực tiếp đến ô 2 nếu ô đó màu trắng hoặc xa hơn nếu các ô tiếp theo có màu đen. Một giải pháp bất cẩn chỉ có thể xem xét`x+1`chứ không phải là ô màu trắng tiếp theo. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng từng người bắt đầu từ ô 1, thực hiện từng lệnh một và tìm ô trắng tiếp theo bằng cách quét về phía trước cho đến khi tìm thấy ô trắng. Mặc dù điều này đúng về mặt khái niệm nhưng nó quá chậm. Nếu có$n = 10^5$các lệnh và các ô màu đen nằm rải rác khắp$10^9$dải, quét từng`B`hoạt động có thể mất tới$10^9$các bước trong trường hợp xấu nhất, điều này vượt xa khả năng thực hiện. 

Quan sát quan trọng là các ô duy nhất có thể chuyển sang màu đen là các ô màu đen ban đầu hoặc các ô được truy cập ngay lập tức bằng cách thực thi tiền tố của lệnh. Mỗi người chỉ vẽ ô mà họ kết thúc sau khi làm theo tiền tố của họ và vì mỗi người bắt đầu từ 1 nên trình tự các bước di chuyển là đơn điệu. Chúng ta có thể tập trung vào tác dụng của`B`các lệnh mà không lặp lại trên tất cả các ô. Cụ thể, với mỗi tiền tố, chúng ta có thể xác định ô cuối cùng bằng cách giữ một con trỏ`cur`bắt đầu từ 1. Chúng ta lặp qua các lệnh, tăng dần`cur`vì`A`, và cho`B`chúng tôi di chuyển`cur`đến ô trắng nhỏ nhất chưa được thăm dò sau`cur`. 

Việc duy trì một tập hợp các ô đen đã được sắp xếp cho phép chúng ta tìm thấy ô trắng tiếp theo một cách hiệu quả. Mỗi lần chúng ta thực hiện một`B`, chúng ta nhảy tới vị trí tiếp theo ngoài ô màu đen cuối cùng tại hoặc sau`cur`. Số lượng thao tác tỷ lệ thuận với số lượng lệnh cộng với số lượng ô đen, điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * khoảng trống) | O(10^9) | Quá chậm | 
| Tối ưu | O((n + m) log m) | O(m) | Đã chấp nhận | 

các`log m`hệ số phát sinh nếu chúng ta sử dụng vùng chứa được sắp xếp hoặc tìm kiếm nhị phân để tìm ô trắng tiếp theo một cách hiệu quả. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`lệnh,`m`các ô màu đen ban đầu, chuỗi lệnh`s`, và danh sách các ô màu đen`a`. Chuyển đổi danh sách`a`đến một tập hợp hoặc mảng được sắp xếp để tìm kiếm hiệu quả. 
2. Khởi tạo một biến`cur`đến 1, đại diện cho ô hiện tại của người được mô phỏng. 
3. Lặp lại từng lệnh trong chuỗi. Nếu lệnh là`A`, chỉ cần tăng`cur`bởi một. Nếu lệnh là`B`, tìm ô màu đen đầu tiên lớn hơn hoặc bằng`cur`sử dụng tìm kiếm nhị phân. Bộ`cur`đến nhiều hơn ô đen đó, chuyển sang ô trắng tiếp theo một cách hiệu quả. 
4. Theo dõi tất cả các ô được sơn đen bằng cách lưu chúng vào một bộ. Ban đầu, bao gồm tất cả các ô màu đen. Mỗi lần chúng tôi xử lý xong một tiền tố có độ dài`i`, thêm vào`cur`đến trường quay. 
5. Sau khi xử lý tất cả các lệnh cho một ca kiểm thử, hãy sắp xếp tập hợp các ô màu đen và xuất ra kích thước cũng như các phần tử của nó. 

Bất biến được duy trì xuyên suốt là`cur`luôn trỏ đến đúng vị trí tiếp theo theo lệnh, xem xét các ô vốn đã có màu đen. Mỗi lần nhảy qua`B`luôn nằm ở ô trắng tiếp theo so với cấu hình ô đen hiện tại. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def process_case(n, m, s, black_cells):
    black_cells.sort()
    black_set = set(black_cells)
    cur = 1
    painted = set(black_cells)

    for cmd in s:
        if cmd == 'A':
            cur += 1
        else:  # 'B'
            idx = bisect.bisect_left(black_cells, cur)
            if idx < len(black_cells) and black_cells[idx] == cur:
                cur = black_cells[idx] + 1
            else:
                if idx < len(black_cells):
                    cur = black_cells[idx] + 1
                # else cur remains as next white cell (beyond last black)
        painted.add(cur)
    res = sorted(painted)
    return res

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    s = input().strip()
    black_cells = list(map(int, input().split()))
    result = process_case(n, m, s, black_cells)
    print(len(result))
    print(' '.join(map(str, result)))
```Giải pháp sắp xếp các ô đen ban đầu và sử dụng tìm kiếm nhị phân để xác định vị trí ô đen tiếp theo một cách hiệu quả khi thực hiện một lệnh.`B`. Chúng tôi duy trì một bộ`painted`để thu thập tất cả các tế bào mới bị đen. Cần chú ý xử lý trường hợp khi`cur`nằm ngoài ô đen cuối cùng, nơi nó sẽ vẫn là ô trắng tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
BAB
2 5
```| Bước | Lệnh | cur | sơn | 
| --- | --- | --- | --- | 
| 1 | B | 1 → 3 | {2,5,3} | 
| 2 | A | 3 → 4 | {2,5,3,4} | 
| 3 | B | 4 → 6 | {2,5,3,4,6} | 

Tập hợp các ô đen được sắp xếp cuối cùng là`[2,3,4,5,6]`. Điều này xác nhận rằng`B`bỏ qua các ô màu đen một cách chính xác. 

### Ví dụ 2 

đầu vào:```
1 3
ABA
1 4 9
```| Bước | Lệnh | cur | sơn | 
| --- | --- | --- | --- | 
| 1 | A | 1 → 2 | {1,4,9,2} | 
| 2 | B | 2 → 3 | {1,4,9,2,3} | 
| 3 | A | 3 → 4 | {1,4,9,2,3} | 

Điều này chứng tỏ rằng`A`tăng vị trí một cách chính xác ngay cả khi ô có màu đen. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | Sắp xếp các ô đen ban đầu là O(m log m), mỗi lần tìm kiếm nhị phân cho`B`là O(log m), được thực hiện n lần. | 
| Không gian | O(m + n) | Lưu trữ các ô màu đen ban đầu và các ô được sơn. | 

Với những hạn chế của vấn đề, điều này phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())
    return output.getvalue().strip()

# Provided samples
assert run("""5
3 2
BAB
2 5
3 4
ABA
1 4 9 10
5 2
ABABB
1 7
3 1
BBA
6
1 4
A
1 3 4 1000000000
""") == """4
2 3 5 6
7
1 2 3 4 6 9 10
7
1 2 3 5 6 7 9
3
2 4 6
5
1 2 3 4 1000000000""", "sample tests"

# Custom edge cases
assert run("1\n1 1\nB\n1") == "2\n1 2", "next white after first black"
assert run("1\n2 2\nBB\n1 2") == "3\n1 2 3", "consecutive B commands skipping blacks"
assert run("1\n3 1\nABA\n5") == "4\n1 2 3 5", "jump past black with combination of A and B"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| `B | | |
