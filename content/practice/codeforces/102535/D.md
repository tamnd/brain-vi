---
title: "CF 102535D - Bám Mo"
description: "Chúng ta có một dòng thuốc nổ được biểu thị bằng mảng E. Một chất nổ ở vị trí thứ i chỉ có thể kích hoạt khi có ít nhất một chất nổ ở đâu đó trước nó và ít nhất một chất nổ ở đâu đó sau nó."
date: "2026-08-07T21:12:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "D"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 82
verified: true
draft: false
---

[CF 102535D - Clingy Mo](https://codeforces.com/problemset/problem/102535/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có dòng thuốc nổ được biểu diễn bằng một mảng`E`. Một vụ nổ ở vị trí`i`chỉ có thể kích hoạt khi có ít nhất một chất nổ ở đâu đó trước nó và ít nhất một chất nổ ở đâu đó sau nó. Nhiệm vụ là thêm xếp hạng của chính xác những chất nổ thỏa mãn điều kiện này. 

Đầu vào đưa ra số lượng chất nổ theo sau là xếp hạng của chúng từ trái sang phải. Đầu ra là tổng công suất được đóng góp bởi tất cả các vị trí không nằm ở hai đầu tuyến và được bao quanh bởi các chất nổ khác. 

Ràng buộc`n <= 100`là rất nhỏ. Mô phỏng trực tiếp hoặc kiểm tra lặp đi lặp lại đã đủ nhanh vì ngay cả một`O(n^2)`giải pháp sẽ chỉ thực hiện khoảng mười nghìn hoạt động. Tuy nhiên, phần thú vị của vấn đề là nhận ra tình trạng thay vì phức tạp hóa nó. Giới hạn nhỏ cũng có nghĩa là chúng tôi không cần cấu trúc dữ liệu hoặc tối ưu hóa nâng cao. 

Xếp hạng có thể lớn tới 1000, do đó tổng cuối cùng có thể đạt khoảng 100000. Số nguyên Python xử lý việc này một cách dễ dàng và không cần xử lý tràn đặc biệt. 

Một sai lầm phổ biến là quên rằng quả nổ đầu tiên và cuối cùng không bao giờ có thể nổ được vì thiếu một bên. Ví dụ:```
Input:
3
5 8 2
```Đầu ra đúng là:```
8
```Chất nổ ở giữa có hàng xóm ở cả hai bên, trong khi hai đầu thì không. Việc triển khai bất cẩn chỉ kiểm tra xem mảng có nhiều phần tử hay không có thể bao gồm các điểm cuối không chính xác. 

Một trường hợp khác là đường dây có ít hơn ba quả nổ:```
Input:
2
5 10
```Đầu ra đúng là:```
0
```Không chất nổ nào có cả mặt trái và mặt phải. Việc triển khai lặp lại không chính xác trên toàn bộ mảng và truy cập các hàng xóm không tồn tại có thể bị lỗi hoặc vô tình đếm điểm cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là kiểm tra từng chất nổ và hỏi xem liệu có thứ gì đó ở cả hai bên hay không. Đối với mỗi vị trí, chúng ta có thể quét các phần tử trước và sau vị trí đó. Nếu cả hai lần quét tìm thấy ít nhất một chất nổ, chúng tôi sẽ thêm xếp hạng của nó vào câu trả lời. Điều này đúng vì điều kiện cho một vụ nổ chính xác là sự tồn tại của hàng xóm bên trái và bên phải ở đâu đó trong mảng. 

Vấn đề với phương pháp này là nó lặp lại công việc. Khi kiểm tra nhiều vị trí, thông tin tương tự về bên trái và bên phải được phát hiện lại nhiều lần. Trong trường hợp xấu nhất, mọi vị trí đều thực hiện quét gần như toàn bộ mảng, đưa ra kết quả gần đúng.`n * n`hoạt động. 

Quan sát quan trọng là điều kiện này hoàn toàn không phụ thuộc vào giá trị của các chất nổ khác. Nó chỉ phụ thuộc vào vị trí. Mọi vị trí ngoại trừ vị trí đầu tiên và cuối cùng đều tự động có ít nhất một phần tử ở bên trái và ít nhất một phần tử ở bên phải. Điều này biến vấn đề từ việc kiểm tra nhiều lần một điều kiện thành việc đơn giản tính tổng phần giữa của mảng. 

Phương pháp vũ lực hoạt động vì nó trực tiếp kiểm tra quy tắc cho mọi chất nổ, nhưng nó thất bại do phải mất thời gian khám phá lại thông tin vị trí tương tự. Việc quan sát thấy rằng chỉ các điểm cuối là không hợp lệ sẽ giảm vấn đề xuống còn một lần vượt qua phạm vi hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Được chấp nhận cho những ràng buộc này, nhưng không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng chất nổ và xếp hạng của chúng. Thứ tự mảng rất quan trọng vì điều kiện phụ thuộc vào vị trí của chất nổ. 
2. Khởi tạo câu trả lời về 0. Chúng tôi chỉ cần một khoản tiền hiện có vì mỗi chất nổ được xem xét độc lập. 
3. Lặp lại các vị trí từ chỉ mục`1`lập chỉ mục`n - 2`. Đây chính xác là những vị trí có ít nhất một phần tử trước chúng và ít nhất một phần tử sau chúng. 
4. Thêm xếp hạng ở từng vị trí đã ghé thăm vào câu trả lời. Mỗi vị trí được ghé thăm đều thỏa mãn điều kiện nổ chỉ bằng vị trí của nó. 
5. In số tiền tích lũy. 

Tại sao nó hoạt động: 

Vị trí duy nhất không đạt yêu cầu là hai đầu của mảng. Vị trí đầu tiên không có chất nổ ở bên trái và vị trí cuối cùng không có chất nổ ở bên phải. Mỗi vị trí khác có ít nhất một chỉ số nhỏ hơn và ít nhất một chỉ số lớn hơn, vì vậy mọi phần tử ở giữa phải đóng góp xếp hạng của nó. Thuật toán tính tổng chính xác các phần tử ở giữa này và loại trừ chính xác các điểm cuối không hợp lệ, điều này chứng tỏ kết quả là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    e = list(map(int, input().split()))

    ans = 0
    for i in range(1, n - 1):
        ans += e[i]

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ trong danh sách vì chúng ta cần truy cập trực tiếp vào các vị trí. Vòng lặp bắt đầu lúc`1`thay vì`0`vì quả nổ đầu tiên không thể nổ được. Nó dừng lại trước`n - 1`vì chất nổ cuối cùng cũng không thể nổ được. 

Khi`n`là`1`hoặc`2`, phạm vi`range(1, n - 1)`trống rỗng. Điều này đương nhiên đưa ra câu trả lời bằng 0 mà không yêu cầu điều kiện biên riêng. 

Mã chỉ lưu trữ mảng đầu vào và một bộ tích lũy số nguyên. Thứ tự tính tổng không tạo ra bất kỳ vấn đề tràn nào trong Python. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
6
1 3 5 2 3 10
```Giá trị đầu tiên và cuối cùng được loại trừ. Các giá trị ở giữa đều hợp lệ. 

| Chỉ mục | Đánh giá | Bao gồm? | Tổng hiện tại | 
| --- | --- | --- | --- | 
| 1 | 3 | Có | 3 | 
| 2 | 5 | Có | 8 | 
| 3 | 2 | Có | 10 | 
| 4 | 3 | Có | 13 | 

Câu trả lời cuối cùng là`13`. Dấu vết này cho thấy thuật toán bao gồm mọi vị trí có các phần tử ở cả hai phía. 

Đối với mẫu thứ hai:```
2
5 10
```Không có vị trí trung gian. 

| Chỉ mục | Đánh giá | Bao gồm? | Tổng hiện tại | 
| --- | --- | --- | --- | 
| Không có | Không có | Không có vị trí hợp lệ | 0 | 

Câu trả lời cuối cùng vẫn còn`0`. Điều này thể hiện trường hợp ranh giới trong đó toàn bộ mảng chỉ bao gồm các điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vụ nổ ở giữa được truy cập một lần | 
| Không gian | O(1) không bao gồm lưu trữ đầu vào | Chỉ biến trả lời được sử dụng trong quá trình xử lý | 

Với`n <= 100`, giải pháp tuyến tính dễ dàng trong thời gian giới hạn. Ngay cả phương pháp bậc hai kém hiệu quả hơn cũng có thể vượt qua, nhưng cách tiếp cận tuyến tính phản ánh trực tiếp cấu trúc của điều kiện và tránh những kiểm tra không cần thiết. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    lines = data.strip().split()
    if not lines:
        return ""
    n = int(lines[0])
    e = list(map(int, lines[1:]))

    ans = 0
    for i in range(1, n - 1):
        ans += e[i]
    return str(ans)

def run(inp: str) -> str:
    return solve(inp)

assert run("6\n1 3 5 2 3 10\n") == "13", "sample 1"
assert run("2\n5 10\n") == "0", "sample 2"

assert run("1\n7\n") == "0", "single explosive has no sides"
assert run("3\n4 9 6\n") == "9", "only middle explosive counts"
assert run("5\n1 1 1 1 1\n") == "3", "all equal values"
assert run("100\n" + " ".join(["1000"] * 100) + "\n") == "98000", "maximum size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`nổ |`0`| Kích thước tối thiểu và phạm vi lặp trống | 
|`3`chất nổ | Giá trị trung bình | Chỉ tồn tại một vị trí hợp lệ | 
| Năm xếp hạng bằng nhau | Tổng ba giá trị ở giữa | Xác nhận loại trừ điểm cuối | 
| Một trăm xếp hạng tối đa |`98000`| Kích thước đầu vào tối đa và số tiền lớn | 

## Vỏ cạnh 

Đối với trường hợp điểm cuối:```
Input:
3
5 8 2
```Vòng lặp bắt đầu tại chỉ mục`1`và kết thúc tại chỉ mục`1`, vậy chỉ có giá trị`8`được thêm vào. Thuật toán không bao giờ xem xét chỉ số`0`hoặc chỉ mục`2`, phù hợp với quy tắc điểm cuối không thể phát nổ. Đầu ra là`8`. 

Đối với trường hợp hai phần tử:```
Input:
2
5 10
```Vòng lặp trở thành`range(1, 1)`, không chứa vị trí nào. Câu trả lời vẫn là 0, xử lý chính xác tình huống không có chất nổ nào có sẵn cả hai bên. 

Đối với một loại thuốc nổ:```
Input:
1
100
```Không có vị trí trung gian có thể. Vòng lặp không thực thi và đầu ra là`0`. Điều này ngăn ngừa lỗi xảy ra trong đó phần tử duy nhất có thể được tính mặc dù không có hàng xóm.
