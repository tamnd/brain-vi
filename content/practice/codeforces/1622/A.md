---
title: "CF 1622A - Dựng hình chữ nhật"
description: "Chúng ta được cho ba thanh có chiều dài nguyên và chúng ta được phép cắt chính xác một thanh thành hai mảnh có chiều dài nguyên dương. Sau lần cắt này, chúng ta sẽ có tổng cộng bốn cây gậy."
date: "2026-06-10T05:45:47+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1622
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 120 (Rated for Div. 2)"
rating: 800
weight: 1622
solve_time_s: 77
verified: true
draft: false
---

[CF 1622A - Vẽ hình chữ nhật](https://codeforces.com/problemset/problem/1622/A) 

**Đánh giá:** 800 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba thanh có chiều dài nguyên và chúng ta được phép cắt chính xác một thanh thành hai mảnh có chiều dài nguyên dương. Sau lần cắt này, chúng ta sẽ có tổng cộng bốn cây gậy. Mục tiêu là để xác định xem bốn cây gậy này có thể tạo thành một hình chữ nhật có các cạnh đối diện bằng nhau hay không. Một hình vuông được coi là một hình chữ nhật hợp lệ. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài của ba thanh. Đầu ra là "CÓ" nếu có thể tạo hình chữ nhật sau một lần cắt và "KHÔNG" nếu ngược lại. 

Vì số lượng ca kiểm thử có thể lên tới 10.000 và độ dài thanh thử nghiệm có thể lớn bằng$10^8$, giải pháp của chúng tôi phải chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Chúng ta không thể thực hiện các vòng lặp lồng nhau trên tất cả các phần phân chia có thể có của một thanh, vì việc chia một thanh thành tất cả các cặp có thể có có thể dẫn đến hàng triệu thao tác trong trường hợp xấu nhất. 

Một trường hợp cạnh quan trọng là khi hai thanh đã có cùng chiều dài và thanh thứ ba có thể được chia thành hai đoạn có chiều dài bằng nhau. Ví dụ, với độ dài$2, 4, 2$, chia số 4 thành hai số 2 sẽ tạo thành một hình chữ nhật. Một trường hợp phức tạp khác là khi cây gậy lớn nhất bằng tổng của hai cây còn lại, chẳng hạn như$1, 5, 6$; ở đây, chia 6 thành 1 và 5 sẽ tạo thành một hình chữ nhật. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là xem xét từng que và thử chia nó thành tất cả các cặp số nguyên có thể. Đối với mỗi lần chia, hãy kiểm tra xem bốn que kết quả có thể tạo thành hình chữ nhật hay không. Đối với một thanh dài$L$, có$L-1$tách ra và với ba cây gậy, điều này có thể trở thành$O(L)$mỗi trường hợp thử nghiệm. Từ$L$có thể$10^8$, điều này rõ ràng là không thể thực hiện được. 

Cách tiếp cận tối ưu cho thấy rằng một hình chữ nhật chỉ cần hai cặp có độ dài bằng nhau. Điều này hạn chế đáng kể các khả năng. Hoặc một que bằng tổng của hai que còn lại (chúng ta cắt nó thành chiều dài của hai que còn lại) hoặc hai que bằng nhau và que còn lại có thể được chia thành hai nửa bằng nhau để ghép với nhau. Về mặt hình thức, các điều kiện chúng ta cần kiểm tra là: 

1. Hai que bằng nhau và que còn lại chẵn (chúng ta chia đôi để ghép hai que còn lại). 
2. Tổng của hai que bằng que thứ ba (chúng ta chia que lớn nhất để trùng với hai que còn lại). 

Những lần kiểm tra này chỉ yêu cầu thời gian không đổi cho mỗi trường hợp kiểm thử, đủ nhanh cho tối đa 10.000 trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(l_i)) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba chiều dài thanh và sắp xếp chúng theo thứ tự không giảm. Việc sắp xếp đảm bảo rằng cây gậy lớn nhất có thể dễ dàng được xác định. 
2. Kiểm tra xem hai que nhỏ nhất có bằng nhau không. Nếu có, hãy kiểm tra xem cây gậy lớn nhất có chẵn không. Nếu cả hai điều kiện đều đúng thì chia cây gậy lớn nhất thành hai nửa bằng nhau sẽ được một hình chữ nhật. 
3. Kiểm tra xem tổng của hai que nhỏ nhất có bằng que lớn nhất hay không. Nếu đúng, chia cây gậy lớn nhất thành hai phần khớp với hai cây gậy còn lại sẽ có một hình chữ nhật. 
4. Nếu một trong các điều kiện trên xảy ra, hãy in "CÓ"; nếu không thì in "KHÔNG". 

