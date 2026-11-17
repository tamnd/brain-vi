---
title: "CF 195B - Sau Luyện Tập"
description: "Chúng ta được yêu cầu mô phỏng quá trình phân phát những quả bóng đá được đánh số vào một hàng rổ theo một thứ tự cụ thể. Mỗi quả bóng mới phải được đưa vào rổ hiện có ít quả bóng nhất."
date: "2026-06-05T00:51:52+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 195
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 123 (Div. 2)"
rating: 1300
weight: 195
solve_time_s: 69
verified: true
draft: false
---

[CF 195B - Sau khi đào tạo](https://codeforces.com/problemset/problem/195/B) 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, cách triển khai, toán học 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng quá trình phân phát những quả bóng đá được đánh số vào một hàng rổ theo một thứ tự cụ thể. Mỗi quả bóng mới phải được đưa vào rổ hiện có ít quả bóng nhất. Nếu có nhiều rổ được buộc để có ít bóng nhất, chúng ta sẽ chọn rổ gần giữa hàng nhất. Nếu vẫn hòa thì ta chọn rổ có chỉ số nhỏ hơn. 

Đầu vào cho hai số nguyên: tổng số quả bóng`n`và tổng số giỏ`m`. Đầu ra là một chuỗi có độ dài`n`, ở đâu`i`-Số thứ là chỉ số rổ nhận bóng`i`. Trình tự phải được tính theo thứ tự các quả bóng được thêm vào, vì mỗi quả bóng mới phụ thuộc vào cấu hình rổ hiện tại. 

Những hạn chế là đáng kể. Cả hai`n`Và`m`có thể lớn tới 100.000. Một cách tiếp cận đơn giản là quét tất cả các rổ để tìm từng quả bóng sẽ yêu cầu`n*m`hoạt động, có thể đạt tới 10^10 trong trường hợp xấu nhất-quá lớn để thực hiện trong 2 giây. Điều này ngụ ý rằng chúng ta cần một chiến lược tránh quét toàn bộ và tận dụng cấu trúc có thể dự đoán được của các quy tắc lựa chọn. 

Trường hợp khó phát hiện xảy ra khi số lượng bóng nhỏ hơn số lượng rổ. Ví dụ, nếu`n = 2`Và`m = 5`, quả bóng đầu tiên sẽ vào rổ giữa (`3`) và quả bóng thứ hai sẽ được chuyển đến rổ tiếp theo gần tâm nhất (`2`hoặc`4`). Việc triển khai bất cẩn cho rằng tất cả các giỏ sẽ luôn được lấp đầy ít nhất một lần sẽ thất bại trong những trường hợp thưa thớt như vậy. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Chúng ta có thể duy trì một mảng có độ dài`m`đại diện cho số lượng bóng trong mỗi giỏ. Đối với mỗi quả bóng, chúng tôi sẽ quét mảng để tìm rổ có số lượng tối thiểu, sau đó ngắt dây buộc trước tiên theo khoảng cách đến tâm và sau đó theo chỉ số. Cách tiếp cận này hoạt động chính xác nhưng đòi hỏi`O(n * m)`hoạt động, điều này không thực tế đối với đầu vào lớn. Vì`n = m = 10^5`, nó sẽ bao gồm khoảng 10 tỷ hoạt động. 

Cái nhìn sâu sắc quan trọng là các tiêu chí lựa chọn tạo ra một trình tự có cấu trúc chặt chẽ. Quả bóng đầu tiên luôn đi vào rổ giữa. Quả bóng thứ hai đi vào một trong hai rổ ngay cạnh tâm. Quả bóng thứ ba đi về phía bên kia của trung tâm. Việc tiếp tục theo mô hình zig-zag này đảm bảo rằng bất kỳ lúc nào chúng ta cũng sẽ đặt quả bóng tiếp theo vào rổ trống hoặc có ít bóng nhất và gần tâm nhất. Vì thứ tự sắp xếp không phụ thuộc vào số bóng chính xác mà chỉ phụ thuộc vào kiểu dáng của các rổ nên chúng ta có thể tính toán trước thứ tự các rổ trong`O(m)`thời gian và sau đó lặp đi lặp lại nhiều lần để gán tất cả`n`quả bóng. 

Điều này làm giảm độ phức tạp`O(n)`với một cái nhỏ`O(m)`bước tiền xử lý, đủ nhanh cho các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(m) | Quá chậm | 
| Tối ưu | O(n) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tâm của hàng giỏ là`(m + 1) // 2`. Đây là rổ gần giữa nhất theo định nghĩa bài toán. 
2. Tạo một chuỗi các chỉ số giỏ bắt đầu từ giữa, sau đó xen kẽ trái và phải, mở rộng ra bên ngoài. Điều này đưa ra thứ tự chính xác của các rổ sẽ nhận bóng nếu chúng ta lấp đầy từng quả bóng một theo quy tắc. Mẫu này là: center, center-1, center+1, center-2, center+2, v.v. 
3. Khởi tạo một mảng để lưu trữ kết quả gán bóng. 
4. Lặp lại tất cả các quả bóng từ 1 đến`n`. Gán từng quả bóng vào rổ tiếp theo theo trình tự được tính toán trước. Nếu chúng ta đến cuối chuỗi (xảy ra khi`n > m`), hãy tiếp tục thực hiện theo trình tự vì giờ đây mỗi rổ sẽ có nhiều quả bóng. 
5. In chỉ số rổ của mỗi quả bóng theo thứ tự được chỉ định. 

Tại sao nó hoạt động: điều bất biến ở đây là trình tự được tính toán trước luôn chọn rổ có số bóng tối thiểu và gần tâm nhất. Bằng cách thực hiện tuần tự này, chúng tôi tái tạo chính xác sơ đồ của Valeric. Bởi vì mọi giỏ đều được xem lại theo đúng thứ tự, nên các mối ràng buộc được xử lý tự động và bộ ngắt ràng buộc chỉ số nhỏ nhất cũng được đáp ứng một cách tự nhiên bằng cách tạo ra chuỗi từ trái sang phải xung quanh tâm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

center = (m + 1) // 2
order = [center]

# expand left and right alternately
for d in range(1, m):
    if center - d >= 1:
        order.append(center - d)
    if center + d <= m:
        order.append(center + d)

# assign balls
res = []
for i in range(n):
    res.append(str(order[i % m]))

print("\n".join(res))
```Đầu tiên, mã tính toán giỏ trung tâm, sau đó xây dựng chuỗi giỏ được tính toán trước mở rộng ra bên ngoài. Vòng lặp`for i in range(n)`lần lượt chỉ định từng quả bóng, tuần tự thực hiện theo trình tự khi số bóng vượt quá số rổ. Tránh từng lỗi một bằng cách sử dụng kiểm tra toàn diện`>= 1`Và`<= m`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`n = 4, m = 3`| Bóng | Giỏ được chỉ định | Vị Trí Trình Tự | 
| --- | --- | --- | 
| 1 | 2 | trung tâm | 
| 2 | 1 | bên trái trung tâm | 
| 3 | 3 | bên phải trung tâm | 
| 4 | 2 | trung tâm một lần nữa | 

Bảng hiển thị các quả bóng theo trình tự zig-zag được tính toán trước. Quả bóng thứ tư quay trở lại chính giữa một cách chính xác vì tất cả các rổ hiện có một quả bóng và tâm gần nhất với giữa. 

Đầu vào tùy chỉnh 2:`n = 7, m = 5`| Bóng | Giỏ được chỉ định | Vị Trí Trình Tự | 
| --- | --- | --- | 
| 1 | 3 | trung tâm | 
| 2 | 2 | trái | 
| 3 | 4 | đúng | 
| 4 | 1 | ngoài cùng bên trái | 
| 5 | 5 | ngoài cùng bên phải | 
| 6 | 3 | trung tâm (vòng hai) | 
| 7 | 2 | trái (vòng hai) | 

Điều này chứng tỏ cách thuật toán tuần hoàn theo trình tự khi`n > m`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán trước thứ tự mất O(m), sau đó gán n quả bóng mất O(n) | 
| Không gian | O(m) | Lưu trữ chuỗi giỏ được tính toán trước | 

Được cho`n, m <= 10^5`, thuật toán thực hiện tối đa 200.000 thao tác để xây dựng chuỗi cộng với 100.000 phép gán, nằm trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    center = (m + 1) // 2
    order = [center]
    for d in range(1, m):
        if center - d >= 1:
            order.append(center - d)
        if center + d <= m:
            order.append(center + d)
    res = []
    for i in range(n):
        res.append(str(order[i % m]))
    return "\n".join(res)

# provided samples
assert run("4 3\n") == "2\n1\n3\n2", "sample 1"

# custom tests
assert run("1 1\n") == "1", "single ball, single basket"
assert run("2 5\n") == "3\n2", "fewer balls than baskets"
assert run("7 5\n") == "3\n2\n4\n1\n5\n3\n2", "more balls than baskets"
assert run("5 5\n") == "3\n2\n4\n1\n5", "balls equal baskets"
assert run("10 2\n") == "1\n2\n1\n2\n1\n2\n1\n2\n1\n2", "two baskets repeated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào tối thiểu | 
|`2 5`|`3\n2`| Ít bóng hơn rổ | 
|`7 5`|`3\n2\n4\n1\n5\n3\n2`| Nhiều bóng hơn rổ, trình tự đạp xe | 
|`5 5`|`3\n2\n4\n1\n5`| Bóng bằng rổ, đầy đủ vòng 1 | 
|`10 2`|`1\n2\n1\n2\n1\n2\n1\n2\n1\n2`| Hai giỏ lặp đi lặp lại | 

## Vỏ cạnh 

Khi nào`n < m`, chẳng hạn như`n = 2, m = 5`, thuật toán gán quả bóng đầu tiên vào giữa (`3`) và thứ hai vào giỏ gần nhất tiếp theo (`2`). Các rổ còn lại bị bỏ qua vì không đủ bóng để tiếp cận. Trình tự được tính toán trước đảm bảo rằng các ràng buộc dành cho ít quả bóng nhất bị phá vỡ do khoảng cách gần với tâm và thứ tự chỉ số một cách tự động. Điều này tránh mọi hoạt động quét số lượng tối thiểu thủ công, vốn là nguồn phổ biến gây ra các lỗi tinh vi trong quá trình triển khai đơn giản
