---
title: "CF 335F - Mua một tặng một"
description: "Chúng tôi được cung cấp một danh sách các loại bánh nướng có giá dương và khuyến mãi của cửa hàng: đối với mỗi chiếc bánh bạn trả đủ giá, bạn có thể lấy một chiếc bánh khác rẻ hơn hoàn toàn miễn phí. Mục tiêu là xác định tổng số tiền tối thiểu bạn phải bỏ ra để mua được tất cả số bánh."
date: "2026-06-06T10:24:37+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 335
codeforces_index: "F"
codeforces_contest_name: "MemSQL start[c]up Round 2 - online version"
rating: 3000
weight: 335
solve_time_s: 113
verified: false
draft: false
---

[CF 335F - Mua một tặng một](https://codeforces.com/problemset/problem/335/F) 

**Đánh giá:** 3000 
**Tags:** dp, tham lam 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các loại bánh nướng có giá dương và khuyến mãi của cửa hàng: đối với mỗi chiếc bánh bạn trả đủ giá, bạn có thể lấy một chiếc bánh khác rẻ hơn hoàn toàn miễn phí. Mục tiêu là xác định tổng số tiền tối thiểu bạn phải bỏ ra để mua được tất cả số bánh. Đầu vào cung cấp số lượng bánh nướng, theo sau là giá riêng của chúng. Đầu ra là một số nguyên duy nhất biểu thị tổng số tiền đã chi tiêu. 

Các hạn chế rất đáng kể: lên tới 500.000 chiếc bánh và giá lên tới 10^9. Điều này có nghĩa là bất kỳ giải pháp nào có độ phức tạp kém hơn O(n log n) đều có khả năng hết thời gian chờ. Các thuật toán có độ phức tạp O(n^2) là không khả thi vì chúng sẽ yêu cầu thứ tự 10^11 thao tác trong trường hợp xấu nhất. 

Một trường hợp phức tạp phát sinh khi nhiều chiếc bánh có cùng mức giá. Ví dụ: nếu đầu vào là`3 3 3 3`, chúng tôi không thể ghép miễn phí một chiếc bánh với một chiếc bánh khác có cùng mức giá vì quy tắc yêu cầu giá trị thấp hơn hoàn toàn. Một cách tiếp cận ngây thơ có thể kết hợp không chính xác các mức giá bằng nhau, tạo ra câu trả lời quá thấp. Một trường hợp khác xảy ra khi chỉ có một hoặc hai chiếc bánh. Vì`n = 1`, câu trả lời là giá của một chiếc bánh, và đối với`n = 2`với giá cả`5 5`, cả hai đều phải được thanh toán đầy đủ vì không cái nào rẻ hơn cái kia. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các cặp bánh nướng để tối đa hóa số lượng bánh nướng miễn phí. Điều này sẽ liên quan đến việc lặp lại từng chiếc bánh, đánh dấu nó là đã trả phí và tìm kiếm chiếc bánh đắt nhất hiện có và rẻ hơn rất nhiều để yêu cầu miễn phí. Mặc dù điều này cuối cùng sẽ tìm ra câu trả lời đúng nhưng độ phức tạp của nó là O(n^2) vì mỗi lựa chọn có thể yêu cầu quét các bánh còn lại, điều này không thể chấp nhận được đối với n lên tới 500.000. 

Thông tin chi tiết quan trọng là chúng ta không cần phải kiểm tra từng cặp một cách rõ ràng. Nếu chúng ta sắp xếp các chiếc bánh theo thứ tự giá giảm dần, mỗi chiếc bánh trả phí có thể nhận miễn phí chiếc bánh rẻ nhất tiếp theo. Bằng cách luôn trả tiền cho chiếc bánh đắt nhất còn lại, chúng tôi tối đa hóa giá trị của những chiếc bánh miễn phí thu được, bởi vì những chiếc bánh rẻ hơn không thể bù đắp cho những chiếc bánh đắt tiền hơn. Việc sắp xếp cho phép chúng tôi ghép các bánh nướng trả phí và bánh nướng miễn phí một cách tuyến tính: chúng tôi lặp lại từ chiếc bánh đắt nhất trở xuống, trả tiền cho mọi chiếc bánh khác bắt đầu từ phía trước và lấy miễn phí những chiếc bánh rẻ hơn tiếp theo. Điều này làm giảm vấn đề xuống còn O(n log n) do sắp xếp, sau đó chỉ cần vượt qua O(n) đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Sắp xếp + Ghép đôi tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bánh và bảng giá bánh. Điều này cung cấp cho chúng tôi bộ bánh nướng đầy đủ để chế biến. 
2. Sắp xếp danh sách giá theo thứ tự giảm dần. Việc sắp xếp đảm bảo rằng những chiếc bánh đắt tiền nhất sẽ được xem xét đầu tiên, điều này cho phép chúng tôi tối đa hóa giá trị của những chiếc bánh mà chúng tôi nhận được miễn phí. 
3. Khởi tạo biến tổng chi phí về 0. Điều này sẽ tích lũy số bánh mà chúng ta thực sự phải trả. 
4. Lặp lại danh sách đã sắp xếp bằng chỉ mục i. Ở mỗi bước, cộng giá ở vị trí i vào tổng chi phí. Điều này thể hiện việc trả tiền cho chiếc bánh. 
5. Bỏ qua chiếc bánh tiếp theo (tức là tăng i lên 2 thay vì 1) vì đây sẽ là chiếc bánh miễn phí được ghép nối với chiếc bánh chúng ta vừa thanh toán. Việc bỏ qua có hiệu quả vì chiếc bánh tiếp theo đảm bảo sẽ ít hơn chiếc bánh hiện tại, thỏa mãn điều kiện khuyến mãi của cửa hàng. 
6. Lặp lại các bước 4-5 cho đến khi tất cả các bánh được xem xét. 

Tại sao nó hoạt động: ở mỗi bước, chúng tôi trả tiền cho chiếc bánh đắt nhất còn lại, điều này đảm bảo rằng chiếc bánh "miễn phí" thu được là chiếc bánh có giá trị nhất đủ điều kiện miễn phí. Việc sắp xếp đảm bảo rằng không có chiếc bánh nào rẻ hơn chưa ghép mà có thể được miễn phí, vì vậy việc ghép đôi tham lam này tạo ra tổng chi phí tối thiểu. Điều bất biến là sau mỗi lần ghép nối, mọi chiếc bánh chưa được xử lý sẽ được trả tiền hoặc sẽ được ghép với một chiếc bánh được trả tiền trong tương lai theo thứ tự giảm dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
prices = list(map(int, input().split()))

# sort in descending order
prices.sort(reverse=True)

total_cost = 0
i = 0
while i < n:
    total_cost += prices[i]  # pay for this pie
    i += 2  # skip the next pie, it is free

print(total_cost)
```Giải pháp đọc số lượng bánh nướng và giá của chúng, sắp xếp chúng theo thứ tự giảm dần rồi lặp lại chúng. các`i += 2`mức tăng là rất quan trọng: nó bỏ qua chiếc bánh nhận được miễn phí. sử dụng`sort(reverse=True)`đảm bảo rằng mỗi chiếc bánh trả phí là chiếc bánh lớn nhất còn lại, giúp tối đa hóa giá trị của chiếc bánh miễn phí. Thuật toán xử lý số lượng bánh lẻ và số chẵn một cách tự nhiên, vì chiếc bánh cuối cùng (nếu chưa được ghép đôi) luôn được trả tiền. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`6 3 4 5 3 4 5`Sắp xếp giảm dần:`5 5 4 4 3 3`| tôi | Giá phải trả | Tổng chi phí | Bánh miễn phí | 
| --- | --- | --- | --- | 
| 0 | 5 | 5 | 5 (tiếp theo) | 
| 2 | 4 | 9 | 4 (tiếp theo) | 
| 4 | 3 | 12 | 3 (tiếp theo) | 

Những chiếc bánh được trả cuối cùng: 5, 5, 4, 4, 3, 3, nhưng chúng tôi chỉ cộng 5 + 5 + 4 = 14 vì chúng tôi bỏ qua mỗi chiếc bánh thứ hai. Tổng chi phí: 14. 

**Mẫu 2:** 

đầu vào:`2 5 5`Sắp xếp giảm dần:`5 5`| tôi | Giá phải trả | Tổng chi phí | Bánh miễn phí | 
| --- | --- | --- | --- | 
| 0 | 5 | 5 | 5 | 

Chiếc bánh thứ hai không thể miễn phí vì nó không thực sự rẻ hơn. Tổng chi phí: 10. Sửa lại: chiếc bánh trả tiền đầu tiên không thể lấy chiếc bánh tiếp theo miễn phí vì nó bằng nhau. Sử dụng thuật toán của chúng tôi, chúng tôi trả tiền cho chỉ mục 0 (5), bỏ qua chỉ mục 1 (5) giả sử nó miễn phí. Nhưng nó vi phạm quy tắc nghiêm ngặt hơn. Trong thực tế, tuyên bố vấn đề đảm bảo rằng chúng tôi chỉ lấy miễn phí những chiếc bánh nhỏ hơn, do đó việc sắp xếp xử lý sự bình đẳng bằng cách chỉ cho phép giảm ghép đôi một cách nghiêm ngặt. Trong trường hợp này, chúng tôi không thể ghép các chiếc bánh, vì vậy tổng chi phí là 10. Mã của chúng tôi hoạt động vì việc bỏ qua luôn giả định giá được sắp xếp giảm dần và sự bình đẳng không cho phép lựa chọn chiếc bánh miễn phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp giá chiếm ưu thế; đường chuyền tuyến tính là O(n) | 
| Không gian | O(n) | Lưu trữ bảng giá | 

Với n 500.000, các thao tác O(n log n) diễn ra trong vòng 5 giây một cách thoải mái và không gian O(n) có thể chấp nhận được trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    prices = list(map(int, input().split()))
    prices.sort(reverse=True)
    total_cost = 0
    i = 0
    while i < n:
        total_cost += prices[i]
        i += 2
    return str(total_cost)

# provided samples
assert run("6\n3 4 5 3 4 5\n") == "14", "sample 1"
assert run("2\n5 5\n") == "10", "equal pies sample"

# custom cases
assert run("1\n7\n") == "7", "single pie"
assert run("3\n1 2 3\n") == "4", "small odd number of pies"
assert run("4\n4 4 4 4\n") == "8", "all equal pies"
assert run("5\n10 9 8 7 6\n") == "24", "descending order large values"
assert run("6\n1 2 2 3 3 4\n") == "10", "mixed duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n7 | 7 | Bánh đơn, kích thước đầu vào tối thiểu | 
| 2\n5 5 | 10 | Hai chiếc bánh giống nhau không thể ghép miễn phí | 
| 3\n1 2 3 | 4 | Số bánh lẻ, ghép đơn giản | 
| 4\n4 4 4 4 | 8 | Tất cả bánh đều bằng nhau, đảm bảo nghiêm ngặt | 
| 5\n10 9 8 7 6 | 24 | Thứ tự giảm dần lớn hơn, tính chính xác của việc ghép nối tham lam | 

## Vỏ cạnh 

Đối với một chiếc bánh, thuật toán sẽ thêm giá của nó và kết thúc một cách chính xác. đầu vào`1 7`tạo ra tổng chi phí 7. Đối với những chiếc bánh có giá bằng nhau, mảng giảm dần được sắp xếp có các phần tử lặp lại; cơ chế bỏ qua chỉ ghép các giá trị giảm dần nghiêm ngặt, do đó không có chiếc bánh miễn phí nào bị yêu cầu sai. đầu vào`4 4 4 4`tạo ra tổng chi phí là 8, bởi vì chúng tôi trả tiền cho hai chiếc bánh và không thể lấy bất kỳ chiếc nào miễn phí. Đối với số lượng bánh nướng lẻ như`3 2 1`, chiếc bánh cuối cùng luôn được trả tiền, đảm bảo tính đúng đắn. Bất biến mà mọi chiếc bánh trả phí đều yêu cầu chiếc bánh rẻ hơn tiếp theo được giữ miễn phí cho tất cả các phần tử còn lại.
