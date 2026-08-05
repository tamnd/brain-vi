---
title: "CF 102788G - Alice Và Bob"
description: "Trò chơi được chơi trên một hàng số nguyên dương. Alice di chuyển đầu tiên. Trong một lượt, người chơi chọn hai số lân cận có ước chung lớn hơn một. Cặp được chọn được đơn giản hóa bằng cách chia cả hai số cho ước số chung lớn nhất của chúng."
date: "2026-08-03T15:12:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102788
codeforces_index: "G"
codeforces_contest_name: "2017-2018 ICPC Central Quarter Final of Northeastern European Regional Collegiate Programming Contest"
rating: 0
weight: 102788
solve_time_s: 106
verified: true
draft: false
---

[CF 102788G - Alice và Bob](https://codeforces.com/problemset/problem/102788/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên một hàng số nguyên dương. Alice di chuyển đầu tiên. Trong một lượt, người chơi chọn hai số lân cận có ước chung lớn hơn một. Cặp được chọn được đơn giản hóa bằng cách chia cả hai số cho ước số chung lớn nhất của chúng. Khi không có cặp lân cận nào chia sẻ bất kỳ ước số nào, người chơi hiện tại không có nước đi hợp pháp và thua cuộc. 

Nhiệm vụ không phải là tìm ra chính mảng cuối cùng mà chỉ là người chơi nào đến vị trí thua cuộc. Đầu vào bao gồm độ dài của hàng và các giá trị ban đầu trong hàng đó. Đầu ra là tên của người chơi chiến thắng với cách chơi tối ưu. 

Các con số có thể lớn như$10^9$, nhưng chỉ có 50 người trong số họ. Không thể tìm kiếm trò chơi trực tiếp vì thứ tự di chuyển tạo ra nhiều trạng thái có thể xảy ra. Tuy nhiên, mỗi thao tác sẽ loại bỏ nghiêm ngặt các thừa số nguyên tố khỏi mảng. Nhiều nhất là một số$10^9$chứa tối đa 29 thừa số nguyên tố có bội số nên tổng số lần loại bỏ có thể là nhỏ. Điều này gợi ý nên tìm kiếm một quá trình đơn điệu thay vì khám phá tất cả các trạng thái trò chơi. 

Quan sát quan trọng là mỗi lần di chuyển sẽ làm giảm tổng lượng thông tin về thừa số nguyên tố trong mảng. Chính xác hơn, nếu một nước đi chia hai số cho gcd của chúng thì mọi thừa số nguyên tố bị loại bỏ sẽ biến mất khỏi cả hai số. Không có phép toán nào có thể đưa ra ước số chung mới giữa các phép toán lân cận. Vì hành vi đơn điệu này, trò chơi không có tác dụng phân nhánh chiến lược đối với tính chẵn lẻ của số nước đi. Bất kỳ chuỗi nước đi hợp lệ nào cũng đạt đến trạng thái cuối cùng sau cùng một số nước đi. 

Một sai lầm phổ biến là nghĩ rằng việc chọn một cặp khác có thể thay đổi người chiến thắng. Ví dụ: với:```
3
2 6 3
```việc chọn cặp đầu tiên mang lại`[1, 3, 3]`, và sau đó có thể thực hiện thêm một động thái nữa. Việc chọn cặp thứ hai mang lại`[2, 2, 1]`, và một lần nữa có thể thực hiện được một động thái nữa. Cả hai đường đều cần hai nước đi nên Bob thắng. 

Một trường hợp cạnh khác là khi tất cả các cặp lân cận đều nguyên tố cùng nhau:```
2
5 7
```Câu trả lời là`Bob`, bởi vì Alice không thể di chuyển được chút nào. Một giải pháp giả định có ít nhất một thao tác tồn tại sẽ tính sai một nước đi. 

Trường hợp tinh vi cuối cùng là khi một thao tác loại bỏ nhiều thừa số nguyên tố cùng một lúc:```
2
60 30
```Gcd là 30, do đó việc di chuyển trực tiếp tạo ra`[2, 1]`. Chỉ đếm một số nguyên tố chung thay vì toàn bộ gcd sẽ đánh giá quá cao số lần di chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là coi mọi lựa chọn có thể có của các số liền kề như một nhánh trong cây trò chơi. Đối với mỗi trạng thái, chúng tôi thử đệ quy mọi nước đi hợp pháp và xác định xem có tồn tại nước đi nào đẩy đối thủ vào trạng thái thua cuộc hay không. Điều này đúng với những trò chơi khách quan, nhưng nó hoàn toàn không thực tế ở đây. Ngay cả một mảng nhỏ cũng có thể có nhiều lệnh di chuyển và số lượng trạng thái tăng theo cấp số nhân. 

Cấu trúc quan trọng là trò chơi không thực sự yêu cầu minimax. Mọi động thái chỉ loại bỏ các thừa số nguyên tố và không bao giờ thay đổi chúng thành một thứ khác. Nếu chúng ta xem toàn bộ quá trình như việc áp dụng lặp đi lặp lại các phép rút gọn thì thứ tự của các phép rút gọn không ảnh hưởng đến số lần rút gọn cần thiết cho đến khi tất cả các cặp liền kề trở thành nguyên tố cùng nhau. 

Brute-force hoạt động vì nó tuân theo định nghĩa về cách chơi tối ưu, nhưng nó thất bại vì nó khám phá nhiều thứ tự tương đương với cùng mức giảm. Nhận xét rằng quy trình này hợp lưu cho phép chúng tôi thay thế việc tìm kiếm trò chơi bằng một mô phỏng đơn giản. Chúng ta có thể áp dụng nhiều lần bất kỳ nước đi nào có sẵn và chỉ đếm xem có bao nhiêu nước đi xảy ra. Nếu số đếm là số lẻ, Alice sẽ thực hiện nước đi cuối cùng. Nếu hòa thì Bob thực hiện nước đi cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(n \cdot F)$|$O(1)$| Đã chấp nhận | 

Đây$F$là số lần di chuyển tối đa. Vì mỗi bước di chuyển sẽ loại bỏ ít nhất một thừa số nguyên tố khỏi hai số và tổng số thừa số nguyên tố là nhỏ,$F$bị giới hạn. 

## Hướng dẫn thuật toán 

1. Lưu trữ mảng và liên tục tìm kiếm cặp liền kề có gcd lớn hơn một. Một cặp như vậy thể hiện một động thái hợp pháp vì cả hai con số đều có thể giảm xuống. 
2. Khi tìm thấy một cặp hợp lệ, chia cả hai số cho gcd của chúng và tăng bộ đếm nước đi. Cặp chính xác được chọn không thành vấn đề vì tất cả các lựa chọn hợp lệ đều dẫn đến cùng một nước đi cuối cùng chẵn lẻ. 
3. Tiếp tục cho đến khi không có cặp liền kề nào có gcd lớn hơn một. Lúc này trò chơi đã kết thúc vì người chơi tiếp theo không có động thái hợp pháp. 
4. Nếu số nước đi là số lẻ thì in ra`Alice`. Nếu không thì in`Bob`. 

Tại sao nó hoạt động: 

Bất biến đằng sau thuật toán là mảng chỉ mất các thừa số nguyên tố. Một bước di chuyển không thể tạo ra một mối quan hệ ước số mới, vì vậy mọi phép toán hợp pháp đều là một phần của cùng một quá trình rút gọn. Vì vị trí cuối cùng và tính chẵn lẻ của số lần giảm không phụ thuộc vào thứ tự di chuyển nên việc đếm bất kỳ chuỗi hợp lệ nào sẽ mang lại người chiến thắng chính xác. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    moves = 0

    while True:
        changed = False

        for i in range(n - 1):
            g = math.gcd(a[i], a[i + 1])
            if g > 1:
                a[i] //= g
                a[i + 1] //= g
                moves += 1
                changed = True
                break

        if not changed:
            break

    print("Alice" if moves % 2 else "Bob")

if __name__ == "__main__":
    solve()
```Vòng lặp tìm kiếm một nước đi có sẵn tại một thời điểm. Chúng tôi dừng lại sau lần giảm thành công đầu tiên trong mỗi lần vượt qua vì trạng thái tiếp theo mới là điều quan trọng, chứ không phải động thái pháp lý cụ thể nào được chọn. 

Phép tính gcd sử dụng thuật toán Euclide tích hợp của Python, thuật toán này dễ dàng xử lý các giá trị lên đến$10^9$. Phép chia số nguyên là an toàn vì gcd luôn chia chính xác cả hai số đã chọn. 

Lỗi triển khai duy nhất có thể xảy ra là quên rằng gcd bằng 1 không phải là một động thái hợp pháp. Chia cho 1 sẽ tạo ra một vòng lặp vô hạn vì trạng thái sẽ không thay đổi. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
2 4 2
```| Bước | Mảng | Cặp đôi được chọn | GCD | Di chuyển | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [2, 4, 2] | [2,4] | 2 | 0 | 
| 1 | [1, 2, 2] | [2,2] | 2 | 1 | 
| 2 | [1,1,1] | không | - | 2 | 

Trò chơi kết thúc sau hai nước đi nên Alice không thể thực hiện nước đi cuối cùng. Bob thắng. 

Đối với đầu vào:```
4
3 9 6 18
```| Bước | Mảng | Cặp đôi được chọn | GCD | Di chuyển | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [3,9,6,18] | [9,6] | 3 | 0 | 
| 1 | [3,3,2,18] | [3,3] | 3 | 1 | 
| 2 | [3,1,2,18] | [2,18] | 2 | 2 | 
| 3 | [3,1,1,9] | không | - | 3 | 

Số nước đi là số lẻ nên Alice thực hiện nước đi cuối cùng và giành chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot F)$| Mỗi lần lặp sẽ quét hàng và$F$là số lần giảm. | 
| Không gian |$O(1)$| Chỉ có mảng đầu vào và bộ đếm được lưu trữ. | 

