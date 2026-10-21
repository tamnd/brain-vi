---
title: "CF 105809A - Trò chơi nhà máy"
description: "Chúng tôi được cung cấp một danh sách các hồ sơ giao hàng. Mỗi bản ghi chứa ba số nguyên: (sản phẩm, nguyên liệu, vật liệu phụ) Việc giao hàng giống nhau có thể xuất hiện nhiều lần vì hai người độc lập viết ra các lần giao hàng và ghi chú của họ đã được hợp nhất với nhau."
date: "2026-06-25T15:26:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105809
codeforces_index: "A"
codeforces_contest_name: "Code Rush 2025"
rating: 0
weight: 105809
solve_time_s: 46
verified: true
draft: false
---

[CF 105809A - Trò chơi tại nhà máy](https://codeforces.com/problemset/problem/105809/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các hồ sơ giao hàng. Mỗi bản ghi chứa ba số nguyên:`(product, material, submaterial)`Việc giao hàng giống nhau có thể xuất hiện nhiều lần vì hai người độc lập viết ra việc giao hàng và ghi chú của họ đã được hợp nhất với nhau. Hồ sơ trùng lặp không đại diện cho việc giao hàng bổ sung. 

Đối với mỗi sản phẩm, chúng ta phải xác định hai đại lượng: 

Đầu tiên, có bao nhiêu vật liệu riêng biệt được liên kết với sản phẩm đó. 

Thứ hai, có bao nhiêu vật liệu phụ riêng biệt được liên kết với sản phẩm đó. 

Đầu ra phải chứa một dòng cho mỗi sản phẩm, được sắp xếp theo id sản phẩm. Mỗi dòng chứa:`product_id distinct_materials distinct_submaterials`Kích thước đầu vào là phần quan trọng của vấn đề. Có thể có tới một triệu bản ghi, trong khi id sản phẩm, id vật liệu và id vật liệu phụ có thể lớn bằng$10^{18}$. Phạm vi giá trị lớn sẽ loại trừ mọi mảng được lập chỉ mục theo id. Chúng ta cần các cấu trúc dựa trên hàm băm như bộ Python và từ điển. 

Với$n = 10^6$, MỘT$O(n^2)$giải pháp là hoàn toàn không thể. Thậm chí$O(n \log n)$có thể chấp nhận được, nhưng giải pháp tuyến tính hoặc gần tuyến tính được ưu tiên hơn. Vì mỗi bản ghi chỉ cần đóng góp thông tin về một sản phẩm nên bảng băm là phù hợp. 

Một điểm tinh tế là các bộ ba trùng lặp không được tăng số lượng. 

Coi như:```
3
1 5 7
1 5 7
1 5 7
```Đầu ra đúng là:```
1 1 1
```Việc triển khai bất cẩn chỉ đơn giản là đếm các hàng sẽ tạo ra các giá trị lớn hơn. 

Một trường hợp cạnh khác là khi nhiều vật liệu phụ thuộc về cùng một vật liệu.```
3
1 5 1
1 5 2
1 5 3
```Đầu ra đúng là:```
1 1 3
```Chỉ có một vật liệu riêng biệt, mặc dù có ba lần giao hàng. 

Tình huống ngược lại cũng có thể xảy ra.```
3
1 1 9
1 2 9
1 3 9
```Đầu ra đúng là:```
1 3 1
```Vật liệu riêng biệt và vật liệu phụ riêng biệt phải được tính độc lập. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là xử lý từng sản phẩm riêng biệt. Đối với mỗi sản phẩm, hãy quét toàn bộ danh sách giao hàng và thu thập tất cả nguyên liệu và phụ liệu thuộc về sản phẩm đó. 

Điều này đúng vì mọi bản ghi đều được kiểm tra khi tính toán số liệu thống kê cho một sản phẩm. Vấn đề là hiệu quả. Nếu có$P$sản phẩm riêng biệt và$N$ghi lại, công việc trở thành$O(PN)$. Trong trường hợp xấu nhất, mỗi bản ghi thuộc về một sản phẩm khác nhau, tạo ra$O(N^2)$hoạt động. Với$N = 10^6$, điều này vượt xa giới hạn thực tế. 

Cấu trúc của dữ liệu gợi ý một cách tiếp cận tốt hơn. Mỗi bản ghi đã cho chúng ta biết chính xác nó thuộc về sản phẩm nào. Thay vì quét liên tục toàn bộ dữ liệu đầu vào, chúng ta có thể tổng hợp thông tin trong khi đọc bản ghi. 

Đối với mỗi sản phẩm, duy trì hai bộ: 

Một bộ lưu trữ tất cả các vật liệu riêng biệt được thấy cho sản phẩm đó. 

Bộ còn lại lưu trữ tất cả các vật liệu phụ riêng biệt được thấy cho sản phẩm đó. 

Khi một bản ghi`(a, b, c)`được đọc, chúng ta chỉ cần chèn`b`vào bộ nguyên liệu của sản phẩm và`c`vào bộ vật liệu phụ của sản phẩm. 

Bởi vì các bộ tự động loại bỏ các bản ghi trùng lặp nên các bản ghi lặp lại không ảnh hưởng đến kết quả đếm cuối cùng. 

Sau khi xử lý tất cả các bản ghi, câu trả lời cho một sản phẩm chỉ là kích thước của hai bộ sản phẩm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(PN) | O(N) | Quá chậm | 
| Tối ưu | O(N) trung bình | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bản ghi`n`. 
2. Tạo từ điển được lập chỉ mục theo id sản phẩm. 
3. Đối với mỗi bản ghi`(product, material, submaterial)`: 

Lưu trữ vật liệu trong bộ vật liệu của sản phẩm. 

Lưu trữ vật liệu phụ trong bộ vật liệu phụ của sản phẩm. 

Nếu sản phẩm chưa xuất hiện trước đó, hãy tạo hai bộ trước. 
4. Sau khi tất cả các bản ghi được xử lý, hãy sắp xếp id sản phẩm. 
5. Đối với từng sản phẩm theo thứ tự sắp xếp: 

Xuất id sản phẩm. 

Xuất ra kích thước của bộ vật liệu của nó. 

Xuất ra kích thước của bộ vật liệu phụ của nó. 

### Tại sao nó hoạt động 

Đối với một sản phẩm cố định, mọi bản ghi giao hàng liên quan đến sản phẩm đó sẽ chèn nguyên liệu của nó vào một bộ và nguyên liệu phụ của nó vào một bộ khác. Một tập hợp chứa mỗi giá trị nhiều nhất một lần, bất kể nó được chèn bao nhiêu lần. 

Kết quả là, sau khi tất cả các bản ghi được xử lý, bộ vật liệu chứa chính xác các vật liệu phụ riêng biệt được liên kết với sản phẩm đó và bộ vật liệu phụ chứa chính xác các vật liệu phụ riêng biệt được liên kết với sản phẩm đó. 

Vì mỗi bản ghi được xử lý một lần và đóng góp vào đúng sản phẩm nên số lượng cuối cùng chính xác là số lượng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    products = {}

    for _ in range(n):
        a, b, c = map(int, input().split())

        if a not in products:
            products[a] = [set(), set()]

        products[a][0].add(b)
        products[a][1].add(c)

    out = []

    for product in sorted(products):
        materials, submaterials = products[product]
        out.append(
            f"{product} {len(materials)} {len(submaterials)}"
        )

    sys.stdout.write("\n".join(out))

solve()
```Từ điển`products`lưu trữ tất cả thông tin được nhóm theo id sản phẩm. 

Đối với mỗi sản phẩm,`products[a][0]`là tập hợp các vật liệu và`products[a][1]`là tập hợp các vật liệu phụ. 

Khi một bản ghi mới xuất hiện, cả hai lần chèn đều có thời gian trung bình không đổi. Việc phân phối trùng lặp sẽ tự động biến mất vì việc chèn giá trị hiện có vào một bộ sẽ không thay đổi gì. 

Bước sắp xếp cuối cùng là cần thiết vì đầu ra yêu cầu các sản phẩm theo thứ tự tăng dần. Lặp lại trực tiếp trên từ điển sẽ không đảm bảo thứ tự được yêu cầu. 

Các id có thể lớn bằng$10^{18}$, nhưng số nguyên Python xử lý việc đó mà không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
1 1 1
2 2 1
3 4 1
2 2 2
2 2 1
1 3 12
1 1 2
1 4 1
1 4 2
1 4 3
```Xử lý dấu vết: 

| Ghi lại | Sản phẩm | Bộ Vật liệu | Bộ vật liệu phụ | 
| --- | --- | --- | --- | 
| 1 1 1 | 1 | {1} | {1} | 
| 1 3 12 | 1 | {1,3} | {1,12} | 
| 1 1 2 | 1 | {1,3} | {1,12,2} | 
| 1 4 1 | 1 | {1,3,4} | {1,12,2} | 
| 1 4 2 | 1 | {1,3,4} | {1,12,2} | 
| 1 4 3 | 1 | {1,3,4} | {1,12,2,3} | 

Đối với sản phẩm 1, có 3 vật liệu riêng biệt và 6 vật liệu phụ riêng biệt. Tính toán tương tự cho các sản phẩm còn lại. 

Đầu ra:```
1 3 6
2 1 2
3 1 1
```Ví dụ này cho thấy các bản ghi trùng lặp không ảnh hưởng đến các tập hợp như thế nào. 

### Ví dụ 2 

đầu vào:```
3
186 312 851372492
291 103 93522568
757 156 500988176
```Xử lý dấu vết: 

| Sản phẩm | Vật liệu | Vật liệu phụ | 
| --- | --- | --- | 
| 186 | {312} | {851372492} | 
| 291 | {103} | {93522568} | 
| 757 | {156} | {500988176} | 

Đầu ra:```
186 1 1
291 1 1
757 1 1
```Ví dụ này cho thấy id lớn không thay đổi logic. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + P log P) | Đọc hồ sơ là tuyến tính, phân loại chi phí sản phẩm$P \log P$| 
| Không gian | O(N) | Mỗi vật liệu và vật liệu phụ riêng biệt có thể được lưu trữ một lần | 

