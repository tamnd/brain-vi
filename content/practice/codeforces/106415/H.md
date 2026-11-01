---
title: "CF 106415H - Biểu thức cào"
description: "Chúng ta cần đếm xem có bao nhiêu chuỗi có độ dài cho trước tạo thành một biểu thức số học hợp lệ có giá trị là 0 modulo m. Chuỗi gốc được tạo thống nhất từ ​​bảng chữ cái gồm 15 ký tự: chữ số, ba toán tử nhị phân và hai dấu ngoặc đơn."
date: "2026-06-25T09:44:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106415
codeforces_index: "H"
codeforces_contest_name: "Winter Cup 8.0 Online Mirror Contest"
rating: 0
weight: 106415
solve_time_s: 44
verified: true
draft: false
---

[CF 106415H - Biểu thức cào](https://codeforces.com/problemset/problem/106415/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm xem có bao nhiêu chuỗi có độ dài cho trước tạo thành một biểu thức số học hợp lệ có giá trị là`0`modulo`m`. Chuỗi gốc được tạo thống nhất từ ​​bảng chữ cái gồm 15 ký tự: chữ số, ba toán tử nhị phân và hai dấu ngoặc đơn. Câu trả lời không phải là số lượng mà là xác suất, vì vậy sau khi đếm các chuỗi thắng hợp lệ, chúng ta chia cho`15^n`modulo`1e9+7`. 

Khó khăn xuất phát từ thực tế là tính hợp lệ không chỉ là tài sản cục bộ. Một chuỗi có thể thất bại vì dấu ngoặc đơn sai, vì toán tử không có toán hạng hoặc vì giá trị số học khác 0. Vì độ dài tối đa chỉ là 200 nên chúng ta có thể sử dụng giải pháp lập trình động theo độ dài, nhưng chúng ta phải tránh phân tích các chuỗi riêng lẻ. 

Độ dài 200 là đủ nhỏ để lập trình động đa thức. Nó loại trừ việc liệt kê vì có`15^200`các chuỗi có thể, nhưng nó cho phép các trạng thái liên quan đến mọi độ dài và mọi modulo dư`m`. Từ`m`tối đa là 100, việc lưu trữ số lượng biểu thức cho mỗi dư lượng là khả thi. 

Các trường hợp chính là các biểu thức trong đó cú pháp và giá trị tương tác với nhau. 

Ví dụ:```
1
```không có toán tử và là một biểu thức hợp lệ, nhưng câu trả lời chỉ tích cực khi`m = 1`, bởi vì giá trị là`1`. 

Một trường hợp khác:```
(0)
```có độ dài 3 và phải được tính là biểu thức chiến thắng cho mọi mô đun. Giải pháp chỉ xử lý số và toán tử mà quên dấu ngoặc đơn sẽ thua trường hợp này. 

Trường hợp thứ ba:```
00
```là một số hợp lệ, không phải hai biểu thức. Nó đánh giá bằng 0, vì vậy nó phải được tính. Trình phân tích cú pháp xử lý các chữ số liên tiếp dưới dạng mã thông báo riêng biệt sẽ từ chối nó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tạo ra mọi chuỗi có độ dài có thể`n`, kiểm tra xem đó có phải là biểu thức hợp lệ hay không, đánh giá nó và đếm những người chiến thắng. Điều này đúng vì mọi thẻ có thể đều được kiểm tra, nhưng số lượng chuỗi là`15^n`, điều này trở nên không thể ngay cả đối với rất nhỏ`n`. 

Quan sát hữu ích là các biểu thức hợp lệ có cấu trúc ngữ pháp. Chúng ta không cần các chuỗi thực tế, chỉ cần có bao nhiêu chuỗi với mỗi độ dài tạo ra mỗi dư lượng có thể có. 

Cú pháp biểu thức tự nhiên chia thành ba cấp độ. Một số là một chuỗi các chữ số. Một thừa số là một số hoặc một biểu thức được đặt trong ngoặc đơn. Một số hạng là một dãy các thừa số được nối với nhau bằng phép nhân. Biểu thức là một chuỗi các số hạng được nối bằng phép cộng hoặc phép trừ. 

Điều này phù hợp với các quy tắc ưu tiên của số học, vì vậy mọi chuỗi hợp lệ đều thuộc về chính xác một trong các danh mục này. 

Các trạng thái lập trình động là:`num[len][r]`: số chiều dài`len`có giá trị`r`.`fac[len][r]`: hệ số chiều dài`len`có giá trị`r`.`term[len][r]`: biểu thức nhân của độ dài`len`.`expr[len][r]`: biểu thức đầy đủ về độ dài`len`. 

Quá trình chuyển đổi sẽ phân chia một đối tượng dài hơn ở toán tử đầu tiên của nó. Ví dụ: một thuật ngữ có thể là một thừa số hoặc một thuật ngữ nhỏ hơn, một`*`, và một yếu tố khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(15^n) | O(n) | Quá chậm | 
| Lập trình động | O(n²m²) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số chuỗi chữ số có độ dài và dư lượng. Việc thêm một chữ số sẽ biến đổi một giá trị`x`vào trong`(10*x+d) mod m`, vì vậy chúng ta có thể xây dựng dần dần tất cả các trạng thái số. 
2. Xây dựng các yếu tố. Một thừa số là một số hoặc một biểu thức được bao quanh bởi dấu ngoặc đơn. Dấu ngoặc đơn thêm hai ký tự, do đó hệ số độ dài`i`có thể đến từ một biểu thức về độ dài`i-2`. 
3. Xây dựng các điều khoản. Bắt đầu với một yếu tố duy nhất. Để mở rộng một thuật ngữ, hãy thêm`*`và một yếu tố khác. Số hạng trước đó đã biểu thị vế trái của phép nhân, do đó, số hạng này tự động tuân theo tính kết hợp bên trái. 
4. Xây dựng biểu thức. Bắt đầu với một thuật ngữ. Để mở rộng một biểu thức, hãy nối thêm một trong hai`+`hoặc`-`và một thuật ngữ khác. Cả hai toán tử đều được xử lý riêng biệt vì phép trừ làm thay đổi phần dư thu được. 
5. Đối với mỗi độ dài được yêu cầu, hãy lấy`expr[n][0]`, vì chỉ có số dư bằng 0 mới là quân bài thắng. Chia cho tổng số chuỗi,`15^n`, sử dụng nghịch đảo mô đun. 

Tại sao nó hoạt động: mọi biểu thức hợp lệ đều có sự phân tách duy nhất thành các mức số, thừa số, số hạng và biểu thức. Quá trình chuyển đổi tuân theo chính xác các quy tắc ngữ pháp, vì vậy mọi chuỗi hợp lệ đều được tính một lần và mọi chuỗi không hợp lệ sẽ không bao giờ được tạo. Phần dư được lưu trữ chính xác là giá trị được tạo ra bằng cách đánh giá modulo biểu thức tương ứng`m`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve_case(m, queries):
    nmax = max(queries)
    
    def conv(a, b, op):
        res = [0] * m
        for i, x in enumerate(a):
            if x:
                for j, y in enumerate(b):
                    if y:
                        if op == 0:
                            k = (i + j) % m
                        else:
                            k = (i * j) % m
                        res[k] = (res[k] + x * y) % MOD
        return res

    num = [[0] * m for _ in range(nmax + 1)]
    num[0][0] = 1
    for length in range(1, nmax + 1):
        for r in range(m):
            if num[length - 1][r]:
                val = num[length - 1][r]
                for d in range(10):
                    num[length][(r * 10 + d) % m] = (
                        num[length][(r * 10 + d) % m] + val
                    ) % MOD

    fac = [[0] * m for _ in range(nmax + 1)]
    term = [[0] * m for _ in range(nmax + 1)]
    expr = [[0] * m for _ in range(nmax + 1)]

    for length in range(1, nmax + 1):
        fac[length] = num[length].copy()
        if length >= 2:
            for r in range(m):
                fac[length][r] = (fac[length][r] + expr[length - 2][r]) % MOD

        term[length] = fac[length].copy()
        for left in range(1, length - 1):
            right = length - left - 1
            add = conv(term[left], fac[right], 1)
            for r in range(m):
                term[length][r] = (term[length][r] + add[r]) % MOD

        expr[length] = term[length].copy()
        for left in range(1, length - 1):
            right = length - left - 1
            plus = conv(expr[left], term[right], 0)
            minus = [0] * m
            for i, x in enumerate(expr[left]):
                if x:
                    for j, y in enumerate(term[right]):
                        if y:
                            minus[(i - j) % m] = (
                                minus[(i - j) % m] + x * y
                            ) % MOD
            for r in range(m):
                expr[length][r] = (
                    expr[length][r] + plus[r] + minus[r]
                ) % MOD

    ans = []
    for n in queries:
        denominator = pow(pow(15, n, MOD), MOD - 2, MOD)
        ans.append(str(expr[n][0] * denominator % MOD))
    return ans

def main():
    t, m = map(int, input().split())
    queries = list(map(int, input().split()))
    print("\n".join(solve_case(m, queries)))

if __name__ == "__main__":
    main()
```Đầu tiên, mã xây dựng các con số vì tất cả các cấp độ ngữ pháp cao hơn đều phụ thuộc vào chúng. Việc chuyển đổi yếu tố sử dụng`expr[length - 2]`, đó là lý do tại sao độ dài được xử lý từ nhỏ đến lớn. 

Các lớp nhân và cộng sử dụng tích chập trên phần dư. Để nhân dư lượng mới là tích của hai dư lượng cũ. Đối với phép cộng và phép trừ, phần dư là tổng hoặc hiệu modulo`m`. 

Việc chuyển đổi cuối cùng sang xác suất chỉ được thực hiện sau khi quá trình đếm kết thúc. Mẫu số không bao giờ chia hết cho`1e9+7`, vì vậy đảo ngược mô-đun là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²m²) | Có sự phân chia ngữ pháp O(n2) và mỗi kết hợp dư lượng có giá O(m2). | 
| Không gian | O(nm) | Bốn bảng lập trình động lưu trữ độ dài và phần dư. | 

Với`n <= 200`Và`m <= 100`, không gian trạng thái đủ nhỏ để đạt được các giới hạn cần thiết. 

## Vỏ cạnh 

Đối với độ dài đầu vào ký tự đơn`1`, biểu thức hợp lệ duy nhất là các chữ số. Số DP tạo ra tất cả mười biểu thức có một chữ số có thể và việc kiểm tra số dư cuối cùng chỉ giữ lại những số bằng 0 theo modulo`m`. 

Đối với số 0 trong ngoặc đơn, chẳng hạn như`(0)`, quá trình chuyển đổi nhân tố thêm vào`expr[length - 2]`vào bảng nhân tố. Đây chính xác là quá trình chuyển đổi cần thiết để đếm dấu ngoặc đơn mà không coi chúng là toán tử số học. 

Đối với các số có nhiều chữ số có số 0 đứng đầu, chẳng hạn như`00`, số DP nối thêm các chữ số mà không hạn chế chữ số đầu tiên. Phần dư thu được được tính toán bình thường, do đó tất cả các trường hợp số 0 đứng đầu đều được tính đến.
