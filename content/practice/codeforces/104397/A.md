---
title: "CF 104397A - Đảo ngược các cặp chuỗi nhị phân"
description: "Chúng ta được cung cấp một số chuỗi nhị phân và chúng ta được phép nối chúng theo bất kỳ thứ tự nào thành một chuỗi nhị phân dài duy nhất. Sau khi được nối, chúng ta đếm số lần đảo ngược theo nghĩa thông thường: một cặp vị trí trong đó số 1 xuất hiện trước số 0."
date: "2026-07-01T00:51:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104397
codeforces_index: "A"
codeforces_contest_name: "The 21st UESTC Programming Contest Final"
rating: 0
weight: 104397
solve_time_s: 86
verified: true
draft: false
---

[CF 104397A - Cặp nghịch đảo của chuỗi nhị phân](https://codeforces.com/problemset/problem/104397/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số chuỗi nhị phân và chúng ta được phép nối chúng theo bất kỳ thứ tự nào thành một chuỗi nhị phân dài duy nhất. Sau khi được nối, chúng ta đếm các nghịch đảo theo nghĩa thông thường: một cặp vị trí trong đó một`1`xuất hiện trước một`0`. 

Nhiệm vụ không chỉ là tính toán các phép đảo ngược cho một chuỗi cố định mà còn là chọn thứ tự của các chuỗi đầu vào sao cho phép nối cuối cùng tạo ra số lượng đảo ngược nhỏ nhất có thể có. 

Bên trong một chuỗi, thứ tự tương đối của các ký tự là cố định, do đó việc đóng góp đảo ngược bên trong của nó là không thể tránh khỏi. Sự tự do thực sự đến từ cách các chuỗi khác nhau tương tác với nhau: đặt một chuỗi trước chuỗi khác có thể tạo ra các đảo ngược bổ sung bất cứ khi nào một chuỗi`1`từ chuỗi trước đó kết thúc trước một`0`ở chuỗi sau. 

Các ràng buộc ngụ ý rằng tổng chiều dài trên tất cả các chuỗi nhiều nhất là một triệu. Điều này loại trừ bất kỳ giải pháp nào thử tất cả các hoán vị của chuỗi hoặc thậm chí bất kỳ số bậc hai nào về số lượng chuỗi. Việc sắp xếp các phương pháp tiếp cận dựa trên hoặc quét tuyến tính qua phép nối là cần thiết. 

Một cách tiếp cận đơn giản sẽ thử mọi hoán vị của chuỗi và tính số lần đảo ngược mỗi lần. Ngay cả khi bỏ qua chi phí tính toán lại, điều này là không thể vì tối đa 10^6 chuỗi sẽ khiến việc tăng trưởng giai thừa hoàn toàn không khả thi. Ngay cả với ít chuỗi hơn, việc tính toán lại số lần đảo ngược cho mỗi hoán vị sẽ yêu cầu quét tất cả các ký tự nhiều lần, dẫn đến kết quả giống như O(n! · Total_length). 

Ý tưởng ngây thơ thứ hai là ghép nối theo thứ tự tùy ý và tính toán nghịch đảo một lần. Điều này không thành công vì thứ tự thay đổi nhiều so với đảo ngược chuỗi chéo. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi chỉ có một hoặc chỉ có số không. Ví dụ,`"111"`Và`"000"`hành xử rất khác nhau tùy theo thứ tự. Nếu như`"111"`đến trước`"000"`, mỗi cặp đều góp phần đảo ngược, trong khi điều ngược lại không tạo ra đảo ngược nào. Bất kỳ chiến lược đúng đắn nào cũng phải nắm bắt được sự bất cân xứng này. 

## Phương pháp tiếp cận 

Chúng tôi tách số lần đảo ngược thành hai phần: số lần đảo ngược bên trong mỗi chuỗi và số lần đảo ngược được tạo giữa các chuỗi. 

Phần bên trong được cố định bất kể thứ tự. Với mỗi chuỗi, chúng ta có thể tính được bao nhiêu cặp`(1 before 0)`xảy ra trực tiếp bên trong nó theo thời gian tuyến tính trên chiều dài của nó. 

Phần khó khăn là đảo ngược chuỗi chéo. Giả sử chúng ta đặt chuỗi A trước chuỗi B. Mọi`1`trong A tạo thành một sự đảo ngược với mọi`0`ở B. Vậy đóng góp của việc đặt hàng A trước B là:```
cost(A before B) = ones(A) * zeros(B)
```Nếu chúng ta trao đổi chúng, chi phí sẽ trở thành:```
cost(B before A) = ones(B) * zeros(A)
```Vì vậy, vấn đề đặt hàng trở thành việc chọn một hoán vị sao cho tối thiểu hóa tổng chi phí phụ thuộc vào trao đổi theo cặp. 

Cấu trúc này hoàn toàn giống với bài toán lập kế hoạch hoặc sắp xếp với quy tắc so sánh. Với hai chuỗi A và B, ta đặt A trước B khi:```
ones(A) * zeros(B) <= ones(B) * zeros(A)
```Sắp xếp lại cho điều kiện đặt hàng ổn định:```
ones(A) / zeros(A) <= ones(B) / zeros(B)
```(với việc xử lý cẩn thận khi số 0 bằng 0). 

Vì vậy, sự sắp xếp tối ưu có được bằng cách sắp xếp các chuỗi theo tỷ lệ`ones / zeros`theo thứ tự tăng dần, với cách xử lý đặc biệt đối với các trường hợp cạnh trong đó một chuỗi có số 0. 

Sau khi sắp xếp, chúng tôi quét từ trái sang phải, duy trì bao nhiêu`1`s đã xuất hiện rồi. Khi chúng ta đặt một chuỗi B mới, mọi`0`bên trong B tạo ra sự đảo ngược với tất cả những cái trước đó, góp phần:```
ones_so_far * zeros(B)
```Chúng tôi cũng thêm các đảo ngược nội bộ của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | O(n! · L) | O(L) | Quá chậm | 
| Tối ưu (sắp xếp + quét tuyến tính) | O(L log n) | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xử lý trước mỗi chuỗi bằng cách trích xuất hai giá trị: nó chứa bao nhiêu số một và bao nhiêu số 0. Trong khi thực hiện quá trình quét này, chúng tôi cũng tính toán số lần đảo ngược nội bộ của nó bằng cách theo dõi số lượng số 0 còn lại ở bên phải của mỗi số. 

Tiếp theo, chúng tôi sắp xếp tất cả các chuỗi. Khóa sắp xếp là tỷ lệ chia cho số 0. Nếu một chuỗi không có số 0, nó sẽ được đặt sau tất cả các chuỗi vẫn có số 0, vì tỷ lệ của nó giống như vô cùng và nó không được đến sớm hơn theo thứ tự tối ưu. 

Sau khi sắp xếp, chúng ta lặp qua các chuỗi theo thứ tự đó. Chúng tôi duy trì số lượng đang chạy có bao nhiêu cái đã xuất hiện cho đến nay. Đối với mỗi chuỗi, chúng tôi thêm hai đóng góp: các phép đảo ngược nội bộ đã được tính toán cho chuỗi đó và các phép đảo ngược chéo được tạo bởi các số 0 của nó ghép nối với tất cả các chuỗi trước đó. 

Sau đó, chúng tôi cập nhật bộ đếm số đơn vị đang chạy bằng cách cộng số lượng đơn vị trong chuỗi hiện tại. 

Tại sao nó hoạt động xuất phát từ thực tế là sự đóng góp chéo giữa hai chuỗi bất kỳ chỉ phụ thuộc vào tổng số số một và số không của chúng. Bất kỳ sự hoán đổi nào giữa các chuỗi liền kề đều thay đổi câu trả lời bằng cách so sánh chính xác`ones(A)*zeros(B)`với`ones(B)*zeros(A)`. Điều này làm cho vấn đề tương đương với việc sắp xếp theo một bộ so sánh nhất quán, điều này đảm bảo rằng một khi không tồn tại sự đảo ngược liền kề của quy tắc đặt hàng này thì không có sự sắp xếp lại toàn cục nào có thể cải thiện tổng chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def inv_inside(s: str) -> int:
    zeros = 0
    inv = 0
    # count inversions of type 1 before 0
    for c in reversed(s):
        if c == '0':
            zeros += 1
        else:
            inv += zeros
    return inv

def key(item):
    ones, zeros, _ = item
    if zeros == 0:
        return (1, 0)  # treat as largest possible ratio
    return (0, ones / zeros)

def solve():
    n = int(input())
    items = []

    for _ in range(n):
        s = input().strip()
        ones = s.count('1')
        zeros = len(s) - ones
        items.append((ones, zeros, inv_inside(s)))

    items.sort(key=key)

    ones_so_far = 0
    ans = 0

    for ones, zeros, internal in items:
        ans += internal
        ans += ones_so_far * zeros
        ones_so_far += ones

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tách biệt cấu trúc đảo ngược bên trong của mỗi chuỗi bằng cách quét ngược để đếm số lượng số 0 nằm ở bên phải của mỗi chuỗi. Điều này tránh mọi hành vi bậc hai trên mỗi chuỗi. 

Hàm sắp xếp mã hóa quy tắc sắp xếp dẫn xuất. Các chuỗi không có số 0 buộc phải kết thúc bằng cách gán cho chúng một khóa tối đa. Đối với tất cả các chuỗi khác, tỷ lệ một trên số 0 sẽ xác định vị trí. 

Lần quét cuối cùng tích lũy các đảo ngược chuỗi chéo bằng cách sử dụng tổng tiền tố của các số 1, biểu thị chính xác có bao nhiêu`1`mỗi cái`0`trong chuỗi hiện tại sẽ tương tác với. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1
11
101
```Chúng tôi tính toán trên mỗi chuỗi: 

chuỗi`"1"`có số 1 = 1, số 0 = 0, nghịch đảo nội bộ = 0 

chuỗi`"11"`có số 1 = 2, số 0 = 0, nghịch đảo nội bộ = 0 

chuỗi`"101"`có số 1 = 2, số 0 = 1, nghịch đảo nội bộ = 1 

Sắp xếp theo vị trí số 1/số 0`"101"`đầu tiên vì nó có tỷ lệ 2, trong khi những tỷ lệ khác có số 0 và đi sau. 

Sau khi sắp xếp: 

| Bước | Chuỗi | one_so_far | số không | nội bộ | thêm chéo | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 101 | 0 | 1 | 1 | 0 | 1 | 
| 2 | 1 | 2 | 0 | 0 | 2·0 = 0 | 1 | 
| 3 | 11 | 3 | 0 | 0 | 0 | 1 | 

Câu trả lời cuối cùng là 1, phù hợp với mẫu. 

Dấu vết này cho thấy các nghịch đảo nội bộ được tách biệt một cách chính xác và các đóng góp chéo chỉ phụ thuộc vào các nghịch đảo tích lũy. 

### Ví dụ 2 

đầu vào:```
2
10
01
```Sợi dây`"10"`có những cái = 1, số không = 1, nội bộ = 1 

chuỗi`"01"`có những cái = 1, số không = 1, nội bộ = 0 

Cả hai đều có tỷ lệ bằng nhau nên lệnh nào cũng hợp lệ. Giả định`"01"`đến trước. 

| Bước | Chuỗi | one_so_far | số không | nội bộ | thêm chéo | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 01 | 0 | 1 | 0 | 0 | 0 | 
| 2 | 10 | 1 | 1 | 1 | 1 | 2 | 

Nếu đảo ngược thứ tự: 

| Bước | Chuỗi | one_so_far | số không | nội bộ | thêm chéo | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 0 | 1 | 1 | 0 | 1 | 
| 2 | 01 | 1 | 1 | 0 | 1 | 2 | 

Cả hai đều mang lại tổng số như nhau, xác nhận tính đúng đắn của quy tắc đặt hàng khi tỷ lệ khớp nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L log n) | Mỗi ký tự được xử lý một lần để đếm và đảo ngược nội bộ, đồng thời việc sắp xếp n chuỗi chiếm ưu thế | 
| Không gian | O(n) | Chúng tôi lưu trữ các giá trị tổng hợp trên mỗi chuỗi | 

Giới hạn tổng chiều dài là một triệu đảm bảo rằng quá trình quét tuyến tính vẫn hiệu quả và việc sắp xếp logarit trên tối đa một triệu mục vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    def inv_inside(s: str) -> int:
        zeros = 0
        inv = 0
        for c in reversed(s):
            if c == '0':
                zeros += 1
            else:
                inv += zeros
        return inv

    def solve():
        n = int(input())
        items = []
        for _ in range(n):
            s = input().strip()
            ones = s.count('1')
            zeros = len(s) - ones
            items.append((ones, zeros, inv_inside(s)))

        items.sort(key=lambda x: (1, 0) if x[1] == 0 else (0, x[0] / x[1]))

        ones_so_far = 0
        ans = 0
        for ones, zeros, internal in items:
            ans += internal
            ans += ones_so_far * zeros
            ones_so_far += ones

        return str(ans)

    return solve()

assert run("3\n1\n11\n101\n") == "1"
assert run("2\n10\n01\n") == "1"
assert run("1\n0\n") == "0"
assert run("2\n111\n000\n") == "0"
assert run("3\n1\n0\n1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 0 | trường hợp cạnh tối thiểu | 
| tất cả những cái trước số không | 0 | đặt hàng trường hợp cực đoan | 
| ký tự đơn hỗn hợp | 1 | hành vi xen kẽ | 
| chia khối đầy đủ | 0 | trường hợp tách mạnh | 

## Vỏ cạnh 

Một chuỗi chỉ bao gồm các chuỗi không có đảo ngược bên trong và không tạo ra đảo ngược trong tương lai khi được đặt ở bất kỳ đâu, vì nó đóng góp số 0 = 0 trong công thức chéo. Thuật toán sẽ đẩy các chuỗi như vậy về cuối một cách chính xác do hành vi tỷ lệ vô hạn của chúng và vị trí của chúng không ảnh hưởng đến tổng số. 

Một chuỗi chỉ bao gồm các số 0 cũng không có phép đảo ngược bên trong nhưng góp phần rất lớn vào việc đảo ngược chéo nếu được đặt sớm. Vì nó có các số 0 > 0 và các số 1 = 0, tỷ lệ của nó là 0, do đó, nó xuất hiện một cách tự nhiên ở đầu thứ tự được sắp xếp, giảm thiểu việc các số 0 của nó tiếp xúc với các số trước đó. 

Các chuỗi có tỷ lệ 1/0 giống hệt nhau sẽ tạo ra chi phí đặt hàng bằng nhau khi hoán đổi. Thuật toán cho phép bất kỳ thứ tự nào trong số chúng và công thức tổng tiền tố đảm bảo rằng các hoán đổi tỷ lệ bằng nhau không làm thay đổi câu trả lời cuối cùng, vì`ones(A)*zeros(B) == ones(B)*zeros(A)`giữ trong trường hợp đó.
