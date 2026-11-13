---
title: "CF 1697B - Khuyến mãi"
description: "Chúng ta có một cửa hàng có n mặt hàng, mỗi mặt hàng có một mức giá cụ thể. Cửa hàng thực hiện chương trình khuyến mãi, trong đó, nếu khách hàng mua ít nhất x mặt hàng thì mặt hàng y rẻ nhất trong số đó sẽ miễn phí. Đối với mỗi truy vấn (x, y), chúng tôi muốn xác định tổng giá trị tối đa của các mặt hàng mà khách hàng có thể nhận miễn phí."
date: "2026-06-09T22:25:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1697
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 130 (Rated for Div. 2)"
rating: 900
weight: 1697
solve_time_s: 126
verified: true
draft: false
---

[CF 1697B - Khuyến mại](https://codeforces.com/problemset/problem/1697/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cửa hàng với`n`các mặt hàng, mỗi mặt hàng có một mức giá cụ thể. Cửa hàng thực hiện chương trình khuyến mãi, nếu khách hàng mua ít nhất`x`các mặt hàng, các`y`rẻ nhất trong số những mặt hàng đó là miễn phí. Đối với mỗi truy vấn`(x, y)`, chúng tôi muốn xác định tổng giá trị tối đa của các mặt hàng mà khách hàng có thể nhận được miễn phí. 

Đầu vào đưa ra giá mặt hàng dưới dạng một mảng`p`kích thước`n`và sau đó`q`các truy vấn độc lập. Mỗi truy vấn chỉ định các tham số khuyến mãi`(x, y)`và đầu ra là một số nguyên duy nhất cho mỗi truy vấn: tổng tối đa của giá trị rẻ nhất`y`các mục trong số bất kỳ lựa chọn nào của`x`mặt hàng. 

Những hạn chế chính đó là`n`Và`q`có thể lên tới 200.000 và giá vật phẩm có thể lên tới một triệu. Một cách tiếp cận ngây thơ lặp lại trên tất cả các tập hợp con có kích thước`x`là không thể vì số lượng tập hợp con tăng lên một cách tổ hợp. Thậm chí lặp đi lặp lại tất cả những gì có thể`x`-các lựa chọn mục rõ ràng sẽ vượt quá`10^10`hoạt động trong trường hợp xấu nhất. Chúng ta cần một giải pháp hoạt động trong thời gian gần tuyến tính đối với`n`, lý tưởng`O(n log n)`cộng thêm`O(1)`mỗi truy vấn. 

Các trường hợp khó khăn không rõ ràng bao gồm khi tất cả các mặt hàng có cùng mức giá, khi`x`bằng`n`buộc khách hàng phải mua mọi thứ, hoặc khi`y`bằng`x`vì vậy mọi mục được chọn đều miễn phí. Ví dụ, nếu giá là`[5, 5, 5]`và truy vấn là`(3, 3)`, giá trị tự do là`15`. Một cách tiếp cận bất cẩn luôn cố gắng chọn những mặt hàng rẻ nhất trên toàn cầu có thể tính sai số tiền cho những mặt hàng khác.`x`các giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các tập hợp con có thể có kích thước`x`đối với một truy vấn, sắp xếp từng tập hợp con, lấy`y`những mặt hàng rẻ nhất và tính tổng giá của chúng. Điều này hoạt động hợp lý nhưng cực kỳ tốn kém. Ngay cả đối với mức độ vừa phải`n = 20`, có hơn 100.000 tập hợp con cần xem xét và`n = 2 * 10^5`là hoàn toàn khó chữa. 

Quan sát quan trọng là để tối đa hóa giá trị của`y`các mặt hàng miễn phí, chúng tôi muốn chọn`x`những mặt hàng đắt tiền nhất có thể, bởi vì`y`rẻ nhất trong số đó vẫn sẽ có giá trị cao hơn các mặt hàng rẻ hơn ngoài danh sách lựa chọn. Sắp xếp toàn bộ mảng theo thứ tự giảm dần cho phép chúng ta chọn mảng đầu tiên`x`các mặt hàng một cách nhanh chóng. Sau đó, trong số đó`x`các mặt hàng, các`y`rẻ nhất có thể được tính bằng cách sử dụng mảng tổng tiền tố. Điều này làm giảm vấn đề thành một thao tác sắp xếp đơn cộng với số học đơn giản cho mỗi truy vấn. 

Cái nhìn sâu sắc tham lam này là chính xác vì các vật phẩm miễn phí luôn được lấy từ những thứ đã chọn`x`, vì vậy việc tối đa hóa tổng số vật phẩm miễn phí tương đương với việc lấy vật phẩm đắt nhất`x`và tổng hợp của họ`y`rẻ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n chọn x * x log x) | O(x) mỗi truy vấn | Quá chậm | 
| Tối ưu | O(n log n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số mục`n`và số lượng truy vấn`q`. Đọc giá mặt hàng vào một mảng`p`. 
2. Sắp xếp`p`theo thứ tự giảm dần. Điều này đảm bảo điều đầu tiên`x`các yếu tố của`p`cho bất kỳ truy vấn nào`(x, y)`là sự lựa chọn đắt giá nhất. 
3. Xây dựng mảng tổng tiền tố`prefix`Ở đâu`prefix[i]`là tổng của số đầu tiên`i`các yếu tố của`p`. Điều này cho phép tính toán nhanh các khoản tiền trên bất kỳ phân đoạn liền kề nào. 
4. Đối với mỗi truy vấn`(x, y)`, cái`x`những món đồ đắt tiền nhất là món đầu tiên`x`các yếu tố của`p`. Trong số này,`y`rẻ nhất là cái cuối cùng`y`các phần tử trong phân khúc này. Tính tổng của chúng là`prefix[x] - prefix[x-y]`. 
5. Xuất tổng cho mỗi truy vấn. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng mọi`x`Lựa chọn có kích thước tối đa hóa tổng số vật phẩm miễn phí là lựa chọn đầu tiên`x`các phần tử. Tổng tiền tố cho phép tính toán theo thời gian không đổi của`y`rẻ nhất trong lựa chọn đó. Thuật toán không bao giờ chọn một tập hợp tối ưu vì di chuyển bất kỳ mục nào đắt tiền hơn ra ngoài tập hợp đầu tiên.`x`sẽ làm giảm tổng số vật phẩm miễn phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
p = list(map(int, input().split()))

p.sort(reverse=True)

# prefix[i] = sum of first i elements
prefix = [0] * (n + 1)
for i in range(n):
    prefix[i + 1] = prefix[i] + p[i]

for _ in range(q):
    x, y = map(int, input().split())
    # sum of y cheapest in first x items
    print(prefix[x] - prefix[x - y])
```Đầu tiên, mã sẽ sắp xếp giá theo thứ tự giảm dần để đảm bảo chúng ta luôn có thể chọn giá đắt nhất`x`mặt hàng. Mảng tổng tiền tố cho phép chúng ta tính tổng các phân đoạn liền kề trong thời gian không đổi. Điểm tinh tế là việc lập chỉ mục trong`prefix[x] - prefix[x - y]`:`prefix[x]`tổng hợp đầu tiên`x`các mục và trừ`prefix[x - y]`loại bỏ số tiền đắt hơn`x-y`các mục, chỉ để lại`y`rẻ nhất. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1** 

| Bước | Sắp xếp giá | Tiền tố | x | y | Tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| Truy vấn 1 | [5,5,3,2,1] | [0,5,10,13,15,16] | 3 | 2 | 13-5=8 | 
| Truy vấn 2 | [5,5,3,2,1] | giống nhau | 1 | 1 | 5-0=5 | 
| Truy vấn 3 | [5,5,3,2,1] | giống nhau | 5 | 3 | 16-10=6 | 

Điều này chứng tỏ rằng việc sắp xếp các tổng tiền tố cho phép chúng ta chọn`x`các mục và tính tổng`y`rẻ nhất ở O(1) cho mỗi truy vấn. 

**Ví dụ về trường hợp cạnh** 

Giá cả`[1,1,1]`, truy vấn`(3,3)`. Sắp xếp giá`[1,1,1]`, tiền tố`[0,1,2,3]`. Tổng 3 số cuối của 3 số đầu là`3-0=3`. Điều này xác nhận việc xử lý thuật toán`y=x`một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q) | Việc sắp xếp mất O(n log n), mỗi truy vấn là O(1) sử dụng tổng tiền tố | 
| Không gian | O(n) | Mảng tiền tố có kích thước n+1 | 

Đối với những hạn chế`n, q ≤ 2 * 10^5`, việc sắp xếp chiếm ưu thế và vừa vặn thoải mái trong vòng 2 giây. Việc sử dụng bộ nhớ là tuyến tính trong`n`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # or paste solution here
    return output.getvalue().strip()

# Provided samples
assert run("5 3\n5 3 1 5 2\n3 2\n1 1\n5 3\n") == "8\n5\n6", "sample 1"

# Minimum size input
assert run("1 1\n10\n1 1\n") == "10", "min size"

# All equal values
assert run("4 2\n5 5 5 5\n2 1\n4 2\n") == "5\n10", "all equal"

# Maximum x
assert run("5 1\n1 2 3 4 5\n5 2\n") == "3", "x=n"

# Random large case
import random
n = 10
prices = " ".join(str(random.randint(1,10)) for _ in range(n))
assert run(f"{n} 1\n{prices}\n{n} 3\n")  # just ensure no crash
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 1\n10\n1 1" | "10" | Đầu vào kích thước tối thiểu | 
| "4 2\n5 5 5 5\n2 1\n4 2" | "5\n10" | Tất cả các mức giá bằng nhau, nhiều truy vấn | 
| "5 1\n1 2 3 4 5\n5 2" | "3" | x bằng n | 
| Hộp 10 món ngẫu nhiên | khác nhau | Đầu vào ngẫu nhiên lớn, kiểm tra không có sự cố | 

## Vỏ cạnh 

Nếu tất cả các mức giá đều bằng nhau thì thuật toán vẫn tính tổng chính xác`y`rẻ nhất, vì`prefix[x] - prefix[x-y]`hoạt động bất kể sự bình đẳng. Vì`y=x`, tổng của tất cả đầu tiên`x`hàng được tính toán chính xác. Nếu như`y=1`, nó vẫn chọn cái cuối cùng của cái đầu tiên`x`các mục sau khi sắp xếp, đảm bảo tối thiểu phân khúc đó. Điều này xử lý tất cả các ranh giới tinh tế mà không cần vỏ đặc biệt.
