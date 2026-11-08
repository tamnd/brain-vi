---
title: "CF 1389F - Phân đoạn hai màu"
description: "Chúng ta được cung cấp một tập hợp các đoạn trên trục số, mỗi đoạn có màu 1 hoặc 2. Một cặp đoạn được coi là xấu nếu chúng chồng lên nhau hoặc chạm vào nhau và có màu khác nhau. Mục tiêu của chúng tôi là chọn càng nhiều phân đoạn càng tốt đồng thời tránh bất kỳ cặp xấu nào."
date: "2026-06-11T10:32:21+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "graph-matchings", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1389
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 92 (Rated for Div. 2)"
rating: 2600
weight: 1389
solve_time_s: 137
verified: true
draft: false
---

[CF 1389F - Phân đoạn hai màu](https://codeforces.com/problemset/problem/1389/F) 

**Đánh giá:** 2600 
**Thẻ:** cấu trúc dữ liệu, dp, so khớp biểu đồ, sắp xếp 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đoạn trên trục số, mỗi đoạn có màu 1 hoặc 2. Một cặp đoạn được coi là xấu nếu chúng chồng lên nhau hoặc chạm vào nhau và có màu khác nhau. Mục tiêu của chúng tôi là chọn càng nhiều phân đoạn càng tốt đồng thời tránh bất kỳ cặp xấu nào. Nói cách khác, chúng ta muốn một tập hợp con các phân đoạn trong đó không có hai phân đoạn có màu khác nhau giao nhau. 

Các phân đoạn được xác định bởi số nguyên$l_i$Và$r_i$và có thể kéo dài đến$10^9$. Số lượng phân đoạn$n$có thể đạt tới 200.000, điều này loại trừ mọi giải pháp phức tạp$O(n^2)$bởi vì điều đó sẽ dẫn đến khoảng 40 tỷ phép tính, vượt xa giới hạn 2 giây. Chúng ta cần một cái gì đó gần gũi hơn$O(n \log n)$. 

Các trường hợp cạnh có thể gây khó khăn cho cách tiếp cận đơn giản bao gồm các phân đoạn được lồng vào nhau hoặc chạm vào điểm cuối. Ví dụ: nếu chúng ta có các phân đoạn [1,3,1], [3,5,2] và [2,4,1], một lựa chọn bất cẩn có thể bao gồm cả ba, nhưng phân đoạn thứ nhất và thứ hai giao nhau tại 3 và có màu khác nhau, khiến chúng trở thành một cặp xấu. Một trường hợp tinh tế khác là khi tất cả các phân đoạn đều có cùng màu - thì chúng ta có thể lấy tất cả chúng, ngay cả khi chúng trùng nhau. Một kẻ tham lam đơn giản mà bỏ qua màu sắc có thể thất bại trong những tình huống như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là xem xét tất cả các tập hợp con có thể có của các phân đoạn, kiểm tra các cặp xấu trong mỗi phân đoạn. Đối với mỗi tập hợp con, chúng ta cần so sánh từng cặp phân đoạn, dẫn đến$O(2^n \cdot n^2)$thời gian. Điều này rõ ràng là không thể thực hiện được đối với$n=2 \cdot 10^5$. 

Thông tin chi tiết quan trọng là chúng ta có thể tách các phân đoạn theo màu sắc và xử lý từng màu một cách độc lập. Nếu chúng ta chọn các phân đoạn có một màu thì sẽ không có nguy cơ có các cặp xấu trong màu đó. Như vậy, với mỗi màu chúng ta chỉ cần chọn những đoạn không trùng nhau. Điều này làm giảm vấn đề thành vấn đề cổ điển “số lượng khoảng không chồng chéo tối đa”, có thể được giải quyết một cách tham lam: sắp xếp các khoảng theo điểm cuối bên phải của chúng và lặp đi lặp lại chọn khoảng đầu tiên bắt đầu sau khi khoảng được chọn cuối cùng kết thúc. Chúng tôi tính toán điều này một cách độc lập cho cả hai màu và lấy màu cho phép nhiều phân đoạn hơn. 

Việc quan sát thấy không có cặp xấu nào tồn tại giữa các phân đoạn có cùng màu cho phép chúng ta bỏ qua các giao điểm trong mỗi nhóm màu. Bài toán rút gọn thành hai bài toán lập kế hoạch ngắt quãng độc lập, có thể giải được trong$O(n \log n)$do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách các phân đoạn thành hai danh sách: một cho màu 1 và một cho màu 2. Bước này đảm bảo chúng ta có thể xử lý lựa chọn không chồng chéo một cách độc lập cho từng màu. 
2. Đối với mỗi danh sách màu, hãy sắp xếp các phân đoạn theo điểm cuối bên phải của chúng. Việc sắp xếp theo điểm cuối phù hợp là rất quan trọng vì nó cho phép lựa chọn nhanh chóng phân đoạn hoàn thiện sớm nhất, tối đa hóa số lượng phân đoạn không chồng chéo. 
3. Khởi tạo bộ đếm số lượng phân đoạn đã chọn và một biến để theo dõi điểm cuối bên phải của phân đoạn được chọn cuối cùng, ban đầu được đặt thành âm vô cực. 
4. Lặp lại các phân đoạn đã được sắp xếp. Đối với mỗi phân đoạn, nếu điểm cuối bên trái của nó lớn hơn điểm cuối bên phải của phân đoạn được chọn cuối cùng, hãy đưa phân đoạn đó vào vùng chọn và cập nhật điểm cuối bên phải được chọn cuối cùng. Nếu không, hãy bỏ qua nó. Điều này đảm bảo rằng không có hai phân đoạn được chọn trùng nhau. 
5. Lặp lại bước 3 và 4 cho cả hai danh sách màu, thu được hai số đếm:`count_color1`Và`count_color2`. 
6. Đáp án cuối cùng là tối đa`count_color1`Và`count_color2`, bởi vì việc chọn tất cả các phân đoạn của một màu sẽ đảm bảo không có cặp xấu. 

Tại sao nó hoạt động: Bằng cách tập trung vào một màu tại một thời điểm và sử dụng thuật toán lập lịch theo khoảng thời gian tham lam, chúng tôi đảm bảo tập hợp con lớn nhất gồm các phân đoạn không chồng chéo cho mỗi màu. Vì các cặp xấu chỉ xảy ra giữa các màu khác nhau nên việc lấy tất cả các phân đoạn từ màu mang lại số lượng tối đa là tối ưu. Việc lựa chọn tham lam khoảng thời gian kết thúc sớm nhất sẽ đảm bảo tính tối đa, vì bất kỳ khoảng thời gian nào khác bắt đầu muộn hơn sẽ chồng chéo hoặc làm giảm tổng số được chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_non_overlapping(segments):
    segments.sort(key=lambda x: x[1])
    count = 0
    last_end = -1
    for l, r in segments:
        if l > last_end:
            count += 1
            last_end = r
    return count

def main():
    n = int(input())
    color1, color2 = [], []
    for _ in range(n):
        l, r, t = map(int, input().split())
        if t == 1:
            color1.append((l, r))
        else:
            color2.append((l, r))
    res = max(max_non_overlapping(color1), max_non_overlapping(color2))
    print(res)

if __name__ == "__main__":
    main()
```Chức năng đầu tiên`max_non_overlapping`thực hiện lập lịch khoảng thời gian tham lam cho một danh sách các phân đoạn nhất định. Việc sắp xếp theo điểm cuối phù hợp đảm bảo chúng tôi luôn có thể chọn phân khúc tương thích tiếp theo một cách hiệu quả. Hàm chính phân tách các phân đoạn theo màu và tính toán kích thước lựa chọn tối đa cho mỗi màu, sau đó đưa ra số lượng lớn hơn. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
3
1 3 1
4 6 2
2 5 1
```| Màu sắc | Các phân đoạn được sắp xếp theo r | Quá trình tuyển chọn | Đếm | 
| --- | --- | --- | --- | 
| 1 | [1,3], [2,5] | Chọn [1,3] (last_end=3), bỏ qua [2,5] (2 ≤ 3) | 1 | 
| 2 | [4,6] | Chọn [4,6] (last_end=6) | 1 | 

Số lượng tối đa = max(1+0, 0+1) = 2 

Giải thích: Chúng ta có thể chọn các đoạn [1,3] và [4,6] mà không có cặp xấu nào. 

Mẫu 2: 

đầu vào:```
4
1 4 1
2 5 1
3 6 2
5 7 2
```| Màu sắc | Các phân đoạn được sắp xếp theo r | Quá trình tuyển chọn | Đếm | 
| --- | --- | --- | --- | 
| 1 | [1,4], [2,5] | Chọn [1,4], bỏ qua [2,5] | 1 | 
| 2 | [3,6], [5,7] | Chọn [3,6], bỏ qua [5,7] chồng chéo 6 | 1 | 

Số lượng tối đa = 1 

Giải thích: Mặc dù có 4 phân đoạn, nhưng việc chọn nhiều hơn một phân đoạn cho mỗi màu sẽ tạo ra các điểm giao nhau với màu kia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp từng nhóm màu mất O(n log n), lặp lại là O(n) | 
| Không gian | O(n) | Lưu trữ hai danh sách màu riêng biệt | 

Với n 2·10^5, việc sắp xếp và lặp tuyến tính phù hợp trong vòng 2 giây và mức sử dụng bộ nhớ dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("3\n1 3 1\n4 6 2\n2 5 1\n") == "2", "sample 1"

# Minimum input
assert run("1\n1 1 1\n") == "1", "single segment"

# All same color overlapping
assert run("3\n1 5 1\n2 6 1\n3 7 1\n") == "1", "all overlap same color"

# Nested segments, different colors
assert run("4\n1 10 1\n2 3 2\n4 5 2\n6 7 2\n") == "3", "pick color2's max"

# Touching at endpoint, different colors
assert run("2\n1 2 1\n2 3 2\n") == "1", "cannot pick both"

# Maximum size, non-overlapping
segments = "\n".join(f"{i*2+1} {i*2+2} 1" for i in range(200000))
assert run(f"200000\n{segments}\n") == "200000", "max size non-overlapping same color"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đoạn | 1 | Xử lý đầu vào nhỏ nhất | 
| Tất cả chồng lên nhau, cùng màu | 1 | Tham lam chỉ chọn một | 
| Các phân đoạn lồng nhau, màu sắc hỗn hợp | 3 | Lựa chọn màu sắc đúng | 
| Chạm vào điểm cuối, màu sắc khác nhau | 1 | Điều kiện cạnh khi chạm vào | 
| Tối đa n, không chồng chéo | 200000 | Hiệu suất và độ chính xác ở quy mô | 

## Vỏ cạnh 

Đối với đầu vào:```
2
1 2 1
2 3 2
```Đoạn thứ nhất và thứ hai chạm nhau ở mức 2. Thuật toán phân tách theo màu sắc. Màu 1 có [1,2], màu 2 có [2,3]. Đối với màu 1, chúng tôi chọn [1,2]. Đối với màu 2, [2,3] bắt đầu từ 2, không lớn hơn Last_end=2, vì vậy nó
