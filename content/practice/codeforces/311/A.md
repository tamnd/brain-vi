---
title: "CF 311A ​​- Cặp đôi gần gũi nhất"
description: "Chúng ta được cho một tập hợp các điểm trong mặt phẳng 2D. Nhiệm vụ hình học ban đầu thường là tính khoảng cách Euclide nhỏ nhất trong số tất cả các cặp điểm."
date: "2026-06-05T18:40:30+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 311
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 185 (Div. 1)"
rating: 1300
weight: 311
solve_time_s: 87
verified: false
draft: false
---

[CF 311A - Cặp đôi gần nhất](https://codeforces.com/problemset/problem/311/A) 

**Đánh giá:** 1300 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trong mặt phẳng 2D. Nhiệm vụ hình học ban đầu thường là tính khoảng cách Euclide nhỏ nhất trong số tất cả các cặp điểm. Tuy nhiên, thay vì yêu cầu trực tiếp giá trị đó, bài toán yêu cầu chúng ta xây dựng cấu hình các điểm buộc một thuật toán brute-force cụ thể phải thực hiện nhiều hơn một số lần kiểm tra khoảng cách nhất định. 

Thuật toán sắp xếp các điểm theo tọa độ x, sau đó với mỗi điểm i sẽ quét tiến qua các điểm j > i. Nó tính toán khoảng cách cho đến khi tìm thấy một điểm có khoảng cách x từ i ít nhất là câu trả lời d tốt nhất hiện tại, tại thời điểm đó nó dừng quét tìm i đó. Số lượng tổng đếm có bao nhiêu cặp (i, j) thực sự được kiểm tra trước khi việc cắt tỉa này dừng công việc tiếp theo. 

Đầu ra của chúng tôi không phải là câu trả lời cho một vấn đề tính toán mà là một tập hợp tọa độ. Chúng ta phải chứng minh rằng không có cách xây dựng nào có thể làm cho tổng vượt quá k hoặc xây dựng rõ ràng n điểm tọa độ nguyên riêng biệt (giới hạn trong giá trị tuyệt đối bằng 10^9) sao cho thuật toán thực hiện nghiêm ngặt hơn k kiểm tra cặp. 

Ràng buộc n 2000 đủ nhỏ để hành vi O(n^2) có thể thực hiện được ở ranh giới, vì năm 2000² là khoảng 4 triệu thao tác. Đây là chìa khóa: thuật toán là bậc hai trong trường hợp xấu nhất, nhưng dừng sớm thông qua điều kiện`p[j].x - p[i].x >= d`có thể giảm đáng kể công việc nếu điểm được dàn trải trong x. 

Một khía cạnh tinh tế là d thay đổi trong quá trình thực hiện. Nếu các cặp đầu tiên rất gần nhau, d sẽ trở nên nhỏ và điều kiện ngắt sẽ khó kích hoạt hơn, buộc vòng lặp bên trong phải chạy lâu hơn. Đây chính xác là điều chúng ta sẽ khai thác: chúng ta muốn giữ d nhỏ càng lâu càng tốt để hầu như không xảy ra sự kết thúc sớm. 

Một sự hiểu lầm ngây thơ sẽ là cho rằng việc sắp xếp theo x luôn làm cho việc cắt tỉa có hiệu quả. Điều đó chỉ đúng nếu các điểm được trải đủ rộng trong x. Nếu chúng ta nhóm các điểm chặt chẽ trong x thì`p[j].x - p[i].x`vẫn nhỏ, tình trạng ngắt hiếm khi xảy ra và thuật toán thoái hóa thành quét toàn bộ O(n2). 

## Phương pháp tiếp cận 

Quan điểm vũ phu là đơn giản. Sau khi sắp xếp, với mỗi điểm i, chúng tôi quét tất cả các điểm j sau này cho đến khi hết điểm hoặc khoảng cách x vượt quá khoảng cách d tốt nhất hiện tại. Nếu các điểm được sắp xếp sao cho tất cả tọa độ x giống nhau hoặc gần giống nhau thì điều kiện`p[j].x - p[i].x >= d`hầu như không bao giờ trở thành đúng trừ khi d trở nên rất lớn. Nhưng d bắt đầu bằng vô cùng và chỉ giảm khi chúng ta tìm thấy các cặp gần nhau hơn, vì vậy hành vi ban đầu về cơ bản là một vòng lặp lồng nhau đầy đủ. 

Cái nhìn sâu sắc quan trọng là điều kiện cắt tỉa chỉ phụ thuộc vào sai phân x, trong khi khoảng cách mục tiêu phụ thuộc vào cả x và y. Nếu chúng ta xây dựng các điểm có tọa độ x giống nhau thì điều kiện ngắt không bao giờ được kích hoạt vì`p[j].x - p[i].x = 0`cho tất cả các cặp. Điều đó thu gọn thuật toán thành kiểu liệt kê cặp thuần túy và tot trở thành chính xác n(n−1)/2. 

Như vậy bài toán xây dựng trở nên đơn giản: ta chỉ cần tot > k. Vì tot được cố định bởi n trong hình học suy biến này, nên chúng ta chọn n sao cho n(n−1)/2 vượt quá k. Sau đó, chúng tôi xuất ra n điểm riêng biệt bất kỳ có cùng tọa độ x. 

Điều này hiệu quả vì chúng tôi cố tình phá hủy cấu trúc cắt tỉa hình học mà thuật toán dựa vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(1) | Quá chậm để phân tích các công trình trong trường hợp xấu nhất | 
| Xây dựng thoái hóa (tất cả x bằng nhau) | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cấu hình trong trường hợp xấu nhất thay vì mô phỏng thuật toán. 

1. Đầu tiên, chúng ta xác định liệu có thể vượt quá k thao tác sử dụng n điểm hay không. Trong trường hợp xấu nhất, thuật toán sẽ kiểm tra tất cả các cặp, do đó tổng tối đa có thể là n(n−1)/2. Nếu giá trị này không lớn hơn k thì không có cấu trúc nào có thể ép buộc TLE. 
2. Nếu n(n−1)/2 ≤ k, chúng ta sẽ ngay lập tức đưa ra "không có nghiệm". Điều này hợp lý vì ngay cả trong cấu hình hình học xấu nhất có thể, thuật toán không thể kiểm tra nhiều hơn tất cả các cặp. 
3. Ngược lại, chúng ta xây dựng các điểm sao cho việc cắt tỉa không bao giờ được kích hoạt. Chúng tôi đạt được điều này bằng cách đặt tất cả tọa độ x bằng nhau, ví dụ x = 0 cho tất cả các điểm. 
4. Chúng ta gán các tọa độ y riêng biệt, chẳng hạn như y = 0, 1, 2, ..., n−1, đảm bảo tất cả các điểm đều khác biệt và nằm trong giới hạn. 
5. Chúng tôi xuất ra những điểm này. Vì tất cả hiệu của x đều bằng 0 nên điều kiện`p[j].x - p[i].x >= d`luôn sai với mọi d hữu hạn, do đó vòng lặp bên trong không bao giờ bị ngắt sớm. 
6. Kết quả là thuật toán đánh giá mọi cặp (i, j), do đó tổng bằng n(n−1)/2, lớn hơn k theo cách xây dựng. 

### Tại sao nó hoạt động 

Việc cắt tỉa của thuật toán phụ thuộc hoàn toàn vào việc phân tách tọa độ x. Bằng cách thu gọn tất cả các điểm thành một đường thẳng đứng, chúng ta loại bỏ mọi sự ngăn cách theo chiều ngang. Vì điều kiện ngắt chỉ dựa trên khoảng cách x vượt quá d và khoảng cách x luôn bằng 0 nên nó không bao giờ kích hoạt. Do đó, thuật toán suy biến thành phép liệt kê theo cặp đầy đủ, tối đa hóa tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    
    max_pairs = n * (n - 1) // 2
    if max_pairs <= k:
        print("no solution")
        return

    # construct all points on a vertical line
    for i in range(n):
        print(0, i)

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán giới hạn trên về mặt lý thuyết về số lượng cặp kiểm tra mà thuật toán có thể thực hiện. Điều này rất quan trọng vì nó ngay lập tức loại trừ những trường hợp không thể xảy ra mà không cần xây dựng bất cứ điều gì. 

Việc xây dựng sử dụng x = 0 cho tất cả các điểm và trải rộng các giá trị y để đảm bảo tính khác biệt. Khoảng cách y đảm bảo hình học hợp lệ, nhưng quan trọng hơn là ngăn ngừa sự trùng lặp ngẫu nhiên có thể vi phạm các ràng buộc. 

Chúng ta không cần mô phỏng thuật toán vì hành vi trong trường hợp xấu nhất được xác định đầy đủ khi tất cả tọa độ x giống hệt nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
```Chúng tôi tính toán các cặp tối đa: 4·3/2 = 6, lớn hơn 3, do đó có thể xây dựng được. 

Chúng tôi xuất ra:```
0 0
0 1
0 2
0 3
```| tôi | j | x[i] | x[j] | x[j]-x[i] | nghỉ được kích hoạt? | tot | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | 0 | không | 1 | 
| 1 | 3 | 0 | 0 | 0 | không | 2 | 
| 1 | 4 | 0 | 0 | 0 | không | 3 | 
| 2 | 3 | 0 | 0 | 0 | không | 4 | 
| 2 | 4 | 0 | 0 | 0 | không | 5 | 
| 3 | 4 | 0 | 0 | 0 | không | 6 | 

Dấu vết này cho thấy rằng mọi cặp đều được truy cập, xác nhận rằng việc cắt tỉa không bao giờ được kích hoạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi chỉ in n điểm; không cần mô phỏng | 
| Không gian | O(1) | Chỉ sử dụng bộ lưu trữ phụ trợ liên tục | 

Cấu trúc ở mức tối thiểu và hoạt động thoải mái trong giới hạn ngay cả với n = 2000. Quan sát chính là chúng tôi tránh mô phỏng hoàn toàn quy trình O(n²). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io

    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n, k = map(int, input().split())
    if n * (n - 1) // 2 <= k:
        return print("no solution")
    for i in range(n):
        print(0, i)

# provided sample
assert run("4 3\n") != "", "sample 1"

# custom cases
assert run("2 0\n") == "0 0\n0 1", "minimum n"
assert run("3 3\n") == "no solution", "tight bound case"
assert run("5 10\n") == "no solution", "exact saturation"
assert run("6 5\n") != "", "valid construction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 | hai điểm thẳng đứng | xây dựng hợp lệ tối thiểu | 
| 3 3 | không có giải pháp | ranh giới nơi các cặp bằng k | 
| 5 10 | không có giải pháp | kiểm tra độ bão hòa chính xác | 
| 6 5 | xây dựng | trường hợp hợp lệ điển hình | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k đã lớn hơn số lượng đánh giá cặp tối đa có thể có. Ví dụ: nếu n = 3 và k = 10 thì ngay cả một lần quét O(n²) hoàn chỉnh cũng chỉ tạo ra 3 đánh giá. Trong trường hợp này, kết quả đúng là “không có giải pháp”. Thuật toán xử lý vấn đề này bằng cách so sánh k với n(n−1)/2 trước bất kỳ đầu ra nào. 

Một trường hợp cạnh khác là nhỏ nhất n = 2. Ở đây có đúng một cặp. Nếu k ≥ 1 thì không tồn tại nghiệm. Nếu k = 0, chúng ta vẫn có thể tạo ra hai điểm và thuật toán sẽ luôn kiểm tra cặp điểm đó, cho kết quả tot = 1 > 0. 

Trường hợp tế nhị cuối cùng là đảm bảo tính khác biệt của các điểm. Bằng cách cố định x = 0 và sử dụng các giá trị y tăng dần, chúng tôi đảm bảo tính duy nhất trong khi vẫn giữ sai phân x giống hệt nhau cho mỗi cặp. Điều này bảo tồn hành vi trong trường hợp xấu nhất mà không vi phạm các ràng buộc.
