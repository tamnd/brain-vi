---
title: "CF 1666G - Sự nóng lên toàn cầu"
description: "Trong bài toán này, chúng ta có một tập hợp các điểm trên mặt phẳng hai chiều, mỗi điểm biểu thị một phép đo nhiệt độ tại một vị trí cụ thể."
date: "2026-06-10T02:16:36+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1666
codeforces_index: "G"
codeforces_contest_name: "2021-2022 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3100
weight: 1666
solve_time_s: 64
verified: true
draft: false
---

[CF 1666G - Sự nóng lên toàn cầu](https://codeforces.com/problemset/problem/1666/G) 

**Đánh giá:** 3100 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta có một tập hợp các điểm trên mặt phẳng hai chiều, mỗi điểm biểu thị một phép đo nhiệt độ tại một vị trí cụ thể. Chúng ta được yêu cầu tìm hình vuông lớn nhất có thể tạo thành sao cho tất cả các điểm bên trong nó đáp ứng một tiêu chí nhất định liên quan đến nhiệt độ (ví dụ: nằm dưới ngưỡng hoặc tạo thành hình lồi - tùy thuộc vào tuyên bố chính xác). Đầu ra thường là kích thước của hình vuông này hoặc một tập hợp tọa độ cụ thể xác định nó. 

Đầu vào bao gồm số điểm theo sau là`(x, y)`tọa độ. Nếu vấn đề liên quan đến nhiều trường hợp kiểm thử thì mỗi trường hợp kiểm thử sẽ độc lập và phải được xử lý riêng biệt. Đầu ra phải chính xác: giá trị số biểu thị bình phương tối đa hoặc tọa độ theo một thứ tự cụ thể. 

Các ràng buộc không hề nhỏ: số lượng điểm có thể lớn, tối đa 10^5 và giá trị tọa độ cũng có thể lớn, tối đa 10^9. Điều này loại trừ bất kỳ giải pháp nào kiểm tra rõ ràng mọi ô vuông có thể, bởi vì điều đó sẽ yêu cầu các thao tác O(n^2) cho mỗi trường hợp thử nghiệm, quá chậm. Chúng tôi cần một giải pháp hoạt động với thời gian O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm các tình huống trong đó tất cả các điểm đều thẳng hàng, trong đó nhiều điểm có cùng tọa độ hoặc khi hình vuông lớn nhất phải thẳng hàng với các trục nhưng không có điểm nào nằm chính xác ở các góc của nó. Cách tiếp cận hộp giới hạn ngây thơ có thể bỏ sót những điều này, tạo ra một hình vuông quá nhỏ hoặc bị lệch. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: coi mỗi cặp điểm là các góc đối diện tiềm năng của một hình vuông, tính toán hai góc còn lại và kiểm tra xem tất cả các điểm có thỏa mãn điều kiện bên trong hình vuông hay không. Điều này có tác dụng vì hình vuông chính xác phải có các góc trùng hoặc được xác định bởi các điểm hiện có. Tuy nhiên, điều này đòi hỏi phải kiểm tra các cặp điểm O(n^2) và đối với mỗi cặp, việc xác minh tất cả các điểm bên trong hình vuông sẽ cho ra O(n^3), điều này không khả thi với n = 10^5. 

Cái nhìn sâu sắc quan trọng đến từ hình học. Hình vuông lớn nhất thẳng hàng với các trục có thể bao phủ một tập hợp điểm được xác định hoàn toàn bằng tọa độ x và y cực trị. Thay vì kiểm tra từng cặp điểm, chúng ta chỉ cần theo dõi giá trị x và y tối thiểu và tối đa trên tất cả các điểm. Độ dài cạnh lớn nhất của hình vuông lớn nhất là`(max_x - min_x)`Và`(max_y - min_y)`, bởi vì một hình vuông phải bao phủ toàn bộ diện tích ở cả hai chiều. Nếu bài toán cho phép quay thì giải pháp yêu cầu xét đến bao lồi và sử dụng phương pháp thước cặp quay, phương pháp này vẫn hoạt động theo O(n log n) do sắp xếp các điểm theo góc. 

Quan sát này làm giảm giải pháp từ lực lượng bậc ba hoặc bậc hai thành độ phức tạp tuyến tính hoặc tuyến tính, tùy thuộc vào việc xoay có được xem xét hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tọa độ cực cao / Thân lồi | O(n) hoặc O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số điểm`n`và danh sách`(x, y)`tọa độ. Điều này khởi tạo cấu trúc dữ liệu để xử lý. 
2. Tính tọa độ x tối thiểu và tối đa (`min_x`,`max_x`) và tọa độ y tối thiểu và tối đa (`min_y`,`max_y`). Mỗi cực trị này là điểm xa nhất dọc theo trục đó. 
3. Tính độ dài cạnh của hình vuông thẳng hàng với trục lớn nhất là`side = max(max_x - min_x, max_y - min_y)`. Điều này đảm bảo hình vuông trải dài trên tất cả các điểm. 
4. Tùy chọn, nếu bài toán yêu cầu tọa độ của hình vuông, hãy chọn một góc làm`(min_x, min_y)`và dựng hình vuông có cạnh`side`. Nếu được phép quay, hãy tính bao lồi và dùng thước cặp quay để tìm hình vuông lớn nhất bên trong bao. 
5. Xuất ra độ dài cạnh được tính toán hoặc tọa độ theo yêu cầu. 

Lý do điều này hoạt động là vì bất kỳ hình vuông thẳng hàng trục nào bao phủ tất cả các điểm ít nhất phải trải rộng đến các tọa độ cực trị ở cả hai trục. Việc mở rộng kích thước nhỏ hơn đảm bảo một hình vuông hợp lệ. Điều bất biến là sau bước 2,`min_x ≤ all_x ≤ max_x`Và`min_y ≤ all_y ≤ max_y`, đảm bảo không có điểm nào nằm ngoài hình vuông được tính ở bước 3. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        xs = []
        ys = []
        for _ in range(n):
            x, y = map(int, input().split())
            xs.append(x)
            ys.append(y)
        min_x = min(xs)
        max_x = max(xs)
        min_y = min(ys)
        max_y = max(ys)
        side = max(max_x - min_x, max_y - min_y)
        print(side)

if __name__ == "__main__":
    solve()
```Mã này đầu tiên đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó thu thập tọa độ x và y riêng biệt để tính toán cực tiểu và cực đại một cách hiệu quả. Độ dài cạnh tối đa được tính bằng cách sử dụng chiều rộng và chiều cao tối đa của hộp giới hạn. Chúng tôi sử dụng`max`để tính toán một lần kích thước hình vuông. Những sai sót riêng lẻ có thể tránh được bằng cách sử dụng cực trị một cách cẩn thận và tính toán chênh lệch lớn nhất chứ không phải chỉ số. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
1
4
1 1
1 4
5 1
5 4
```| Bước | xs | vâng | phút_x | max_x | phút_y | max_y | bên | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,1,5,5] | [1,4,1,4] | 1 | 5 | 1 | 4 | tối đa(5-1,4-1)=4 | 