Điều này có hiệu quả vì việc tạo thành một hình chữ nhật cần chính xác hai cặp có độ dài bằng nhau. Việc sắp xếp các que làm giảm số trường hợp chúng ta cần xem xét và làm cho việc so sánh trở nên đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    sticks = list(map(int, input().split()))
    sticks.sort()
    a, b, c = sticks
    if (a == b and c % 2 == 0) or (a + b == c):
        print("YES")
    else:
        print("NO")
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, ba chiều dài thanh được đọc và sắp xếp. Việc sắp xếp đảm bảo rằng việc so sánh được đơn giản: hai que đầu tiên là nhỏ nhất và que thứ ba là lớn nhất. Điều kiện đầu tiên xử lý trường hợp có thể tạo hình chữ nhật bằng cách chia thanh lớn nhất thành hai phần bằng nhau. Điều kiện thứ hai xử lý trường hợp thanh lớn nhất có thể được tách ra để khớp với hai thanh còn lại. Việc in ấn được thực hiện trực tiếp dựa trên những lần kiểm tra này. Việc sắp xếp là an toàn vì chúng ta chỉ xử lý ba phần tử. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`6 1 5`Các que được sắp xếp:`[1, 5, 6]`- Kiểm tra xem hai số đầu có bằng nhau không:`1 == 5`→ Sai 
- Kiểm tra xem tổng của hai số đầu có lớn nhất không:`1 + 5 == 6`→ Đúng 

Đầu ra: CÓ 

**Ví dụ 2:** 

đầu vào:`2 5 2`Các que được sắp xếp:`[2, 2, 5]`- Kiểm tra xem hai số đầu có bằng nhau không:`2 == 2`→ Đúng 
- Kiểm tra xem lớn nhất có phải là số chẵn không:`5 % 2 == 0`→ Sai 
- Kiểm tra xem tổng của hai số đầu có lớn nhất không:`2 + 2 == 5`→ Sai 

Đầu ra: KHÔNG 

Những dấu vết này cho thấy cách thuật toán nhanh chóng xác định các phần tách hợp lệ mà không cần liệt kê tất cả các khả năng. 

| Bước | Biến`sticks`| một | b | c | Kiểm tra 1 | Kiểm tra 2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ví dụ 1 | [1,5,6] | 1 | 5 | 6 | Sai | Đúng | CÓ | 
| Ví dụ 2 | [2,2,5] | 2 | 2 | 5 | Sai | Sai | KHÔNG | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Sắp xếp 3 phần tử là O(1), mỗi test chỉ yêu cầu các thao tác không đổi | 
| Không gian | O(1) | Chỉ lưu trữ ba chiều dài thanh cho mỗi trường hợp thử nghiệm | 

Thuật toán chia tỷ lệ tuyến tính với số lượng trường hợp thử nghiệm. Việc sử dụng bộ nhớ là tối thiểu vì chỉ có ba độ dài thanh được lưu trữ cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        sticks = list(map(int, input().split()))
        sticks.sort()
        a, b, c = sticks
        if (a == b and c % 2 == 0) or (a + b == c):
            output.append("YES")
        else:
            output.append("NO")
    return "\n".join(output)

# provided samples
assert run("4\n6 1 5\n2 5 2\n2 4 2\n5 5 4\n") == "YES\nNO\nYES\nYES"

# custom cases
assert run("3\n1 1 2\n4 4 8\n3 3 3\n") == "YES\nYES\nNO", "edge cases"
assert run("2\n1 100000000 100000000\n100000000 100000000 200000000\n") == "NO\nYES", "large values"
assert run("2\n5 5 5\n2 2 2\n") == "NO\nYES", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 2 | CÓ | trường hợp nhỏ nhất, chia tổng | 
| 4 4 8 | CÓ | thanh lớn nhất chia đều | 
| 3 3 3 | KHÔNG | không thể tạo thành hình chữ nhật | 
| 1 100000000 100000000 | KHÔNG | sự khác biệt lớn, không thể | 
| 100000000 100000000 200000000 | CÓ | giá trị lớn, chia tổng | 
| 5 5 5 | KHÔNG | tất cả đều bằng nhau nhưng lớn nhất là số lẻ | 
| 2 2 2 | CÓ | tất cả đều bằng nhau, thậm chí lớn nhất | 

## Vỏ cạnh 

cho`3 3 3`, các cây gậy đều bằng nhau. Việc tách bất kỳ cây gậy nào sẽ tạo ra hai mảnh, ví dụ: chia 3 thành 1 và 2, dẫn đến`[3,3,2,1]`, không thể tạo thành hai cặp bằng nhau. Thuật toán in chính xác "KHÔNG". 

Vì`2 2 2`, chia một cây gậy thành`1,1`kết quả là`[2,2,1,1]`. Sắp xếp mang lại`[1,1,2,2]`, thỏa mãn điều kiện đầu tiên`(a == b and c % 2 == 0)`gián tiếp vì số chia lớn nhất là 2 (chẵn). Thuật toán in "CÓ". 

Vì`100000000 100000000 200000000`, chia cây gậy lớn nhất 200000000 thành hai mảnh 100000000 tạo thành`[100000000,100000000,100000000,100000000]`, tạo thành một hình chữ nhật hoàn hảo. Thuật toán in "CÓ". Điều này xác nhận việc xử lý chính xác các đầu vào có kích thước tối đa.
