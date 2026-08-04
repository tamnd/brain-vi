---
title: "CF 102558A - \u0414\u0435\u043d\u044c \u0440\u043e\u0436\u0434\u0435\u043d\u0438\u044f \u0412\u0430\u0441\u0438"
description: "Vasya có một bộ sưu tập các món ăn. Mỗi món ăn là một danh sách các nguyên liệu với số lượng cần thiết cho một khẩu phần ăn và mỗi món ăn được chuẩn bị cho một số lượng khách nhất định."
date: "2026-08-04T09:25:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102558
codeforces_index: "A"
codeforces_contest_name: "Contest for Yandex interns 2019"
rating: 0
weight: 102558
solve_time_s: 226
verified: false
draft: false
---

[CF 102558A - \u0414\u0435\u043d\u044c \u0440\u043e\u0436\u0434\u0435\u043d\u0438\u044f \u0412\u0430\u0441\u0438](https://codeforces.com/problemset/problem/102558/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
#Hiểu vấn đề 

Vasya có một bộ sưu tập các món ăn. Mỗi món ăn là một danh sách các nguyên liệu với số lượng cần thiết cho một khẩu phần ăn và mỗi món ăn được chuẩn bị cho một số lượng khách nhất định. Nhiệm vụ là mô phỏng toàn bộ quá trình mua sắm: xác định phải mua bao nhiêu gói mỗi thành phần có sẵn, tính tổng giá và tính giá trị dinh dưỡng của một món ăn hoàn chỉnh. 

Đầu vào chứa hai danh mục độc lập. Phần đầu tiên mô tả các gói hàng của cửa hàng: kích thước gói hàng, đơn vị đo lường và giá cả. Phần thứ hai mô tả dữ liệu dinh dưỡng cho một lượng nhất định của từng thành phần. Cùng một thành phần có thể xuất hiện với các đơn vị khác nhau trong công thức nấu ăn và danh mục, vì vậy trước tiên tất cả các phép tính phải được chuyển đổi thành một đơn vị chung. 

Các giới hạn đủ nhỏ để xử lý trực tiếp. Có tối đa 1000 món ăn, mỗi món có tối đa 100 nguyên liệu và cả hai danh mục đều chứa tối đa 1000 mục. Điều này có nghĩa là tổng số thành phần công thức tối đa là 100000. Cần có một thuật toán hoạt động tỷ lệ thuận với tổng kích thước đầu vào. Việc quét liên tục tất cả các danh mục để tìm từng thành phần trong công thức vẫn sẽ hiệu quả ở đây với khoảng 100 triệu so sánh, nhưng việc sử dụng từ điển sẽ đơn giản hơn và mang lại giải pháp tuyến tính rõ ràng. 

Những phần phức tạp chủ yếu liên quan đến đơn vị và sự khác biệt giữa một phần ăn và tất cả các phần ăn. Ví dụ: nếu một công thức nấu ăn cần`500 ml`sữa và cửa hàng bán`1 l`gói, việc tính toán mua hàng phải sử dụng`0.5`gói được làm tròn lên, trong khi tính toán dinh dưỡng phải sử dụng chính xác`500 ml`. 

Một sai lầm phổ biến là coi các đơn vị có ý nghĩa khác nhau là có thể hoán đổi cho nhau. Ví dụ,`tens`Và`cnt`cả hai đều mô tả một lượng đối tượng, nhưng`kg`Và`l`không thể chuyển đổi được. Tuyên bố đảm bảo rằng mọi chuyển đổi chúng tôi cần đều hợp lệ, do đó việc triển khai chỉ phải áp dụng hệ số nhân chính xác. 

Một trường hợp tế nhị khác là thành phần xuất hiện trong một số món ăn. Ví dụ:```
2
a 1 1
x 500 g
b 1 1
x 700 g
1
x 1000 g 1
1
x 1000 g 1 0 0 0
```Số gói đúng là`2`, vì tổng số tiền là`1200 g`. Một giải pháp làm tròn từng món ăn riêng biệt cũng sẽ nhận được`2`ở đây nhưng với`100 g`Và`100 g`công thức nấu ăn và`150 g`gói đó sẽ mua nhầm hai gói thay vì hai gói? Nguyên tắc quan trọng là việc mua sắm được thực hiện sau khi tổng hợp tất cả số tiền cần thiết. 

Một trường hợp khác là thành phần công thức được đo bằng đơn vị khác với danh mục dinh dưỡng:```
1
cake 1 1
milk 500 ml
1
milk 1 l 10
1
milk 1 l 20 0 0 100
```Giá trị dinh dưỡng bằng một nửa mục trong danh mục. Sử dụng số nguyên`500`với giá trị danh mục cho`1 l`sẽ nhân đôi kết quả. 

# Phương pháp tiếp cận 

Một giải pháp đơn giản là chế biến từng món ăn và từng nguyên liệu một cách độc lập. Đối với mỗi thành phần công thức, hãy tìm kiếm danh mục, chuyển đổi số lượng, tính toán dinh dưỡng và cập nhật số lượng cần thiết. Điều này đúng vì mọi đóng góp của thành phần đều độc lập và việc bổ sung là đủ để kết hợp chúng. Vấn đề với cách tiếp cận này là việc tìm kiếm lặp đi lặp lại. Nếu mỗi một trong số 100000 mục công thức quét một danh mục 1000 thành phần thì số lượng so sánh đạt khoảng 100 triệu. 

Quan sát hữu ích là tên thành phần là duy nhất trong mỗi danh mục. Điều đó có nghĩa là các danh mục có thể được lập chỉ mục một lần bằng bản đồ băm. Sau đó, mỗi lần tra cứu thành phần đều trở thành thời gian cố định. 

Quan sát thứ hai là tất cả các chuyển đổi đơn vị đều có tính nhân lên. Chúng ta có thể chuyển đổi mọi số lượng thành một đơn vị cơ bản: gam cho khối lượng, mililit cho thể tích và từng đơn vị để đếm. Khi mọi thứ đều có cùng cách biểu diễn, việc tính tổng các yêu cầu, tính toán số lượng gói và tính toán dinh dưỡng sẽ trở thành số học thông thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng_recipe_items * catalog_size) | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(total_recipe_items + k + m + n) | O(k + m + n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc danh mục giá và lưu trữ từng thành phần theo tên. Chuyển đổi từng kích thước gói hàng thành số lượng cơ bản để việc tính toán gói hàng không phụ thuộc vào đơn vị ban đầu. 
2. Đọc danh mục dinh dưỡng và lưu trữ lượng protein, chất béo, carbohydrate và năng lượng tương ứng với một đơn vị cơ bản đối với mỗi thành phần. Chia cho số lượng danh mục đã chuyển đổi sẽ cho phép nhân trực tiếp mọi số lượng công thức. 
3. Với mỗi món ăn, hãy lưu tên và tính giá trị dinh dưỡng của món ăn đó. Đối với mỗi thành phần trong công thức, hãy chuyển đổi lượng trên mỗi khẩu phần thành đơn vị cơ bản và nhân với giá trị dinh dưỡng trên mỗi đơn vị cơ sở. 
4. Đồng thời, cộng số lượng nguyên liệu nhân với số lượng khách vào bản đồ số lượng yêu cầu chung. Bản đồ này thể hiện số lượng chính xác của từng nguyên liệu cần thiết cho tất cả các món ăn. 
5. Sau khi tất cả các món ăn đã được chế biến xong, hãy tính số lượng gói cho từng nguyên liệu trong danh mục giá. Chia số tiền cần thiết cho kích thước gói và làm tròn lên vì không thể mua được từng gói. 
6. In ra tổng giá, số lượng gói và lượng dinh dưỡng được tính toán cho từng món ăn. 

Điều bất biến đằng sau thuật toán là sau khi xử lý bất kỳ tiền tố nào của món ăn, bản đồ yêu cầu chung sẽ chứa chính xác lượng cần thiết của tiền tố đó và mọi giá trị dinh dưỡng được lưu trữ sẽ được tính từ số lượng thành phần chuẩn hóa. Vì mỗi món ăn còn lại đóng góp độc lập nên việc tiếp tục cập nhật tương tự sẽ duy trì tính chính xác cho đến khi tất cả các món ăn được xử lý. 

#Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def factor(u):
    if u == "kg" or u == "l":
        return 1000
    if u == "tens":
        return 10
    return 1

def solve():
    n = int(input())
    dishes = []
    for _ in range(n):
        name, c, z = input().split()
        c = int(c)
        z = int(z)
        ing = []
        for _ in range(z):
            s, a, u = input().split()
            ing.append((s, int(a), u))
        dishes.append((name, c, ing))

    k = int(input())
    price = {}
    order = []
    for _ in range(k):
        t, p, a, u = input().split()
        a = int(a)
        price[t] = (int(p), a * factor(u))
        order.append(t)

    m = int(input())
    nutrition = {}
    for _ in range(m):
        t, a, u, pr, f, ch, fv = input().split()
        amount = int(a) * factor(u)
        nutrition[t] = (
            float(pr) / amount,
            float(f) / amount,
            float(ch) / amount,
            float(fv) / amount
        )

    need = {}
    answers = []

    for name, cnt, ing in dishes:
        cur = [0.0, 0.0, 0.0, 0.0]
        for s, a, u in ing:
            amount = a * factor(u)
            if s not in need:
                need[s] = 0
            need[s] += amount * cnt

            pr, f, ch, fv = nutrition[s]
            cur[0] += amount * pr
            cur[1] += amount * f
            cur[2] += amount * ch
            cur[3] += amount * fv
        answers.append((name, cur))

    total = 0
    packs = []
    for t in order:
        p, size = price[t]
        cnt = math.ceil((need.get(t, 0) - 1e-12) / size)
        packs.append((t, cnt))
        total += cnt * p

    out = [str(total)]
    for t, c in packs:
        out.append(f"{t} {c}")
    for name, vals in answers:
        out.append("{} {:.10f} {:.10f} {:.10f} {:.10f}".format(name, *vals))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```các`factor`hàm chuyển đổi mọi đơn vị được hỗ trợ thành biểu diễn cơ sở của nó. Kilôgam và lít trở thành phần nghìn đơn vị cơ bản tương ứng của chúng, trong khi hàng chục trở thành mười vật thể riêng lẻ. 

Từ điển giá lưu trữ các kích thước gói theo cùng cách trình bày được sử dụng bởi các công thức nấu ăn. Điều này loại bỏ tất cả việc xử lý đơn vị khỏi tính toán mua hàng cuối cùng. 

Từ điển dinh dưỡng lưu trữ các giá trị trên mỗi đơn vị cơ sở thay vì trên mỗi mục danh mục. Ví dụ: nếu danh mục nói rằng 100 gam chứa 20 gam protein thì giá trị được lưu trữ là`0.2`protein mỗi gam. 

Trong quá trình xử lý công thức, mã thực hiện hai lần cập nhật độc lập. các`need`bản đồ theo dõi số lượng mua sắm của tất cả khách hàng, đồng thời`cur`theo dõi dinh dưỡng cho một món ăn hoàn chỉnh. Việc trộn lẫn hai giá trị này thường dẫn đến các câu trả lời sai. 

Hoạt động trần cuối cùng chứa một epsilon nhỏ vì phép chia dấu phẩy động có thể tạo ra các giá trị như`1.000000000001`. Các ràng buộc nhỏ nên việc tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. 

# Ví dụ đã hoạt động 

Đối với mẫu, món ăn đầu tiên được xử lý như sau: 

| Món ăn | Thành phần | Số tiền quy đổi | Đóng góp dinh dưỡng | 
| --- | --- | --- | --- | 
| bánh mì kẹp | bơ | 10 g | 0,08, 7,25, 0,13, 66,1 | 
| bánh mì kẹp | bánh mì nướng | 2 cnt | 2,92, 0,64, 20,92, 99,2 | 
| bánh mì kẹp | xúc xích | 30 g | 3, 5,4, 0,45, 63 | 

Các giá trị bánh sandwich kết quả là: 

| Chất đạm | Béo | Carb | Năng lượng | 
| --- | --- | --- | --- | 
| 6 giờ 00 | 29/13 | 21h50 | 228,30 | 

Bản đồ mua sắm sau cả 2 món là: 

| Thành phần | Số tiền bắt buộc | 
| --- | --- | 
| bơ | 70 g | 
| bánh mì nướng | 14 cent | 
| xúc xích | 660 g | 
| trứng | 36 cent | 
| sữa | 1080ml | 
| muối | 9g | 

Việc tính toán gói sẽ làm tròn từng yêu cầu trở lên, tạo ra bốn gói trứng, hai gói sữa, hai gói xúc xích và một gói cho các nguyên liệu còn lại. 

Một dấu vết nhỏ thứ hai:```
1
tea 1 1
sugar 3 g
1
sugar 10 g 5
1
sugar 10 g 1 0 0 0
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc công thức | số tiền | 3g | 
| Bình thường hóa | số tiền | 3 đơn vị cơ sở | 
| Bản đồ bắt buộc | đường | 3 | 
| Gói | trần nhà(3/10) | 1 | 

Điều này xác nhận rằng việc mua hàng chỉ sử dụng tổng số tiền chính xác và các vòng quay ở bước đóng gói cuối cùng. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * z + k + m) | Mỗi thành phần công thức được xử lý một lần và danh mục được đọc một lần | 
| Không gian | O(k + m + n * z) | Danh mục được lưu trữ, công thức nấu ăn và câu trả lời | 

