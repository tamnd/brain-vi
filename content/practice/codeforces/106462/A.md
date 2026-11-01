---
title: "CF 106462A - \u041f\u043e\u0432\u0442\u043e\u0440\u044f\u0435\u043c \u0442\u0430\u0431\u043b\u0438\u0446\u0443 \u0443\u043c\u043d\u043e\u0436\u0435\u043d\u0438\u044f"
description: "Chúng ta được cho một bảng nhân khái niệm có n hàng và m cột. Ở hàng i và cột j, giá trị được lưu trữ đơn giản là i × j."
date: "2026-06-25T08:58:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106462
codeforces_index: "A"
codeforces_contest_name: "\u041f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0441\u0440\u0435\u0434\u0438 \u043d\u0430\u0447\u0438\u043d\u0430\u044e\u0449\u0438\u0445 2026"
rating: 0
weight: 106462
solve_time_s: 39
verified: true
draft: false
---

[CF 106462A - \u041f\u043e\u0432\u0442\u043e\u0440\u044f\u0435\u043c \u0442\u0430\u0431\u043b\u0438\u0446\u0443 \u0443\u043c\u043d\u043e\u0436\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/106462/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bảng nhân khái niệm với`n`hàng và`m`cột. Trong hàng`i`và cột`j`, giá trị được lưu trữ chỉ đơn giản là`i × j`. Nếu chúng ta san phẳng tất cả`n × m`các giá trị vào một danh sách duy nhất và sắp xếp chúng theo thứ tự không giảm, chúng ta được yêu cầu tìm giá trị xuất hiện trong`k`-vị trí thứ. 

Vì vậy, thay vì xây dựng bảng một cách rõ ràng, nhiệm vụ là tìm hiểu việc phân phối sản phẩm`i × j`và trả lời một truy vấn xếp hạng trên nhiều tập hợp đó. 

Các ràng buộc rất lớn: cả hai chiều có thể lên tới`5 · 10^5`, điều này làm cho kích thước bảng có khả năng`2.5 · 10^11`. Bất kỳ cách tiếp cận nào cố gắng liệt kê các mục trực tiếp đều là không thể ngay lập tức, thậm chí việc lưu trữ cấu trúc cũng không thể thực hiện được. Điều này buộc chúng tôi phải đưa ra một giải pháp trong đó chúng tôi không bao giờ hiện thực hóa lưới mà thay vào đó đếm các thuộc tính của nó. 

Một cách giải thích ngây thơ có thể cố gắng tạo ra tất cả các sản phẩm và sắp xếp chúng, nhưng ngay cả việc tạo ra một hàng cũng sẽ gây tốn kém`O(m)`và làm điều đó cho tất cả các hàng dẫn đến`O(nm)`hoạt động vượt xa khả năng thực hiện. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều sản phẩm bằng nhau, đặc biệt là số lượng nhỏ. Ví dụ, trong một`3 × 3`bảng, giá trị`2`chỉ xuất hiện hai lần, trong khi`4`xuất hiện ba lần, đến từ các cặp khác nhau. Bất kỳ cách tiếp cận nào giả định tính độc đáo của sản phẩm sẽ thất bại. 

Một vấn đề khác là suy nghĩ tràn: sản phẩm phù hợp với số nguyên 32 bit, nhưng đếm xem có bao nhiêu giá trị`≤ x`có thể đạt được`n × m`, yêu cầu số học 64-bit. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: tính toán mọi giá trị`i × j`, lưu trữ chúng trong một mảng, sắp xếp nó và chọn`k`-phần tử thứ. Điều này đúng vì nó trực tiếp xây dựng nhiều tập hợp được mô tả trong bài toán. Tuy nhiên, nó đòi hỏi phải tạo`n × m`giá trị, trong trường hợp xấu nhất là về`2.5 · 10^11`phép nhân và bộ nhớ cho cùng một số phần tử. Ngay cả khi ký ức bị bỏ qua, chỉ riêng thời gian cũng khiến điều này trở nên bất khả thi. 

Quan sát cấu trúc quan trọng là chúng ta không cần biết tất cả các giá trị, chỉ cần biết có bao nhiêu giá trị nhỏ hơn hoặc bằng một số ứng cử viên`x`. Nếu chúng ta có thể tính toán số lượng này một cách hiệu quả thì chúng ta có thể tìm kiếm câu trả lời theo hệ nhị phân. Lý do điều này hoạt động là tính đơn điệu: nếu một giá trị`x`đủ lớn để ít nhất là`k`-phần tử nhỏ nhất thì bất kỳ giá trị lớn hơn nào cũng hợp lệ và mọi giá trị nhỏ hơn thì không. Điều này tạo ra một vị từ đơn điệu thích hợp cho tìm kiếm nhị phân. 

Vì vậy, vấn đề giảm xuống còn việc đếm, đối với một giá trị cố định`x`, có bao nhiêu cặp`(i, j)`thỏa mãn`i × j ≤ x`. Đối với mỗi hàng`i`, có hiệu lực`j`giá trị đi từ`1`lên tới`min(m, x // i)`. Tính tổng tất cả các hàng sẽ cho ra tổng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm log(nm)) do sắp xếp | O(nm) | Quá chậm | 
| Tìm kiếm nhị phân + Đếm | O(n log(giá trị tối đa)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi tìm kiếm giá trị nhỏ nhất`x`ít nhất như vậy`k`các số trong bảng là`≤ x`. Điều này biến vấn đề xếp hạng thành một vấn đề quyết định. 
2. Chúng tôi đặt giới hạn tìm kiếm nhị phân từ`1`ĐẾN`n × m`, vì giá trị nhỏ nhất là`1 × 1`và lớn nhất là`n × m`. 
3. Đối với giá trị ứng viên`mid`, ta tính được bao nhiêu sản phẩm`i × j`là`≤ mid`. Đối với mỗi hàng`i`, chúng tôi đếm các cột hợp lệ là`min(m, mid // i)`. Điều này hoạt động vì trong hàng`i`, các giá trị tăng tuyến tính như`i, 2i, 3i, ...`. 
4. Chúng tôi tính tổng các số liệu này`i`từ`1`ĐẾN`n`. Nếu tổng số ít nhất là`k`, sau đó`mid`đủ lớn nên chúng ta di chuyển tìm kiếm sang trái. Nếu không, chúng ta di chuyển sang phải. 
5. Sau khi tìm kiếm nhị phân hội tụ, đáp án nhỏ nhất`mid`đó thỏa mãn điều kiện. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là hàm`F(x)`được định nghĩa là “số lượng mục trong bảng ≤ x” không giảm trong`x`. Một khi là một giá trị`x`tạo ra số đếm ≥`k`, mọi giá trị lớn hơn cũng tạo ra số lượng ≥`k`. Cấu trúc đơn điệu này đảm bảo tìm kiếm nhị phân hội tụ đến giá trị hợp lệ tối thiểu, chính xác là giá trị đó.`k`-phần tử nhỏ nhất trong tập hợp được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())

