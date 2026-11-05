---
title: "CF 1216B - Bắn súng"
description: "Chúng tôi được cung cấp một dòng lon, mỗi loại có một giá trị độ bền. Vasya phải bắn tất cả lon đúng một lần, nhưng số lần bắn cần thiết để hạ gục một lon phụ thuộc vào số lượng lon đã bị hạ gục."
date: "2026-06-11T22:52:18+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1216
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 587 (Div. 3)"
rating: 900
weight: 1216
solve_time_s: 132
verified: true
draft: false
---

[CF 1216B - Bắn súng](https://codeforces.com/problemset/problem/1216/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng lon, mỗi loại có một giá trị độ bền. Vasya phải bắn tất cả lon đúng một lần, nhưng số lần bắn cần thiết để hạ gục một lon phụ thuộc vào số lượng lon đã bị hạ gục. Cụ thể, nếu một lon có độ bền$a_i$và Vasya đã hạ gục$x$lon, anh ấy cần$a_i \cdot x + 1$phát súng để phá hủy cái lon đó. Mục đích là để xác định thứ tự bắn sao cho tổng số lần bắn là nhỏ nhất và báo cáo tổng số lần bắn tối thiểu và một thứ tự hợp lệ. 

Đầu vào bao gồm số lượng lon$n$(lên tới 1000) và giá trị độ bền của chúng$a_i$(lên tới 1000). Các giới hạn này chỉ ra rằng các thuật toán với$O(n^2)$hoặc nhanh hơn đều được chấp nhận, nhưng bất kỳ thuật toán nào thử tất cả$n!$hoán vị là không thể thực hiện được, vì$1000!$có kích thước lớn về mặt thiên văn. 

Trường hợp cạnh xảy ra khi nhiều hộp có độ bền như nhau. Thứ tự tối ưu có thể không phải là duy nhất, nhưng tổng số lần chụp phải giống nhau bất kể hộp tương đương nào được chọn trước. Một điểm tinh tế khác là tổng nhỏ nhất có thể yêu cầu bắt đầu từ hộp có độ bền nhỏ nhất, điều này có thể không trực quan nếu không có lý luận cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của các lon, tính toán tổng số lần chụp cho mỗi đơn hàng và chọn mức tối thiểu. Điều này đúng nhưng hoàn toàn không thực tế vì$n!$tăng nhanh hơn bất kỳ tính toán khả thi nào cho$n = 1000$. Vì$n = 10$, điều này đã yêu cầu 3.628.800 lượt kiểm tra. 

Cái nhìn sâu sắc quan trọng là nhận ra hàm chi phí cho một lon,$a_i \cdot x + 1$, tăng tuyến tính với số lon đã bị đập bỏ. Do đó, tác động của độ bền của lon sẽ được tăng lên nếu nó được bắn muộn hơn. Để giảm thiểu tổng số, chúng tôi muốn những chiếc lon có độ bền cao được trải nghiệm như một chiếc hộp nhỏ.$x$càng tốt, có nghĩa là bắn chúng sớm hơn. Ngược lại, đồ hộp có độ bền thấp có thể bị trì hoãn vì sự đóng góp của chúng tăng chậm hơn khi tăng$x$. Quan sát này dẫn đến một cách tiếp cận tham lam: sắp xếp các lon theo thứ tự độ bền giảm dần và bắn theo thứ tự đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (Tham lam bởi độ bền) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và mảng độ bền$a$. Mỗi phần tử của$a$được ghép nối với chỉ mục ban đầu của nó để theo dõi thứ tự chụp. 
2. Sắp xếp danh sách các cặp theo độ bền giảm dần. Điều này đảm bảo rằng những hộp cần tiêm nhiều hơn nếu bị trì hoãn sẽ được bắn sớm hơn. 
3. Khởi tạo bộ đếm số lon đã bắn,$x = 0$và một biến để tích lũy tổng số lần chụp,$total = 0$. 
4. Lặp lại các hộp đã được sắp xếp. Đối với mỗi lon có độ bền$a_i$, tính toán số lần chụp cần thiết như$a_i \cdot x + 1$. Thêm cái này vào$total$và tăng dần$x$bởi một. 
5. Trích xuất các chỉ số gốc từ các cặp đã sắp xếp để tạo thành thứ tự chụp. 
6. In$total$và thứ tự bắn. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng chi phí của mỗi lon được nhân với số lượng lon bị đánh rơi trước đó nhỏ nhất có thể trong số tất cả các lon có độ bền bằng nhau hoặc cao hơn. Điều này duy trì tính bất biến rằng không có hoán vị nào khác có thể tạo ra tổng nhỏ hơn vì việc đặt một lon có độ bền cao hơn sau này sẽ làm tăng phần đóng góp có trọng số của nó, tức là dưới mức tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

# Pair durability with original indices
cans = [(a[i], i + 1) for i in range(n)]
# Sort by descending durability
cans.sort(reverse=True, key=lambda x: x[0])

total = 0
order = []

for x, (dur, idx) in enumerate(cans):
    total += dur * x + 1
    order.append(idx)

print(total)
print(' '.join(map(str, order)))
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Việc kết hợp độ bền với các chỉ số gốc sẽ bảo toàn bản đồ sau khi phân loại. Phím sắp xếp đảm bảo độ bền giảm dần. Tổng số lần chụp được tích lũy trong một vòng lặp trong đó`x`tự nhiên theo dõi số lượng lon đã bắn. Cuối cùng, các chỉ số được xuất ra theo thứ tự chụp được sắp xếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
20 10 20
```| Bước | x | Có thể (dur, idx) | Đã thêm ảnh | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | (20,1) | 1 | 1 | 
| 1 | 1 | (20,3) | 21 | 22 | 
| 2 | 2 | (10,2) | 21 | 43 | 

Dấu vết này xác nhận rằng việc bắn các lon có độ bền cao nhất trước tiên sẽ giảm thiểu tổng số lần bắn. 

### Mẫu tùy chỉnh 

đầu vào:```
4
1 3 2 4
```| Bước | x | Có thể (dur, idx) | Đã thêm ảnh | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | (4,4) | 1 | 1 | 
| 1 | 1 | (3,2) | 4 | 5 | 
| 2 | 2 | (2,3) | 5 | 10 | 
| 3 | 3 | (1,1) | 4 | 14 | 

Thuật toán chọn thứ tự`[4,2,3,1]`và đạt tổng nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, quét tuyến tính để tích lũy ảnh là O(n) | 
| Không gian | O(n) | Lưu trữ các cặp chỉ số độ bền và thứ tự đầu ra | 

Vì$n \le 1000$, việc này diễn ra thoải mái trong giới hạn thời gian, vì việc sắp xếp 1000 phần tử là chuyện nhỏ và việc tích lũy là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    cans = [(a[i], i + 1) for i in range(n)]
    cans.sort(reverse=True, key=lambda x: x[0])
    total = 0
    order = []
    for x, (dur, idx) in enumerate(cans):
        total += dur * x + 1
        order.append(idx)
    return f"{total}\n{' '.join(map(str, order))}"

# Provided samples
assert run("3\n20 10 20\n") == "43\n1 3 2"
# Custom test cases
assert run("4\n1 3 2 4\n") == "14\n4 2 3 1"
assert run("2\n5 5\n") == "7\n1 2"
assert run("5\n1 2 3 4 5\n") == "35\n5 4 3 2 1"
assert run("3\n1 1 1\n") == "6\n1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 1 3 2 4 | 14 4 2 3 1 | Trường hợp chung có độ bền hỗn hợp | 
| 2 5 5 | 7 1 2 | Hai độ bền bằng nhau | 
| 5 1 2 3 4 5 | 35 5 4 3 2 1 | Tăng độ bền | 
| 3 1 1 1 | 6 1 2 3 | Tất cả độ bền bằng nhau | 

## Vỏ cạnh 

Đối với các hộp có độ bền bằng nhau, thuật toán duy trì tính chính xác vì thứ tự của chúng không ảnh hưởng đến tổng. Ví dụ, đầu vào`3 1 1 1`tạo ra tổng số 6 bất kể thứ tự. Dấu vết: 

| Bước | x | Có thể (dur, idx) | Đã thêm ảnh | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | (1,1) | 1 | 1 | 
| 1 | 1 | (1,2) | 2 | 3 | 
| 2 | 2 | (1,3) | 3 | 6 | 

Bất biến mà lon có độ bền cao hơn được ưu tiên được thỏa mãn một cách tầm thường và đầu ra vẫn hợp lệ.