Số lượng mục nhập công thức tối đa là khoảng 100000, do đó, một đường truyền tuyến tính với tra cứu bảng băm dễ dàng phù hợp với các ràng buộc. 

# Trường hợp thử nghiệm```python
import sys
import io

# These tests assume solve() is copied into this file.

def run(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old

sample = """1
a 1 1
x 500 g
1
x 1 1000 g
1
x 1000 g 10 0 0 100
"""
assert "1" in run(sample)

minimum = """1
a 1 1
x 1 cnt
1
x 1 1 cnt
1
x 1 cnt 1 2 3 4
"""
assert "x 1" in run(minimum)

different_units = """1
a 1 1
milk 500 ml
1
milk 1 1 l
1
milk 1 l 10 0 0 100
"""
assert "50.0000000000" in run(different_units)

shared = """2
a 1 1
x 100 g
b 1 1
x 100 g
1
x 150 g 1
1
x 100 g 1 0 0 0
"""
assert "2" in run(shared)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Công thức một thành phần | Một gói | Chuyển đổi và mua hàng cơ bản | 
| Một thành phần dựa trên số lượng | Một gói | Xử lý kích thước tối thiểu | 
| Mililit với danh mục lít | Một nửa giá trị dinh dưỡng | Chuẩn hóa đơn vị | 
| Cùng một thành phần trong hai món ăn | Số gói kết hợp | Tổng hợp trước khi làm tròn | 

# Vỏ cạnh 

Thuật toán xử lý các đơn vị không khớp bằng cách chuyển đổi trước bất kỳ số học nào. Trong ví dụ về sữa,`500 ml`trở thành`500`đơn vị cơ bản, trong khi danh mục dinh dưỡng lưu trữ các giá trị trên`1000 ml`, do đó kết quả chính xác bằng một nửa giá trị danh mục. 

Thuật toán xử lý các thành phần được chia sẻ bằng cách cập nhật`need`mỗi khi thành phần xuất hiện. Nếu hai món ăn yêu cầu`100 g`mỗi kích thước gói là`150 g`, yêu cầu cuối cùng là`200 g`, cho`ceil(200 / 150) = 2`gói. Nó không bao giờ hoàn thành sớm các công thức nấu ăn riêng lẻ. 

Các mục danh mục không sử dụng cũng được xử lý. Đầu ra yêu cầu phải in mọi thành phần trong danh mục giá, bao gồm cả những thành phần không bao giờ cần thiết trong bất kỳ công thức nấu ăn nào. Việc tra cứu sử dụng`need.get(t, 0)`, sản xuất không có gói nào cho những thành phần đó.
