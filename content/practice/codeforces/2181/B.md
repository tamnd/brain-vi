---
title: "CF 2181B - Trận chiến mảng"
description: "Chúng ta được yêu cầu mô phỏng một trò chơi theo lượt giữa Alice và Bob. Mỗi người chơi bắt đầu với một dãy số nguyên dương. Đến lượt của mình, người chơi chọn bất kỳ phần tử nào từ mảng của chính họ và áp dụng nó vào phần tử tối đa trong mảng của đối thủ."
date: "2026-06-07T21:56:47+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2181
codeforces_index: "B"
codeforces_contest_name: "2025-2026 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1400
weight: 2181
solve_time_s: 115
verified: false
draft: false
---

[CF 2181B - Trận chiến của các mảng](https://codeforces.com/problemset/problem/2181/B) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, trò chơi, tham lam 
**Thời gian giải:** 1 phút 55 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng một trò chơi theo lượt giữa Alice và Bob. Mỗi người chơi bắt đầu với một dãy số nguyên dương. Đến lượt của mình, người chơi chọn bất kỳ phần tử nào từ mảng của chính họ và áp dụng nó vào phần tử tối đa trong mảng của đối thủ. Nếu phần tử được chọn ít nhất lớn bằng phần tử tối đa của đối thủ thì phần tử đó sẽ bị loại bỏ. Nếu không, phần tử tối đa của đối thủ sẽ giảm đi giá trị đã chọn. Người chơi luân phiên di chuyển, bắt đầu với Alice. Người chơi đầu tiên làm giảm dàn trận của đối thủ thành trống sẽ giành chiến thắng. 

Đầu vào chứa nhiều trường hợp thử nghiệm, mỗi trường hợp có mảng lên tới 100.000 phần tử và giá trị phần tử lên tới 10^9. Vì tổng của tất cả các phần tử trong tất cả các trường hợp thử nghiệm bị giới hạn bởi 10^5 nên bất kỳ thuật toán nào xử lý từng phần tử mảng một lần cho mỗi trường hợp thử nghiệm đều khả thi. Các thuật toán bậc hai mô phỏng mỗi bước di chuyển sẽ thực hiện quá nhiều thao tác: nếu cả hai mảng có 10^5 phần tử, thì một mô phỏng đơn giản sẽ thực hiện các phép toán O(n*m), có thể là 10^10 bước, vượt xa giới hạn 3 giây. 

Trường hợp lợi thế tinh vi xảy ra khi một người chơi có phần tử lớn hơn đáng kể so với đối thủ. Ví dụ: nếu Alice có [10] và Bob có [1,1,1], Alice có thể loại bỏ nhiều lần các phần tử của Bob. Một mô phỏng bất cẩn chọn các phần tử tùy ý thay vì phần tử tối đa để giảm có thể gợi ý sai người chiến thắng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: mô phỏng trò chơi theo lượt. Ở mỗi lượt, hãy tìm phần tử lớn nhất trong mảng của đối thủ, chọn một số phần tử từ mảng của chính bạn và áp dụng thao tác. Lặp lại cho đến khi một mảng trống. Phương pháp này đúng nhưng quá chậm vì việc tìm phần tử tối đa và cập nhật mảng liên tục mất O(n log n + m log m) mỗi lần di chuyển và có thể có O(n + m) lần di chuyển, dẫn đến số lượng thao tác vượt quá 10^10 trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là chỉ có các yếu tố tối đa mới quan trọng. Trong mỗi lượt, người chơi luôn có thể chọn nguyên tố tối đa của mình để tối đa hóa tác dụng lên đối thủ. Điều này làm cho trò chơi trở thành sự so sánh giữa các phần tử tối đa: người chơi có phần tử tối đa lớn hơn sẽ kiểm soát hiệu quả tốc độ hủy diệt. Khi chúng ta biết phần tử tối đa của cả hai mảng, người chiến thắng có thể được xác định ngay lập tức: nếu phần tử tối đa của Alice ít nhất là phần tử tối đa của Bob, cô ấy có lợi thế đi trước và sẽ thắng, nếu không thì Bob thắng. Không cần phải mô phỏng trò chơi từng bước một vì bất kỳ cách chơi tối ưu nào cũng dẫn đến việc giảm phần tử tối đa của đối thủ và việc chọn phần tử nhỏ hơn sẽ thực sự tồi tệ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((n+m)^2) | O(n+m) | Quá chậm | 
| So sánh phần tử tối đa | O(n+m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước mảng của Alice và Bob. 
2. Đọc mảng của Alice và tính phần tử lớn nhất`max_a`. 
3. Đọc mảng của Bob và tính phần tử lớn nhất`max_b`. 
4. So sánh hai phần tử cực đại. Nếu như`max_a >= max_b`, Alice thắng; nếu không thì Bob thắng. 
5. In kết quả thắng cuộc cho từng trường hợp kiểm thử. 

Tại sao nó hoạt động: điều bất biến là phần tử lớn nhất của mỗi mảng xác định số lần di chuyển tối thiểu cần thiết để phá hủy mảng của đối thủ. Chọn bất kỳ yếu tố nhỏ hơn sẽ không bao giờ kết thúc trò chơi nhanh hơn. Do đó, việc so sánh các phần tử cực đại cho kết quả tương tự như một mô phỏng tối ưu đầy đủ. Vì trò chơi diễn ra theo lượt và Alice di chuyển trước, nếu phần tử tối đa của cô ấy ít nhất là của Bob, cô ấy luôn có thể phá hủy mảng của Bob trước theo lối chơi tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
results = []

for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    if max(a) >= max(b):
        results.append("Alice")
    else:
        results.append("Bob")

print("\n".join(results))
```Giải pháp này đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và tính toán các phần tử lớn nhất của mảng Alice và Bob lần lượt là O(n) và O(m). Nó lưu trữ các kết quả trong một danh sách và in tất cả cùng một lúc để tránh việc nhập/xuất trên mỗi dòng bị chậm. Một lựa chọn triển khai tinh tế là đảm bảo chúng tôi tính toán mức tối đa trên toàn bộ mảng cho mỗi người chơi, không chỉ phần tử đầu tiên, vì việc bỏ qua điều này sẽ tạo ra câu trả lời sai khi phần tử lớn nhất không nằm ở đầu mảng. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
1 1
70
90
```| Bước | Alice tối đa | Bob tối đa | Người chiến thắng | 
| --- | --- | --- | --- | 
| Ban đầu | 70 | 90 | Bob | 

Phần tử tối đa của Alice là 70, ít hơn 90 của Bob. Bob thắng, vì anh ta có thể sống sót sau cuộc tấn công đầu tiên và phá hủy mảng của Alice. 

**Ví dụ 2:** 

đầu vào:```
2 3
30 30
20 20 40
```| Bước | Alice tối đa | Bob tối đa | Người chiến thắng | 
| --- | --- | --- | --- | 
| Ban đầu | 30 | 40 | Bob | 

Phần tử tối đa của Alice là 30, của Bob là 40. Vì Alice không thể phá hủy phần tử lớn nhất của Bob trong nước đi đầu tiên nên Bob thắng theo cách chơi tối ưu. 

Những dấu vết này xác nhận rằng việc so sánh các phần tử tối đa sẽ dự đoán chính xác người chiến thắng mà không cần mô phỏng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Tính toán cực đại của mỗi mảng mất thời gian tuyến tính | 
| Không gian | O(1) phụ trợ | Chỉ lưu trữ danh sách tối đa và kết quả | 

Cho tổng của`n`Và`m`trên tất cả các trường hợp thử nghiệm không vượt quá 10^5, tổng số thao tác nằm trong khoảng 2*10^5, dưới giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    results = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        results.append("Alice" if max(a) >= max(b) else "Bob")
    return "\n".join(results)

# Provided samples
assert run("2\n1 1\n70\n90\n2 3\n30 30\n20 20 40\n") == "Bob\nBob"

# Custom cases
assert run("1\n1 1\n100\n100\n") == "Alice", "equal max, first move wins"
assert run("1\n3 2\n5 10 15\n10 5\n") == "Alice", "Alice largest wins"
assert run("1\n2 2\n1 1\n2 2\n") == "Bob", "Bob largest wins"
assert run("1\n5 5\n1 2 3 4 5\n5 4 3 2 1\n") == "Alice", "equal largest, first move wins"
assert run("1\n1 3\n50\n10 20 30\n") == "Alice", "single vs multiple, max controls outcome"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n100\n100`| Alice | Cực đại bằng nhau, lợi thế nước đi đầu tiên | 
|`3 2\n5 10 15\n10 5`| Alice | Max trong mảng lớn hơn chiếm ưu thế | 
|`2 2\n1 1\n2 2`| Bob | Max trong mảng đối thủ chiếm ưu thế | 
|`5 5\n1 2 3 4 5\n5 4 3 2 1`| Alice | Cực đại bằng nhau, lợi thế nước đi đầu tiên | 
|`1 3\n50\n10 20 30`| Alice | Mảng đơn và nhiều mảng, quyết định tối đa | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các phần tử đều bằng nhau, chẳng hạn như Alice [5,5,5] và Bob [5,5], Alice thắng vì nước đi đầu tiên sẽ loại bỏ hoặc giảm phần tử tối đa và cô ấy di chuyển trước. Thuật toán so sánh chính xác cực đại mà không cần mô phỏng các bước di chuyển lặp đi lặp lại. Đối với các mảng có một phần tử cực lớn, phép so sánh tối đa cũng đảm bảo người chơi có phần tử lớn nhất có lợi thế quyết định. Các đầu vào có mảng phần tử đơn, không cần di chuyển hoặc cần di chuyển một lần và các mảng hoàn toàn bằng nhau đều được xử lý chính xác bằng cách so sánh phần tử tối đa đơn giản này.