Số lần giảm tối đa là nhỏ vì mỗi lần di chuyển sẽ loại bỏ vĩnh viễn các thừa số nguyên tố. Với$n \le 50$và giá trị lớn nhất$10^9$, mô phỏng dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def run(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])
    a = list(map(int, data[1:]))

    moves = 0
    while True:
        ok = False
        for i in range(n - 1):
            g = math.gcd(a[i], a[i + 1])
            if g > 1:
                a[i] //= g
                a[i + 1] //= g
                moves += 1
                ok = True
                break
        if not ok:
            break

    return ("Alice" if moves % 2 else "Bob") + "\n"

assert run("3\n2 4 2\n") == "Bob\n", "sample 1"
assert run("4\n3 9 6 18\n") == "Alice\n", "sample 2"

assert run("2\n5 7\n") == "Bob\n", "no moves"
assert run("2\n60 30\n") == "Alice\n", "large gcd removal"
assert run("5\n2 2 2 2 2\n") == "Bob\n", "repeated equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 7`| Bob | Kiểm tra vị trí thua ban đầu. | 
|`60 30`| Alice | Kiểm tra việc loại bỏ một gcd có nhiều thừa số nguyên tố cùng một lúc. | 
|`2 2 2 2 2`| Bob | Kiểm tra việc giảm lặp lại và xử lý tính chẵn lẻ. | 

## Vỏ cạnh 

Đối với trường hợp:```
2
5 7
```thuật toán quét cặp liền kề duy nhất, tìm gcd bằng 1 và không thực hiện di chuyển nào. Bộ đếm vẫn bằng 0 nên Bob thắng. 

Đối với trường hợp:```
3
2 6 3
```thuật toán có thể chọn một trong hai cặp liền kề. Lựa chọn`[2,6]`tạo ra`[1,3,3]`và bước tiếp theo sẽ loại bỏ gcd của cặp cuối cùng. Lựa chọn`[6,3]`tạo ra`[2,2,1]`, tiếp theo là một lần giảm nữa. Cả hai đường đi đều có tính chẵn lẻ như nhau nên kết quả vẫn là Bob. 

Đối với trường hợp:```
2
60 30
```gcd là 30, không chỉ một thừa số nguyên tố. Hoạt động này tạo ra`[2,1]`ngay lập tức. Thuật toán đếm một nước đi và trả về chính xác Alice.
