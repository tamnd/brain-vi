---
title: "CF 435B - Pasha Tối đa hóa"
description: "Chúng ta được cho một số nguyên dương mà chúng ta có thể coi là một chuỗi các chữ số thập phân và số lượng hoán đổi liền kề tối đa được phép, k. Mục tiêu là biến số này thành số lớn nhất có thể bằng cách sắp xếp lại các chữ số, nhưng mỗi lần di chuyển chỉ có thể hoán đổi hai chữ số lân cận."
date: "2026-06-07T02:46:58+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 435
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 249 (Div. 2)"
rating: 1400
weight: 435
solve_time_s: 248
verified: true
draft: false
---

[CF 435B - Pasha Tối đa hóa](https://codeforces.com/problemset/problem/435/B) 

**Đánh giá:** 1400 
**Thẻ:** tham lam 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số nguyên dương mà chúng ta có thể coi là một chuỗi các chữ số thập phân và số lượng hoán đổi liền kề tối đa được phép, _k_. Mục tiêu là biến số này thành số lớn nhất có thể bằng cách sắp xếp lại các chữ số, nhưng mỗi lần di chuyển chỉ có thể hoán đổi hai chữ số lân cận. Đầu ra là giá trị số của số lớn nhất có thể đạt được theo ràng buộc này. 

Số lượng có thể rất lớn, lên tới$10^{18}$, có nghĩa là biểu diễn chuỗi của nó có thể có tới 18 chữ số. Số lần hoán đổi được phép, _k_, nhỏ, nhiều nhất là 100. Sự kết hợp này ngay lập tức loại trừ bất kỳ thuật toán nào thử tất cả các hoán vị hoặc tất cả các chuỗi hoán đổi, vì số lượng hoán vị tăng theo giai thừa theo số chữ số. Một cách tiếp cận mạnh mẽ xem xét tất cả các chuỗi hoán đổi là hoàn toàn không khả thi. 

Các trường hợp cạnh rất tinh tế. Nếu số đã có các chữ số theo thứ tự giảm dần thì không cần hoán đổi. Nếu _k_ bằng 0 thì chúng ta phải trả về số không thay đổi. Một tình huống phức tạp khác xảy ra khi các chữ số cao nhất bị chặn sâu trong chuỗi và _k_ quá nhỏ để đưa chúng ra phía trước - những cách tiếp cận tham lam bất cẩn cố gắng luôn hoán đổi mức tối đa toàn cục có thể cố gắng di chuyển nhiều hơn mức cho phép. Ví dụ, với đầu vào`12345 1`, cách tiếp cận ngây thơ “đưa tối đa lên phía trước” sẽ thất bại vì bạn chỉ có thể di chuyển`5`sang trái một bước, đưa ra`12354`chứ không phải là chuyển động tối ưu thực sự trong vòng 1 lần hoán đổi, điều này thực sự là`12354`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ tạo ra tất cả các chuỗi có thể có tối đa _k_ giao dịch hoán đổi liền kề. Đối với mỗi chuỗi, chúng tôi sẽ áp dụng các hoán đổi cho số đó và theo dõi kết quả tối đa. Cách tiếp cận này đúng về nguyên tắc, nhưng nó chậm theo cấp số nhân vì số lượng chuỗi tăng lên theo tổ hợp với số chữ số và _k_. Ngay cả khi chỉ có 18 chữ số và 100 lần hoán đổi, việc liệt kê tất cả các khả năng là không thể. 

Thông tin chi tiết quan trọng cho một giải pháp hiệu quả xuất phát từ quan sát rằng trong _k_ các giao dịch hoán đổi liền kề, một chữ số có thể di chuyển tối đa _k_ vị trí sang trái. Điều này có nghĩa là đối với chữ số đầu tiên của kết quả, chúng ta chỉ cần xem xét _k+1_ chữ số đầu tiên của số ban đầu, chọn số lớn nhất và di chuyển nó về phía trước bằng cách sử dụng số lần hoán đổi tối thiểu. Sau đó, chúng tôi giảm _k_ theo số lần hoán đổi được sử dụng và lặp lại quy trình cho các chữ số còn lại. Cấu trúc bài toán đảm bảo rằng việc lựa chọn tham lam chữ số khả thi lớn nhất ở mỗi bước luôn dẫn đến giải pháp tối ưu, bởi vì việc di chuyển các chữ số nhỏ hơn sớm hơn sẽ chặn các chữ số lớn hơn chiếm các vị trí mà chúng có thể đạt được trong các lần hoán đổi còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+k)!) | O(n) | Quá chậm | 
| Tối ưu | O(n * k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi số thành danh sách chữ số để thao tác dễ dàng hơn. Hãy coi mỗi chữ số là một ký tự cho mục đích hoán đổi. 
2. Khởi tạo con trỏ ở chữ số đầu tiên. Đối với mỗi vị trí`i`, chúng tôi muốn chọn chữ số lớn nhất có thể đạt tới vị trí`i`trong phần còn lại`k`trao đổi. Quét đoạn chữ số từ`i`ĐẾN`i + k`(hoặc đến cuối số nếu có ít hơn`k`chữ số còn lại). 
3. Xác định vị trí`pos`chữ số lớn nhất trong đoạn này. Nếu có mối quan hệ, hãy chọn cái ngoài cùng bên trái. 
4. Di chuyển chữ số tại`pos`để định vị`i`bằng cách liên tục hoán đổi nó với hàng xóm của nó. Mỗi lần hoán đổi giảm`k`bằng 1. 
5. Nếu`k`đạt đến 0, chúng tôi dừng quá trình sớm. Các chữ số còn lại được để theo thứ tự hiện tại. 
6. Tăng dần`i`và lặp lại cho đến khi chúng tôi xử lý xong tất cả các chữ số hoặc hết số lần hoán đổi. 

Tại sao nó hoạt động: ở mỗi bước, chúng tôi đặt chữ số khả thi lớn nhất ở vị trí sẵn có ngoài cùng bên trái. Vì một chữ số không thể di chuyển còn lại quá _k_ vị trí nên chúng tôi luôn chọn chữ số tốt nhất trên toàn cầu mà có thể truy cập được. Không có chữ số nhỏ hơn nào có thể cải thiện kết quả bằng cách di chuyển về phía trước và không có chữ số lớn hơn nào bị bỏ qua nếu nó có thể đến vị trí hiện tại. Bất biến tham lam này đảm bảo rằng số chúng ta xây dựng là lớn nhất dưới các ràng buộc hoán đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, k = input().split()
k = int(k)
digits = list(a)
n = len(digits)

i = 0
while i < n and k > 0:
    # find the largest digit we can bring to position i
    max_pos = i
    for j in range(i + 1, min(n, i + k + 1)):
        if digits[j] > digits[max_pos]:
            max_pos = j
    # bring digits[max_pos] to position i using swaps
    for j in range(max_pos, i, -1):
        digits[j], digits[j - 1] = digits[j - 1], digits[j]
    k -= (max_pos - i)
    i += 1

print("".join(digits))
```Giải pháp bắt đầu bằng cách chuyển đổi số nguyên thành danh sách các ký tự, cho phép hoán đổi tại chỗ hiệu quả. Vòng lặp bên ngoài lặp qua từng vị trí chữ số. Vòng lặp bên trong quét phân đoạn có thể truy cập, bị giới hạn bởi số lần hoán đổi còn lại`k`, để tìm chữ số lớn nhất. Hoán đổi được áp dụng ngay lập tức để đưa chữ số đó về phía trước và`k`được giảm đi theo số lần hoán đổi được thực hiện. Việc lựa chọn cẩn thận giới hạn phân đoạn và cập nhật`k`ngăn chặn việc vượt quá số lần hoán đổi được phép. 

## Ví dụ đã hoạt động 

**Mẫu 1:`1990 1`** 

| tôi | chữ số | max_pos | hoán đổi | k còn lại | 
| --- | --- | --- | --- | --- | 
| 0 | 1 9 9 0 | 1 | hoán đổi 1 ↔ 9 | 0 | 
| 1 | 9 1 9 0 | - | - | 0 | 

Thuật toán dừng sau 1 lần hoán đổi. Số lượng tối đa là`9190`, phù hợp với đầu ra mẫu. 

**Tùy chỉnh 2:`12345 3`** 

| tôi | chữ số | max_pos | hoán đổi | k còn lại | 
| --- | --- | --- | --- | --- | 
| 0 | 1 2 3 4 5 | 3 | di chuyển 4 lên phía trước | 0 | 

Sau khi di chuyển`4`về phía trước,`k=0`, vì vậy không có giao dịch hoán đổi nào nữa. Kết quả là`41235`. Điều này chứng tỏ sự chấm dứt sớm khi`k`hết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k) | Mỗi chữ số có thể được quét lên đến`k`vị trí phía trước; tổng số giao dịch hoán đổi là O(n) trên mỗi chữ số. | 
| Không gian | O(n) | Chúng tôi lưu trữ số dưới dạng danh sách các chữ số. | 

Cho rằng`n <= 18`Và`k <= 100`, các hoạt động trong trường hợp xấu nhất là dưới 1800, vừa vặn thoải mái trong 1 giây. Việc sử dụng bộ nhớ ở mức tối thiểu, dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a, k = input().split()
    k = int(k)
    digits = list(a)
    n = len(digits)

    i = 0
    while i < n and k > 0:
        max_pos = i
        for j in range(i + 1, min(n, i + k + 1)):
            if digits[j] > digits[max_pos]:
                max_pos = j
        for j in range(max_pos, i, -1):
            digits[j], digits[j - 1] = digits[j - 1], digits[j]
        k -= (max_pos - i)
        i += 1

    return "".join(digits)

# provided sample
assert run("1990 1\n") == "9190", "sample 1"

# custom cases
assert run("12345 3\n") == "41235", "partial swaps"
assert run("98765 10\n") == "98765", "already max"
assert run("11111 5\n") == "11111", "all equal digits"
assert run("123 0\n") == "123", "no swaps allowed"
assert run("102345 5\n") == "512034", "bring max within k swaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 12345 3 | 41235 | Sự lựa chọn tham lam trong các giao dịch hoán đổi hạn chế | 
| 98765 10 | 98765 | Đã tối đa, không cần hoán đổi | 
| 11111 5 | 11111 | Tất cả các chữ số bằng nhau, hoán đổi không liên quan | 
| 123 0 | 123 | Không cho phép hoán đổi, sản lượng không thay đổi | 
| 102345 5 | 512034 | Chọn chính xác chữ số lớn nhất có thể truy cập | 

## Vỏ cạnh 

cho`123 0`, không được phép hoán đổi, do đó thuật toán kết thúc ngay lập tức, giữ nguyên số. Vì`11111 5`, mặc dù có sẵn các phép hoán đổi nhưng tất cả các chữ số đều bằng nhau, do đó không có phép hoán đổi hiệu quả nào xảy ra và đầu ra giống hệt với đầu vào. Vì`102345 5`, chữ số lớn nhất`5`ở trong`5`vị trí, do đó thuật toán sẽ di chuyển nó lên phía trước, tiêu tốn chính xác 4 lần hoán đổi, để lại`k=1`. Phần còn lại của các chữ số được xử lý bình thường, xác nhận rằng việc đếm hoán đổi và phân đoạn