def count_le(x):
    total = 0
    for i in range(1, n + 1):
        total += min(m, x // i)
    return total

lo, hi = 1, n * m

while lo < hi:
    mid = (lo + hi) // 2
    if count_le(mid) >= k:
        hi = mid
    else:
        lo = mid + 1

print(lo)
```chức năng`count_le`là thành phần quan trọng. Đối với mỗi chỉ mục hàng`i`,`x // i`cho chúng ta biết chúng ta có thể đi bao xa trong hàng đó trước khi vượt quá`x`. Chúng tôi kẹp nó bằng cách`m`bởi vì mỗi hàng chỉ có`m`cột. 

Tìm kiếm nhị phân được thực hiện trên miền giá trị, không phải trên chỉ mục. Sự khác biệt này rất quan trọng vì mảng không được xây dựng rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 4
```Chúng tôi đang nhìn vào bảng:```
1 2 3
2 4 6
```Chúng tôi tìm kiếm giá trị nhị phân. 

| giữa | đếm ≤ giữa | quyết định | 
| --- | --- | --- | 
| 3 | 4 | đi bên trái hoặc chấp nhận | 
| 2 | 3 | đi bên phải | 
| 3 | 4 | cuối cùng | 

Giá trị đầu tiên đạt ít nhất 4 phần tử là`3`, vậy câu trả lời là`3`. 

Điều này xác nhận rằng các bản sao và thứ tự trên các hàng được xử lý chính xác bằng hàm đếm. 

### Ví dụ 2 

đầu vào:```
3 3 5
```Bàn:```
1 2 3
2 4 6
3 6 9
```| giữa | đếm ≤ giữa | quyết định | 
| --- | --- | --- | 
| 4 | 5 | ứng cử viên | 
| 3 | 4 | quá nhỏ | 
| 4 | 5 | cuối cùng | 

Chúng tôi thấy điều đó`4`là giá trị nhỏ nhất có ít nhất 5 phần tử không vượt quá nó nên là giá trị nhỏ thứ 5. 

Ví dụ này cho thấy các giá trị được lặp lại như thế nào`6`hiện diện hai lần không phá vỡ logic, vì thuật toán tính bội số một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(nm)) | Mỗi bước tìm kiếm nhị phân sẽ quét tất cả các hàng một lần và có nhiều bước theo logarit trong phạm vi giá trị | 
| Không gian | O(1) | Chỉ có một số bộ đếm và biến được sử dụng | 

Phương pháp này chia tỷ lệ thoải mái cho`n, m ≤ 5 · 10^5`, vì tìm kiếm nhị phân chạy khoảng 30 lần lặp và mỗi lần lặp là quét tuyến tính trên`n`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    def count_le(x):
        total = 0
        for i in range(1, n + 1):
            total += min(m, x // i)
        return total

    lo, hi = 1, n * m
    while lo < hi:
        mid = (lo + hi) // 2
        if count_le(mid) >= k:
            hi = mid
        else:
            lo = mid + 1

    return str(lo)

# provided samples
assert run("2 2 2") == "2"
assert run("2 3 4") == "3"
assert run("1 10 5") == "5"

# custom cases
assert run("1 1 1") == "1", "single cell"
assert run("3 3 1") == "1", "minimum element"
assert run("3 3 9") == "9", "maximum element"
assert run("4 4 8") == "4", "duplicate-heavy region"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | lưới nhỏ nhất có thể | 
| 3 3 1 | 1 | lựa chọn cạnh tối thiểu | 
| 3 3 9 | 9 | ranh giới tối đa | 
| 4 4 8 | 4 | trùng lặp và đếm tầm trung | 

## Vỏ cạnh 

Một lưới nhỏ như`1 × m`hoạt động giống như một chuỗi đơn giản`1, 2, ..., m`. Thuật toán giảm chính xác vì mỗi hàng đóng góp nhiều nhất một phần tử và`min(m, x // i)`trở thành`min(m, x)`khi`i = 1`. 

Khi`k = n × m`, câu trả lời phải là tích lớn nhất`n × m`. Trong quá trình tìm kiếm nhị phân, chỉ các giá trị dưới mức tối đa này không đúng vị từ, do đó tìm kiếm hội tụ chính xác ở giới hạn trên. 

Khi nhiều sản phẩm lặp lại, chẳng hạn như nhiều cách để tạo thành`6`trong một`3 × 3`lưới, chức năng đếm sẽ bao gồm chính xác tất cả các lần xuất hiện vì mỗi lần xuất hiện hợp lệ`(i, j)`cặp được tính độc lập thông qua`min(m, x // i)`.
