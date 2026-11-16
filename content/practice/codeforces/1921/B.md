---
title: "CF 1921B - Sắp Xếp Mèo"
description: "Nhiệm vụ là chuyển đổi cách sắp xếp mèo trong hộp ban đầu thành cách sắp xếp mục tiêu bằng cách sử dụng ít thao tác nhất. Mỗi hộp chứa một con mèo hoặc trống và trạng thái ban đầu được cho bởi một chuỗi nhị phân s, trong khi trạng thái đích được cho bởi một chuỗi nhị phân f."
date: "2026-06-08T19:22:03+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1921
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 920 (Div. 3)"
rating: 800
weight: 1921
solve_time_s: 123
verified: true
draft: false
---

[CF 1921B - Sắp xếp Mèo](https://codeforces.com/problemset/problem/1921/B) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chuyển đổi cách sắp xếp mèo trong hộp ban đầu thành cách sắp xếp mục tiêu bằng cách sử dụng ít thao tác nhất. Mỗi hộp chứa một con mèo hoặc trống và trạng thái ban đầu được cung cấp bởi một chuỗi nhị phân`s`, trong khi trạng thái đích được cung cấp bởi một chuỗi nhị phân`f`. Các hoạt động bao gồm đặt một con mèo mới vào một hộp trống, lấy một con mèo ra khỏi một hộp đã đầy hoặc di chuyển một con mèo từ hộp này sang hộp khác. Mỗi hoạt động tiêu tốn một ngày, vì vậy mục tiêu là giảm thiểu số ngày cần thiết. 

Những ràng buộc cho phép`n`để được`10^5`trong một trường hợp thử nghiệm duy nhất và tổng số`n`trên tất cả các trường hợp thử nghiệm cũng tùy thuộc vào`10^5`. Điều này ngụ ý rằng bất kỳ giải pháp nào cũng phải tuyến tính trong`n`mỗi trường hợp thử nghiệm. Các phương pháp bậc hai cố gắng mô phỏng việc mèo di chuyển từng hộp sẽ quá chậm. 

Các trường hợp cạnh tranh phát sinh khi sự sắp xếp ban đầu và cuối cùng có cùng số lượng mèo, khi tất cả các ô ban đầu đều trống hoặc đầy hoặc khi một nước đi có thể thay thế trực tiếp cả việc loại bỏ và thêm vào. Ví dụ, chuyển đổi`s = 10010`ĐẾN`f = 00001`yêu cầu di chuyển một con mèo từ hộp đầu tiên đến hộp thứ năm và loại bỏ con mèo thừa ở hộp thứ tư, dẫn đến hai thao tác. Những cách tiếp cận ngây thơ coi các bước di chuyển như những sự bổ sung và loại bỏ độc lập có thể bị tính quá mức. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng quá trình bằng cách liên tục tìm một con mèo để di chuyển hoặc một chiếc hộp để lấp đầy hoặc làm trống, theo dõi các hoạt động một cách rõ ràng. Điều này sẽ hoạt động chính xác nhưng sẽ quá chậm, lên đến`O(n^2)`trong trường hợp xấu nhất. 

Một cách tiếp cận tốt hơn là xử lý vấn đề bằng một chiến lược đếm đơn giản. Đối với mỗi vị trí, hãy xác định xem có cần thêm một con mèo hay không (`f[i] = 1, s[i] = 0`) hoặc bị loại bỏ (`f[i] = 0, s[i] = 1`). Cho phép`add_count`là số vị trí cần một con mèo và`remove_count`là số vị trí cần loại bỏ. Mèo có thể được di chuyển từ hộp cần loại bỏ sang hộp cần bổ sung, giúp giảm số lượng thao tác riêng biệt. Do đó, số ngày tối thiểu là số ngày tối đa`add_count`Và`remove_count`, bởi vì mỗi nước đi đồng thời được tính cho cả việc thêm và loại bỏ một con mèo. Quan sát này làm giảm vấn đề xuống`O(n)`mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Đếm + Số lần thêm và xóa tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`add_count`Và`remove_count`về không. Những thứ này sẽ theo dõi số lượng hộp yêu cầu một con mèo được thêm vào hoặc loại bỏ tương ứng. 
2. Lặp lại qua từng vị trí`i`từ 0 đến`n-1`. 
3. Nếu`s[i] = 0`Và`f[i] = 1`, tăng`add_count`. Điều này cho thấy một con mèo phải được thêm vào đây. 
4. Nếu`s[i] = 1`Và`f[i] = 0`, tăng`remove_count`. Điều này cho thấy một con mèo phải được loại bỏ khỏi đây. 
5. Sau khi xử lý tất cả các vị trí, số ngày tối thiểu cần thiết là`max(add_count, remove_count)`. Các bước di chuyển có thể đáp ứng đồng thời cả việc thêm và loại bỏ, do đó, mức tối đa sẽ được tính mức tối thiểu thực sự. 
6. Xuất giá trị này. 

Tại sao nó hoạt động: Mọi thao tác thêm hoặc xóa không thể ghép nối phải được xử lý riêng lẻ. Mọi thao tác di chuyển đều xử lý một lần cộng và một lần xóa cùng nhau, do đó, số lượng tối đa của hai lần đếm sẽ ghi lại chính xác tổng số ngày cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    f = input().strip()
    add_count = 0
    remove_count = 0
    for i in range(n):
        if s[i] == '0' and f[i] == '1':
            add_count += 1
        elif s[i] == '1' and f[i] == '0':
            remove_count += 1
    print(max(add_count, remove_count))
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và lặp lại mỗi chuỗi chính xác một lần. Việc sử dụng`max(add_count, remove_count)`tính toán chính xác các bước di chuyển có thể thực hiện đồng thời một thao tác bổ sung và loại bỏ, đây là điểm tinh tế mà việc thực hiện bất cẩn có thể bỏ qua. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`s = 10010`,`f = 00001`: 

| tôi | s[i] | f[i] | thêm_count | xóa_count | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 1 | 
| 1 | 0 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 0 | 1 | 
| 3 | 1 | 0 | 0 | 2 | 
| 4 | 0 | 1 | 1 | 2 |`max(add_count, remove_count) = max(1, 2) = 2`, phù hợp với đầu ra dự kiến. 

Vì`s = 000`,`f = 111`: 

| tôi | s[i] | f[i] | thêm_count | xóa_count | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 0 | 
| 1 | 0 | 1 | 2 | 0 | 
| 2 | 0 | 1 | 3 | 0 |`max(add_count, remove_count) = max(3, 0) = 3`, theo yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại từng vị trí một lần để đếm số lần thêm và bớt. | 
| Không gian | O(1) | Chỉ có hai bộ đếm số nguyên được sử dụng cho mỗi trường hợp thử nghiệm, không cần cấu trúc dữ liệu bổ sung. | 

Độ phức tạp tuyến tính trên mỗi trường hợp thử nghiệm đảm bảo rằng ngay cả với tối đa`10^5`tổng số hộp, chương trình sẽ chạy hiệu quả trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        f = input().strip()
        add_count = 0
        remove_count = 0
        for i in range(n):
            if s[i] == '0' and f[i] == '1':
                add_count += 1
            elif s[i] == '1' and f[i] == '0':
                remove_count += 1
        output.append(str(max(add_count, remove_count)))
    return "\n".join(output)

# provided samples
assert run("6\n5\n10010\n00001\n1\n1\n1\n3\n000\n111\n4\n0101\n1010\n3\n100\n101\n8\n10011001\n11111110\n") == "2\n0\n3\n2\n1\n4"

# custom cases
assert run("2\n1\n0\n1\n1\n1\n0\n") == "1\n1", "single box add/remove"
assert run("2\n3\n111\n111\n3\n000\n000\n") == "0\n0", "no operations needed"
assert run("1\n5\n10101\n01010\n") == "3", "alternating positions"
assert run("1\n4\n0000\n1111\n") == "4", "all additions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n0\n1 | 1 | Hộp đơn, cần thêm mèo | 
| 1\n1\n0 | 1 | Hộp đơn, cần bỏ mèo | 
| 3\n111\n111 | 0 | Không cần thao tác | 
| 5\n10101\n01010 | 3 | Các bước di chuyển xen kẽ yêu cầu bổ sung và loại bỏ ghép nối | 
| 4\n0000\n1111 | 4 | Tất cả các bổ sung phải được thực hiện | 

## Vỏ cạnh 

Khi tất cả các hộp ban đầu khớp với mục tiêu,`add_count = remove_count = 0`, do đó đầu ra là 0. Khi tất cả các ô trống và tất cả phải được điền,`remove_count = 0`, và mức tối đa chỉ đơn giản là`add_count`. Khi có số lượng bổ sung và loại bỏ bằng nhau, các bước di chuyển có thể ghép chúng một cách tối ưu, do đó kết quả đầu ra chính xác là số lượng này, như được minh họa trong trường hợp kiểm tra vị trí xen kẽ. Điều này xác nhận rằng thuật toán xử lý chính xác cả trường hợp cực đoan và trường hợp hỗn hợp.
