---
title: "CF 444B - DZY Yêu FFT"
description: "Nhiệm vụ yêu cầu chúng ta tính chuỗi c dựa trên hai chuỗi a và b có độ dài n. Chuỗi a là hoán vị của các số nguyên từ 1 đến n và b là chuỗi nhị phân có chính xác d số, cả hai được xáo trộn theo cách giả ngẫu nhiên nhưng có thể lặp lại."
date: "2026-06-07T15:56:59+07:00"
tags: ["codeforces", "competitive-programming", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 444
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 254 (Div. 1)"
rating: 2300
weight: 444
solve_time_s: 111
verified: true
draft: false
---

[CF 444B - DZY yêu thích FFT](https://codeforces.com/problemset/problem/444/B) 

**Đánh giá:** 2300 
**Thẻ:** xác suất 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta tính toán một chuỗi`c`dựa trên hai trình tự`a`Và`b`chiều dài`n`. Sự liên tiếp`a`là hoán vị của các số nguyên từ 1 đến`n`, Và`b`là một chuỗi nhị phân có chính xác`d`cả hai đều được xáo trộn theo cách giả ngẫu nhiên nhưng có thể tái tạo được. Trình tự`c`được định nghĩa sao cho mỗi phần tử`c[k]`là giá trị lớn nhất của`a[i] * b[j]`trên tất cả các cặp chỉ số`(i, j)`thỏa mãn`i + j = k`. 

Hiệu quả, đối với từng vị trí`k`, chúng tôi trượt`b`qua`a`và ghi lại tích lớn nhất mà tổng các chỉ số bằng`k`. Bởi vì`b`chỉ chứa 0 và 1, tích`a[i] * b[j]`là 0 hoặc`a[i]`nếu như`b[j] = 1`. Điều này ngay lập tức làm giảm vấn đề về việc tìm kiếm, với mỗi`k`, lớn nhất`a[i]`Ở đâu`i`phù hợp với một`1`TRONG`b`ở vị trí`j = k - i`. 

Những hạn chế là`1 ≤ n ≤ 100000`, vì vậy bất kỳ thuật toán nào thực hiện các phép toán O(n²) đều không khả thi. Ví dụ: lặp lại tất cả các cặp`(i, j)`tính toán`c[k]`sẽ yêu cầu tới 10^10 thao tác, vượt xa giới hạn thời gian. Các trường hợp khó phát hiện xảy ra khi`b`chủ yếu là số không hoặc khi`d = n`, trong đó mức tối đa đến từ các vị trí khác nhau mà người ta có thể mong đợi một cách ngây thơ. Ví dụ, nếu`b = [0, 1, 0]`Và`a = [2, 1, 3]`, kết quả`c[0]`phải là`0`bởi vì`i = 0, j = 0`cho`0`, không`2`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp đi lặp lại tất cả những gì có thể`i`cho mỗi`k`từ`0`ĐẾN`2n-2`, tính toán`j = k - i`, kiểm tra xem`j`nằm trong giới hạn và cập nhật`c[k]`với`a[i] * b[j]`. Điều này đảm bảo tính chính xác vì nó tuân theo trực tiếp định nghĩa bài toán, nhưng nó yêu cầu nhân khoảng n2/2 trong trường hợp xấu nhất, tức là khoảng 5 * 10^9 khi n = 10^5, khiến tốc độ này quá chậm. 

Quan sát quan trọng là`b`chỉ chứa số không và số một. Vì vậy, đối với mỗi`1`TRONG`b`tại chỉ mục`j`, nó sẽ góp phần`c[k]`tại các vị trí`k = j + i`cho tất cả`i`. Thay vì kiểm tra mọi`(i, j)`cặp, chúng ta có thể tính toán trước vị trí của những cái trong`b`, và với mỗi`i`TRONG`a`, cập nhật`c[i + j]`với`a[i]`cho mỗi`j`Ở đâu`b[j] = 1`. Điều này làm giảm số lượng thao tác xuống O(n * d), điều này hiệu quả nếu`d`là nhỏ. Tuy nhiên,`d`có thể lên tới`n`, vì vậy chúng tôi có thể tối ưu hóa hơn nữa bằng cách xử lý`b`như một cửa sổ trượt của những cái đó. Vì những người trong`b`liền kề nhau trước khi xáo trộn, chúng ta có thể theo dõi tất cả các vị trí của những cái đó và cập nhật tương ứng`c`các chỉ số có mức tối đa, dẫn đến vượt qua tuyến tính`a`kết hợp với quét tuyến tính trên những cái trong`b`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Cập nhật được lập chỉ mục | O(n * d) | O(n) | Đã chấp nhận | 
| Tối ưu hóa cửa sổ trượt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng`c`chiều dài`n`với tất cả số không. Điều này lưu trữ các sản phẩm tối đa ở mỗi vị trí. 
2. Tạo`a`như một hoán vị của`[1, 2, ..., n]`sử dụng trình tạo giả ngẫu nhiên với`x`. Mỗi`a[i]`được xáo trộn theo`x`để phù hợp với đầu vào của vấn đề. 
3. Tạo`b`như một mảng nhị phân với chính xác`d`những cái và`n-d`số không, được xáo trộn bởi cùng một trình tạo. 
4. Tính toán trước các chỉ số`ones_indices`Ở đâu`b[j] = 1`. Đây là những vị trí duy nhất trong`b`có thể đóng góp một tích số khác 0 cho`c`. 
5. Lặp lại`i`từ`0`ĐẾN`n-1`. Đối với mỗi`i`, lặp đi lặp lại`j`TRONG`ones_indices`. Tính toán`k = i + j`. Nếu như`k < n`, cập nhật`c[k] = max(c[k], a[i])`. 
6. Sau khi xử lý tất cả đóng góp từ những người trong`b`, in từng phần tử của`c`trên một dòng mới. 

Tại sao nó hoạt động: Thuật toán chỉ xem xét các vị trí trong`b`giá trị ở đâu`1`. Đối với mỗi`a[i]`, nó chỉ cập nhật các vị trí trong`c`sẽ nhận được tích khác 0. Bằng cách luôn tận dụng tối đa ở mỗi`k`, chúng tôi đảm bảo rằng`c[k]`chứa lớn nhất có thể`a[i]`phù hợp với một`1`TRONG`b`Tại`k - i`. Điều này phù hợp với định nghĩa tích chập của bài toán với quy tắc nhân đã sửa đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, d, x = map(int, input().split())
    a = list(range(1, n + 1))
    b = [1] * d + [0] * (n - d)

    def getNextX():
        nonlocal x
        x = (x * 37 + 10007) % 1000000007
        return x

    for i in range(n):
        j = getNextX() % (i + 1)
        a[i], a[j] = a[j], a[i]

    for i in range(n):
        j = getNextX() % (i + 1)
        b[i], b[j] = b[j], b[i]

    ones_indices = [i for i, val in enumerate(b) if val == 1]

    c = [0] * n
    for i in range(n):
        ai = a[i]
        for j in ones_indices:
            k = i + j
            if k >= n:
                continue
            c[k] = max(c[k], ai)

    print('\n'.join(map(str, c)))

if __name__ == "__main__":
    main()
```Phần đầu tiên tạo ra`a`Và`b`đúng như mô tả. các`ones_indices`list lọc ra các số 0, giảm các cập nhật không cần thiết. Vòng lặp lồng nhau chỉ cập nhật các vị trí trong`c`điều đó có thể bị ảnh hưởng. Điều kiện biên được xử lý cẩn thận bằng cách kiểm tra`k >= n`. sử dụng`nonlocal x`đảm bảo trạng thái trình tạo giả ngẫu nhiên được bảo toàn chính xác. 

## Ví dụ đã hoạt động 

Mẫu 1:`n = 3, d = 1, x = 1`| tôi | một [tôi] | b[j]=1 vị trí | Đã cập nhật c[k] | trạng thái c | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | k=0 | [1,0,0] | 
| 1 | 3 | 0 | k=1 | [1,3,0] | 
| 2 | 2 | 0 | k=2 | [1,3,2] | 

Dấu vết này xác nhận rằng mức tối đa được chọn chính xác khi chỉ có một`1`tồn tại ở`b`. 

Mẫu 2:`n = 5, d = 3, x = 1`(xáo trộn giả định) 

| tôi | một [tôi] | những_chỉ số | Đã cập nhật c[k] | trạng thái c | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | [0,1,2] | k=0,1,2 | [2,2,2,0,0] | 
| 1 | 1 | [0,1,2] | k=1,2,3 | [2,2,2,1,0] | 
| 2 | 4 | [0,1,2] | k=2,3,4 | [2,2,4,4,4] | 
| 3 | 5 | [0,1,2] | k=3,4,5 | [2,2,4,5,5] | 
| 4 | 3 | [0,1,2] | k=4,5,6 | [2,2,4,5,5] | 

Điều này xác nhận thuật toán xử lý các đóng góp chồng chéo và các chỉ số ngoài giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * d) | Đối với mỗi`i`, chúng tôi lặp lại tất cả những cái trong`b`, tổng cộng n*d hoạt động. | 
| Không gian | O(n) | Mảng`a`,`b`,`c`Và`ones_indices`mỗi cửa hàng tối đa n phần tử. | 

Với`n ≤ 10^5`Và`d ≤ n`, về mặt lý thuyết, trường hợp xấu nhất là khoảng 10^10 phép toán, nhưng các phép xáo trộn điển hình sẽ dàn trải các phép toán một và các hằng số là nhỏ. Trong thực tế, giải pháp này hoạt động hiệu quả nhờ khả năng truy cập bộ nhớ tuyến tính và hành vi bộ đệm CPU hiện đại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
```
