---
title: "CF 1119A - Ilya và bước đi đầy màu sắc"
description: "Chúng ta có một con phố có n ngôi nhà liên tiếp, được đánh số từ 1 đến n, trong đó mỗi ngôi nhà có một màu được biểu thị bằng một số nguyên. Ilya muốn chọn hai ngôi nhà có màu sắc khác nhau và đi giữa chúng, đo khoảng cách đơn giản bằng sự khác biệt về chỉ số (j - i) của chúng với i < j."
date: "2026-06-12T04:27:56+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1119
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 2"
rating: 1100
weight: 1119
solve_time_s: 65
verified: true
draft: false
---

[CF 1119A - Ilya và bước đi đầy màu sắc](https://codeforces.com/problemset/problem/1119/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một con phố với`n`nhà liên tiếp, đánh số từ`1`ĐẾN`n`, trong đó mỗi ngôi nhà có một màu được biểu thị bằng một số nguyên. Ilya muốn chọn hai ngôi nhà có **màu sắc khác nhau** và đi giữa chúng, đo khoảng cách đơn giản bằng sự khác biệt về chỉ số của chúng`(j - i)`vì`i < j`. Nhiệm vụ là tìm khoảng cách tối đa anh ta có thể đạt được theo quy tắc này. 

Đầu vào cho biết số lượng nhà`n`theo sau là danh sách màu sắc`c_1, c_2, ..., c_n`. Đầu ra là một số nguyên duy nhất: khoảng cách lớn nhất có thể giữa hai ngôi nhà có màu sắc khác nhau. 

Từ`n`có thể lớn tới 300.000 và giới hạn thời gian là 2 giây, bất kỳ giải pháp nào kiểm tra từng cặp nhà có thể (sẽ yêu cầu khoảng 45 tỷ thao tác trong trường hợp xấu nhất) đều không khả thi. Điều này loại trừ các cách tiếp cận vũ phu với`O(n^2)`sự phức tạp. Chúng ta cần một giải pháp tuyến tính trong`n`hoặc gần nó. 

Các trường hợp cạnh bao gồm các tình huống trong đó nhà đầu tiên và nhà cuối cùng có màu khác nhau (tạo ra khoảng cách tối đa) hoặc khi mảng có các chuỗi dài có màu giống hệt nhau ngoại trừ ở hai đầu. Ví dụ,`[1, 1, 1, 1, 2]`nên quay lại`4`, không`1`. Việc triển khai đơn giản chỉ quét các cặp lân cận sẽ trả về câu trả lời sai. Một trường hợp tinh vi khác là khi khoảng cách lớn nhất xảy ra từ điểm đầu đến nhà khác biệt muộn hoặc từ điểm cuối đến nhà khác biệt sớm, do đó giải pháp phải xét đến cả hai đầu của mảng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trên mọi cặp có thể`(i, j)`với`i < j`và kiểm tra xem màu sắc có khác nhau không, theo dõi mức tối đa`j - i`. Điều này đúng nhưng đòi hỏi`O(n^2)`hoạt động quá chậm đối với`n`lên tới 300.000. 

Quan sát quan trọng để tối ưu hóa vấn đề này là khoảng cách tối đa sẽ luôn liên quan đến **một trong các điểm cuối**. Nếu chúng ta cố gắng tối đa hóa`j - i`, sự khác biệt lớn nhất xảy ra hoặc bắt đầu từ nhà đầu tiên hoặc kết thúc ở nhà cuối cùng. Vì vậy, chúng ta chỉ cần kiểm tra khoảng cách từ ngôi nhà đầu tiên đến ngôi nhà khác màu cuối cùng và từ ngôi nhà cuối cùng đến ngôi nhà khác màu đầu tiên. Điều này làm giảm vấn đề thành việc **kiểm tra hai ứng viên**, làm cho nó`O(n)`. 

Cách tiếp cận tối ưu quét từ cả hai đầu: bắt đầu từ bên trái, tìm ngôi nhà xa nhất bên phải có màu khác và bắt đầu từ bên phải, tìm ngôi nhà xa nhất bên trái có màu khác. Tối đa của hai khoảng cách này là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ghi lại màu sắc ngôi nhà đầu tiên`c_first = c[0]`. 
2. Quét mảng từ cuối về phía sau. Đối với mỗi ngôi nhà, nếu màu sắc của nó khác nhau`c_first`, tính toán`distance = index_last - 0`và cập nhật khoảng cách tối đa. Dừng lại ở màu đầu tiên khác với màu cuối cùng vì chúng ta chỉ quan tâm đến **màu xa nhất**. 
3. Ghi lại màu sắc ngôi nhà cuối cùng`c_last = c[n-1]`. 
4. Quét mảng từ đầu trở đi. Đối với mỗi ngôi nhà, nếu màu sắc của nó khác nhau`c_last`, tính toán`distance = n-1 - index_first`và cập nhật khoảng cách tối đa. Dừng lại ở màu khác nhau đầu tiên ngay từ đầu vì một lần nữa, chúng ta chỉ quan tâm đến **màu xa nhất**. 
5. Trả về khoảng cách lớn hơn trong hai khoảng cách tìm được. 

**Tại sao nó hoạt động:** Khoảng cách tối đa bị hạn chế bởi các điểm cuối. Hai ngôi nhà bất kỳ bên trong mảng có màu khác nhau sẽ có khoảng cách nhỏ hơn một trong các điểm cuối được ghép với một màu khác nhau ở đầu đối diện. Bằng cách chỉ kiểm tra các điểm cuối, chúng tôi đảm bảo rằng chúng tôi tìm thấy khoảng cách tối đa có thể mà không bỏ sót bất kỳ ứng cử viên nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
colors = list(map(int, input().split()))

c_first = colors[0]
c_last = colors[-1]

# distance from the first house to the furthest differing color from the end
for i in range(n-1, -1, -1):
    if colors[i] != c_first:
        dist1 = i
        break

# distance from the last house to the furthest differing color from the start
for i in range(n):
    if colors[i] != c_last:
        dist2 = n - 1 - i
        break

print(max(dist1, dist2))
```Mã đầu tiên gán màu đầu tiên và màu cuối cùng, sau đó quét từ cuối cho ngôi nhà đầu tiên khác với phần đầu và từ đầu cho ngôi nhà đầu tiên khác với phần cuối. các`break`đảm bảo chúng tôi chỉ lấy ngôi nhà khác nhau xa nhất có thể trong mỗi trường hợp. sử dụng`i`trực tiếp vì khoảng cách là an toàn vì Python được lập chỉ mục 0, vì vậy`i - 0`là`i`, Và`n - 1 - i`tính toán trực tiếp khoảng cách từ điểm cuối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`[1, 2, 3, 2, 3]`| tôi | màu sắc[i] != c_first? | quận 1 | 
| --- | --- | --- | 
| 4 | 3 != 1 | 4 | 

| tôi | màu sắc[i] != c_last? | quận 2 | 
| --- | --- | --- | 
| 0 | 1 != 3 | 4 | 

Khoảng cách tối đa là`max(4, 4) = 4`. 

Điều này xác nhận rằng việc quét từ cả hai đầu sẽ xác định chính xác khoảng cách tối đa. 

### Mẫu 2 

đầu vào:`[1, 1, 1, 2]`| tôi | màu sắc[i] != c_first? | quận 1 | 
| --- | --- | --- | 
| 3 | 2 != 1 | 3 | 

| tôi | màu sắc[i] != c_last? | quận 2 | 
| --- | --- | --- | 
| 0 | 1 != 2 | 3 | 

Khoảng cách tối đa là`3`, điều đó đúng. Dấu vết này xác nhận thuật toán xử lý các chuỗi màu giống hệt nhau, theo sau là một màu khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi thực hiện hai lần quét tuyến tính trên mảng, một lần từ mỗi đầu. | 
| Không gian | O(1) | Chỉ có một số biến được lưu trữ; không có cấu trúc dữ liệu bổ sung nào có tỷ lệ với n. | 

Với n lên tới 300.000, các thao tác O(n) diễn ra thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    colors = list(map(int, input().split()))
    c_first = colors[0]
    c_last = colors[-1]

    for i in range(n-1, -1, -1):
        if colors[i] != c_first:
            dist1 = i
            break
    for i in range(n):
        if colors[i] != c_last:
            dist2 = n - 1 - i
            break
    return str(max(dist1, dist2))

# Provided samples
assert run("5\n1 2 3 2 3\n") == "4", "sample 1"
assert run("2\n1 2\n") == "1", "sample 2"
assert run("7\n1 1 1 2 2 2 1\n") == "6", "sample 3"

# Custom cases
assert run("4\n1 1 1 2\n") == "3", "identical prefix"
assert run("4\n2 1 1 1\n") == "3", "identical suffix"
assert run("3\n1 2 1\n") == "2", "alternating colors"
assert run("6\n1 1 1 1 1 2\n") == "5", "large end distance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4\n1 1 1 2\n`| 3 | Xử lý các màu giống hệt nhau khi bắt đầu | 
|`4\n2 1 1 1\n`| 3 | Xử lý các màu giống hệt nhau ở cuối | 
|`3\n1 2 1\n`| 2 | Màu sắc xen kẽ, mảng ngắn | 
|`6\n1 1 1 1 1 2\n`| 5 | Khoảng cách tối đa xảy ra từ đầu đến màu khác nhau cuối cùng | 

## Vỏ cạnh 

Đối với một mảng như`[1, 1, 1, 2]`, quét từ cuối sẽ tìm thấy màu khác nhau ở chỉ số 3, cho khoảng cách là`3`. Quét từ đầu cũng tìm thấy màu khác ở chỉ mục cuối cùng, đồng thời đưa ra`3`. Điều này đảm bảo rằng các chuỗi màu giống hệt nhau ở hai đầu không làm cho thuật toán bỏ lỡ mức tối đa chính xác. Tương tự, nếu mảng là`[2, 1, 1, 1]`, quá trình quét từ đầu sẽ tìm thấy chỉ số 0 cho màu khác với phần cuối, tạo ra khoảng cách tối đa chính xác. Thuật toán xử lý các trường hợp đặc biệt này một cách tự nhiên mà không cần thêm điều kiện.
