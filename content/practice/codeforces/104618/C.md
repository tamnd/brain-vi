---
title: "CF 104618C - Lựa chọn ngọt ngào"
description: "Chúng tôi được cung cấp ba danh sách chuỗi độc lập: hương vị kem có sẵn, lượng mưa phùn có sẵn và lớp phủ có sẵn. Một món tráng miệng hợp lệ bao gồm việc chọn đúng hai muỗng kem, sau đó chọn một loại kem và một loại topping."
date: "2026-06-29T17:29:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104618
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 1"
rating: 0
weight: 104618
solve_time_s: 68
verified: true
draft: false
---

[CF 104618C - Những lựa chọn ngọt ngào](https://codeforces.com/problemset/problem/104618/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp ba danh sách chuỗi độc lập: hương vị kem có sẵn, lượng mưa phùn có sẵn và lớp phủ có sẵn. Một món tráng miệng hợp lệ bao gồm việc chọn đúng hai muỗng kem, sau đó chọn một loại kem và một loại topping. Hai muỗng được chọn từ danh sách hương vị và được phép lặp lại, nhưng việc hoán đổi hai muỗng không tạo ra món tráng miệng mới. Sau khi sửa các muỗng, chúng tôi độc lập chọn một loại mưa phùn và một loại trên cùng. 

Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu món tráng miệng khác nhau theo những quy tắc này. Sự khác biệt hoàn toàn là sự kết hợp: hai món tráng miệng khác nhau nếu bất kỳ cặp hương vị, mưa phùn hoặc lớp phủ nào được chọn khác nhau. Vì mưa phùn và lớp phủ là những thành phần có một lựa chọn duy nhất nên chúng hoạt động giống như các hệ số nhân độc lập. 

Các ràng buộc rất nhỏ: mỗi danh sách có tối đa 100 phần tử. Điều này ngay lập tức ngụ ý rằng bất kỳ giải pháp nào lên tới khoảng vài triệu thao tác đều an toàn. Việc liệt kê tổ hợp bậc hai hoặc bậc ba đối với các hương vị có thể được chấp nhận, nhưng bất kỳ điều gì tệ hơn bậc ba vẫn có thể vượt qua do giới hạn. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác. 

Nếu chỉ có một hương vị, hãy nói`A`, thì cặp tin sốt dẻo hợp lệ duy nhất là`(A, A)`. Một cách tiếp cận ngây thơ chỉ xem xét các cặp hương vị riêng biệt sẽ trả về số 0 không chính xác. Ví dụ: đầu vào:```
A
B
C
```Đầu ra đúng là`1`, bởi vì chúng ta vẫn có thể chọn`(A, A)`. 

Nếu có nhiều hương vị nhưng được phép lựa chọn lặp lại, chúng tôi phải đảm bảo các cặp không có thứ tự sẽ không được tính hai lần. Ví dụ,`(A, B)`Và`(B, A)`phải được đối xử như một. 

Một trường hợp tinh tế khác là đảm bảo rằng chúng tôi bao gồm chính xác tất cả các cặp tự`(A, A)`cho mọi hương vị, không chỉ các cặp riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp rất đơn giản. Chúng tôi liệt kê mọi cặp hương vị có thể có, cho phép lặp lại và coi các cặp là không có thứ tự. Đối với mỗi cặp như vậy, chúng tôi chọn mưa phùn và lớp trên một cách độc lập. Nếu có`n`hương vị,`d`mưa phùn và`t`lớp phủ bên trên, lực lượng vũ phu sẽ lặp đi lặp lại trên tất cả`n × n`các cặp được sắp xếp, lọc các bản sao bằng cách thực thi ràng buộc về thứ tự và nhân với`d × t`. 

Điều này hoạt động chính xác nhưng cấu trúc nặng nề và che khuất không cần thiết. Quan sát quan trọng là mưa phùn và lớp phủ hoàn toàn độc lập với việc lựa chọn muỗng. Khi chúng ta đếm các cặp muỗng không có thứ tự hợp lệ, phần còn lại chỉ là phép nhân. 

Vì vậy, vấn đề giảm xuống việc đếm các kết hợp có lặp lại: số cách chọn 2 phần tử từ`n`hương vị mà thứ tự không quan trọng và được phép lặp lại. Đây là cách đếm tổ hợp cổ điển:$$\binom{n+1}{2} = \frac{n(n+1)}{2}$$Sau khi tính toán giá trị này, chúng tôi nhân nó với số lượng mưa phùn và lớp phủ. 

Điều này làm giảm toàn bộ vấn đề thành phân tích cú pháp đơn giản cộng với số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(n2 + d + t) | O(1) | Đã chấp nhận | 
| Công thức tổ hợp | O(n + d + t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách các hương vị và đếm xem có bao nhiêu hương vị. Gọi đây`n`. Đây là đặc tính duy nhất của hương vị mà chúng ta cần vì tên riêng lẻ không ảnh hưởng đến việc đếm. 
2. Đọc mưa phùn và tính chúng là`d`. 
3. Đọc phần trên và đếm chúng như`t`. 
4. Tính số cặp hương vị không có thứ tự được phép lặp lại theo công thức`n * (n + 1) // 2`. Điều này giải thích cho cả các cặp riêng biệt và các cặp tự ghép. 
5. Nhân số cặp hương vị với`d`Và`t`để có được câu trả lời cuối cùng. 
6. Xuất kết quả. 

Bước quan trọng là sử dụng nhận dạng tổ hợp cho nhiều bộ có kích thước 2. Lý do là mỗi món tráng miệng hợp lệ được xác định đầy đủ bằng cách chọn nhiều bộ có kích thước 2 từ các hương vị và sau đó chọn độc lập một loại mưa phùn và một loại topping. 

### Tại sao nó hoạt động 

Mỗi món tráng miệng tương ứng với chính xác một bộ ba: một bộ gồm hai hương vị, một vị mưa phùn và một lớp trên cùng. Thành phần hương vị độc lập với hai lựa chọn còn lại. Số lượng nhiều bộ có kích thước 2 được rút ra từ một`n`-bộ phần tử chính xác`n(n+1)/2`, vì chúng ta có thể chọn hai phần tử giống nhau trong`n`cách hoặc chọn hai yếu tố riêng biệt trong`n(n-1)/2`cách. Tổng hợp những điều này mang lại`n(n+1)/2`. Vì mỗi món tráng miệng hợp lệ được xác định duy nhất bởi một bộ nhiều món như vậy và một lựa chọn từ hai danh sách còn lại, nên việc nhân số lượng sẽ ra tổng số món tráng miệng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    flavors = input().split()
    drizzles = input().split()
    toppings = input().split()

    n = len(flavors)
    d = len(drizzles)
    t = len(toppings)

    scoop_pairs = n * (n + 1) // 2
    print(scoop_pairs * d * t)

if __name__ == "__main__":
    main()
```Việc triển khai chỉ dựa vào việc đếm mã thông báo trên mỗi dòng. Vì tên không liên quan ngoài danh tính nên việc tách từng dòng và lấy độ dài là đủ. 

Sự tinh tế duy nhất là sử dụng`n * (n + 1) // 2`còn hơn là`n * n`, điều này sẽ xử lý không chính xác`(A, B)`Và`(B, A)`như khác biệt. Phép chia số nguyên đảm bảo việc đếm tổ hợp chính xác mà không gặp rủi ro về dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Grass Licorice
Motor-Oil
Bone-Dust
```Chúng tôi tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| hương vị n | 2 | 
| mưa phùn d | 1 | 
| lớp phủ trên | 1 | 
| cặp muỗng | 2 * 3/2 = 3 | 
| kết quả | 3 | 

Điều này phù hợp với ba lựa chọn tin sốt dẻo không có thứ tự có thể có:`(Grass, Grass)`,`(Grass, Licorice)`,`(Licorice, Licorice)`, mỗi thứ kết hợp với một cơn mưa phùn và lớp trên cùng. 

### Ví dụ 2 

đầu vào:```
A B C
X Y
Z
```| Bước | Giá trị | 
| --- | --- | 
| hương vị n | 3 | 
| mưa phùn d | 2 | 
| lớp phủ trên | 1 | 
| cặp muỗng | 3 * 4/2 = 6 | 
| kết quả | 12 | 

Sáu phần ăn nhiều muỗng sẽ mở rộng thành 12 món tráng miệng khi mỗi phần được kết hợp với hai cơn mưa phùn. 

Dấu vết này cho thấy rằng việc tăng các tùy chọn mưa phùn sẽ có kết quả tuyến tính và độc lập với các kết hợp muỗng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + d + t) | Chúng tôi chỉ quét mỗi dòng đầu vào một lần để đếm token | 
| Không gian | O(1) | Chỉ bộ đếm được lưu trữ, không cần giữ danh sách | 

Các ràng buộc giới hạn mỗi danh sách ở 100 phần tử, do đó, quá trình này diễn ra trong thời gian thực tế không đổi. Ngay cả với đầu vào tối đa, các hoạt động vẫn không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

def main():
    flavors = input().split()
    drizzles = input().split()
    toppings = input().split()

    n = len(flavors)
    d = len(drizzles)
    t = len(toppings)

    scoop_pairs = n * (n + 1) // 2
    print(scoop_pairs * d * t)

# provided sample
assert run("Grass Licorice\nMotor-Oil\nBone-Dust\n") == "3"

# minimum case (1 flavor, 1 drizzle, 1 topping)
assert run("A\nB\nC\n") == "1"

# all equal flavor structure edge case still single flavor line
assert run("A\nB C\nD E F\n") == "1 * 2 * 3"

# two flavors, multiple extras
assert run("A B\nX Y Z\nQ\n") == str((2*3//2)*3*1)

# maximum-ish small sanity
assert run(" ".join(["F"]*100) + "\nD\nT\n") == str((100*101//2)*1*1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mỗi cái một | 1 | độ chính xác cấu hình tối thiểu | 
| 2 hương vị + nhiều tính năng bổ sung | tính toán | tính chính xác của việc ghép nối không có thứ tự | 
| 100 hương vị giống hệt nhau | 5050 | tính đúng đắn của ranh giới tổ hợp | 
| nhiều mưa phùn/topping | thu nhỏ | độc lập nhân | 

## Vỏ cạnh 

Đối với một hương vị duy nhất, chẳng hạn như:```
A
B
C
```chúng tôi có`n = 1`, vậy thì múc cặp =`1 * 2 / 2 = 1`. Thuật toán chính xác bao gồm`(A, A)`. Phép nhân với`d`Và`t`làm cho kết quả không thay đổi, tạo ra`1`. 

Cho hai hương vị:```
A B
X Y
Z
```thuật toán tính toán`n = 2`, cặp muỗng = 3. Đây là`(A, A)`,`(B, B)`,`(A, B)`. Mỗi món được kết hợp với một lớp phủ trên cùng và hai lớp kem phủ, tạo ra tổng cộng 6 món tráng miệng. Công thức chính xác tránh tính hai lần`(B, A)`bởi vì biểu thức tổ hợp đã thực thi lựa chọn không có thứ tự. 

Để lặp lại tối đa:```
F F F ... (100 times)
D
T
```mặc dù tất cả các tên đều giống nhau nhưng chúng vẫn được tính là các thành phần riêng biệt trong danh sách hương vị, vì vậy`n = 100`. Thuật toán tính toán`100 * 101 / 2 = 5050`, khớp với số lượng lựa chọn dựa trên chỉ mục của hai vị trí được phép lặp lại. Điều này xác nhận mô hình dựa trên các vị trí trong danh sách chứ không phải tính duy nhất của chuỗi vượt quá sự bình đẳng.
