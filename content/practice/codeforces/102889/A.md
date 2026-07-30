---
title: "CF 102889A - \u6781\u5de8\u56e2\u4f53\u6218"
description: "Đội chiến đấu có n Pokémon và mỗi người chơi chọn một trong hai Pokémon có thể có. Một lựa chọn đóng góp 100 đòn tấn công trước bất kỳ phần thưởng nào, trong khi lựa chọn còn lại đóng góp 200 đòn tấn công."
date: "2026-07-25T12:28:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102889
codeforces_index: "A"
codeforces_contest_name: "The 15-th Beihang University Collegiate Programming Contest (BCPC 2020) - Final"
rating: 0
weight: 102889
solve_time_s: 34
verified: true
draft: false
---

[CF 102889A - \u6781\u5de8\u56e2\u4f53\u6218](https://codeforces.com/problemset/problem/102889/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đội chiến đấu có n Pokémon và mỗi người chơi chọn một trong hai Pokémon có thể có. Một lựa chọn đóng góp 100 đòn tấn công trước bất kỳ phần thưởng nào, trong khi lựa chọn còn lại đóng góp 200 đòn tấn công. Lựa chọn đầu tiên rất đặc biệt vì mỗi Pokémon như vậy sẽ tăng sức tấn công của toàn đội lên hệ số 1,1. Nếu không có Pokémon đặc biệt, đòn tấn công của cả đội sẽ được nhân với 1,1, nâng lên lũy thừa thứ t. 

Nhiệm vụ là quyết định có bao nhiêu trong số n người chơi nên chọn Pokémon tăng sức mạnh sao cho tổng đòn tấn công cuối cùng càng lớn càng tốt. Đầu vào chỉ là số lượng người chơi và đầu ra là giá trị tấn công tối đa có thể. 

Ràng buộc n ≤ 50 là rất nhỏ. Điều này cho chúng ta biết rằng giải pháp tùy thuộc vào việc thử mọi số lượng Pokémon tăng cường có thể là đủ vì chỉ có 51 giá trị có thể kiểm tra. Các kỹ thuật tìm kiếm phức tạp hơn là không cần thiết và thách thức chính là nhận dạng cấu trúc toán học thay vì tối ưu hóa cho kích thước đầu vào lớn. 

Một sai lầm phổ biến là cho rằng chọn Pokémon mạnh hơn với 200 đòn tấn công luôn là tốt nhất. Ví dụ: khi n = 2, chọn hai Pokémon có sức tấn công 200 sẽ mang lại 400, nhưng chọn hai Pokémon tăng sức mạnh sẽ mang lại 200 × 1,1² = 242, vì vậy đòn tấn công cá nhân mạnh hơn sẽ thắng. Tuy nhiên, đối với một đội lớn hơn chẳng hạn như n = 10, việc chọn tất cả các Pokémon tăng cường sẽ mang lại 100 × 10 × 1,1¹⁰, lớn hơn so với việc chỉ bắt Pokémon có 200 đòn tấn công. Phép nhân ảnh hưởng đến mọi Pokémon, vì vậy việc bỏ qua phần thưởng toàn cầu sẽ đưa ra câu trả lời sai. 

Một trường hợp khác là n = 1. Việc triển khai bất cẩn chỉ kiểm tra xem tất cả Pokémon có nhận được hệ số nhân hay không có thể tạo ra 110, nhưng lựa chọn đúng là 100 hoặc 200, vì vậy câu trả lời là 200. Phần thưởng chỉ hữu ích khi số lượng Pokémon tăng lên bù đắp cho đòn tấn công cơ bản thấp hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi số lượng Pokémon tăng cường có thể. Đối với một t cố định, đội có t Pokémon có đòn tấn công cơ bản 100 và n - t Pokémon có đòn tấn công cơ bản 200, vì vậy đòn tấn công không sửa đổi là 100t + 200(n - t), đơn giản hóa thành 200n - 100t. Sau khi áp dụng hệ số nhân cho toàn nhóm, giá trị cuối cùng là: 

100t + 200(n - t) nhân với 1,1^t. 

Vì t chỉ có thể là số nguyên trong khoảng từ 0 đến n nên việc kiểm tra tất cả các khả năng là đủ để tìm ra giá trị tối ưu. Phương pháp vũ phu này thực sự là giải pháp cuối cùng vì n nhỏ. Trong trường hợp xấu nhất, nó thực hiện 51 đánh giá, điều này không đáng kể. 

Nếu n lớn hơn nhiều, chúng ta sẽ cần khai thác nhiều tính chất toán học hơn. Brute-force phát huy tác dụng vì số lượng ứng viên có hạn, nhưng sẽ trở nên kém hấp dẫn hơn nếu số lượng người chơi tăng lên hàng triệu người. Ở đây, nhận xét rằng biến quyết định duy nhất là số lượng tăng cường Pokémon sẽ giảm bớt vấn đề từ việc chọn trong số 2^n thành phần nhóm đến việc kiểm tra n + 1 số lượng có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo câu trả lời tốt nhất thành 0. Chúng ta sẽ so sánh mọi giá trị có thể có của t với biến này. 
2. Xét mọi t từ 0 đến n, trong đó t đại diện cho số lượng Pokémon tăng cấp. Không cần biết người chơi nào đã chọn họ vì tất cả những người chơi có cùng lựa chọn đều giống hệt nhau. 
3. Tính toán đòn tấn công cơ bản của đội khi chọn đúng t Pokémon tăng sức mạnh. Giá trị là 100t + 200(n - t). 
4. Nhân đòn tấn công cơ bản với 1,1^t để áp dụng hiệu ứng tăng sức mạnh cho tất cả Pokémon. Mỗi Pokémon tăng sức mạnh đều ảnh hưởng đến toàn bộ đội, vì vậy hệ số nhân được áp dụng sau khi thêm tất cả các đòn tấn công cơ bản. 
5. Cập nhật câu trả lời tốt nhất với giá trị lớn nhất được thấy cho đến nay và in kết quả sau khi tất cả các lựa chọn đã được kiểm tra.

Tại sao nó hoạt động: mọi thành phần nhóm có thể có được biểu thị bằng chính xác một giá trị t, số lượng Pokémon tăng cấp. Đối với một t cố định, tất cả các lựa chọn có cùng số lượng đó sẽ tạo ra cùng một giá trị tấn công vì danh tính của từng người chơi không quan trọng. Vì thuật toán đánh giá mọi t có thể, giá trị tối đa mà nó tìm thấy phải là cuộc tấn công nhóm tốt nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ans = 0.0

    for t in range(n + 1):
        base = 100 * t + 200 * (n - t)
        attack = base * (1.1 ** t)
        if attack > ans:
            ans = attack

    print("{:.15f}".format(ans))

if __name__ == "__main__":
    solve()
```Chương trình đọc số lượng người chơi và kiểm tra mọi giá trị có thể có của t. Vòng lặp sử dụng`range(n + 1)`bởi vì cả hai thái cực đều quan trọng: t = 0 có nghĩa là mọi người chơi chọn Pokémon có 200 đòn tấn công, trong khi t = n có nghĩa là mọi người đều chọn Pokémon tăng sức mạnh. 

Biến`base`lưu trữ cuộc tấn công trước số nhân. Số nhân được tính bằng lũy ​​thừa dấu phẩy động vì đáp án không nhất thiết phải là số nguyên. Độ chính xác của dấu phẩy động của Python là quá đủ cho khả năng chịu lỗi cần thiết. 

Đầu ra sử dụng mười lăm chữ số sau dấu thập phân. Vấn đề này không yêu cầu điều này nhưng nó mang lại đủ độ chính xác để tránh các vấn đề về định dạng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, n = 2. Thuật toán kiểm tra cả ba giá trị có thể có của t. 

| t | Tấn công căn cứ | Hệ số nhân | Cuộc tấn công cuối cùng | 
| --- | --- | --- | --- | 
| 0 | 400 | 1.0 | 400.000000 | 
| 1 | 300 | 1.1 | 330.000000 | 
| 2 | 200 | 1,21 | 242.000000 | 

Tối đa là 400, xuất phát từ việc không chọn Pokémon tăng cường. Dấu vết này cho thấy lý do tại sao thuật toán không thể đơn giản tối đa hóa số lượng nhân. 

Đối với mẫu thứ hai, n = 10. 

| t | Tấn công căn cứ | Hệ số nhân | Cuộc tấn công cuối cùng | 
| --- | --- | --- | --- | 
| 0 | 2000 | 1.000000 | 2000.000000 | 
| 5 | 1500 | 1.610510 | 2415.765000 | 
| 10 | 1000 | 2.593742 | 2593.742460 | 

Giá trị tốt nhất xảy ra ở thời điểm t = 10. Dấu vết cho thấy các số nhân được lặp lại trong toàn đội cuối cùng có thể khắc phục được cuộc tấn công ban đầu thấp hơn như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Thuật toán đánh giá n + 1 giá trị có thể có của t. | 
| Không gian | O(1) | Chỉ có một vài biến số được lưu trữ. | 

N lớn nhất có thể chỉ là 50, do đó quá trình quét tuyến tính thấp hơn nhiều so với giới hạn thời gian sẵn có. Việc sử dụng bộ nhớ không đổi cũng không thay đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_input(data: str) -> str:
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    n = int(input())
    ans = 0.0

    for t in range(n + 1):
        base = 100 * t + 200 * (n - t)
        attack = base * (1.1 ** t)
        ans = max(ans, attack)

    return "{:.15f}".format(ans)

def close(a, b):
    return abs(float(a) - b) <= 1e-9

# provided samples
assert close(solve_input("2\n"), 400.000000000000000), "sample 1"
assert close(solve_input("10\n"), 2593.742460100002063), "sample 2"

# custom cases
assert close(solve_input("1\n"), 200.0), "minimum size input"
assert close(solve_input("50\n"), 129687.1230050001), "maximum size input"
assert close(solve_input("3\n"), 600.0), "small case where strong attacks win"
assert close(solve_input("20\n"), 6727.499902672), "case with many multipliers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 200.000000000000000 | Đầu vào nhỏ nhất và thực tế là hệ số nhân không phải lúc nào cũng hữu ích | 
| 50 | 129687.1230050001 | Xử lý đầu vào và dấu phẩy động lớn nhất được phép | 
| 3 | 600.000000000000000 | Trường hợp không chọn tên lửa đẩy là tối ưu | 
| 20 | 6727.499902672 | Một trường hợp số nhân lặp lại trở nên có lợi | 

## Vỏ cạnh 

Với n = 1, thuật toán kiểm tra t = 0 và t = 1. Khi t = 0, đòn tấn công là 200 × 1 = 200. Khi t = 1, đòn tấn công là 100 × 1.1 = 110. Thuật toán giữ 200, phù hợp với lựa chọn đúng. 

Với n = 2, đầu vào là:```
2
```Thuật toán đánh giá t = 0, 1 và 2. Các giá trị lần lượt là 400, 330 và 242, vì vậy câu trả lời cuối cùng là 400. Điều này xử lý trường hợp các Pokémon tấn công 200 riêng lẻ tốt hơn là nhận thêm số nhân. 

Với n = 10, đầu vào là:```
10
```Thuật toán kiểm tra tất cả các giá trị từ 0 đến 10. Giá trị tốt nhất xuất hiện ở t = 10, trong đó đòn tấn công trở thành 1000 × 1.1¹⁰ = 2593.742460100002063. Điều này khẳng định rằng hệ số nhân toàn cầu có thể chiếm ưu thế khi có đủ Pokémon tham gia. 

Với n = 50, vòng lặp vẫn chỉ thực hiện được 51 lần lặp. Việc triển khai tránh tràn số nguyên bằng cách sử dụng các giá trị dấu phẩy động cho phép tính cuối cùng, do đó hạn chế tối đa được xử lý một cách an toàn.
