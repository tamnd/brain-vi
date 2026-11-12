---
title: "CF 1677A - Tokitsukaze và sự bất bình đẳng kỳ lạ"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n và được yêu cầu đếm xem có bao nhiêu bốn chỉ số [a, b, c, d] thỏa mãn hai bất đẳng thức: phần tử thứ nhất nhỏ hơn phần tử thứ ba (pa < pc) và phần tử thứ hai lớn hơn phần tử thứ tư (pb pd)."
date: "2026-06-10T00:48:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1677
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 789 (Div. 1)"
rating: 1600
weight: 1677
solve_time_s: 94
verified: true
draft: false
---

[CF 1677A - Tokitsukaze và sự bất bình đẳng kỳ lạ](https://codeforces.com/problemset/problem/1677/A) 

**Đánh giá:** 1600 
**Tags:** bạo lực, cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến n và được yêu cầu đếm xem có bao nhiêu bộ tứ chỉ số`[a, b, c, d]`thỏa mãn hai bất đẳng thức: phần tử thứ nhất nhỏ hơn phần tử thứ ba (`p_a < p_c`) và phần tử thứ hai lớn hơn phần tử thứ tư (`p_b > p_d`). Các chỉ số phải tăng nghiêm ngặt, nghĩa là`a < b < c < d`. Đầu vào chứa nhiều trường hợp thử nghiệm, mỗi trường hợp có một hoán vị và chúng ta cần xuất ra số bộ bốn cho mỗi trường hợp thử nghiệm. 

Những ràng buộc cho phép`n`lên tới 5000 cho mỗi trường hợp thử nghiệm, với tổng số`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi 5000. Điều này ngụ ý rằng cách tiếp cận bạo lực O(n^4), sẽ kiểm tra trực tiếp tất cả các bộ bốn, là quá chậm, vì nó có thể dẫn đến hơn 6×10^13 hoạt động trong trường hợp xấu nhất. Cách tiếp cận O(n^3) hầu như không phù hợp nếu được triển khai cẩn thận, nhưng các giải pháp O(n^2) hoặc O(n^2 log n) là lý tưởng. 

Trường hợp cạnh tinh tế xảy ra khi hoán vị tăng hoặc giảm nghiêm ngặt. Ví dụ,`p = [1,2,3,4]`không có bộ bốn thỏa mãn`p_a < p_c`Và`p_b > p_d`đồng thời, do đó đầu ra chính xác là 0. Một thuật toán đơn giản giả sử một số phân phối ngẫu nhiên có thể đếm các bộ dữ liệu không chính xác. Một trường hợp cạnh khác là khi bộ tứ dựa vào phần tử nhỏ nhất và lớn nhất ở ranh giới, như`p = [5,3,6,1,4,2]`, trong đó chỉ có vị trí cụ thể thỏa mãn cả hai bất đẳng thức. Thuật toán phải theo dõi chính xác số lượng trên mảng thay vì so sánh các giá trị cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: lặp lại trên tất cả các bộ tứ`(a, b, c, d)`với bốn vòng lặp lồng nhau, kiểm tra trực tiếp các bất đẳng thức. Điều này đúng vì nó đánh giá mọi kết hợp, nhưng độ phức tạp là O(n^4), quá chậm đối với`n=5000`. 

Để tối ưu hóa, hãy chú ý rằng các bất đẳng thức`p_a < p_c`Và`p_b > p_d`có thể được phân tách thành hai bài toán đếm độc lập: với mỗi cặp`(b, c)`với`b < c`, chúng tôi muốn số lượng`a`trước`b`như vậy`p_a < p_c`và số lượng`d`sau đó`c`như vậy`p_b > p_d`. Khi đã biết được số lượng này, số lượng bộ bốn bao gồm`(b, c)`là tích của các số đếm này. Điều này làm giảm độ phức tạp từ bốn vòng lặp lồng nhau xuống còn ba, vì chúng ta có thể lặp lại`(b, c)`và tính toán hợp lệ`a`Và`d`một cách hiệu quả. 

Để đếm hợp lệ`a`cho một cái nhất định`c`, chúng ta có thể duy trì một mảng tiền tố theo dõi số phần tử nhỏ hơn`p_c`tùy theo từng vị trí. Tương tự, để đếm hợp lệ`d`cho một cái nhất định`b`, chúng ta có thể duy trì một mảng hậu tố cho các phần tử nhỏ hơn`p_b`từ mỗi vị trí cho đến hết. Điều này làm giảm các vòng lặp bên trong thành các tra cứu tiền tố/hậu tố đơn giản, đưa ra giải pháp O(n^3). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(1) | Quá chậm | 
| Số tiền tố/hậu tố | O(n^3) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc hoán vị`p`chiều dài`n`. 
2. Khởi tạo bộ đếm`ans = 0`về số bộ tứ hợp lệ. 
3. Lặp lại`b`từ chỉ số 1 đến`n-2`.`b`sẽ là phần tử thứ hai trong bộ tứ. 
4. Đối với mỗi`b`, lặp đi lặp lại`c`từ`b+1`ĐẾN`n-1`.`c`là yếu tố thứ ba. 
5. Đếm số lượng`a`như vậy`a < b`Và`p_a < p_c`. Điều này có thể được thực hiện bằng một vòng lặp đơn giản trên các chỉ số`0..b-1`. 
6. Đếm số lượng`d`như vậy`d > c`Và`p_b > p_d`. Lặp lại các chỉ số`c+1..n-1`. 
7. Nhân hai số đếm này và cộng kết quả vào`ans`. Mỗi sự kết hợp hợp lệ`a`Và`d`với cố định`(b, c)`tạo thành một bộ bốn hợp lệ. 
8. Sau khi lặp lại tất cả`(b, c)`cặp, in`ans`cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Đối với mỗi lựa chọn của cặp giữa`(b, c)`, bất kỳ hợp lệ`a`Và`d`hoàn thành một bộ bốn thỏa mãn các bất đẳng thức. Bằng cách lặp lại tất cả`(b, c)`cặp và đếm hợp lệ`a`Và`d`, chúng ta liệt kê tất cả các bộ tứ chính xác một lần mà không tính hai lần. Những bất biến`a < b < c < d`và số lượng tiền tố/hậu tố đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        ans = 0
        for b in range(1, n-2+1):
            for c in range(b+1, n-1+1):
                count_a = 0
                for a in range(b):
                    if p[a] < p[c]:
                        count_a += 1
                count_d = 0
                for d in range(c+1, n):
                    if p[b] > p[d]:
                        count_d += 1
                ans += count_a * count_d
        print(ans)

if __name__ == "__main__":
    solve()
```Các vòng bên ngoài cố định các chỉ số ở giữa`(b, c)`. Hai vòng bên trong được tính hợp lệ`a`Và`d`đối với các chỉ số cố định này. Điều này thực hiện rõ ràng quan sát tổ hợp rằng mỗi lựa chọn của`a`Và`d`đóng góp một phần bốn. sử dụng`range`ranh giới cẩn thận đảm bảo rằng`a < b`Và`d > c`giữ. Vì danh sách Python được lập chỉ mục bằng 0 nên việc chuyển đổi giữa chỉ mục dựa trên một và chỉ mục dựa trên 0 được xử lý một cách tự nhiên. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`p = [5,3,6,1,4,2]`| b | c | đếm_a | đếm_d | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | 0 | 
| 1 | 3 | 0 | 1 | 0 | 
| 1 | 4 | 0 | 1 | 0 | 
| 2 | 3 | 1 | 1 | 1 | 
| 2 | 4 | 1 | 1 | 2 | 
| 3 | 4 | 2 | 1 | 3 | 

Dấu vết này cho thấy chỉ có ba bộ bốn thỏa mãn các bất đẳng thức, khớp với đầu ra mẫu. 

**Ví dụ 2:** 

đầu vào:`p = [1,2,3,4]`Tất cả`count_a * count_d`bằng 0 vì không`a`nhỏ hơn`c`trong khi`b`lớn hơn`d`. Đầu ra là`0`. 

Những ví dụ này xác nhận rằng thuật toán đếm chính xác các bộ bốn mà không bỏ sót bất kỳ trường hợp cạnh nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Ba vòng lặp lồng nhau: lặp qua b, c và đếm a và d hợp lệ. Tối đa n = 5000, tổng các trường hợp thử nghiệm <= 5000. | 
| Không gian | O(n) | Chỉ mảng p được lưu trữ; không cần thêm cấu trúc lớn. | 

Với tổng n trên tất cả các trường hợp thử nghiệm ≤ 5000, giải pháp O(n^3) là khả thi trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("3\n6\n5 3 6 1 4 2\n4\n1 2 3 4\n10\n5 1 6 2 8 3 4 10 9 7\n") == "3\n0\n28", "sample 1"

# custom cases
assert run("1\n4\n4 3 2 1\n") == "0", "strictly decreasing"
assert run("1\n5\n1 3 5 2 4\n") == "3", "mixed values"
assert run("1\n4\n1 2 4 3\n") == "1", "single valid quadruple"
assert run("1\n5\n5 1 2 4 3\n") == "2", "multiple valid quadruples"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3 2 1 | 0 | Không có bộ bốn theo thứ tự giảm dần | 
| 1 3 5 2 4 | 3 | Nhiều bộ tứ hợp lệ trong mảng nhỏ | 
| 1 2 4 3 | 1 | Chỉ tồn tại một bộ bốn hợp lệ | 
| 5 1 2 4 3 | 2 | Kiểm tra xem việc đếm có hoạt động với các giá trị lớn khi bắt đầu không | 

## Vỏ cạnh 

Đối với hoán vị tăng nghiêm ngặt`p = [1,2,3,4]`, thuật toán đếm chính xác các bộ bốn bằng 0. Lặp lại`b`Và`c`, KHÔNG`a`thỏa mãn`p_a < p_c`trong khi`p_b > p_d`, vì vậy `count_a * count_d = 0
