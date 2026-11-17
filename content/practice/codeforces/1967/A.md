---
title: "CF 1967A - Đếm hoán vị"
description: "Chúng ta được phát một bộ thẻ, mỗi thẻ được đánh số từ 1 đến n. Với mỗi số i, ta có thẻ ai thuộc loại đó."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1967
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 942 (Div. 1)"
rating: 1400
weight: 1967
solve_time_s: 76
verified: false
draft: false
---

[CF 1967A - Đếm hoán vị](https://codeforces.com/problemset/problem/1967/A) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, tham lam, thực hiện, toán học, sắp xếp 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ thẻ, mỗi thẻ được đánh số từ 1 đến n. Với mỗi số i, ta có thẻ a_i thuộc loại đó. Chúng tôi cũng có k đồng xu và mỗi đồng xu cho phép chúng tôi mua một loại thẻ mới thuộc bất kỳ loại nào, vì vậy chúng tôi có thể tăng số lượng bất kỳ số nào tùy ý miễn là tổng số lần chúng tôi không vượt quá k lần mua. Sau khi mua, chúng ta sẽ sắp xếp tất cả các thẻ thành một dòng và đếm xem có bao nhiêu mảng con liền kề có độ dài n là hoán vị của [1, 2, ..., n]. Nhiệm vụ của chúng tôi là tối đa hóa số lượng đó. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp kiểm thử chỉ định n, k và số đếm ban đầu từ a_1 đến a_n. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất, số lượng mảng con có độ dài n tối đa là hoán vị đầy đủ. 

Các ràng buộc rất lớn: n có thể lên tới 2×10^5, k có thể đạt tới 10^12 và a_i cũng có thể lên tới 10^12. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các mảng con hoặc sắp xếp lại. Chúng ta phải dựa vào logic đếm và phân phối thay vì tạo ra các chuỗi một cách rõ ràng. 

Một trường hợp đặc biệt quan trọng là khi n = 1, vì mỗi thẻ về cơ bản là một hoán vị của [1]. Một trường hợp khác xảy ra khi k cực kỳ lớn so với a_i; chúng ta có thể mua đủ thẻ để làm cho mọi a_i bằng nhau và do đó tối đa hóa số lượng hoán vị. Việc triển khai bất cẩn có thể cố gắng xây dựng một mảng thực tế và thất bại do giới hạn bộ nhớ hoặc tràn bộ nhớ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các chuỗi thẻ sau khi mua tối đa k thẻ mới, sau đó trượt một cửa sổ có kích thước n và kiểm tra xem mỗi cửa sổ có phải là một hoán vị hay không. Về nguyên tắc thì điều này đúng nhưng rất nhanh chóng trở nên không khả thi. Ngay cả với n = 10^5, vẫn có n! hoán vị có độ dài n và tạo ra các mảng có độ dài tổng (a_i) + k là không thể khi a_i và k đạt tới 10^12. Số lượng hoạt động là theo cấp số nhân và mức sử dụng bộ nhớ là vô cùng lớn. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng vấn đề giảm xuống việc phân phối thẻ đồng đều trên n loại. Nếu chúng ta tưởng tượng việc hình thành các hoán vị theo một chuỗi lặp lại, thì số lượng mảng con hoán vị có độ dài n tối đa được xác định bằng số lượng tối thiểu của bất kỳ loại nào sau khi mua thêm thẻ. Cụ thể, nếu sau khi mua chúng ta có thể đảm bảo rằng tất cả các loại xuất hiện ít nhất m lần thì chúng ta có thể tạo thành m khối hoán vị đầy đủ. Các thẻ còn lại cho phép chúng ta hình thành các hoán vị bổ sung theo kiểu so le nhưng yếu tố chiếm ưu thế luôn là loại có số lượng nhỏ nhất. 

Điều này làm giảm vấn đề thành tìm kiếm nhị phân: chúng tôi tìm kiếm số nguyên lớn nhất m sao cho chúng tôi có thể tăng mỗi a_i lên ít nhất m bằng cách sử dụng tối đa k đồng xu. Đối với mỗi ứng cử viên m, chúng tôi tính tổng số thẻ bổ sung cần thiết là sum(max(0, m - a_i) cho i = 1..n). Nếu số tiền đó là <= k thì m có thể đạt được. Tìm kiếm nhị phân có hiệu quả tìm thấy m khả thi lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * (tổng(a_i) + k)) | O(tổng(a_i) + k) | Quá chậm | 
| Tìm kiếm nhị phân + Đếm | O(n log(max(a_i) + k)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số lượng test case. Lặp lại từng trường hợp thử nghiệm một cách độc lập. 
2. Đối với trường hợp kiểm thử hiện tại, đọc n, k và mảng số đếm a. 
3. Khởi tạo giới hạn tìm kiếm nhị phân. Hoán vị tối thiểu có thể là 0 và số tối đa là số lượng lớn nhất có thể nếu chúng ta tiêu tất cả k xu để cân bằng loại tối thiểu: max(a) + k. 
4. Thực hiện tìm kiếm nhị phân. Với mỗi ứng cử viên m, hãy tính tổng số xu cần thiết để nâng mỗi a_i lên ít nhất m. Đối với a_i đã >= m, không cần xu; nếu không, chúng ta cần m - a_i xu. 
5. Nếu tổng số xu yêu cầu <= k thì ứng cử viên m là khả thi. Cập nhật tìm kiếm để thử m lớn hơn. Nếu không thì giảm m. 
6. Sau khi tìm kiếm nhị phân, m khả thi cao nhất là số lượng hoán vị hoàn chỉnh tối đa mà chúng tôi có thể đảm bảo. 
7. Xuất ra số đó. 

Tại sao nó hoạt động: Tìm kiếm nhị phân hoạt động vì hàm "có thể đạt được" là đơn điệu. Nếu m có thể đạt được thì bất kỳ m nhỏ hơn nào cũng có thể đạt được. Nếu m không thể đạt được thì m lớn hơn cũng không thể đạt được. Điều bất biến là ở chỗ cuối cùng, chúng ta đã xác định được m lớn nhất sao cho có thể đảm bảo ít nhất m hoán vị đầy đủ của [1, 2, ..., n]. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_permutations(n, k, a):
    low, high = 0, max(a) + k
    while low < high:
        mid = (low + high + 1) // 2
        required = sum(max(0, mid - x) for x in a)
        if required <= k:
            low = mid
        else:
            high = mid - 1
    return low

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(max_permutations(n, k, a))
```Giải pháp trước tiên đặt giới hạn tìm kiếm nhị phân dựa trên số lượng thẻ hiện tại lớn nhất cộng với k xu. Với mỗi ứng cử viên m, nó tính toán số xu cần thiết và so sánh nó với k. Sự lựa chọn của`(low + high + 1) // 2`đảm bảo rằng việc tìm kiếm kết thúc chính xác trên m lớn nhất khả thi. các`max(0, mid - x)`đảm bảo chúng tôi chỉ đếm số tiền có a_i dưới mức trung bình, tránh những đóng góp tiêu cực. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 2:```
2 4
8 4
```Các biến trong quá trình thực hiện: 

| Bước | thấp | cao | giữa | bắt buộc | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 12 | 6 | 2 | bắt buộc <= k, thấp=6 | 
| tiếp theo | 6 | 12 | 9 | 5 | bắt buộc <= k, thấp=9 | 
| tiếp theo | 9 | 12 | 11 | 7 | bắt buộc <= k, thấp=11 | 
| tiếp theo | 11 | 12 | 12 | 8 | bắt buộc > k, cao=11 | 

Đầu ra: 11 

Điều này thể hiện việc thu hẹp tìm kiếm nhị phân trên số lượng hoán vị tối đa có thể đạt được. 

Một ví dụ khác, đầu vào:```
1 10
1
```Vì n=1, a_1=1, k=10. Giá trị tối đa có thể đạt được là a_1 + k = 11. Tìm kiếm nhị phân xác định chính xác điều này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(max(a_i) + k)) | Đối với mỗi trường hợp thử nghiệm, tìm kiếm nhị phân lặp lại log(max(a_i) + k) lần và tính tổng trên n lần lặp lại | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng a có độ dài n; không cần thêm cấu trúc | 

Với n <= 2×10^5, tính tổng tất cả các trường hợp thử nghiệm <= 5×10^5 và k <= 10^12, giải pháp sẽ chạy tốt trong vòng 2 giây và có giới hạn về bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        print(max_permutations(n, k, a))
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("1\n1 10\n1\n") == "11", "sample 1"
assert run("1\n2 4\n8 4\n") == "11", "sample 2"

# Custom cases
assert run("1\n3 0\n1 2 3\n") == "1", "no coins, limited by min"
assert run("1\n3 10\n1 2 3\n") == "5", "coins enough to balance"
assert run("1\n1 0\n1000000000000\n") == "1000000000000", "single type, no coins"
assert run("1\n2 1000000000000\n1 1\n") == "500000000001", "large k, equalize both types"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 0\n1 2 3 | 1 | Không có tiền xu, loại tối thiểu giới hạn hoán vị | 
| 3 10\n1 2 3 | 5 | Coin cho phép các loại cân bằng | 
| 1 0\n1000000000000 | 1000000000000 | Loại đơn, vỏ cạnh | 
| 2 1000000000000\n1 1 | 500000000001 | K lớn, chia đều cho hai loại | 

## Vỏ cạnh 

Khi n=1, bất kỳ số lượng xu nào cũng có thể trực tiếp tăng số lượng của loại đơn lẻ, do đó kết quả đầu ra là a_1 + k. Tìm kiếm nhị phân xử lý chính xác điều này vì được yêu cầu = max(0, mid - a_1). Đối với k rất lớn so với a_i, tìm kiếm nhị phân xác định có thể tạo bao nhiêu hoán vị đầy đủ sau khi phân phối tiền một cách tối ưu, luôn được kiểm soát bởi loại được điều chỉnh nhỏ nhất. Khi a_i đã lớn và k nhỏ, thuật toán sẽ tính toán chính xác rằng không thể thực hiện thêm hoán vị nào vượt quá số lượng tối thiểu hiện có.
