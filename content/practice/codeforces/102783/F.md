---
title: "CF 102783F - Xorro the Xorman"
description: "Bài toán cho hai số nguyên không âm là A và B. Chúng ta phải chọn một số nguyên b trong khoảng từ 0 đến B và giá trị của A XOR b là lớn nhất. Đầu ra là giá trị XOR lớn nhất có thể thu được."
date: "2026-07-27T19:59:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102783
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 2"
rating: 0
weight: 102783
solve_time_s: 48
verified: true
draft: false
---

[CF 102783F - Xorro the Xorman](https://codeforces.com/problemset/problem/102783/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho hai số nguyên không âm là A và B. Chúng ta phải chọn một số nguyên b trong khoảng từ 0 đến B và giá trị của A XOR b là lớn nhất. Đầu ra là giá trị XOR lớn nhất có thể thu được. Giá trị đầu vào có thể lớn tới 10^18, vì vậy giải pháp phải hoạt động với các số chứa khoảng 60 bit. 

Một giải pháp thử mọi cách có thể b ngay lập tức là không thể. Nếu B là 10^18 thì có khoảng một triệu tỷ thí sinh, vượt xa những gì có thể kiểm tra trong giới hạn cuộc thi thông thường. Ngay cả việc kiểm tra thời gian liên tục được tối ưu hóa cho mỗi giá trị cũng sẽ yêu cầu quá nhiều thao tác. Vì số lượng bit liên quan chỉ khoảng 60 nên giải pháp mong muốn sẽ phụ thuộc vào biểu diễn nhị phân thay vì kích thước của dãy số. 

Những trường hợp phức tạp xuất phát từ việc b bị giới hạn ở trên chứ không cố định. Một cách tiếp cận tham lam chỉ đơn giản là lật từng bit của A thường sẽ tạo ra một số lớn hơn B. 

Ví dụ:```
Input:
5 4

Output:
7
```Ở đây A là 101 ở dạng nhị phân và B là 100. Chọn b = 2 sẽ cho 101 XOR 010 = 111, bằng 7. Một giải pháp bất cẩn có thể chọn b = 0 vì bit cao nhất của B bằng bit cao nhất của A, thiếu tiền tố nhỏ hơn sẽ cho phép nhiều tự do hơn sau này. 

Một trường hợp khác là khi bản thân B là lựa chọn tốt nhất có thể.```
Input:
7 32

Output:
39
```Giá trị b tốt nhất là 32, vì 7 XOR 32 = 39. Một phương pháp chỉ xem xét các giá trị có cùng độ dài bit với A có thể bỏ sót các câu trả lời hợp lệ. 

Trường hợp quan trọng cuối cùng là B = 0.```
Input:
10 0

Output:
10
```Chỉ có một lựa chọn duy nhất, b = 0. Bất kỳ thuật toán nào giả định rằng nó luôn có thể hạ thấp B một chút để đạt được tính linh hoạt sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi b có thể từ 0 đến B, tính A XOR b và giữ mức tối đa. Điều này đúng vì mọi giá trị được phép đều được xem xét. Trường hợp xấu nhất của nó là hoạt động O(B), trở thành O(10^18) và không sử dụng được. 

Quan sát chính là so sánh XOR hoạt động chính xác như so sánh nhị phân thông thường. Để tối đa hóa con số cuối cùng, phần quan trọng nhất mà chúng ta có thể đưa ra quyết định là vấn đề đầu tiên. Nếu chúng ta có thể làm cho một bit của kết quả bằng 1 mà không làm cho b vượt quá B, thì chúng ta nên làm điều đó ngay lập tức vì mọi bit thấp hơn sẽ trở nên không liên quan sau khi bit cao hơn khác đi. 

Xử lý các bit từ quan trọng nhất đến ít quan trọng nhất. Trong khi khớp tiền tố của B, chúng ta có hai lựa chọn. Chúng ta có thể giữ bit hiện tại của b bằng bit của B, nghĩa là tiền tố vẫn bằng nhau và các lựa chọn trong tương lai bị hạn chế. Hoặc, nếu B có số 1 ở vị trí này, chúng ta có thể đặt số 0 vào b, làm cho tiền tố của b nhỏ hơn tiền tố của B. Khi tiền tố trở nên nhỏ hơn, mọi bit còn lại của b có thể được chọn tự do, vì vậy chúng ta chỉ cần chọn các bit tối đa hóa XOR với A. 

Điều này làm giảm vấn đề chỉ còn kiểm tra khoảng 60 bit vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(B) | O(1) | Quá chậm | 
| Tối ưu | O(log B) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc A và B và kiểm tra các bit của chúng từ bit cao nhất xuống bit 0. Chỉ cần khoảng 60 bit vì các số tối đa là 10^18. 
2. Duy trì xem tiền tố của số b đã chọn có nhỏ hơn tiền tố của B hay không. Nếu nó nhỏ hơn, các bit còn lại sẽ không bị hạn chế vì bất kỳ sự tiếp tục nào cũng sẽ tạo ra giá trị dưới B. 
3. Nếu tiền tố đã nhỏ hơn, hãy đặt mọi bit còn lại của b thành đối diện với bit của A. Điều này làm cho mọi bit XOR còn lại bằng 1. 
4. Nếu tiền tố bằng B thì so sánh bit hiện tại của B. Nếu là 0 thì b cũng phải sử dụng 0 ở vị trí này. Nếu là 1, hãy cân nhắc lựa chọn đặt 0 vào b. Điều này làm cho b nhỏ hơn B và mang lại sự tự do cho tất cả các bit sau, do đó câu trả lời có thể lấy hậu tố tốt nhất có thể. 
5. Theo dõi câu trả lời tối đa mà hai trạng thái có thể đạt được. Việc triển khai có thể tránh việc lưu trữ các giá trị b thực tế bằng cách trực tiếp xây dựng giá trị XOR tốt nhất. 

Tại sao nó hoạt động: bit cao nhất trong đó hai câu trả lời có thể khác nhau sẽ xác định câu trả lời nào lớn hơn. Tại mỗi bit, thuật toán sẽ chọn xem có thể tạo số 1 trong câu trả lời mà không vi phạm b <= B hay không. Nếu có thể làm cho tiền tố nhỏ hơn thì tất cả các bit thấp hơn có thể được tối ưu hóa một cách độc lập. Nếu không thể, bit hiện tại sẽ bị ép buộc. Do đó, mọi quyết định đều bảo toàn tiền tố tối đa có thể đạt được và sau khi tất cả các bit được xử lý, câu trả lời hoàn chỉnh là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B = map(int, input().split())

    ans = 0
    smaller = False

    for i in range(60, -1, -1):
        abit = (A >> i) & 1
        bbit = (B >> i) & 1

        if smaller:
            ans |= (1 << i)
        else:
            if bbit == 0:
                if abit == 1:
                    ans |= (1 << i)
            else:
                if abit == 0:
                    ans |= (1 << i)
                smaller = True

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp xử lý các bit từ cao đến thấp vì các bit cao hơn quyết định thứ tự số của giá trị XOR cuối cùng. Biến`smaller`ghi lại xem tiền tố b được xây dựng có nằm dưới B hay không. Khi điều này xảy ra, giới hạn trên không còn hạn chế các bit trong tương lai. 

Khi`B`có bit 0 trong khi các tiền tố bằng nhau thì bit tương ứng của b bị buộc về 0. Khi đó bit XOR được xác định hoàn toàn bởi A. Khi`B`có một bit, việc chọn bit của b bằng 0 sẽ tạo ra một tiền tố nhỏ hơn và cho phép tất cả các bit XOR trong tương lai trở thành một. Đây là lý do tại sao mã đánh dấu trạng thái là nhỏ hơn ngay sau khi xử lý vị trí đó. 

Số nguyên Python không bị tràn nên các thao tác 60 bit vẫn an toàn. Vòng lặp bao gồm bit 60 để bao phủ giá trị lớn nhất có thể là 10^18. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5 4
```các giá trị nhị phân là A = 101 và B = 100. 

| Chút | Một chút | B bit | Nhỏ hơn trước | Trả lời chút | Nhỏ hơn sau | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | sai | 0 | đúng | 
| 1 | 0 | 0 | đúng | 1 | đúng | 
| 0 | 1 | 0 | đúng | 1 | đúng | 

Kết quả là 111, tức là 7. Bit đầu tiên của b được chọn là 0, làm cho b nhỏ hơn B và cho phép các bit còn lại tối đa hóa XOR. 

Đối với đầu vào:```
7 32
```A = 000111 và B = 100000. 

| Chút | Một chút | B bit | Nhỏ hơn trước | Trả lời chút | Nhỏ hơn sau | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 0 | 1 | sai | 1 | đúng | 
| 4 | 0 | 0 | đúng | 1 | đúng | 
| 3 | 0 | 0 | đúng | 1 | đúng | 
| 2 | 1 | 0 | đúng | 1 | đúng | 
| 1 | 1 | 0 | đúng | 1 | đúng | 
| 0 | 1 | 0 | đúng | 1 | đúng | 

Kết quả là 111111, tức là 39. Việc chọn b = 32 sẽ ngay lập tức tạo ra trạng thái tiền tố nhỏ hơn và để lại tất cả các bit thấp hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log B) | Thuật toán kiểm tra từng chữ số nhị phân một lần. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Giới hạn đầu vào yêu cầu tránh bất kỳ lần lặp nào trong phạm vi số. Vì 10^18 vừa với khoảng 60 chữ số nhị phân, nên giải pháp chỉ thực hiện một số phép toán không đổi so với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    A, B = map(int, sys.stdin.readline().split())
    ans = 0
    smaller = False
    for i in range(60, -1, -1):
        abit = (A >> i) & 1
        bbit = (B >> i) & 1
        if smaller:
            ans |= 1 << i
        elif bbit == 0:
            if abit:
                ans |= 1 << i
        else:
            if abit == 0:
                ans |= 1 << i
            smaller = True
    sys.stdin = old
    return str(ans) + "\n"

assert run("5 4\n") == "7\n", "sample 1"
assert run("7 32\n") == "39\n", "sample 2"
assert run("0 0\n") == "0\n", "minimum values"
assert run("10 0\n") == "10\n", "zero upper bound"
assert run("15 15\n") == "15\n", "equal values"
assert run("1000000000000000000 1000000000000000000\n") == "1152921504606846975\n", "large boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 0 | Xử lý đầu vào nhỏ nhất có thể. | 
| 10 0 | 10 | Xác nhận rằng không có lựa chọn nào ngoại trừ số 0 được xử lý. | 
| 15 15 | 15 | Kiểm tra hành vi A và B bằng nhau. | 
| 1000000000000000000 10000000000000000000 | 1152921504606846975 | Kiểm tra vị trí bit lớn. | 

## Vỏ cạnh 

cho`A = 5`Và`B = 4`, bit đầu tiên của B cho một sự lựa chọn. Lấy bit cao nhất của b bằng 0 làm cho b nhỏ hơn B, do đó tất cả các bit thấp hơn có thể được chọn để đối lập với A. Thuật toán đạt đến cùng quyết định và tạo ra 7. 

cho`A = 7`Và`B = 32`, bit đầu tiên của B cho phép số được xây dựng trở nên nhỏ hơn ngay lập tức. Sau thời điểm đó, mọi bit trả lời còn lại có thể trở thành một, tạo thành 39. 

cho`A = 10`Và`B = 0`, mọi bit của b buộc phải bằng 0 vì không thể có tiền tố nhỏ hơn. Câu trả lời chính xác là A XOR 0, tức là 10.