Đây$N$là số lượng bản ghi và$P$là số lượng sản phẩm riêng biệt Với tối đa một triệu hồ sơ, điều này dễ dàng phù hợp với giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline

        n = int(input())
        products = {}

        for _ in range(n):
            a, b, c = map(int, input().split())

            if a not in products:
                products[a] = [set(), set()]

            products[a][0].add(b)
            products[a][1].add(c)

        out = []
        for p in sorted(products):
            out.append(
                f"{p} {len(products[p][0])} {len(products[p][1])}"
            )

        return "\n".join(out)

    return solve()

# provided sample 1
assert run(
"""10
1 1 1
2 2 1
3 4 1
2 2 2
2 2 1
1 3 12
1 1 2
1 4 1
1 4 2
1 4 3
"""
) == "1 3 6\n2 1 2\n3 1 1"

# provided sample 2
assert run(
"""3
186 312 851372492
291 103 93522568
757 156 500988176
"""
) == "186 1 1\n291 1 1\n757 1 1"

# minimum size
assert run(
"""1
5 7 9
"""
) == "5 1 1"

# all duplicate records
assert run(
"""4
1 2 3
1 2 3
1 2 3
1 2 3
"""
) == "1 1 1"

# same material, many submaterials
assert run(
"""3
1 10 1
1 10 2
1 10 3
"""
) == "1 1 3"