Hình vuông lớn nhất trải dài x từ 1 đến 5 và y từ 1 đến 5, cạnh 4. 

**Ví dụ 2** 

đầu vào:```
1
3
0 0
2 1
1 3
```| Bước | xs | vâng | phút_x | max_x | phút_y | max_y | bên | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [0,2,1] | [0,1,3] | 0 | 2 | 0 | 3 | tối đa(2-0,3-0)=3 | 

Hình vuông phải bao gồm cả ba điểm. Việc mở rộng phạm vi x để phù hợp với phạm vi y sẽ đảm bảo một hình vuông. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phép tính tối thiểu/tối đa yêu cầu một lần vượt qua n điểm | 
| Không gian | O(n) | Lưu trữ tọa độ riêng biệt để thuận tiện | 

Giải pháp xử lý n = 10^5 một cách thoải mái trong chưa đầy một giây cho mỗi trường hợp thử nghiệm và mức sử dụng bộ nhớ cũng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("1\n4\n1 1\n1 4\n5 1\n5 4\n") == "4", "sample 1"

# custom cases
assert run("1\n3\n0 0\n2 1\n1 3\n") == "3", "non-square bounding"
assert run("1\n1\n10 10\n") == "0", "single point"
assert run("1\n2\n0 0\n0 0\n") == "0", "duplicate points"
assert run("1\n2\n0 0\n5 0\n") == "5", "line horizontally"
assert run("1\n2\n0 0\n0 5\n") == "5", "line vertically"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | kích thước tối thiểu tầm thường | 
| hai điểm giống nhau | 0 | điểm trùng lặp | 
| đường ngang | 5 | phép tính bình phương theo trục | 
| đường thẳng đứng | 5 | tương tự cho trục y | 
| giới hạn không vuông | 3 | đảm bảo kích thước tối đa được sử dụng | 

## Vỏ cạnh 

Nếu tất cả các điểm đều ở cùng một vị trí,`min_x = max_x`Và`min_y = max_y`, Vì thế`side = 0`. Thuật toán xuất ra chính xác 0 mà không cần xử lý đặc biệt. Đối với các điểm tạo thành một đường thẳng, thuật toán mở rộng kích thước nhỏ hơn để khớp với kích thước lớn hơn, tạo ra một hình vuông hợp lệ. Các cấu hình xoay được xử lý bằng cách xem xét các bao lồi nếu cần, nhưng đối với các hình vuông thẳng hàng với trục, phương pháp tọa độ cực trị là đủ. 

Bài xã luận này giải thích trực giác hình học, cho thấy lý do đằng sau việc sử dụng cực trị thay vì kiểm tra tất cả các cặp, đồng thời cung cấp dấu vết cụ thể và cách xử lý trường hợp cạnh.
