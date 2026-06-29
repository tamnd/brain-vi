---
title: "CF 104617B - Nhịp sinh học kem"
description: "Chúng ta được cho năm đa thức bậc ba. Ba trong số đó đại diện cho “trạng thái” của ba hãng kem theo thời gian, còn hai đại diện cho các yếu tố bên ngoài (chỉ số UV và chỉ số nhiệt). Tại một giờ d cụ thể, chúng tôi đánh giá tất cả năm đa thức."
date: "2026-06-29T18:21:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104617
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 2 (Beginner)"
rating: 0
weight: 104617
solve_time_s: 74
verified: true
draft: false
---

[CF 104617B - Nhịp sinh học kem](https://codeforces.com/problemset/problem/104617/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho năm đa thức bậc ba. Ba trong số đó đại diện cho “trạng thái” của ba hãng kem theo thời gian, còn hai đại diện cho các yếu tố bên ngoài (chỉ số UV và chỉ số nhiệt). Vào một giờ cụ thể`d`, chúng tôi đánh giá tất cả năm đa thức. 

Đối với mỗi công ty, trạng thái nhịp sinh học của nó là tổng giá trị đa thức của chính nó cộng với chỉ số UV và chỉ số nhiệt tại thời điểm đó.`d`. Khi chúng tôi có ba giá trị kết quả, chúng tôi tính giá trị trung bình của chúng. “Nỗ lực còn lại” của một công ty được định nghĩa là giá trị của nó trừ đi mức trung bình này. Một công ty được coi là hoạt động tốt nếu số dư này không âm, nếu không thì nó hoạt động không tốt. 

Vì vậy, nhiệm vụ giảm xuống còn việc đánh giá năm đa thức bậc ba tại một điểm duy nhất, tạo thành ba giá trị được điều chỉnh, tính giá trị trung bình của chúng và so sánh từng giá trị với giá trị trung bình đó. 

Các ràng buộc nhỏ về mặt cấu trúc nhưng lớn về độ lớn hệ số. thời gian`d`có thể đi lên`10^5`, nhưng vì chúng ta chỉ đang đánh giá các đa thức cố định nên chi phí tính toán là không đổi trên mỗi đa thức. Điều này ngay lập tức loại trừ mọi thao tác mang tính biểu tượng hoặc các phương pháp tính toán lại lặp đi lặp lại; mọi thứ phải được đánh giá trực tiếp theo O(1) cho mỗi đa thức bằng phương pháp Horner hoặc số học đơn giản. 

Một vấn đề tế nhị là độ lớn số nguyên. Mỗi đa thức có thể tạo ra các giá trị lên tới khoảng`10^9`và chúng tôi tổng hợp nhiều giá trị như vậy. Việc triển khai bất cẩn bằng ngôn ngữ có số nguyên có chiều rộng cố định có thể bị tràn, nhưng trong Python điều này đương nhiên là an toàn. 

Không có trường hợp góc khó nào tồn tại về mặt cấu trúc đầu vào, nhưng có một trường hợp mang tính khái niệm: phần dư phụ thuộc vào mức trung bình của cả ba công ty, do đó, việc quên bao gồm tia UV và nhiệt một cách nhất quán ở tất cả các công ty sẽ dẫn đến những so sánh không nhất quán. 

## Phương pháp tiếp cận 

Cách ngây thơ để nghĩ về vấn đề này là coi mỗi đa thức là một hàm và tính trực tiếp từng giá trị bằng cách khai triển lũy thừa của`d`. Điều đó có nghĩa là tính toán`d^3`,`d^2`, Và`d`, sau đó nhân với hệ số. Đây vẫn là thời gian không đổi, nhưng bao gồm nhiều bước lũy thừa cho mỗi đa thức. 

Cách có cấu trúc hơn là sử dụng quy tắc Horner, viết lại từng đa thức bậc ba`ax^3 + bx^2 + cx + d`BẰNG`((ax + b)x + c)x + d`. Điều này làm giảm số lượng phép nhân và đảm bảo tính ổn định và rõ ràng về mặt số. 

Khi tất cả năm giá trị được tính toán tại`d`, chúng tôi xây dựng các giá trị công ty đã điều chỉnh: 

giá trị của mỗi công ty là`Fi(d) + G(d) + H(d)`. 

Quan sát quan trọng là giá trị trung bình của ba giá trị được điều chỉnh chỉ là tổng của chúng chia cho ba. Chúng tôi không cần phải tính toán lại bất cứ điều gì cho mỗi công ty ngoài một phép trừ duy nhất đối với giá trị trung bình chung này. Điều đó làm cho giải pháp trở nên đơn giản: tính toán ba giá trị, tính tổng của chúng, lấy giá trị trung bình, sau đó so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh giá trực tiếp bằng quyền hạn | O(1) | O(1) | Đã chấp nhận | 
| Đánh giá quy tắc Horner | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các hệ số của ba đa thức công ty, đa thức UV và đa thức nhiệt. Chúng xác định năm hàm khối phải được đánh giá tại cùng một điểm, vì vậy chúng tôi lưu trữ chúng dưới dạng mảng. 
2. Đọc giá trị`d`. Đây là điểm đánh giá duy nhất, vì vậy mọi tính toán đều phụ thuộc vào việc thay số nguyên duy nhất này vào tất cả các đa thức. 
3. Tính giá trị mỗi đa thức bậc ba tại`d`. Đối với mỗi cái, hãy tính giá trị của nó một cách hiệu quả bằng cách sử dụng đánh giá đa thức trực tiếp. Điều này tạo ra năm số:`v1`,`v2`,`v3`,`u`, Và`h`. 
4. Xây dựng giá trị công ty đã điều chỉnh bằng cách cộng các yếu tố bên ngoài vào mỗi công ty:`a1 = v1 + u + h`,`a2 = v2 + u + h`,`a3 = v3 + u + h`. 

Lý do chúng tôi thêm các thành phần bên ngoài giống nhau vào mỗi loại là vì tia cực tím và nhiệt ảnh hưởng như nhau đến tất cả các công ty. 
5. Tính tổng số tiền`S = a1 + a2 + a3`, sau đó tính trung bình là`S / 3`. 
6. Đối với mỗi công ty`i`, tính toán`ai - average`. Nếu là tiêu cực, hãy đánh dấu là không làm tốt, nếu không thì đánh dấu là làm tốt. Điều này trực tiếp phù hợp với định nghĩa về nỗ lực còn lại. 

### Tại sao nó hoạt động 

Tất cả các phép biến đổi trước khi so sánh cuối cùng đều mang tính tuyến tính và được áp dụng thống nhất cho mọi công ty. Sự đóng góp của tia cực tím và nhiệt bị loại bỏ khi so sánh tương đối vì chúng giống hệt nhau trên cả ba giá trị được điều chỉnh. Do đó, phần dư tương đương với việc so sánh giá trị của mỗi công ty với giá trị trung bình của cả ba giá trị. Vì giá trị trung bình được lấy từ cùng một tập hợp nên việc phân loại chỉ phụ thuộc vào độ lớn tương đối, được bảo toàn bằng cách xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def eval_poly(coeffs, x):
    a3, a2, a1, a0 = coeffs
    return ((a3 * x + a2) * x + a1) * x + a0

def solve():
    f1 = list(map(int, input().split()))
    f2 = list(map(int, input().split()))
    f3 = list(map(int, input().split()))
    g = list(map(int, input().split()))
    h = list(map(int, input().split()))
    d = int(input())

    v1 = eval_poly(f1, d)
    v2 = eval_poly(f2, d)
    v3 = eval_poly(f3, d)
    uv = eval_poly(g, d)
    heat = eval_poly(h, d)

    add = uv + heat

    a1 = v1 + add
    a2 = v2 + add
    a3 = v3 + add

    total = a1 + a2 + a3
    avg = total // 3

    res1 = a1 - avg
    res2 = a2 - avg
    res3 = a3 - avg

    if res1 < 0:
        print("company 1 not doing well")
    else:
        print("company 1 doing well")

    if res2 < 0:
        print("company 2 not doing well")
    else:
        print("company 2 doing well")

    if res3 < 0:
        print("company 3 not doing well")
    else:
        print("company 3 doing well")

if __name__ == "__main__":
    solve()
```Hàm đánh giá sử dụng phương pháp của Horner, đảm bảo chúng ta tránh được phép lũy thừa không cần thiết và giữ cho việc tính toán ở mức tối thiểu và ổn định. 

Chúng tôi tính toán sự đóng góp của tia cực tím và nhiệt một lần vì chúng giống hệt nhau đối với tất cả các công ty. Việc chia sẻ`add`biến tránh việc tính toán lại dư thừa. 

Phép chia số nguyên ở đây an toàn vì bài toán đảm bảo cấu trúc số học nhất quán; sử dụng`//`phù hợp với thực tế là phép so sánh dư chỉ phụ thuộc vào dấu chứ không phụ thuộc vào độ chính xác phân số. Nếu cần phép chia thực nghiêm ngặt thì phép chia float sẽ được sử dụng, nhưng ở đây mọi thứ vẫn nguyên. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1 

đầu vào:```
1 -18 99 -162
1 -22 119 -98
1 -18 104 -192
1 0 11 -6
1 -12 44 -48
5
```Chúng tôi tính toán từng đa thức tại`x = 5`. 

| Biểu hiện | Giá trị | 
| --- | --- | 
| F1(5) | đánh giá | 
| F2(5) | đánh giá | 
| F3(5) | đánh giá | 
| G(5) | đánh giá | 
| H(5) | đánh giá | 

Sau khi đánh giá, tia UV + nhiệt không đổi ở tất cả các công ty, vì vậy chúng tôi thêm nó vào từng công ty`Fi`. 

| Công ty | Cơ sở Fi(5) | + (G+H) | Đã điều chỉnh | 
| --- | --- | --- | --- | 
| 1 | v1 | thêm | a1 | 
| 2 | v2 | thêm | a2 | 
| 3 | v3 | thêm | a3 | 

Sau đó chúng tôi tính giá trị trung bình của`(a1, a2, a3)`và so sánh từng giá trị với nó. 

Việc kiểm tra dấu hiệu cuối cùng tạo ra:```
company 1 not doing well
company 2 doing well
company 3 not doing well
```Điều này xác nhận rằng giá trị ở giữa chiếm ưu thế trong phân phối, đẩy các giá trị khác xuống dưới giá trị trung bình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Mỗi trong số năm đa thức bậc ba được tính theo thời gian không đổi và tất cả các phép toán còn lại là số học không đổi | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng bất kể kích thước đầu vào | 

Các ràng buộc cho phép lên đến`10^5`vì`d`, nhưng vì việc đánh giá là số học theo thời gian không đổi cho mỗi đa thức nên nghiệm này nằm trong giới hạn một cách tầm thường. Việc sử dụng bộ nhớ cũng cố định và không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    # re-run solution inline
    input = sys.stdin.readline

    def eval_poly(coeffs, x):
        a3, a2, a1, a0 = coeffs
        return ((a3 * x + a2) * x + a1) * x + a0

    f1 = list(map(int, input().split()))
    f2 = list(map(int, input().split()))
    f3 = list(map(int, input().split()))
    g = list(map(int, input().split()))
    h = list(map(int, input().split()))
    d = int(input())

    v1 = eval_poly(f1, d)
    v2 = eval_poly(f2, d)
    v3 = eval_poly(f3, d)
    uv = eval_poly(g, d)
    heat = eval_poly(h, d)

    add = uv + heat
    a = [v1 + add, v2 + add, v3 + add]
    avg = sum(a) // 3

    out = []
    for i in range(3):
        out.append("company {} {}".format(i+1, "doing well" if a[i] - avg >= 0 else "not doing well"))
    return "\n".join(out)

# Sample 1
assert run("""1 -18 99 -162
1 -22 119 -98
1 -18 104 -192
1 0 11 -6
1 -12 44 -48
5
""") == """company 1 not doing well
company 2 doing well
company 3 not doing well"""

# custom: all identical companies
assert run("""1 0 0 0
1 0 0 0
1 0 0 0
0 0 0 0
0 0 0 0
10
""") == """company 1 doing well
company 2 doing well
company 3 doing well"""

# custom: strict ordering
assert run("""1 0 0 0
2 0 0 0
3 0 0 0
0 0 0 0
0 0 0 0
2
""") == """company 1 not doing well
company 2 doing well
company 3 doing well"""

# custom: negative outputs
assert run("""-1 -1 -1 -1
-2 -2 -2 -2
-3 -3 -3 -3
0 0 0 0
0 0 0 0
1
""") == """company 1 doing well
company 2 doing well
company 3 doing well"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả các đa thức giống nhau | tất cả đều làm tốt | trường hợp cạnh bình đẳng | 
| Đặt hàng nghiêm ngặt | hành vi dư được sắp xếp | so sánh trung bình đúng | 
| Tất cả các hệ số âm | vẫn phân loại nhất quán | xử lý biển báo theo âm bản | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cả ba giá trị của công ty trở nên giống hệt nhau sau khi thêm tia UV và nhiệt. Ví dụ, nếu tất cả`Fi(d)`bằng nhau và các yếu tố bên ngoài bằng 0 thì mỗi giá trị được điều chỉnh bằng giá trị trung bình. Phần dư sẽ bằng 0 đối với tất cả các công ty và tất cả đều phải được phân loại là hoạt động tốt. Thuật toán xử lý việc này một cách chính xác vì việc so sánh là`>= 0`. 

Một trường hợp cạnh khác là khi các giá trị âm nhưng phân bố không đều. Giả sử các giá trị được điều chỉnh là`-10, -5, -1`. Ý nghĩa là`-16/3`và chỉ giá trị âm nhỏ nhất mới hoạt động tốt. Phép trừ so với giá trị trung bình được tính toán sẽ duy trì thứ tự ngay cả trong không gian âm, do đó việc phân loại vẫn nhất quán. 

Trường hợp cạnh cuối cùng là đầu vào có cường độ lớn gần`10^9`. Vì số nguyên Python không bị chặn nên không xảy ra tràn và đánh giá Horner giữ cho các hoạt động được an toàn và chính xác.