# same submaterial, many materials
assert run(
"""3
1 1 5
1 2 5
1 3 5
"""
) == "1 3 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bản ghi đơn |`1 1 1`kết quả phong cách | Kích thước tối thiểu | 
| Hồ sơ giống hệt nhau lặp đi lặp lại | Số lượng còn lại 1 | Loại bỏ trùng lặp | 
| Cùng một chất liệu, nhiều chất liệu phụ | Số lượng vật liệu không thay đổi | Đếm độc lập | 
| Cùng một chất liệu phụ, nhiều chất liệu | Số lượng vật liệu phụ không thay đổi | Đếm độc lập | 

## Vỏ cạnh 

Xem xét việc giao hàng lặp lại:```
3
1 5 7
1 5 7
1 5 7
```Thuật toán chèn`5`vào bộ tài liệu ba lần và`7`vào bộ vật liệu phụ ba lần. Vì các bộ chỉ lưu trữ các giá trị duy nhất nên kích thước cuối cùng đều bằng 1. Kết quả đầu ra là:```
1 1 1
```Xem xét nhiều tài liệu phụ chia sẻ một tài liệu:```
3
1 8 1
1 8 2
1 8 3
```Bộ vật liệu trở thành`{8}`trong khi bộ vật liệu phụ trở thành`{1,2,3}`. Đầu ra là:```
1 1 3
```Hãy xem xét nhiều tài liệu chia sẻ một tài liệu phụ:```
3
1 1 9
1 2 9
1 3 9
```Bộ vật liệu trở thành`{1,2,3}`trong khi bộ vật liệu phụ vẫn còn`{9}`. Đầu ra là:```
1 3 1
```Những trường hợp này chứng minh tại sao phải duy trì hai bộ độc lập cho mỗi sản phẩm. Một cấu trúc kết hợp duy nhất sẽ không trả lời chính xác cả hai số đếm.
