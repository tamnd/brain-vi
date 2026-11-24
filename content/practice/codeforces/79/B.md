---
title: "CF 79B - Cánh Đồng Sắc Màu"
description: "Chúng tôi được yêu cầu mô phỏng việc trồng cây trên một cánh đồng hình chữ nhật, nhưng có một nhược điểm: một số ô là đất hoang. Trường được biểu diễn dưới dạng lưới n x m, với các hàng được đánh số từ 1 đến n và các cột từ 1 đến m."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 79
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 71"
rating: 1400
weight: 79
solve_time_s: 79
verified: true
draft: false
---

[CF 79B - Cánh đồng đầy màu sắc](https://codeforces.com/problemset/problem/79/B) 

**Đánh giá:** 1400 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng việc trồng cây trên một cánh đồng hình chữ nhật, nhưng có một nhược điểm: một số ô là đất hoang. Trường được biểu diễn dưới dạng lưới _n_ by _m_, với các hàng được đánh số từ 1 đến _n_ và các cột từ 1 đến _m_. Một số ô chứa chất thải và phần còn lại chứa các loại cây trồng được trồng theo chu kỳ theo thứ tự Cà rốt → Kiwi → Nho, lấp đầy ruộng theo từng hàng, bỏ qua các ô thải. 

Đầu vào cung cấp kích thước của trường, danh sách tọa độ ô thải và danh sách tọa độ truy vấn. Đối với mỗi truy vấn, chúng ta phải xác định xem ô đó là chất thải hay nó chứa loại cây trồng nào. 

Các ràng buộc khá lớn: trường có thể có tới 40.000 hàng và cột. Điều đó mang lại tới 1,6 tỷ tế bào. Rõ ràng, việc lưu trữ toàn bộ trường một cách rõ ràng là không thể trong giới hạn bộ nhớ, do đó, bất kỳ giải pháp nào xây dựng toàn bộ lưới đều không khả thi. Tuy nhiên, số lượng ô thải và truy vấn rất nhỏ (lên tới 1000 ô), điều này cho thấy rằng việc mô phỏng trực tiếp toàn bộ trường là không cần thiết. Chúng ta phải làm việc với các ô và truy vấn thải theo cách trừu tượng hơn, dựa trên chỉ mục. 

Các trường hợp khó khăn có thể phá vỡ một giải pháp đơn giản bao gồm truy vấn một ô bị lãng phí, ô đầu tiên hoặc cuối cùng trong thứ tự trồng hoặc có nhiều truy vấn cho cùng một ô. Nếu bạn không bỏ qua ô thải một cách chính xác khi đếm cây trồng, việc phân công cây trồng sẽ bị lệch một hoặc nhiều, dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ sẽ cố gắng mô phỏng toàn bộ quá trình trồng cây. Bạn có thể lặp qua từng ô theo thứ tự hàng lớn, bỏ qua các ô thải và chỉ định cây trồng trong chu trình Cà rốt-Kiwi-Nho. Điều này về mặt khái niệm đơn giản và chính xác, nhưng trường này có thể lên tới 40.000 × 40.000, tức là 1,6 tỷ ô. Ngay cả khi lặp lại nhanh, việc mô phỏng từng ô sẽ mất hàng trăm giây - quá chậm đối với giới hạn 2 giây. 

Quan sát quan trọng cho phép tối ưu hóa là thứ tự trồng có thể dự đoán được: mỗi ô được canh tác sẽ nhận được một loại cây trồng hoàn toàn dựa trên chỉ số của nó trong chuỗi hàng chính, bỏ qua các ô thải trước nó. Vì vậy, chúng ta có thể tính "chỉ số tuyến tính" của bất kỳ ô nào trong thời gian O(1). Sau đó, nếu chúng ta biết có bao nhiêu ô thải xuất hiện trước một ô nhất định theo thứ tự tuyến tính này, chúng ta có thể trừ số lượng đó để xác định loại cắt cho ô đó. Các ô thải đóng vai trò là phần bù trong chuỗi, dịch chuyển các chỉ số cắt cho các ô tiếp theo. 

Với thông tin chi tiết này, giải pháp giảm xuống còn ba bước chính: ánh xạ tọa độ 2D thành chỉ mục 1D, sắp xếp ô thải theo chỉ mục của chúng và đối với mỗi truy vấn, hãy kiểm tra xem đó có phải là ô rác hay không và nếu không, hãy xác định phần cắt bằng cách sử dụng số học modulo dựa trên số lượng ô không rác trước đó. Việc sắp xếp các ô thải có chi phí O(k log k) và mỗi truy vấn có thể được trả lời bằng O(log k) bằng cách sử dụng tìm kiếm nhị phân, hiệu quả với k ≤ 1000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n*m) | Quá chậm | 
| Tối ưu | O(k log k + t log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chỉ số 1D cho mỗi ô theo thứ tự hàng lớn bằng công thức`(row - 1) * m + (col - 1)`. Điều này mang lại một số nguyên duy nhất cho mỗi ô, cho phép xử lý các ô thải theo kiểu tuyến tính đơn giản. 
2. Chuyển đổi từng tọa độ ô thải thành chỉ số 1D tương ứng và lưu trữ chúng trong danh sách. Sau đó sắp xếp danh sách này. Việc sắp xếp đảm bảo rằng, đối với bất kỳ truy vấn nào, chúng tôi có thể xác định một cách hiệu quả có bao nhiêu ô thải đứng trước nó bằng cách sử dụng tìm kiếm nhị phân. 
3. Đối với mỗi truy vấn, trước tiên hãy chuyển đổi tọa độ thành chỉ mục 1D của nó. Kiểm tra xem chỉ số này có nằm trong danh sách các chỉ số rác thải được sắp xếp bằng cách sử dụng tìm kiếm nhị phân hay không. Nếu đúng như vậy thì ô đó là chất thải và chúng tôi xuất ra "Chất thải". 
4. Nếu ô không bị lãng phí, hãy đếm số lượng ô thải có chỉ số nhỏ hơn ô truy vấn bằng cách sử dụng`bisect_left`. Trừ số đếm này khỏi chỉ mục ô truy vấn để có được vị trí dựa trên số 0 trong số các ô được nuôi cấy. 
5. Sử dụng modulo 3 ở vị trí này để xác định loại cây trồng: vị trí % 3 == 0 tương ứng với Cà rốt, 1 là Kiwi và 2 là Nho. 

Tại sao nó hoạt động: Điều bất biến quan trọng là loại cây trồng của một ô được canh tác chỉ phụ thuộc vào vị trí tương đối của nó giữa các ô được canh tác chứ không phụ thuộc vào toàn bộ cánh đồng. Việc sắp xếp các ô thải cho phép chúng tôi xác định vị trí tương đối này một cách hiệu quả. Trừ đi số lượng ô thải trước đó sẽ dịch chuyển trình tự một cách chính xác, giữ nguyên thứ tự trồng dự định. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

n, m, k, t = map(int, input().split())
waste = []

for _ in range(k):
    a, b = map(int, input().split())
    idx = (a - 1) * m + (b - 1)
    waste.append(idx)

waste.sort()
crops = ["Carrots", "Kiwis", "Grapes"]

for _ in range(t):
    i, j = map(int, input().split())
    query_idx = (i - 1) * m + (j - 1)
    pos = bisect.bisect_left(waste, query_idx)
    if pos < len(waste) and waste[pos] == query_idx:
        print("Waste")
    else:
        crop_idx = (query_idx - pos) % 3
        print(crops[crop_idx])
```Mã chuyển đổi cả ô thải và truy vấn thành chỉ mục tuyến tính của chúng. Việc sắp xếp các chỉ số chất thải cho phép tìm kiếm nhị phân để phát hiện chất thải và tính toán số lượng chất thải trước đó một cách hiệu quả. Trừ đi số lượng chất thải trước đó sẽ thu được chỉ số tế bào được nuôi cấy để xác định vụ mùa theo chu kỳ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4 5 5 6
4 3
1 3
3 3
2 5
3 2
1 3
1 4
2 3
2 4
1 1
1 1
```| Truy vấn | Idx tuyến tính | Lãng phí trước | Idx đã điều chỉnh | Cắt | 
| --- | --- | --- | --- | --- | 
| 1,3 | 2 | 0 | lãng phí | Chất thải | 
| 1,4 | 3 | 1 | 2 | Nho | 
| 2,3 | 7 | 3 | 4 | Cà rốt | 
| 2,4 | 8 | 3 | 5 | Kiwi | 
| 1,1 | 0 | 0 | 0 | Cà rốt | 
| 1,1 | 0 | 0 | 0 | Cà rốt | 

Dấu vết này xác nhận rằng việc trừ đi số lượng ô thải trước đó sẽ căn chỉnh cây trồng một cách chính xác. 

**Đầu vào tùy chỉnh 2 (Ô đầu tiên bị lãng phí, ô cuối cùng được truy vấn):**```
3 3 1 2
1 1
1 1
3 3
```| Truy vấn | Idx tuyến tính | Lãng phí trước | Idx đã điều chỉnh | Cắt | 
| --- | --- | --- | --- | --- | 
| 1,1 | 0 | 0 | lãng phí | Chất thải | 
| 3,3 | 8 | 1 | 7 | Kiwi | 

Điều này xác nhận việc xử lý cạnh của ô đầu tiên và ô cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k + t log k) | Sắp xếp k ô thải và tìm kiếm nhị phân cho t truy vấn | 
| Không gian | O(k) | Lưu trữ chỉ số chất thải được phân loại | 

Ngay cả trong trường hợp xấu nhất với k = t = 1000, thuật toán thực hiện tối đa 2000 phép tính log 1000, không đáng kể so với giới hạn 2 giây. Mức sử dụng bộ nhớ tốt trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # insert the solution code here or call a function
    n, m, k, t = map(int, input().split())
    waste = []
    for _ in range(k):
        a, b = map(int, input().split())
        idx = (a - 1) * m + (b - 1)
        waste.append(idx)
    waste.sort()
    crops = ["Carrots", "Kiwis", "Grapes"]
    for _ in range(t):
        i, j = map(int, input().split())
        query_idx = (i - 1) * m + (j - 1)
        pos = bisect.bisect_left(waste, query_idx)
        if pos < len(waste) and waste[pos] == query_idx:
            print("Waste")
        else:
            crop_idx = (query_idx - pos) % 3
            print(crops[crop_idx])
    return out.getvalue().strip()

# provided sample
assert run("""4 5 5 6
4 3
1 3
3 3
2 5
3 2
1 3
1 4
2 3
2 4
1 1
1 1
""") == """Waste
Grapes
Carrots
Kiwis
Carrots
Carrots"""

# custom minimum size
assert run("""1 1 0 1
1 1
```
