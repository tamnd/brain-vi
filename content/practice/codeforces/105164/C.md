---
title: "CF 105164C - Đóng gói sô cô la"
description: "Cho ta hai hình lăng trụ chữ nhật. Một là một thùng vận chuyển lớn có kích thước $L lần W lần H$, và cái còn lại là một hộp sô cô la giống hệt nhỏ hơn có kích thước $l lần w lần h$. Mục đích là để xác định có thể xếp bao nhiêu hộp nhỏ vào bên trong hộp lớn."
date: "2026-06-27T10:43:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105164
codeforces_index: "C"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105164
solve_time_s: 80
verified: false
draft: false
---

[CF 105164C - Đóng gói sô cô la](https://codeforces.com/problemset/problem/105164/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai hình lăng trụ chữ nhật. Một là container vận chuyển cỡ lớn với kích thước$L \times W \times H$, và cái còn lại là một hộp sô-cô-la nhỏ hơn giống hệt với kích thước$l \times w \times h$. Mục đích là để xác định có thể xếp bao nhiêu hộp nhỏ vào bên trong hộp lớn. 

Hạn chế chính là mọi hộp sô cô la phải có cùng hướng so với hộp vận chuyển. Điều đó có nghĩa là chúng tôi không đóng gói các hộp được xoay tùy ý cho mỗi vị trí mà thay vào đó, chúng tôi chọn một hướng cố định duy nhất của hộp nhỏ rồi đóng gói chúng theo cách căn chỉnh theo dạng lưới dọc theo ba trục của vùng chứa. Mỗi hộp phải thẳng hàng với các trục của hộp vận chuyển. 

Vấn đề trở thành việc chọn hoán vị tốt nhất của$(l, w, h)$được giao cho$(L, W, H)$, sau đó tính toán xem có bao nhiêu khoảng đầy đủ của mỗi chiều phù hợp. 

Các ràng buộc rất nhỏ: tất cả các kích thước tối đa là 1000. Điều này ngay lập tức loại trừ mọi nhu cầu mô phỏng hoặc tìm kiếm trên các vị trí. Ngay cả việc kiểm tra tất cả các hướng có thể có cũng là công việc liên tục vì chỉ có sáu hoán vị của kích thước hộp. 

Một ý tưởng ngây thơ nhưng không chính xác là tính toán$(L // l) \cdot (W // w) \cdot (H // h)$mà không cân nhắc rằng việc xoay chiếc hộp nhỏ có thể cải thiện sự vừa vặn. Ví dụ, nếu$L = 6, W = 5, H = 4$và cái hộp là$2 \times 1 \times 3$, việc sử dụng hướng sai có thể sử dụng không đúng mức không gian, trong khi việc xoay tối ưu mang lại số lượng đóng gói cao hơn. 

Một vấn đề tế nhị khác là giả sử đóng góp phù hợp một phần, nhưng không cho phép các hộp phân số. Bất kỳ không gian còn sót lại nào nhỏ hơn hộp đầy đủ ở bất kỳ kích thước nào đều bị lãng phí. 

## Phương pháp tiếp cận 

Phương pháp vũ phu thử mọi hướng có thể có của hộp sô cô la. Vì có ba chiều nên có đúng sáu hoán vị. Đối với mỗi hoán vị, chúng tôi tính toán có bao nhiêu ô khớp dọc theo mỗi trục bằng phép chia số nguyên, sau đó nhân kết quả. 

Điều này có tác dụng vì khi định hướng được cố định, việc đóng gói sẽ trở thành một lưới 3D hoàn hảo. Số lượng vị trí dọc theo mỗi trục độc lập với các vị trí khác, do đó phép nhân là hợp lệ. 

Cách tiếp cận brute-force đã tối ưu về độ phức tạp vì không gian tìm kiếm có kích thước không đổi. Không có lợi ích gì khi cố gắng mô phỏng các vị trí hoặc xem xét các chiến lược đóng gói từng phần. 

Cái nhìn sâu sắc quan trọng là định hướng là mức độ tự do duy nhất. Sau khi định hướng được khắc phục, sự cố sẽ trở thành sự cố xếp kề theo khối lượng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về định hướng | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước của hộp vận chuyển và hộp sô-cô-la. Chúng xác định hai hình chữ nhật 3D mà chúng ta đang làm việc. 
2. Tạo tất cả sáu hoán vị của kích thước hộp sô cô la. Mỗi hoán vị thể hiện một hướng riêng biệt về cách căn chỉnh hộp nhỏ bên trong hộp lớn. 
3. Đối với mỗi hoán vị, hãy tính xem có bao nhiêu ô vừa với mỗi trục bằng phép chia số nguyên:$L // x$,$W // y$,$H // z$. Điều này đảm bảo chúng tôi chỉ tính những hộp đầy đủ phù hợp hoàn toàn. 
4. Nhân ba giá trị để có tổng số hộp cho hướng đó. Điều này có tác dụng vì các hộp tạo thành một lưới 3D đồng nhất sau khi định hướng được cố định. 
5. Theo dõi giá trị tối đa trên tất cả các hướng. 
6. Xuất ra mức tối đa. 

### Tại sao nó hoạt động 

Khi một hướng cố định được chọn, vấn đề đóng gói sẽ phân tách thành ba vấn đề đóng gói 1D độc lập dọc theo mỗi trục. Vì các hộp không thể chồng lên nhau hoặc xoay riêng lẻ nên việc đóng gói tối ưu cho hướng đó luôn là lưới đầy đủ được hình thành bằng cách sắp xếp tham lam dọc theo mỗi chiều. Bất kỳ không gian còn lại nào đều nhỏ hơn kích thước hộp đầy đủ, vì vậy nó không thể đóng góp thêm hộp. Vì vậy, giải pháp tốt nhất phải đến từ một trong sáu hoán vị căn chỉnh theo trục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    L, W, H = map(int, input().split())
    l, w, h = map(int, input().split())

    dims = (l, w, h)
    from itertools import permutations

    best = 0
    for x, y, z in permutations(dims):
        count = (L // x) * (W // y) * (H // z)
        if count > best:
            best = count

    print(best)

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách đọc cả hai bộ thứ nguyên. Sau đó, nó liệt kê tất cả sáu hướng có thể bằng cách sử dụng hoán vị của kích thước hộp nhỏ. Đối với mỗi hướng, nó tính toán có bao nhiêu hộp nằm dọc theo mỗi trục bằng cách sử dụng phép chia số nguyên, đảm bảo chỉ tính các hộp chứa đầy đủ. Sản phẩm đưa ra tổng số hộp cho hướng đó. Số lượng tối đa trên tất cả các hướng được in. 

Một chi tiết tinh tế nhưng quan trọng là sử dụng phép chia số nguyên ở mọi trục một cách độc lập trước khi nhân. Nhân trước hoặc sử dụng phép chia nổi sẽ dẫn đến hành vi làm tròn không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 5 4
2 1 3
```Chúng tôi đánh giá tất cả các hướng của (2,1,3). 

| Định hướng (x,y,z) | L//x | Có// năm | H//z | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| (2,1,3) | 3 | 5 | 1 | 15 | 
| (2,3,1) | 3 | 1 | 4 | 12 | 
| (1,2,3) | 6 | 2 | 1 | 12 | 
| (1,3,2) | 6 | 1 | 2 | 12 | 
| (3,2,1) | 2 | 2 | 4 | 16 | 
| (3,1,2) | 2 | 5 | 2 | 20 | 

Tối đa là 20. 

Dấu vết này cho thấy sự lựa chọn định hướng chiếm ưu thế trong giải pháp. Việc sắp xếp tốt nhất sẽ không rõ ràng nếu không kiểm tra tất cả các hoán vị, vì các phép gán trục khác nhau sẽ tương tác theo cấp số nhân. 

### Ví dụ 2 

đầu vào:```
1 1 13
3 3 3
```| Định hướng (x,y,z) | L//x | Có// năm | H//z | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| hoán vị nào | 0 hoặc 0 hoặc 4 | ... | ... | 0 | 

Mọi hướng đều không thành công vì ít nhất một kích thước của hộp vượt quá kích thước vùng chứa, tạo ra độ vừa vặn bằng 0. 

Điều này xác nhận rằng thuật toán xử lý chính xác các trường hợp không thể đặt hộp nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có sáu hoán vị được kiểm tra, mỗi hoán vị liên quan đến số học có thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ | 

Các ràng buộc cho phép liệt kê trực tiếp tất cả các hướng. Vì tính toán theo hướng là không đổi, nên giải pháp nằm trong giới hạn tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    L, W, H = map(int, input().split())
    l, w, h = map(int, input().split())

    from itertools import permutations

    best = 0
    for x, y, z in permutations((l, w, h)):
        best = max(best, (L // x) * (W // y) * (H // z))

    return str(best).strip()

# provided samples
assert run("1 1 13\n3 3 3\n") == "0"
assert run("6 5 4\n2 1 3\n") == "20"

# minimum-size inputs
assert run("1 1 1\n1 1 1\n") == "1"

# all-fit single layer
assert run("10 10 10\n2 2 2\n") == "125"

# one dimension blocks everything
assert run("5 5 5\n6 1 1\n") == "0"

# asymmetric optimal rotation case
assert run("4 6 8\n3 2 1\n") == "32"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 / 1 1 1 | 1 | phù hợp chính xác tối thiểu | 
| 10 10 10/2 2 2 | 125 | đóng gói lưới thống nhất | 
| 5 5 5 / 6 1 1 | 0 | vị trí không thể | 
| 4 6 8 / 3 2 1 | 32 | vấn đề luân chuyển | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi một kích thước của hộp sôcôla vượt quá kích thước tương ứng của hộp vận chuyển. Ví dụ: 

đầu vào:```
5 5 5
6 1 1
```Đối với bất kỳ hoán vị nào, ít nhất một trục tạo ra số 0 thông qua phép chia số nguyên, do đó kết quả cuối cùng bằng 0. Thuật toán xử lý điều này một cách tự nhiên vì phép nhân với số 0 lan truyền chính xác. 

Một trường hợp khác là khi tất cả các kích thước đều bằng nhau: 

đầu vào:```
3 3 3
1 1 1
```Mọi định hướng đều mang lại$3 \cdot 3 \cdot 3 = 27$. Vòng lặp hoán vị tính toán các giá trị giống hệt nhau và giá trị tối đa vẫn đúng. 

Cuối cùng, khi các kích thước có tính bất đối xứng cao, giải pháp tối ưu có thể yêu cầu một hoán vị không rõ ràng. Việc kiểm tra toàn diện sáu hướng đảm bảo tính chính xác bất kể độ lệch.
